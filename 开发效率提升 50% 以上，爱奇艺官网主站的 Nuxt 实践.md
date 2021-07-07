# 开发效率提升 50% 以上，爱奇艺官网主站的 Nuxt 实践

## 背景

让每一个用户获取到**稳定、及时**的页面体验，是前端工程师们一直以来努力的方向。



作为一个拥有丰富内容资源的视频网站，爱奇艺官网主站需要频繁进行节目上线或者下线、各种活动配置等操作调整，对于**页面 SSR 服务的可用性及稳定性**，都有着极高的要求。



在 **2019 年之前**，爱奇艺官网主站页面的 SSR 采用的是在 **CMS 平台中书写 Velocity 模板**，由 Java 编译，**优点是渲染速度快**，**但缺点也非常明显**：



（1）**在 CMS 平台中开发体验不好：**没有传统 IDE 方便，不能配置快捷键、不能安装插件等等，导致开发效率低下。



（2）**前后端代码不同构：**由于后端使用 Velocity 模板，而前端需要使用 Vue，导致前后端代码不同构。



（3）**破坏 Vue 组件封装性：**由于 Java 无法编译 Vue 组件，所有的 Vue 组件都需要用 Slot 的方式在 CMS 平台中书写以达到 SEO 和 SSR 的目的。



基于以上所有原因，我们决定使用 **Node** 来进行 SSR。因为我们的**前端框架是 Vue**，因此我们选择了**配套的 Nuxt 框架进行 SSR**。



使用 Nuxt 进行 SSR，**难点并不在于如何使用 Nuxt，而在于如何维护这个服务，保证其性能、稳定性等**，因此，本文将**不会介绍 Nuxt 的使用**，其语法可以参考官网，这里将主要从**性能、缓存、限流、灾备、日志**等几个方面来介绍我们是如何**保证 Nuxt 服务的可用性及稳定性的。**

## Nuxt 稳定性提升之路

### 2.1 页面配置

首先介绍一个很重要的**配置文件**。在我们的**项目根目录**下，创建了一个页面配置文件，用来存放每个页面的**通用配置**，例如页面的**缓存配置、Purge 信息、主题色配置、广告信息配置**等等，该文件导出一个 Object, 键值为页面的 Router Name，Value 值为页面的配置信息：

```
// configs/pageinfo.js
export default {
 'dianshiju-id': {...},
 'zongyi': {
     theme: 'dark', // 页面主题色配置
  },
 'home2020': {...},
 'rank-hot': {...}
}

```

然后我们在 Nuxt 插件中根据请求的**路由信息**，读取**对应的页面配**置，并将其注入到所有的**组件实例中**，方便随时取用：

```
// plugins/pageinfo.js
import config from 'configs/pageinfo.js'
export default ({ route }, inject) => {
     inject('pageInfo', config[route.name]) // 注入页面配置信息
}
```

因此你可以在组件的**任何地方**获取到页面配置信息而不需要通过 Props 一层层传递，页面通用配置也**不会散落在项目各个地方**，方便统一管理。

```
<div :class="$pageInfo.theme">我是综艺页面</div>
```

### 2.2 浏览器容性

虽然 Nuxt 理论上可以支持 IE9，但 IE9 在很多方面都**需要添加 Polyfill**，例如对 History API 的支持等，为了保持**代码的简洁性**，我们放弃了支持 IE9-，但我们依然在框架中**保留了一套机制来支持 jQuery**，使得高低版本可以**共用 HTML**，而不需要单独为低版本写模板，从而最大程度的减少兼容低版本浏览器的成本。

大致思路为，Nuxt 提供了一个**’render.route’**的钩子函数，该钩子函数的执行时机在生成 HTML 后，返回给用户之前。在这个钩子函数中，我们可以根据用户请求的 UA 信息判断用户版本，如果是低版本浏览器用户则移除 HTML 中高版本 JS 并注入低版本打包后的入口文件即可。

```
 // nuxt.config.js
    'render:route': (url, result, { req }) => {
      if (isLowBrowser(req)) { // 根据用户ua信息判断是否是低版本
        const $ = cheerio.load(result.html)
       $('body script[src*=\'pcw/ssr\']').remove() // 移除高版本js
        $('body').append('<script src="//stc.iqiyipic.com/jquery.js"></script') // 添加jquery
        $('body').append('<script src="//stc.iqiyipic.com/index.js"></script') // 添加低版本入口js
        result.html = $.html()
      }
```

### 2.3 性能优化

#### 2.3.1 数据过滤

Nuxt 有一个很重要的机制在于，它会将**所有 asyncData 函数返回的数据挂在`window.__NUXT__`上**，通过 HTML 返回给客户端，从而避免客户端再次请求这些数据，因此，**asyncData 函数**中返回的数据量对性能的影响变得更加重要，它不仅影响了接口数据的**传输时间**，还影响了 **HTML 的体积**，因此，我们需要对这些数据进行压缩，在 NUXT 中，我们尝试了**三种方案**：

**1.在 asyncData 中做数据过滤**

**2. GraphQL**

**3. 数据过滤平台**

在 asyncData 中做数据过滤仅减少了 HTML 体积，却并**无法减少**冗余数据的传输。

GraqhQL 虽然解决了冗余数据的传输问题，但**代码不利于维护**，因为它需要**写大量的查询参数**, 查询参数太长时还需要使用 POST。

```
// 非常不利于维护的查询字符串
const query = `
query {
  qipuGetVideoBriefList (
    album_id: "${params.album_id}"
    type: "EPISODE_LIST"
    play_platform: "PC_QIYI"
    order: "DESC"
  ) {
    rpc_status
    episode {
      id
      g_corner_mark_s
      brief {
        title
        short_title
        subtitle
        page_url
      }
      release {
        publish_time
      }
    }
  }
}
`
axios.get(`http://xxx.iqiyi.com/graphql?query=${query}`)
```

最终，我们搭建了一个**数据过滤平台**，以**可视化**的方式来配置接口数据源、数据的字段过滤和映射，最终生成一个接口，该接口从配置的数据源获取数据，然后经过字段映射和字段过滤，仅仅返回我们需要的字段，这样既过滤了冗余数据又不需要维护 GraphQL 的查询参数，而是将 GraphQL 的查询串可视化为配置。



![img](https://static001.geekbang.org/infoq/d8/d8934cea2339034803df8243ac8c91d0.png)

#### 2.3.2 Layout

Nuxt 提供了 Layout 配置项，看似非常的方便，但通过分析 Nuxt 生成的.nuxt/App.js 入口文件，我们发现**所有的 Layout 不管有没有被使用到，都会被打包进来**，例如 A 页面使用了 LayoutA, B 页面使用了 LayoutB, C 页面使用了 LayoutC，则 A、B、C 三个页面的入口 JS 会有 LayoutA、LayoutB、LayoutC 的所有代码。=

```
// .nuxt/App.js
import _8daa19aa from '../src/layouts/a.vue'
import _8daa19a8 from '../src/layouts/b.vue'
import _8daa19a6 from '../src/layouts/c.vue'
import _6f6c098b from './layouts/default.vue'
```

因此，如果 **Layout 的逻辑很复杂**，并且如果代码量很大，所有页面的 **JS 体积就会变大许多**。基于以上原因，我们**放弃了使用 Nuxt 的 Layout**，而是自己封装了一个 **I71Layout 组件**来提供所有页面的通用功能，以减少冗余代码。

### 2.4 缓存

由于 Vue SSR 是基于**虚拟 DOM**，而 Java 是基于**字符串**，所以性能上相比之前会慢一些，因此我们从**页面和组件**两个粒度上做了**缓存策略**。



我们使用 **Nginx 反向代理**来控制页面级别的缓存，默认每个页面缓存 5 分钟，当 Nuxt 返回**非 200** 时，Nginx 则使用过期缓存返回。



组件缓存我们使用的是**官方的 @nuxtjs/component-cache 模块**，它提供了一个 **serverCacheKey 配置项**，Nuxt 会以这个配置项的值作为缓存的 Key。因此我们为每个需要缓存的组件定义了一个 cache-key 的 Props, 传递后则会根据传递的值做缓存，未传递则无缓存。这样对于所有无缓存的页面在调用组件时，可以传递一个 cache-key 来使得组件被缓存，从而加速页面的 SSR。

### 2.5 purge

对于有缓存的页面，我们需要**对应的 Purge 接口**来清除页面缓存。页面的 Purge 分为**两个部分**，**一部分是我们 Nginx 反向代理的缓存 Purge, 另一部分是 CDN 缓存的 Purge**，他们的 Purge 原理相同，因此这里我们只讲 Nuxt 服务的 Nginx 反向代理的缓存 Purge。

我们希望提供一个 Purge 接口，通过**传递页面名参数来 Purge 指定的页面**。我们的 Nuxt 框架本身是基于 **Koa 搭建**，所以我们只需要在 SSR 之前插入 koa-router，就可以提供我们的 Purge 接口。

```
// server/index.js
const app = new Koa()
const router = new Router()
router.get('/api/purge/page/:pageName', async (ctx) => { // 定义purge接口，支持传递pageName
  ctx.body = await purgePage(ctx) // purge nginx缓存和cdn缓存
})
app.use(router.routes()) // 插入我们需要的api
app.use(ctx => { // nuxt 进行 ssr
    nuxt.render(ctx.req, ctx.res)
})
```

那我们如何知道每个 pageName 要 Purge 哪些 URL 呢？这里我们需要在之前提到的**页面配置文件**中进行配置来**将 pageName 和 Purge URL 关联起来**：

```
// configs/pageinfo.js
zongyi: {
   purge: {
      purgeUrl: [
        'https://zongyi.iqiyi.com/',
        'https://www.iqiyi.com/zongyi'
      ],
   },
}
```

接下来我们只需要 Purge 所有服务上的这些 URL，服务部署在公司的应用平台，一共有 **4 个集群**，上百个 **Docker 容器**，我们需要 Purge 所有宿主机上的 Nginx 缓存，具体操作如下：

首先我们需要在 Nginx 中配置让其支持 Purge:

```
location / {    proxy_cache_purge PURGE from all;}
```

这样就可以通过调用 **http://{宿主机的域名}:{宿主机的端口}/purge/{uri}**来 Purge 该宿主机上 uri 对应的缓存了。



接下来我们只需要**逐个调用**所有宿主机上的 Purge 接口就可以 Purge 所有的宿主机上的页面缓存了。

### 2.6 限流

对于无缓存页面，为了谨防恶意刷量行为，要进行限流。我们从 **WAF, 单 IP 限流, IP 黑名单**进行了三方面的限制。

#### 2.6.1 WAF(Web Application Firewall)

首先我们接入了公司的防火墙平台，通过智能识别以过滤掉一些恶意请求。其次，对于一些动态路由的页面，我们对请求的 URL 进行了正则匹配，不符合正则的请求全部拒绝访问并返回 403。

#### 2.6.2 单 IP 限流

为了防止**单 IP 脚本刷量**，我们在 Nginx 反向代理使用 limit_req 模块进行单 IP 限流。对于普通用户和爬虫，我们设置了不同的访问频次，**超过频次的请求拒绝访问并返回 503。**

#### 2.6.3 IP 黑名单

除此之外，我们通过日志分析会发现一些很明显的刷量 IP，对于这样的 IP，我们希望直接封禁。

如果直接在 Nginx 配置中添加 Deny 语句，会发现 **Deny 并不会生效**，是因为请求经过了网关，到我们的 Nginx 服务时，Remote Address 变成了网关的 IP，而我们 Deny 的是真实用户的 IP，所以我们需要想办法让 Nginx 知道用户的真实 IP 是什么。

通常用户的真实 IP 存储在 **x-forwarded-for 字段**中，为了拿到用户的真实 IP，我们需要在 Nginx 中做以下配置：

```
# nginx.conf
server {
    real_ip_header X-Forwarded-For; # 告诉Nginx,用户的真实IP存储在x-forwarded-for字段中
    real_ip_recursive on;
}
```

但光有以上配置还不够，因为 x-forwarded-for 字段为一个字符串，每经过一个节点，这个节点就会**向里面追加一个 IP**，所以到达我们的 Nginx 时，该字段的值为 **x-forwarded-for: {用户的真实 IP},{网关的 IP}**，而 Nginx 读取 IP 时，会默认从后往前读取 IP, 如果这个 IP 是受信任的 IP，则会继续往前读取，直到不被信任的 IP 就会当做是用户的真实 IP，因此，如果没有额外配置，Nginx 读取到的 IP 依然是网关的 IP，因此，我们还需要将**所有网关 IP 添加到信任 IP 的列表中**，Nginx 才能继续往前读取到用户的真实 IP。我们可以**将整个内网网段都设置成信任 IP:**

```
# nginx.conf
server {
     set_real_ip_from xxx.0.0.0/8; # 设置内网网段为信任IP
    real_ip_header X-Forwarded-For; # 告诉Nginx,用户的真实IP存储在x-forwarded-for字段中
    real_ip_recursive on;
}
```

现在 Nginx 可以读取到用户的真实 IP 了，这时候我们只需要创建一个 IP 黑名单即可：

```
# nginx.conf
server {
     set_real_ip_from xxx.0.0.0/8; # 设置内网网段为信任IP
    real_ip_header X-Forwarded-For; # 告诉Nginx,用户的真实IP存储在x-forwarded-for字段中
    real_ip_recursive on;
    include ip-blacklist.conf # 导入IP黑名单
}
# ip-blacklist.conf
deny xx.xx.xx.xx;
```

### 2.7 灾备

![img](https://static001.geekbang.org/infoq/91/911caba2d9fd61409058ac7dfb276807.png)



对于无缓存的页面，除了限流以外，我们还需要有**灾备方案**，否则一旦服务出错返回非 200，用户将看到错误页面。



我们部署了一套**独立的灾备服务**，使用 Node 脚本**每隔三分钟**从线上服务拉取所有重要页面，如果页面返回 200，则将其存储为 HTML 文件，否则抛弃该页面，然后使用 Nginx 做**反向代理**来 Serve 灾备页面。



CDN 先从**线上服务**拉取页面，若返回非 200，则从灾备服务拉取对应的页面返回给用户，以此保证**用户永远不会看见出错的页面。**

### 2.8 服务端日志

服务端日志主要用来记录 Nuxt 渲染页面的记录、错误信息等，它们对于**排查问题、统计流量**来说是非常重要的，我们的服务端日志分为**两大部分：页面渲染日志、接口请求日志。**

页面渲染日志即每一次来一个页面请求，则写一条日志，**记录页面的 URL、Referer、用户 Cookie、用户 IP 等信息**，若页面渲染**未出错**则**写入到 logs/page/info.log** 中，若页面**渲染出错**，**则写一条日志到 logs/page/error.log** 中。

接口日志是每一次页面渲染中发出的请求日志，封装在底层发送请求的 **HTTP 函数**中，记录了调用该接口的页面 URL、接口 URL、接口参数等信息，若请求成功，则写一条日志到 **logs/api/info.log**, 若请求失败，则写一条日志到 **logs/api/error.log** 中。

```
// nuxt.config.js
hooks: {
   'render:setupMiddleware': app => { // 在nuxt初始化时插入一个中间件，每次请求都生成一个logParams对象
      app.use(async (req, res, next) => {
        req.logParams = {
          requestId: generateRandomString(), // 生成requestId随机串
          pageUrl: req.url
        }
        next()
      })
    },
    'render:routeDone': (url, result, { req, res }) => { // 渲染完毕
      logger.page.info({ type: 'render', ...req.logParams}, req) // 写日志时带上requestId
    },
    'render:errorMiddleware': app => app.use(async (error, req, res, next) => { // 渲染错误
      logger.page.error({ type: 'render', error, ...req.logParams }, req) // 错误日志带上requestId
      next(error)
    }),
}
```

为了让页面渲染日志、这一次渲染的接口日志关联起来，我们会在渲染前生成一个唯一的 **RequestId**, 然后在**该次渲染的所有日志**中都带上这个 RequestId，就可以通过一个 RequestId 查询到页面渲染日志，以及这个页面发出去的所有请求日志了。

```js
// http.js class Resource {  async http (opts)
    let data
    try {
        data = await axios(opts)
        process.server && logger.api.info(opts, this.req.logParams) // api日志带上requestid
     } catch (error) {
         process.server && logger.api.error(opts, error, this.req.logParams) // api错误日志带上requestid
     }
     return data
  }
}
```

### 2.9 日志采集

我们采用了 **Filebeat + Elasticsearch + Kibana 进行日志管理**，**首先**通过 Filebeat 进行实时日志采集，**然后**上报至指定 kafka 集群，**然后**对日志进行分析并建立索引，**最终**生成一个可视化的日志查询页面，这样我们就可以查看一段时间内符合查询条件的日志了。



![img](https://static001.geekbang.org/infoq/99/991194877fcbbae780f3e15f47fbd846.png)

### 2.10 流量监控

基于服务端日志，就可以据此统计流量经由了 **CDN 的缓存、WAF 的拦截、Nginx 反向代理的缓存**，最后计算出到达我们的 Nuxt 服务的实际流量到底有多少。我们可以根据日志的 time 字段筛选出指定时间段且 type= 'render'的日志，就是该时间段内 Nuxt 服务承受的总流量了，如果想看各个页面的流量，还可以进一步对日志中的 pageUrl 字段进行筛选。



![img](https://static001.geekbang.org/infoq/25/25ce438a09c59b8751aff599c36dca0b.png)

## 总结

Nuxt 从根本上解决了之前在 CMS 平台使用 Velocity 开发遇到的**所有问题**，但同时也带来了一些**别的问题**，例如**域名冲突的问题、服务端变量共享的问题、渲染性能问题等**。不过总体来说，瑕不掩瑜，开发体验得到了**质的提升**，**开发效率提升了 50%以上**；**组件复用率**更高、**组件封装性**更好，**代码可读性**、**可维护性**都得到了**飞跃性**的提升；在 CDN 缓存、Nginx 反向代理缓存、组件缓存的强力加持下，页面的渲染性能也并没有下降；由于移除了一些由于**前后端代码**不一致、大量使用 Slot 等一些复杂逻辑后，首屏渲染性能反而提高了许多，服务器响应时间维持在**平均 0.5s** 左右，错误率维持在 **0.2%**左右，而在有灾备服务兜底的情况下，**可访问性也几乎达到 100%**。



最后，期待 **Nuxt3** 的到来以及性能和开发体验上的进一步提升。