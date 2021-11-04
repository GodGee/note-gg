# vue中v-model和computed使用问题（was assigned to but it has no setter）

v-model最基础的用法是在data中声明，再进行绑定

```
data(){
    return {
        value: 5
    }
}
```

但是当需要的value是动态的，或者是vuex中对象的值、或是父组件中传入的变量值，

严格模式中直接修改会抛出一个错误：...was assigned to but it has no setter



解决方案：

官方文档中给出了解决办法：https://vuex.vuejs.org/zh/guide/forms.html

使用带有setter的双向绑定计算属性

```
computed: {
    value: {
        get: function () { return this.defaultValue } ,
        set: function (val) { this.changeValue(val) }
    }
}
```

//set、get内容依据使用场景进行调整
如果set函数不加入事件，控制台不会抛出错误，但实现不了双向绑定的效果
 