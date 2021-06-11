### 预渲染 vue-prerender-spa

如果你调研服务器端渲染 (SSR) 只是用来改善少数营销页面（例如 `/`, `/about`, `/contact` 等）的 SEO，那么你可能需要**预渲染**。无需使用 web 服务器实时动态编译 HTML，而是使用预渲染方式，在构建时 (build time) 简单地生成针对特定路由的静态 HTML 文件。优点是设置预渲染更简单，并可以将你的前端作为一个完全静态的站点。

1. vue init webpack prerender-spa

2. npm i prerender-spa-plugin -D

3. 在webpack.prod.conf.js 文件里配置预渲染插件

   ```javascript
   const PrerenderSPAPlugin = require("prerender-spa-plugin");
   //调用渲染器
   const Renderer = PrerenderSPAPlugin.PuppeteerRenderer;
   
   new PrerenderSPAPlugin({
   staticDir: path.join(\_\_dirname, "../dist"),
   routes: ["/", "/about"],
      //这个配置很重要，如果没有这个，也不会进行预编译
      renderer: new Renderer({
      inject: {
       foo: "bar"
         },
       headless: false,
      //在项目的入口中使用document.dispatchEvent('render-event')
      renderAfterDocumentEvent: "render-event"
      })
     })
   ```

4. 目录结构

   ![image-20210519105908163](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210519105908163.png)

5. npm run build

6. dist文件夹下多出来about文件夹

   ![image-20210519105943920](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210519105943920.png)

7. npm install http-server  -g

8. 在dist文件夹下运行命令 hs -o -p 8088

### 服务端渲染 SSR vue-server-renderer

##### 什么是服务端渲染

Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。

有助于大家更好的理解SSR

```
npm init -yes
npm i vue express vue-server-renderer -S
```

创建server.js文件

```
const Vue = require('vue')
const express = require('express')()
const renderer = require('vue-server-renderer').createRenderer()

//创建vue实例
const app = new Vue({
  template: '<div>Hello gg</div>',
})

//服务端渲染的核心在于 通过vue-server-renderer插件的renderToString() 方法，将Vue实例转换成字符串插入到html文件中
express.get('/', (req, res) => {
  renderer.renderToString(app, (err, html) => {
    if (err) {
      return res.state(500).end('运行错误')
    }
    res.send(`<!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>服务器渲染</title>
      </head>
      <body>
        ${html}
      </body>
    </html>
    `)
  })
})

express.listen(8088, () => {
  console.log('服务已启动……')
})

```

ssr文件目录结构：

![image-20210518100923505](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210518100923505.png)

启动服务

```
node server.js
```

总结：

服务端渲染的核心就在于：通过vue-server-renderer 插件的renderToString()方法，将Vue实例转换为字符串插入到html文件。

我们使用服务端渲染是为了弥补单页面应用SEO能力不足的问题

因此，实际上我们第一次在浏览器地址输入url