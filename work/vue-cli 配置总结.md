### vue-cli 配置总结

##### vue-cli webpack打包后还能看到源码？怎么改 如何避免源码泄露

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTgzMDY5,size_16,color_FFFFFF,t_70)

##### vue.js - 解决vue-cli打包后自动压缩代码

当我们用vue脚手架做完项目后，npm run build打包之后，

有没有查看源码，全是压缩好的。但是我就不想让它压缩，该怎么办呢？

 

首先，我们得了解一点点webpack的知识。

webpack中压缩js 的插件叫 uglifyjs-webpack-plugin，

压缩css 的插件叫 optimize-css-assets-webpack-plugin

然后我们找到/build/webpack.prod.conf.js 文件，

然后你会发现：

```
const OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
```

然后我们就可以在页面中搜索OptimizeCSSPlugin 和 UglifyJsPlugin 这两个关键词所在的地方

配置如下：



```
    // css 压缩代码，将下面代码注释掉
    new OptimizeCSSPlugin({
      cssProcessorOptions: config.build.productionSourceMap
        ? { safe: true, map: { inline: false } }
        : { safe: true }
    }),
```



```
   // 压缩js代码，将下面代码注释掉
    new UglifyJsPlugin({
      uglifyOptions: {
        compress: {
          warnings: false
        }
      },
      sourceMap: config.build.productionSourceMap,
      parallel: true
    }),
```



只要将上面代码注释掉，npm run build 你就会发现，欧了。

然后是html 了，

配置如下：

```
new HtmlWebpackPlugin({
      filename: process.env.NODE_ENV === 'testing'
        ? 'index.html'
        : config.build.index,
      template: 'index.html',
      inject: true,
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
        // more options:
        // https://github.com/kangax/html-minifier#options-quick-reference
      },
```



这里我们将上面的 minify 改成 minify：false

 removeComments: false,
 collapseWhitespace: false,
 removeAttributeQuotes: false