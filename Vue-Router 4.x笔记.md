# Vue-Router 4.x笔记

`Vue 3.x`版本已经发布。对应新版本的`Vue`，`vue-router 4.x`做了许多突破性的变化。

## 安装

使用`createRouter`替换`new Router()`

- 以前版本写法

```
// router/index.js
import Vue from 'vue'
import Router from 'vue-router'
import routes from './routes'

Vue.use(Router)

const router = new Router({
  routes
})
export default router

// main.js
import Vue from 'vue'
import router from './router'
// ...

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

- `vue-router 4.x`版本写法

```
// router/index.js
import { createRouter } from 'vue-router'
import routes from './routes'

const router = createRouter({
  routes
})

// main.js
import { createApp } from 'vue'
import router from './router'

const app = createApp(App)
app.use(router)
app.mount('#app')
```

- 所有导航都是异步的，如果使用`transition`，需要在安装应用程序之前等待路由器准备就绪

```
app.use(router)
// router.onReady() 已经替换为 router.isReady()
// 不带任何参数并返回 Promise
router.isReady().then(() => app.mount('#app'))
```

## mode修改

- `mode: 'history'`选项已替换为更灵活的名称`history`

| `vue-router 3.x` | `vue-router 4.x`         |
| :--------------- | :----------------------- |
| `history`        | `createWebHistory()`     |
| `hash`           | `createWebHashHistory()` |
| `abstract`       | `createMemoryHistory()`  |

- `base`选项移除，修改为`createWebHistory`等方法的第一个参数传递

```
// 原写法
import Vue from 'vue'
import Router from 'vue-router'
import routes from './routes'

Vue.use(Router)

const router = new Router({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})
export default router

// 新写法
import { createRouter, createWebHistory } from 'vue-router'
import routes from './routes'

const router = createRouter({
  // history: createWebHistory(),
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

- 在`SSR`中，需要手动传递适当的`history`

```
let history = isServer ? createMemoryHistory() : createWebHistory()
let router = createRouter({ routes, history })
// somewhere in your server-entry.js
router.push(req.url) // request url
router.isReady().then(() => {
  // resolve the request
})
```

## Composition API

### useRouter、useRoute

```
import { watch, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'

export default {
  setup() {
    const router = useRouter()
    const route = useRoute()
    const userData = ref()

    // 返回 name 是 About 的路由
    // 不存在会报错
    console.log(router.resolve({ name: 'About' }))

    watch(
      () => route.params,
      async newParams => {
        userData.value = await fetchUser(newParams.id)
      }
    )

    function pushWithQuery(query) {
      router.push({
        name: 'search',
        query: {
          ...route.query
        }
      })
    }

    return {
      pushWithQuery,
      userData
    }
  }
}
```

### onBeforeRouteLeave、onBeforeRouteUpdate

```
import { ref } from 'vue'
import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'

async function fetchUser(id) {}

export default {
  setup() {
    const userData = ref()

    onBeforeRouteLeave((to, from) => {
      const answer = window.confirm(
        'Do you really want to leave? you have unsaved changes!'
      )
      if (!answer) return false
    })

    onBeforeRouteUpdate(async (to, from) => {
      if (to.params.id !== from.params.id) {
        userData.value = await fetchUser(to.params.id)
      }
    })

    return {
      userData
    }
  }
}
```

## 动态路由

> - `router.addRoute(route: RouteRecord)`：动态添加路由
> - `router.removeRoute(name: string | symbol)`：动态删除路由
> - `router.hasRoute(name: string | symbol)`: 判断路由是否存在
> - `router.getRoutes()`: 获取路由列表

每当删除路由时，其**所有别名**和**后代路由**都会随之删除

```
router.addRoute({ path: '/about', name: 'about', component: About })
// name 应该是唯一的。会先删除原路由再添加新路由
router.addRoute({ path: '/other', name: 'about', component: Other })

const removeRoute = router.addRoute(routeRecord)
// 删除添加的路由
removeRoute()

router.addRoute({ path: '/about', name: 'about', component: About })
// 添加的时候有name的话，可以直接使用删除对应name的路由
router.removeRoute('about')

// 嵌套路由
router.addRoute({ name: 'admin', path: '/admin', component: Admin })
// 第一个参数admin是父路由的名称
router.addRoute('admin', { path: 'settings', component: AdminSettings })

const routeRecords = router.getRoutes()
```

## 导航守卫

通常用于检查用户是否有权限访问某个页面，验证动态路由参数，或者销毁监听器

自定义逻辑运行后，`next` 回调用于确认路由、声明错误或重定向

`vue-router 4.x`中可以从钩子中返回一个值或`Promise`来代替

> 返回值：
>
> - `false`：取消当前导航。如果浏览器`URL`被更改（由用户手动或通过后退按钮更改），它将被重置为该`from`路由的`URL`
> - 一个`router location`：重定向到不同的位置，通过传递，如果你是调用路径位置`router.push()`，它允许您通过类似的选项`replace: true`或`name: 'home'`。删除当前导航，并使用导航创建新导航`from`
> - 什么也不返回，`undefined`或者`true`返回任何内容，则导航被验证，并调用下一个
> - 引发异常：取消导航并调用回调`router.onError()`

```
// 原写法
router.beforeEach((to, from, next) => {
  if (!isAuthenticated) {
    next(false)
  } else {
    next()
  }
})

// 新写法
router.beforeEach(() => isAuthenticated)
```

## router-view、keep-alive、transition

`transition`和`keep-alive`必须放在`router-view`里面

```
<router-view v-slot="{ Component }">
  <transition>
    <keep-alive>
      <component :is="Component" />
    </keep-alive>
  </transition>
</router-view>
```