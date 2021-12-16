### iBlM 预生产打包报错问题整理

##### 1.问题：element ui  版本不一致导致的问题

   **解决方式：注释该行，当前版本不存在该组件。**

![image-20211116100216280](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211116100216280.png)



##### 2.问题：菜单重复点击报错 via a navigation guard

   **解决方式： 将main.js 下面的代码移植到 router文件下的index.js。**

```
const originalPush = Router.prototype.push
Router.prototype.push = function push (location, onResolve, onReject) {
  if (onResolve || onReject){
    return originalPush.call(this, location, onResolve, onReject)
  }
  return originalPush.call(this, location).catch(err => err)
}

Vue.use(Router) // router版本 ^3.1.3
```

![image-20211116100327753](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211116100327753.png)

##### 3.问题：vue打包时候报错（压缩css错误）window 可行，Linux报错

   **解决方式：**

````
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
````

