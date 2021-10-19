#### element ui 二次封装组件 el-dialog 

##### 1.问题描述

-  子组件直接改变父组件的属性，导致报错
-  弹窗的关闭事件报错
- 归根结底是子组件直接改变父组件的属性
- 使用props，通过父组件给子组件传值，子组件在使用props中的属性时，直接对props中的属性进行了修改。修改方式为直接对props中的属性赋值，或者使用双向绑定。此时会抛出警告：vue.runtime.esm.js?2b0e:619 [Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop’s value. Prop being mutated: “activeName”. activeName 为Props中的一个属性名。
   

##### 2.解决方案

- 通过警告的提示信息可知，在子组件中不可以对父组件通过props传过来的属性进行修改。我们可以使用data或者计算属性转接一下。如果需要同步修改父组件中的值，则需使用$emit调用父组件中的方法对其进行修改。
- 在vue2中，直接修改prop是被视作反模式的。由于在新的渲染机制中，每当父组件重新渲染时，子组件都会被覆盖，所以应该把props看做是不可变对象。–[参考文档](https://blog.csdn.net/u014520745/article/details/75455979)

```
子组件 popup.vue
<el-dialog :visible.sync = 'visible' title="表单设置" @close='handleCancel' width='80%'></el-dialog>

visible:this.dialogVisible

watch:{
  dialogVisible(){
  this.visible = this.dialogVisible
  }
},

props:{
 dialogVisible:{
   type:Boolean,
   default:false
 }
}

methods:{
  this.$emit('update:dialogVisible',false)
},
```

```
父组件调用
<popup :dialogVisible.sync = 'visible' v-if='visible'></popup>

visible:false,


handlePopup{
  this.visible = true
}
```

##### 3.可以通过.sync修饰符来达到双向绑定的效果

- @close="$emit('update: QualityDialogFlag' , false)" 说明改变父组件的数据

- 不添加.sync修饰符, 虽然在关闭弹框的时候修改了父组件的数据, 但是下次再次打开的时候就会失败, 原因是父组件没有监听到子组件的数据改变, 父子组件没有双向绑定
- 添加`.sync` 修饰符, 添加之后可以实现父子组件的双向绑定, 当子组件修改父组件转递的数据之后, 父组件可以获取子组件的数据
  

 

