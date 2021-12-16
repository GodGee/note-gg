#### vue打包时候报错（压缩css错误）

##### 报错信息

   1.window打包通过，Linux 报错

2. building for production...Error processing file: static/css/app.e8b75d3d19abc5bbb
3. unhandlePromiseRejectionWarning:Unhandled promise rejection (rejection id:2):TypeError:Object.entries is not a function

##### 解决方案一（可行）

报错信息如下

```
\ building for production...Error processing file: static/css/app.e8b75d3d19abc5bbbd9bd916f459f0d0.css
 
(node:16424) UnhandledPromiseRejectionWarning: CssSyntaxError: E:\simou\static\css\app.e8b75d3d19abc5bbbd9bd916f459f0d0.css:1995:6: Unknown word
 
    at Input.error (E:\simou\node_modules\optimize-css-assets-webpack-plugin\node_modules\postcss\lib\input.js:130:16)
.
.
.
(node:16424) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 2)
 
(node:16424) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

在webpack.prod.conf.js中注释 （这是webpack压缩css的配置）

     const OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')
     
    new OptimizeCSSPlugin({
     
          cssProcessorOptions: config.build.productionSourceMap
     
            ? { safe: true, map: { inline: false } }
     
            : { safe: true }
     
        }),
注释之后，可正常打包项目，但是css没有被压缩

在utils.js种添加minimize:true

```
const cssLoader = {
    loader: 'css-loader',
    options: {
      sourceMap: options.sourceMap,
      minimize:true
    }
  }
```

这样，项目就可以正常打包，css也可压缩

（搜索结果报错是css语法错误，可是也找不到到底是哪里错误，最后只能如此解决，项目打包后可以正常上线）
 

OptimizeCSSPlugin 是用来优化和压缩文件的，注释掉就不会压缩，再次build打包的时候可以看到dist目录下多了很多文件。
然后测试，再执行一次npm run build,在dist/static/css目录下看到css文件有报错，定位到报错的哪一行，就可以发现有一个文件里面的一句css代码，用了 ‘//’来注释了，
其实是不可以这样子注释的，我们找到这个原文件，把css前的//去掉，然后把刚才注释的//const OptimizeCSSPlugin恢复，重新npm run build就可以了
那问题来了，为什么突然就有这个问题，之前我的项目是可以正常跑起来的，今天从git上拉取回来就出这个问题。
探索了原因：
1.首先看看OptimizeCSSPlugin 这个插件是那里引入，查看package.json，搜索到"optimize-css-assets-webpack-plugin": {"version": "3.2.0"｝，但是在package-lock.json看到optimize-css-assets-webpack-plugin": {"version": "3.2.1"｝
2.发现package.json和package-lock.json的optimize-css-assets-webpack-plugin插件版本没对上
3.分析发现，optimize-css-assets-webpack-plugin-3.2.0没有严格校验，所以可以打包通过，3.2.1有严格校验，所以打包出异常了
4.再次分析，为什么重新执行了npm install，会出现package.json和package-lock.json的依赖版本对不上？正常来说是一致的
5.再查询资料，结论是：npm或者cnpm 安装依赖，不会完全按照package.json中的版本号来，会有稍微的差异，这样的差异可能导致项目起不来，或者报错， 因为某些包只有特定的版本才能正常运行。

##### 解决方案二

一直运行很好的项目突然build报错了，错误信息如下：

```groovy
ERROR in static/js/vendor.f1c68aa2d5e85847d30e.js from UglifyJs

Unexpected token name «i», expected punc «;» [./node_modules/element-ui/src/utils/merge.js:2,0][static/js/vendor.f1c68aa2d5e85847d30e.js:17064,11]

Build failed with errors.
```



在 UglifyJs 的 github issues #78 找到了这样一个解决方案：由于 UglifyJs 只支持 ES5 而 element-ui 可能引入了一部分 ES6 的写法，所以导致 webpack 打包失败。issue 里最后给出的解决方案是用 beta 版本的Uglify-es 来代替 UglifyJs（Beta 版本引入了对 ES2015+）的支持。需要在前端工作目录下用执行命令 `npm i -D uglifyjs-webpack-plugin@beta`。

不过在尝试过后，发现 build error 的问题依然没有解决，在深入查找问题所在后，决定用 bable 来解析 element-ui， 要完成此操作只需要修改前端文件夹下的build/webpack.base.conf.js 文件即可，修改如下：
修改前

```javascript
module: {
rules: [
...
{
test: /\.js$/,
loader: 'babel-loader',
include: [resolve('src'), resolve('test')]
},
```

修改后

```javascript
 module: {
rules: [
...
{
test: /\.js$/,
loader: 'babel-loader',//注意elementUI的源码使用ES6需要解析
include: [resolve('src'), resolve('test'),resolve('/node_modules/element-ui/src'),resolve('/node_modules/element-ui/packages')]
},
...
```



相当于将 element-ui 加入需要 babel 解析的包中。

之后再次执行 `npm run build`， build 成功。