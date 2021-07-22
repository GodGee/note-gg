# nuxt中引入svg

## 1、安装：

```
npm install svg-sprite-loader -D
```

## 2.svg-sprite-loader加载，nuxt.config.js配置如下

官方文档使用webpack进行配置 [Nuxt extend](https://link.zhihu.com/?target=https%3A//zh.nuxtjs.org/api/configuration-build%23extend)

`assets/icons/svg` 目录是我存放svg文件的目录

PS: 默认情况下 Nuxt 使用 vue-loader、file-loader 以及 url-loader 这几个 Webpack 加载器来处理文件的加载和引用。
对于不需要通过 Webpack 处理的静态资源文件，可以放置在 static 目录中。
所以需要排除其他加载器对SVG文件的处理 参考[GitHub Issue Nuxtjs + Vue-svg-loader](https://link.zhihu.com/?target=https%3A//github.com/nuxt/nuxt.js/issues/1332) 

`nuxt.config.js` 看注释吧

```
build: {
    extend (config,ctx) {
      // 排除 nuxt 原配置的影响,Nuxt 默认有vue-loader,会处理svg,img等
      // 找到匹配.svg的规则,然后将存放svg文件的目录排除
      const svgRule = config.module.rules.find(rule => rule.test.test('.svg'))
      svgRule.exclude = [resolve(__dirname, 'assets/icons/svg')]
      //添加loader规则
      config.module.rules.push({
        test: /\.svg$/, //匹配.svg
        include: [resolve(__dirname, 'assets/icons/svg')], //将存放svg的目录加入到loader处理目录
        use: [{ loader: 'svg-sprite-loader',options: {symbolId: 'icon-[name]'}}]
      })
    }
 },
```

## 3.创建`SvgIcon.vue`组件,包装SVG便于引用

创建 `components/SvgIcon.vue`

```
<template>
  <div v-if="isExternal" :style="styleExternalIcon" class="svg-external-icon svg-icon" v-on="$listeners" />
  <svg v-else :class="svgClass" aria-hidden="true" v-on="$listeners">
    <use :xlink:href="iconName" />
  </svg>
</template>

<script>
export default{
  name: 'SvgIcon',
  props: {
    iconClass: {
      type: String,
      required: true
    },
    className: {
      type: String,
      default: ''
    }
  },
  computed: {
    isExternal() {
      return /^(https?:|mailto:|tel:)/.test(this.iconClass)
    },
    //通过iconClass 获取svg文件名
    iconName() {
      return `#icon-${this.iconClass}`
    },
    svgClass() {
      if (this.className) {
        return 'svg-icon ' + this.className
      } else {
        return 'svg-icon'
      }
    },
    //返回url请求位置
    styleExternalIcon() {
      return {
        mask: `url(${this.iconClass}) no-repeat 50% 50%`,
        '-webkit-mask': `url(${this.iconClass}) no-repeat 50% 50%`
      }
    }
  }
}   
</script>

<style scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}

.svg-external-icon {
  background-color: currentColor;
  mask-size: cover!important;
  display: inline-block;
}
</style>
```

## 4.增加插件([Nuxt plugin 使用 vue插件](https://link.zhihu.com/?target=https%3A//zh.nuxtjs.org/guide/plugins%23%E4%BD%BF%E7%94%A8-vue-%E6%8F%92%E4%BB%B6))加载`SvgIcon.vue`组件

Nuxt.js允许您在运行Vue.js应用程序之前执行js插件.
我们需要在程序运行前配置好这个插件。

创建 `plugins/svg-icon.js`

```
import Vue from 'vue'
import SvgIcon from '@/components/SvgIcon'// Nuxt 默认@指向根目录 

// 注册组件
Vue.component('svg-icon', SvgIcon)
//预请求svg组件(通过之前的svg-sprite-loader加载)
const req = require.context('@/assets/icons/svg', false, /\.svg$/)
const requireAll = requireContext => requireContext.keys().map(requireContext)
requireAll(req)
```

#### 注册在`nuxt.congfig.js`中注册`plugins/svg-icon.js`插件 修改 `nuxt.config.js`

```
    //增加 plugins 配置
    //nuxt 会在vue.js程序运行前,执行所有注册的插件
    plugins:{
        '@/plugins/svg-icon' //注册svg插件文件 
    },
```

## 5.页面中使用

下载SVG图标到 `assets/icons/svg/blog.svg` 然后在vue 模板中使用

```
        <!-- icon-class 的值,即svg文件名  -->
         <div style="font-size:16px;color:green;">
           <svg-icon icon-class="test" />
         </div>
```