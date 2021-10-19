### Vue处理静态资源及public/static/assets目录的区别

### Vue 是如何处理静态资源的？

Vue 静态资源可以通过两种方式进行处理：

1、在 JavaScript 被导入或在 template/CSS 中通过**相对路径**被引用。这类引用**会被 webpack 处理**。

2、放置在 `public` 目录下或**通过绝对路径引用**。这类资源将会**直接被拷贝**，而**不会经过 webpack 的处理**。

#### 从相对路径导入

当在 JavaScript、CSS 或 `*.vue` 文件中使用相对路径 (必须以 `.` 开头) 引用一个静态资源时，该资源将会被包含进 webpack 的依赖图中。编译过程中，所有诸如 `<img src="...">`、`background: url(...)` 和 CSS `@import` 的资源 URL 都会被解析为一个模块依赖。

例如，`url(./image.png)` 会被翻译为 `require('./image.png')`，而：

```javascript
<img src="./image.png">
```

将会被编译到：

```javascript
h('img', { attrs: { src: require('./image.png') }})
```

在其内部，Vue 通过 `file-loader` 用版本哈希值和正确的公共基础路径来决定最终的文件路径，再用 `url-loader` 将小于 4kb 的资源内联，以减少 HTTP 请求的数量。

可以通过 [chainWebpack](https://cli.vuejs.org/zh/config/#chainwebpack) 调整内联文件的大小限制。例如，下列代码会将其限制设置为 10kb：

```javascript
// vue.config.jsmodule.exports = {
  chainWebpack: config => {
    config.module      .rule('images')
        .use('url-loader')
          .loader('url-loader')
          .tap(options => Object.assign(options, { limit: 10240 }))
  }}
```

#### URL 转换规则

1、如果 URL 是一个绝对路径 (例如 `/images/foo.png`)，它将会被保留不变。

2、如果 URL 以 `.` 开头，它会作为一个相对模块请求被解释且基于你的文件系统中的目录结构进行解析。

3、如果 URL 以 `~` 开头，其后的任何内容都会作为一个模块请求被解析。这意味着你甚至可以引用 Node 模块中的资源：

```javascript
<img src="~some-npm-package/foo.png">
```

如果 URL 以 `@` 开头，它也会作为一个模块请求被解析。它的用处在于 Vue CLI 默认会设置一个指向 `<projectRoot>/src` 的别名 `@`。(仅作用于模版中)

### `public` 文件夹

任何放置在 `public` 文件夹的静态资源都会被简单的复制，而不经过 webpack 。需要通过绝对路径来引用。

注意 Vue 推荐将资源作为模块依赖图的一部分导入，这样会通过 webpack 的处理并获得如下好处：

1、脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。

2、文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。

3、最终生成的文件名包含了内容哈希，因此你不必担心浏览器会缓存它们的老版本。

`public` 目录提供的是一个应急手段，当通过绝对路径引用时，需要留意应用会部署到哪里。如果没有部署在域名的根部，需要为你的 URL 配置 [publicPath](https://cli.vuejs.org/zh/config/#publicpath) 前缀：

在 `public/index.html` 或其它通过 `html-webpack-plugin` 用作模板的 HTML 文件中，需要通过 `<%= BASE_URL %>` 设置链接前缀：

```javascript
<link rel="icon" href="<%= BASE_URL %>favicon.ico">
```

在模板中，首先需要向你的组件传入基础 URL：

```javascript
data () {
  return {
    publicPath: process.env.BASE_URL
  }
}
```

然后：

```javascript
<img :src="`${publicPath}my-image.png`">
```

#### 何时使用 `public` 文件夹

你需要在构建输出中指定一个文件的名字。

你有上千个图片，需要动态引用它们的路径。

有些库可能和 webpack 不兼容，这时你除了将其用一个独立的 `<script>` 标签引入没有别的选择。

### assets 和 static 目录的区别

其实这个问题应该是 webpack 打包的问题，我找了 webpack 的官方文档，并没有找到有关这两个目录的介绍。

网上查阅资料，给出的的结论是：

`assets` 目录，在编译过程中会被 webpack 处理，当做模块依赖，只支持相对路径的形式。一般放置可能会变动的文件。

`static` 目录，一般存放第三方文件，不会被 webpack 解析，会直接被复制到最终的打包目录（默认是 `dist/static` ）下，必须使用绝对路径引用，这些文件是不会变动的。

我也进行了多次尝试：

在项目的 `src` 目录分别创建了 `assets` 和 `static` 目录，在 `.vue` 文件中进行引用：

```javascript
<img src="@/assets/w3h5.png" alt="">
<img src="../assets/w3h5.png" alt="">
<img src="@/static/w3h5.png" alt="">
<img src="../static/w3h5.png" alt="">
```

发现都会处理，查看 Vue 的 `app.js` 源码可以发现：

```javascript
r("img", { attrs: { src: e("ca4c"), alt: "" }
ca4c: function(t, n, e) {
  t.exports = e.p + "img/w3h5.9c000238.png"
}
```

以上的引入，都被指向打包后的 `dist/img` 目录下的 `w3h5.9c000238.png` 文件。

比较“聪明”的是，如果在不同目录下放置相同的文件，会被统一处理为一个文件，放置在 `img` 目录中进行引用，大大节省了资源。

言归正传，`static` 目录并没有像上面所说的被原封不动的复制到 `dist/static` 目录下。

那么就是说只要在 `src` 目录下的文件都会被 webpack 处理？事情还没完，继续往下看。

接下来我又在根目录创建了 `static` 目录，进行引用。

```javascript
<img src="../../static/w3h5.png" alt="">
```

发现还是会被 webpack 编译。

但是如果放置在 `public` 目录进行引用，就不同了。

打包后 `w3h5.png` 被原封不动的复制到了 `dist` 目录下，而且是在根目录。

![image-20211008145811774](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211008145811774.png)

在网上查了一下，应该是较老版本的 Vue 静态资源是 `static` 目录，从 Vue 2.x 开始就换成 `public` 目录了。

新版本就把 `public` 视为之前的 `static` 目录就可以了。