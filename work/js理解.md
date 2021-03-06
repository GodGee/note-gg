### js 个人总结

#### 1.js 是单线程的，为什么可以执行多个任务？   

- 关键字：<font color='red'>WebAPIs   竞争机制   事件循环</font>

-  javascript从诞生之日起就是一门<font color='red'>单线程的非阻塞的</font>脚本语言。这是由其最初的用途来决定的：<font color='red'>与浏览器交互</font>。

- 单线程意味着，javascript代码在执行的任何时候，都只有一个<font color='red'>主线程</font>来处理所有的任务。

- 非阻塞则是当代码需要进行一项<font color='red'>异步任务</font>（无法立刻返回结果，需要花一定时间才能返回的任务，如I/O事件）的时候，主线程会挂起（pending）这个任务，然后在异步任务返回结果的时候再根据一定规则去执行相应的回调。

- 单线程是必要的，也是javascript这门语言的基石，原因之一在其最初也是最主要的执行环境——浏览器中，我们需要进行各种各样的dom操作。试想一下 如果javascript是多线程的，那么当两个线程同时对dom进行一项操作，例如一个向其添加事件，而另一个删除了这个dom，此时该如何处理呢？因此，为了保证不会 发生类似于这个例子中的情景，javascript选择只用一个主线程来执行代码，这样就保证了程序执行的一致性。

- 当然，现如今人们也意识到，单线程在保证了执行顺序的同时也限制了javascript的效率，因此开发出了<font color='red'>web worker</font>技术。这项技术号称让javascript成为一门多线程语言。

- 然而，使用web worker技术开的多线程有着诸多限制，例如：所有新线程都受主线程的完全控制，不能独立执行。这意味着这些“线程” 实际上应属于主线程的子线程。另外，这些子线程并没有执行I/O操作的权限，只能为主线程分担一些诸如计算等任务。所以严格来讲这些线程并没有完整的功能，也因此这项技术并非改变了javascript语言的单线程本质。

- 可以预见，未来的javascript也会一直是一门单线程的语言。

- 话说回来，前面提到javascript的另一个特点是“非阻塞”，那么javascript引擎到底是如何实现的这一点呢？答案就是今天这篇文章的主角——<font color='red'>event loop（事件循环）</font>

#### 2.浏览器的渲染过程？

#### 3.回流与重绘？

这里Layout和Repaint的概念是有区别的：

- Layout，也称为Reflow，即回流。一般意味着元素的内容、结构、位置或尺寸发生了变化，需要重新计算样式和渲染树
- Repaint，即重绘。意味着元素发生的改变只是影响了元素的一些外观之类的时候（例如，背景色，边框颜色，文字颜色等），此时只需要应用新样式绘制这个元素就可以了

回流的成本开销要高于重绘，而且一个节点的回流往往回导致子节点以及同级节点的回流，
所以优化方案中一般都包括，尽量避免回流。

**什么会引起回流？**

```javascript
1.页面渲染初始化

2.DOM结构改变，比如删除了某个节点

3.render树变化，比如减少了padding

4.窗口resize

5.最复杂的一种：获取某些属性，引发回流，
很多浏览器会对回流做优化，会等到数量足够时做一次批处理回流，
但是除了render树的直接变化，当获取一些属性时，浏览器为了获得正确的值也会触发回流，这样使得浏览器优化无效，包括
    （1）offset(Top/Left/Width/Height)
     (2) scroll(Top/Left/Width/Height)
     (3) cilent(Top/Left/Width/Height)
     (4) width,height
     (5) 调用了getComputedStyle()或者IE的currentStyle
```

回流一定伴随着重绘，重绘却可以单独出现

所以一般会有一些优化方案，如：

- 减少逐项更改样式，最好一次性更改style，或者将样式定义为class并一次性更新
- 避免循环操作dom，创建一个documentFragment或div，在它上面应用所有DOM操作，最后再把它添加到window.document
- 避免多次读取offset等属性。无法避免则将它们缓存到变量
- 将复杂的元素绝对定位或固定定位，使得它脱离文档流，否则回流代价会很高

**注意：改变字体大小会引发回流**

再来看一个示例：

```javascript
var s = document.body.style;

s.padding = "2px"; // 回流+重绘
s.border = "1px solid red"; // 再一次 回流+重绘
s.color = "blue"; // 再一次重绘
s.backgroundColor = "#ccc"; // 再一次 重绘
s.fontSize = "14px"; // 再一次 回流+重绘
// 添加node，再一次 回流+重绘
document.body.appendChild(document.createTextNode('abc!'));
```

#### 4.事件循环机制？

#####  4.1浏览器环境下js引擎的事件循环机制

- 执行栈与事件队列

- 当javascript代码执行的时候会将不同的变量存于内存中的不同位置：<font color='red'>堆（heap）和栈（stack）</font>中来加以区分。其中，堆里存放着一些<font color='red'>对象</font>。而栈中则存放着一些<font color='red'>基础类型变量以及对象的指针</font>。 但是我们这里说的执行栈和上面这个栈的意义却有些不同。

- 我们知道，当我们调用一个方法的时候，js会生成一个与这个方法对应的执行环境（context），又叫执行上下文。这个执行环境中存在着这个方法的私有作用域，上层作用域的指向，方法的参数，这个作用域中定义的变量以及这个作用域的this对象。 而当一系列方法被依次调用的时候，因为js是单线程的，同一时间只能执行一个方法，于是这些方法被排队在一个单独的地方。这个地方被称为<font color='red'>执行栈</font>。

- 当一个脚本第一次执行的时候，js引擎会解析这段代码，并将其中的同步代码按照执行顺序加入执行栈中，然后从头开始执行。如果当前执行的是一个方法，那么js会向执行栈中添加这个方法的执行环境，然后进入这个执行环境继续执行其中的代码。当这个执行环境中的代码 执行完毕并返回结果后，js会退出这个执行环境并把这个执行环境销毁，回到上一个方法的执行环境。。这个过程反复进行，直到执行栈中的代码全部执行完毕。

- 下面这个图片非常直观的展示了这个过程，其中的global就是初次运行脚本时向执行栈中加入的代码：

 

 ![](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\v2-2f761eb83b50f53d741e6aa1f15a9db1_b.webp)

 

- 从图片可知，一个方法执行会向执行栈中加入这个方法的执行环境，在这个执行环境中还可以调用其他方法，甚至是自己，其结果不过是在执行栈中再添加一个执行环境。这个过程可以是无限进行下去的，除非发生了栈溢出，即超过了所能使用内存的最大值。

- 以上的过程说的都是同步代码的执行。那么当一个异步代码（如发送ajax请求数据）执行后会如何呢？前文提过，js的另一大特点是非阻塞，实现这一点的关键在于下面要说的这项机制——<font color='red'>事件队列（Task Queue）</font>。

- js引擎遇到一个异步事件后并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当一个异步事件返回结果后，js会将这个事件加入与当前执行栈不同的另一个队列，我们称之为事件队列。被放入事件队列不会立刻执行其回调，而<font color='red'>是等待当前执行栈中的所有任务都执行完毕， 主线程处于闲置状态时，主线程会去查找事件队列是否有任务</font>。如果有，那么主线程会从中取出排在第一位的事件，并把这个事件对应的回调放入执行栈中，然后执行其中的同步代码...，如此反复，这样就形成了一个无限的循环。这就是这个过程被称为“事件循环（Event Loop）”的原因。

这里还有一张图来展示这个过程：

 

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\v2-da078fa3eadf3db4bf455904ae06f84b_hd.jpg)

 

- 图中的stack表示我们所说的执行栈，web apis则是代表一些异步事件，而callback queue即事件队列。

##### 4.2.macro task与micro task

以上的事件循环过程是一个宏观的表述，实际上因为异步任务之间并不相同，因此他们的执行优先级也有区别。不同的异步任务被分为两类：微任务（micro task）和宏任务（macro task）。

以下事件属于宏任务：

- `setInterval()`
- `setTimeout()`

以下事件属于微任务

- `new Promise()`  文章中提及的微任务中的 new Promise()应该不是个微任务，这个应该是个同步任务，Promise.resolve().then()这个应该才是微任务
- `new MutaionObserver()`

前面我们介绍过，在一个事件循环中，异步事件返回结果后会被放到一个任务队列中。然而，根据这个异步事件的类型，这个事件实际上会被对应的宏任务队列或者微任务队列中去。并且在当前执行栈为空的时候，主线程会 查看微任务队列是否有事件存在。如果不存在，那么再去宏任务队列中取出一个事件并把对应的回到加入当前执行栈；如果存在，则会依次执行队列中事件对应的回调，直到微任务队列为空，然后去宏任务队列中取出最前面的一个事件，把对应的回调加入当前执行栈...如此反复，进入循环。

我们只需记住当当前执行栈执行完毕时会立刻先处理所有微任务队列中的事件，然后再去宏任务队列中取出一个事件<font color='red'>。同一次事件循环中，微任务永远在宏任务之前执行</font>。

这样就能解释下面这段代码的结果：

```text
setTimeout(function () {
    console.log(1);
});

new Promise(function(resolve,reject){
    console.log(2)
    resolve(3)
}).then(function(val){
    console.log(val);
})
```

结果为：

```text
2
3
1
```

##### 4.3浏览器包含了哪些进程

- 主进程
  - 协调控制其他子进程（创建、销毁）
  - 浏览器界面显示，用户交互，前进、后退、收藏
  - 将渲染进程得到的内存中的Bitmap，绘制到用户界面上
  - 处理不可见操作，网络请求，文件访问等
- 第三方插件进程
  - 每种类型的插件对应一个进程，仅当使用该插件时才创建
- GPU进程
  - 用于3D绘制等
- 渲染进程就是我们说的浏览器内核
  - 负责页面渲染，脚本执行，事件处理等
  - 每个tab页一个渲染进程

那么浏览器中包含了这么多的进程，那么对于普通的前端操作来说，最重要的是什么呢？

答案是`渲染进程`，也就是我们常说的`浏览器内核`

##### 4.4浏览器内核（渲染进程）

从前文我们得知，进程和线程是一对多的关系，也就是说一个进程包含了多条线程。

而对于`渲染进程`来说，它当然也是多线程的了，接下来我们来看一下渲染进程包含哪些线程。

- GUI渲染线程
  - 负责渲染页面，布局和绘制
  - 页面需要重绘和回流时，该线程就会执行
  - 与js引擎线程互斥，防止渲染结果不可预期
- JS引擎线程
  - 负责处理解析和执行javascript脚本程序
  - 只有一个JS引擎线程（单线程）
  - 与GUI渲染线程互斥，防止渲染结果不可预期
- 事件触发线程
  - 用来控制事件循环（鼠标点击、setTimeout、ajax等）
  - 当事件满足触发条件时，将事件放入到JS引擎所在的执行队列中
- 定时触发器线程
  - setInterval与setTimeout所在的线程
  - 定时任务并不是由JS引擎计时的，是由定时触发线程来计时的
  - 计时完毕后，通知事件触发线程
- 异步http请求线程
  - 浏览器有一个单独的线程用于处理AJAX请求
  - 当请求完成时，若有回调函数，通知事件触发线程

当我们了解了渲染进程包含的这些线程后，我们思考两个问题：

1. 为什么 javascript 是单线程的
2. 为什么 GUI 渲染线程为什么与 JS 引擎线程互斥

##### 4.5为什么 javascript 是单线程的

首先是历史原因，在创建 javascript 这门语言时，多进程多线程的架构并不流行，硬件支持并不好。

其次是因为多线程的复杂性，多线程操作需要加锁，编码的复杂性会增高。

而且，如果同时操作 DOM ，在多线程不加锁的情况下，最终会导致 DOM 渲染的结果不可预期。

##### 4.6为什么 GUI 渲染线程与 JS 引擎线程互斥

这是由于 JS 是可以操作 DOM 的，如果同时修改元素属性并同时渲染界面(即 `JS线程`和`UI线程`同时运行)， 那么渲染线程前后获得的元素就可能不一致了。

因此，为了防止渲染出现不可预期的结果，浏览器设定 `GUI渲染线程`和`JS引擎线程`为互斥关系， 当`JS引擎线程`执行时`GUI渲染线程`会被挂起，GUI更新则会被保存在一个队列中等待`JS引擎线程`空闲时立即被执行。

##### 4.7从 Event Loop 看 JS 的运行机制

到了这里，终于要进入我们的主题，什么是 Event Loop

先理解一些概念：

- JS 分为同步任务和异步任务
- 同步任务都在JS引擎线程上执行，形成一个`执行栈`
- 事件触发线程管理一个`任务队列`，异步任务触发条件达成，将回调事件放到`任务队列`中
- `执行栈`中所有同步任务执行完毕，此时JS引擎线程空闲，系统会读取`任务队列`，将可运行的异步任务回调事件添加到`执行栈`中，开始执行

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\16cb1d70e5120bea~tplv-t2oaga2asx-watermark.awebp)

在前端开发中我们会通过`setTimeout/setInterval`来指定定时任务，会通过`XHR/fetch`发送网络请求， 接下来简述一下`setTimeout/setInterval`和`XHR/fetch`到底做了什么事

我们知道，不管是`setTimeout/setInterval`和`XHR/fetch`代码，在这些代码执行时， 本身是同步任务，而其中的回调函数才是异步任务。

当代码执行到`setTimeout/setInterval`时，实际上是`JS引擎线程`通知`定时触发器线程`，间隔一个时间后，会触发一个回调事件， 而`定时触发器线程`在接收到这个消息后，会在等待的时间后，将回调事件放入到由`事件触发线程`所管理的`事件队列`中。

当代码执行到`XHR/fetch`时，实际上是`JS引擎线程`通知`异步http请求线程`，发送一个网络请求，并制定请求完成后的回调事件， 而`异步http请求线程`在接收到这个消息后，会在请求成功后，将回调事件放入到由`事件触发线程`所管理的`事件队列`中。

当我们的同步任务执行完，`JS引擎线程`会询问`事件触发线程`，在`事件队列`中是否有待执行的回调函数，如果有就会加入到执行栈中交给`JS引擎线程`执行

用一张图来解释：

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\16cb1d7433f29c46~tplv-t2oaga2asx-watermark.awebp)

再用代码来解释一下：

```javascript
let timerCallback = function() {
  console.log('wait one second');
};
let httpCallback = function() {
  console.log('get server data success');
}

// 同步任务
console.log('hello');
// 同步任务
// 通知定时器线程 1s 后将 timerCallback 交由事件触发线程处理
// 1s 后事件触发线程将 timerCallback 加入到事件队列中
setTimeout(timerCallback,1000);
// 同步任务
// 通知异步http请求线程发送网络请求，请求成功后将 httpCallback 交由事件触发线程处理
// 请求成功后事件触发线程将 httpCallback 加入到事件队列中
$.get('www.xxxx.com',httpCallback);
// 同步任务
console.log('world');
//...
// 所有同步任务执行完后
// 询问事件触发线程在事件事件队列中是否有需要执行的回调函数
// 如果没有，一直询问，直到有为止
// 如果有，将回调事件加入执行栈中，开始执行回调代码

```

总结一下：

- JS引擎线程只执行执行栈中的事件
- 执行栈中的代码执行完毕，就会读取事件队列中的事件
- 事件队列中的回调事件，是由各自线程插入到事件队列中的
- 如此循环



#### 5.微任务、宏任务

首先，JavaScript是一个单线程的脚本语言。

所以就是说在一行代码执行的过程中，必然不会存在同时执行的另一行代码，就像使用`alert()`以后进行疯狂`console.log`，如果没有关闭弹框，控制台是不会显示出一条`log`信息的。

亦或者有些代码执行了大量计算，比方说在前端暴力破解密码之类的鬼操作，这就会导致后续代码一直在等待，页面处于假死状态，因为前边的代码并没有执行完。

所以如果全部代码都是同步执行的，这会引发很严重的问题，比方说我们要从远端获取一些数据，难道要一直循环代码去判断是否拿到了返回结果么？*就像去饭店点餐，肯定不能说点完了以后就去后厨催着人炒菜的，会被揍的。*

于是就有了异步事件的概念，注册一个回调函数，比如说发一个网络请求，我们告诉主程序等到接收到数据后通知我，然后我们就可以去做其他的事情了。

然后在异步完成后，会通知到我们，但是此时可能程序正在做其他的事情，所以即使异步完成了也需要在一旁等待，等到程序空闲下来才有时间去看哪些异步已经完成了，可以去执行。

*比如说打了个车，如果司机先到了，但是你手头还有点儿事情要处理，这时司机是不可能自己先开着车走的，一定要等到你处理完事情上了车才能走。*

- 微任务与宏任务的区别

这个就像去银行办业务一样，先要取号进行排号。一般上边都会印着类似：“您的号码为XX，前边还有XX人。”之类的字样。因为柜员同时职能处理一个来办理业务的客户，这时每一个来办理业务的人就可以认为是银行柜员的一个<font color='red'>宏任务</font>来存在的，当柜员处理完当前客户的问题以后，选择接待下一位，广播报号，也就是下一个宏任务的开始。所以多个宏任务合在一起就可以认为说有一个任务队列在这，里边是当前银行中所有排号的客户。

**<font color='red'>任务队列中的都是已经完成的异步操作</font>，而不是说注册一个异步任务就会被放在这个任务队列中，就像在银行中排号，如果叫到你的时候你不在，那么你当前的号牌就作废了，柜员会选择直接跳过进行下一个客户的业务处理，等你回来以后还需要重新取号** 

而且一个宏任务在执行的过程中，是可以添加一些微任务的，就像在柜台办理业务，你前边的一位老大爷可能在存款，在存款这个业务办理完以后，柜员会问老大爷还有没有其他需要办理的业务，这时老大爷想了一下：“最近P2P爆雷有点儿多，是不是要选择稳一些的理财呢”，然后告诉柜员说，要办一些理财的业务，这时候柜员肯定不能告诉老大爷说：“您再上后边取个号去，重新排队”。 所以本来快轮到你来办理业务，会因为老大爷临时添加的“**理财业务**”而往后推。也许老大爷在办完理财以后还想 **再办一个信用卡**？或者 **再买点儿纪念币**？ 无论是什么需求，只要是柜员能够帮她办理的，都会在处理你的业务之前来做这些事情，这些都可以认为是<font color='red'>微任务</font>。

这就说明：你大爷永远是你大爷**在当前的微任务没有执行完成时，是不会执行下一个宏任务的。**

所以就有了那个经常在面试题、各种博客中的代码片段：

```
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
})

console.log(2)
```

`setTimeout`就是作为宏任务来存在的，而`Promise.then`则是具有代表性的微任务，上述代码的执行顺序就是按照序号来输出的。

**所有会进入的异步都是指的事件回调中的那部分代码**
 也就是说`new Promise`在实例化的过程中所执行的代码都是同步进行的，而`then`中注册的回调才是异步执行的。
 在同步代码执行完成后才回去检查是否有异步任务完成，并执行对应的回调，而微任务又会在宏任务之前执行。
 所以就得到了上述的输出结论`1、2、3、4`。

*+部分表示同步执行的代码*

```
+setTimeout(_ => {
-  console.log(4)
+})

+new Promise(resolve => {
+  resolve()
+  console.log(1)
+}).then(_ => {
-  console.log(3)
+})

+console.log(2)

```

本来`setTimeout`已经先设置了定时器（相当于取号），然后在当前进程中又添加了一些`Promise`的处理（临时添加业务）。

所以进阶的，即便我们继续在`Promise`中实例化`Promise`，其输出依然会早于`setTimeout`的宏任务：

```
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
  Promise.resolve().then(_ => {
    console.log('before timeout')
  }).then(_ => {
    Promise.resolve().then(_ => {
      console.log('also before timeout')
    })
  })
})

console.log(2)

```

当然了，实际情况下很少会有简单的这么调用`Promise`的，一般都会在里边有其他的异步操作，比如`fetch`、`fs.readFile`之类的操作。
 而这些其实就相当于注册了一个宏任务，而非是微任务。

*P.S. 在[Promise/A+的规范](https://link.juejin.cn?target=https%3A%2F%2Fpromisesaplus.com%2F%23notes)中，`Promise`的实现可以是微任务，也可以是宏任务，但是普遍的共识表示(至少`Chrome`是这么做的)，`Promise`应该是属于微任务阵营的*

所以，明白哪些操作是宏任务、哪些是微任务就变得很关键，这是目前业界比较流行的说法：

- 宏任务

| #                       | 浏览器 | Node |
| ----------------------- | ------ | ---- |
| `I/O`                   | ✅      | ✅    |
| `setTimeout`            | ✅      | ✅    |
| `setInterval`           | ✅      | ✅    |
| `setImmediate`          | ❌      | ✅    |
| `requestAnimationFrame` | ✅      | ❌    |

*有些地方会列出来`UI Rendering`，说这个也是宏任务，可是在读了[HTML规范文档](https://link.juejin.cn?target=https%3A%2F%2Fhtml.spec.whatwg.org%2Fmultipage%2Fwebappapis.html%23event-loop-processing-model)以后，发现这很显然是和微任务平行的一个操作步骤*
 *`requestAnimationFrame`姑且也算是宏任务吧，`requestAnimationFrame`在[MDN的定义](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FWindow%2FrequestAnimationFrame)为，下次页面重绘前所执行的操作，而重绘也是作为宏任务的一个步骤来存在的，且该步骤晚于微任务的执行*

- 微任务

| #                            | 浏览器 | Node |
| ---------------------------- | ------ | ---- |
| `process.nextTick`           | ❌      | ✅    |
| `MutationObserver`           | ✅      | ❌    |
| `Promise.then catch finally` | ✅      | ✅    |

 引用：“首先要明确的一点是，<font color='red'>宏任务必然是在微任务之后才执行的（因为微任务实际上是宏任务的其中一个步骤</font>）” 纠正：micro task是macro task的一个步骤，那么应该是“一条macro task先执行”，然后是当前所有micro task逐条执行，然后是“下一条macro task执行”， 重复上面loop。备注，script标签是第一个要执行的macro task。

微任务是JS级别的，宏任务是宿主级别的，是包含关系，不是先后关系



宏任务、微任务

当我们基本了解了什么是执行栈，什么是事件队列之后，我们深入了解一下事件循环中`宏任务`、`微任务`

什么是宏任务

我们可以将每次执行栈执行的代码当做是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）， 每一个宏任务会从头到尾执行完毕，不会执行其他。

我们前文提到过`JS引擎线程`和`GUI渲染线程`是互斥的关系，浏览器为了能够使`宏任务`和`DOM任务`有序的进行，会在一个`宏任务`执行结果后，在下一个`宏任务`执行前，`GUI渲染线程`开始工作，对页面进行渲染。

```javascript
// 宏任务-->渲染-->宏任务-->渲染-->渲染．．．

```

主代码块，setTimeout，setInterval等，都属于宏任务

**第一个例子：**

```javascript
document.body.style = 'background:black';
document.body.style = 'background:red';
document.body.style = 'background:blue';
document.body.style = 'background:grey';

```

我们可以将这段代码放到浏览器的控制台执行以下，看一下效果：

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\16caca3e44d7d357~tplv-t2oaga2asx-watermark.awebp)

我们会看到的结果是，页面背景会在瞬间变成灰色，以上代码属于同一次`宏任务`，所以全部执行完才触发`页面渲染`，渲染时`GUI线程`会将所有UI改动优化合并，所以视觉效果上，只会看到页面变成灰色。

**第二个例子：**

```javascript
document.body.style = 'background:blue';
setTimeout(function(){
    document.body.style = 'background:black'
},0)

```

执行一下，再看效果：

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\16caca3ed44e6b16~tplv-t2oaga2asx-watermark.awebp)

我会看到，页面先显示成蓝色背景，然后瞬间变成了黑色背景，这是因为以上代码属于两次`宏任务`，第一次`宏任务`执行的代码是将背景变成蓝色，然后触发渲染，将页面变成蓝色，再触发第二次宏任务将背景变成黑色。

什么是微任务

我们已经知道`宏任务`结束后，会执行渲染，然后执行下一个`宏任务`， 而微任务可以理解成在当前`宏任务`执行后立即执行的任务。

也就是说，当`宏任务`执行完，会在渲染前，将执行期间所产生的所有`微任务`都执行完。

Promise，process.nextTick等，属于`微任务`。

**第一个例子：**

```javascript
document.body.style = 'background:blue'
console.log(1);
Promise.resolve().then(()=>{
    console.log(2);
    document.body.style = 'background:black'
});
console.log(3);

```

执行一下，再看效果：

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\16cad85d2378ccb5~tplv-t2oaga2asx-watermark.awebp)

控制台输出 1 3 2 , 是因为 promise 对象的 then 方法的回调函数是异步执行，所以 2 最后输出

页面的背景色直接变成黑色，没有经过蓝色的阶段，是因为，我们在宏任务中将背景设置为蓝色，但在进行渲染前执行了微任务， 在微任务中将背景变成了黑色，然后才执行的渲染

**第二个例子：**

```javascript
setTimeout(() => {
    console.log(1)
    Promise.resolve(3).then(data => console.log(data))
}, 0)

setTimeout(() => {
    console.log(2)
}, 0)

// print : 1 3 2

```

上面代码共包含两个 setTimeout ，也就是说除主代码块外，共有两个`宏任务`， 其中第一个`宏任务`执行中，输出 1 ，并且创建了`微任务队列`，所以在下一个`宏任务`队列执行前， 先执行`微任务`，在`微任务`执行中，输出 3 ，微任务执行后，执行下一次`宏任务`，执行中输出 2

总结

- 执行一个`宏任务`（栈中没有就从`事件队列`中获取）
- 执行过程中如果遇到`微任务`，就将它添加到`微任务`的任务队列中
- `宏任务`执行完毕后，立即执行当前`微任务队列`中的所有`微任务`（依次执行）
- 当前`宏任务`执行完毕，开始检查渲染，然后`GUI线程`接管渲染
- 渲染完毕后，`JS线程`继续接管，开始下一个`宏任务`（从事件队列中获取）

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\16cb1d7bb4bd9fd2~tplv-t2oaga2asx-watermark.awebp)



著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 5.promise