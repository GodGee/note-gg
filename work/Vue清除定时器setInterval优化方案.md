# Vue清除定时器setInterval优化方案

两种方案清除定时器，开发者经常使用方案1，建议使用方案2

**方案1**

首先我在data函数里面进行定义定时器名称：

```vue
 data() {      
  return {               
    timer: null // 定时器名称     
  }    
},
```

然后这样使用定时器：

```
this.timer = (() => {
  // 某些操作
}, 1000)
```

最后在beforeDestroy()生命周期内清除定时器：

```
beforeDestroy() {
  clearInterval(this.timer);    
  this.timer = null;
}
```

**方案1有两点不好的地方，引用尤大的话来说就是：**

它需要在这个组件实例中保存这个 timer，如果可以的话最好只有生命周期钩子可以访问到它。这并不算严重的问题，但是它可以被视为杂物。

我们的建立代码独立于我们的清理代码，这使得我们比较难于程序化的清理我们建立的所有东西。

**方案2**

该方法是通过$once这个事件侦听器器在定义完定时器之后的位置来清除定时器。

以下是完整代码：

```
 const timer = setInterval(() =>{          
  // 某些定时器操作        
}, 500);      
// 通过$once来监听定时器，在beforeDestroy钩子可以被清除。
this.$once('hook:beforeDestroy', () => {      
  clearInterval(timer);                  
})
```

**其他程序化的事件侦听器**

通过 $on(eventName, eventHandler) 侦听一个事件

通过 $once(eventName, eventHandler) 一次性侦听一个事件

通过 $off(eventName, eventHandler) 停止侦听一个事件

附官网详细地址：[程序化事件侦听器](https://cn.vuejs.org/v2/guide/components-edge-cases.html#程序化的事件侦听器)

**补充知识：****vue在mounted中创建定时器与清除定时器**

我就废话不多说了，大家还是直接看代码吧~

```
mounted(){
   var that=this;
    var num = 1;
    var ss='';
   var a=setInterval(()=>{
    a+=10;
     if(this num===100){
      ss='success';
      console.log(ss)  
      clearInterval(a)  //当num等于100时清除定时器
   } 
   }, 1000); 
  }

```