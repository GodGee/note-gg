# 浅谈vue中provide和inject 用法

provide：Object | () => Object
 inject：Array<string> | { [key: string]: string | Symbol | Object }
 provide 和 inject 主要为高阶插件/组件库提供用例。并不推荐直接用于应用程序代码中。是2.2.0版本 新增的。
 这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。

provide 选项应该是一个对象或返回一个对象的函数。该对象包含可注入其子孙的属性。在该对象中你可以使用 ES2015 Symbols 作为 key，但是只在原生支持 Symbol 和 Reflect.ownKeys 的环境下可工作。

inject 选项应该是：
 一个字符串数组，或
 一个对象，对象的 key 是本地的绑定名，value 是：
 在可用的注入内容中搜索用的 key (字符串或 Symbol)，或
 一个对象，该对象的：
 from 属性是在可用的注入内容中搜索用的 key (字符串或 Symbol)
 default 属性是降级情况下使用的 value

**使用场景：由于vue有$parent属性可以让子组件访问父组件。但孙组件想要访问祖先组件就比较困难。通过provide/inject可以轻松实现跨级访问祖先组件的数据**

一种最常见的用法是刷新vue组件
 app.vue



```xml
<template>
  <div
    id="app"
  >
    <router-view
      v-if="isRouterAlive"
    />
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    MergeTipDialog,
    BreakNetTip
  },
  data () {
    return {
      isShow: false,
      isRouterAlive: true
  },

// 父组件中返回要传给下级的数据
  provide () {
    return {
      reload: this.reload
    }
  },
  methods: {
    reload () {
      this.isRouterAlive = false
      this.$nextTick(() => {
        this.isRouterAlive = true
      })
    }
  }
}
</script>
```



```xml
<template>
  <popup-assign
    :id="id"
    @success="successHandle"
  >
    <div class="confirm-d-tit"><span class="gray-small-btn">{{ name }}</span></div>
    <strong>将被分配给</strong>
    <a
      slot="reference"
      class="unite-btn"
    >
      指派
    </a>
  </popup-assign>
</template>
<script>
import PopupAssign from '../PopupAssign'
export default {
//引用vue reload方法
  inject: ['reload'],
  components: {
    PopupAssign
  },
methods: {
    // ...mapActions(['freshList']),
    async successHandle () {
      this.reload()
    }
  }
}
</script>
```

这样就实现了子组件调取reload方法就实现了刷新vue组件的功能,个人认为它实现了组件跨越组件传递数据方法。
 下面一个例子祖组件的数据，祖孙元素调取
 Ancestor.vue



```xml
<template>
    <div id="app">
    </div>
</template>
    <script>
        export default {
            data () {
                    return {
                        datas: [
                            {
                                id: 1,
                                label: '产品一'
                            },
                            {
                                id: 1,
                                label: '产品二'
                            },
                            {
                                id: 1,
                                label: '产品三'
                            }
                        ]
                    }
            },
            provide {
                return {
                    datas: this.datas
                }
            }
        }
    </script>
```

后代组件



```xml
<template>
    <div>
        <ul>
        <li v-for="(item, index) in datas" :key="index">
            {{ item.label }}
        </li>
        </ul>
    </div>
</template>
    <script>
        export default {
            inject: ['datas']
        }
    </script>
```

后代元素引入被注入数据datas，并在组件内循环输出

实际上，你可以把依赖注入看作一部分“大范围有效的 prop”，除了：
 祖先组件不需要知道哪些后代组件使用它提供的属性
 后代组件不需要知道被注入的属性来自哪里

**提示：provide 和 inject 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。**



 