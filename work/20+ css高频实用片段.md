# 20+ css高频实用片段

## 前言

> `修改input placeholder样式`、`多行文本溢出`、`隐藏滚动条`、`修改光标颜色`、`水平垂直居中`...多么熟悉的功能呀！前端童鞋几乎每天都会和他们打交道，一起来总结我们的css幸福小片段吧！下次不用百度、不用谷歌，这里就是你的港湾。

[点击查看源码地址”持续更新中“](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fqianlongo%2Fhot-styles)

## 1. 解决图片5px间距

> 你是否经常遇到图片底部莫名其妙多出来5px的间距，不急，这里有4种方式让它消失

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dae7449e2a3a4c9283b2c4d379d5bb87~tplv-k3u1fbpfcp-watermark.awebp?)

**方案1：给父元素设置`font-size: 0`**

**效果**

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/421903b189284bce9b4220857fdc514b~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```html
<div class="img-container">
  <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202002%2F05%2F20200205093101_yfocq.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1636215521&t=203563292576c66ba434651680281e8a" alt="">
</div>

复制代码
```

**css**

```css
html,body{
  margin: 0;
  padding: 0;
}

.img-container{
  background-color: lightblue;
  font-size: 0;
}

img{
  width: 100%;
}

复制代码
```

**方案2：给img设置`display: block`**

**效果同上**

**html**

```html
<div class="img-container">
  <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202002%2F05%2F20200205093101_yfocq.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1636215521&t=203563292576c66ba434651680281e8a" alt="">
</div>

复制代码
```

**css**

```css
html,body{
  margin: 0;
  padding: 0;
}

.img-container{
  background-color: lightblue;
}

img{
  width: 100%;
  /*关键css*/
  display: block;
}

复制代码
```

**方案3：给img设置`vertical-align: bottom`**

**效果同上**

**html**

```html
<div class="img-container">
  <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202002%2F05%2F20200205093101_yfocq.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1636215521&t=203563292576c66ba434651680281e8a" alt="">
</div>

复制代码
```

**css**

```css
html,body{
  margin: 0;
  padding: 0;
}

.img-container{
  background-color: lightblue;
}

img{
  width: 100%;
  /*关键css*/
  vertical-align: bottom;
}

复制代码
```

**方案4：给父元素设置`line-height: 5px;`**

**效果同上**

**html**

```html
<div class="img-container">
  <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202002%2F05%2F20200205093101_yfocq.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1636215521&t=203563292576c66ba434651680281e8a" alt="">
</div>

复制代码
```

**css**

```css
html,body{
  margin: 0;
  padding: 0;
}

.img-container{
  background-color: lightblue;
  /*关键css*/
  line-height: 5px;
}

img{
  width: 100%;
}

复制代码
```

## 2.元素高度跟随窗口

> 有时候希望某个元素的高度和窗口是一致的，如果用百分比设置，那html、body等元素也要跟着一顿设置`height: 100%`有没有更简单的方法呢？

**效果**

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fd011833f75a4a9f96a88cacee7dd98a~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```html
<div class="app">
  <div class="child"></div>
</div>

复制代码
```

**css**

```css
*{
  margin: 0;
  padding: 0;
}

.child{
  width: 100%;
  /*关键css*/
  height: 100vh;
  background-image: linear-gradient(180deg, #2af598 0%, #009efd 100%);
}

复制代码
```

## 3.修改input placeholder样式

**第一个是经过改写的placeholder，第二个是原生的**

**效果**

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a14bb9cce35843ae847d54860133b33e~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<input type="text" class="placehoder-custom" placeholder="请输入用户名搜索">
<input type="text" placeholder="请输入用户名搜索">
复制代码
```

**css**

```css
input{
  width: 300px;
  height: 30px;
  border: none;
  outline: none;
  display: block;
  margin: 15px;
  border: solid 1px #dee0e9;
  padding: 0 15px;
  border-radius: 15px;
}

.placehoder-custom::-webkit-input-placeholder{
  color: #babbc1;
  font-size: 12px;
}

复制代码
```

## 4. 巧用not选择器

> 有些情况下`所有`的元素都需要某些样式，唯独`最后一个`不需要，这时候使用not选择器将会特别方便

如下图：最后一个元素没有下边框

**效果**

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/af66215604c34646ab9923b1add0c6bd~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```html
<ul>
    <li>
      <span>单元格</span>
      <span>内容</span>
    </li>
    <li>
      <span>单元格</span>
      <span>内容</span>
    </li>
    <li>
      <span>单元格</span>
      <span>内容</span>
    </li>
    <li>
      <span>单元格</span>
      <span>内容</span>
    </li>
</ul>

复制代码
```

**关键css**

```css
li:not(:last-child){
  border-bottom: 1px solid #ebedf0;
}

复制代码
```

## 5. 使用flex布局实现智能固定底部

> 内容不够时，`规则说明`要处于底部，内容足够多时，`规则说明`随着内容往下沉，大家一定也遇到过类似的需求，使用flex巧妙实现布局。

![flex1.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e39706b10544ef08f31306f14ea1472~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```javascript
<div class="container">
  <div class="main">我是内容区域</div>
  <div class="footer">规则说明</div>
</div>

复制代码
```

**css**

```css
 .container{
  height: 100vh;
  /* 关键css处 */
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.main{
  /* 关键css处 */
  flex: 1;
  background-image: linear-gradient(45deg, #ff9a9e 0%, #fad0c4 99%, #fad0c4 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
}

.footer{
  padding: 15px 0;
  text-align: center;
  color: #ff9a9e;
  font-size: 14px;
}

复制代码
```

## 6. 使用caret-color改变光标颜色

> 在做表单相关需求的时候，有时候需要修改一闪一闪光标的颜色。`caret-color`属性完美支持这个需求。

![光标.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b88edad0af6d4b0eb9382c3096473fb1~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```html
<input type="text" class="caret-color" />

复制代码
```

**css**

```css
.caret-color {
  /* 关键css */
  caret-color: #ffd476;
}

复制代码
```

## 7. 移除`type="number"`尾部的箭头

> 默认情况下`input type="number"`时尾部会出现小箭头，但是很多时候我们想去掉它，应该怎么办呢？

如图：第一个输入框没有去掉小箭头的效果，第二个去掉了。

**效果**

![数字.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/070684e74bec496abb50fc704e5bb52f~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```html
<input type="number" />
<input type="number" class="no-arrow" />

复制代码
```

**css**

```css
/* 关键css */
.no-arrow::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
复制代码
```

## 8. `outline:none`移除input状态线

> 输入框选中时，默认会带蓝色状态线，使用`outline:none`一键移除

**效果**

如图：第一个输入框移除了，第二个没有移除

![状态线.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b56e0808842b442e86466c11cd7bfbd1~tplv-k3u1fbpfcp-watermark.awebp?)

**html**

```html
<input type="number" />
<input type="number" class="no-outline" />

复制代码
```

**css**

```css
.no-outline{
  outline: none;
}
复制代码
```

## 9.解决IOS滚动条卡顿

> 在IOS机器上，经常遇到元素滚动时卡顿的情况，只需要一行css即可让其支持弹性滚动

```css
body,html{   
  -webkit-overflow-scrolling: touch;
}
复制代码
```

## 10.画三角形

**效果**

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/603fad52b1814b0a8e0856798e338756~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<div class="box">
  <div class="box-inner">
    <div class="triangle bottom"></div>
    <div class="triangle right"></div>
    <div class="triangle top"></div>
    <div class="triangle left"></div>
  </div>
</div>
复制代码
```

**css**

```css
.triangle {
  display: inline-block;
  margin-right: 10px;
  /* 基础样式 */
  border: solid 10px transparent;
}
  /*下*/
.triangle.bottom {
  border-top-color: #0097a7;
}
  /*上*/
.triangle.top {
  border-bottom-color: #b2ebf2;
}
/*左*/
.triangle.left {
  border-right-color: #00bcd4;
}
/*右*/
.triangle.right {
  border-left-color: #009688;
}
复制代码
```

## 11.画小箭头

**效果**

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5176c21b14842de82cee29532265079~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<div class="box">
  <div class="box-inner">
    <div class="arrow bottom"></div>
    <div class="arrow top"></div>
    <div class="arrow right"></div>
    <div class="arrow left"></div>
  </div>
</div>
复制代码
```

**css**

```css
  .arrow {
    display: inline-block;
    margin-right: 10px;
    /* 基础样式 */
    width: 0;
    height: 0;
    /* 基础样式 */
    border: 16px solid;
    border-color: transparent #CDDC39 transparent transparent;
    position: relative;
  }

  .arrow::after {
    content: "";
    position: absolute;
    /* 通过位移覆盖背景 */
    right: -20px;
    top: -16px;
    border: 16px solid;
    border-color: transparent #fff transparent transparent;
  }
  /*下*/
  .arrow.bottom {
    transform: rotate(270deg);
  }
  /*上*/
  .arrow.top {
    transform: rotate(90deg);
  }
  /*左*/
  .arrow.left {
    transform: rotate(180deg);
  }
  /*右*/
  .arrow.right {
    transform: rotate(0deg);
  }
复制代码
```

## 12.图片尺寸自适应

**vw vs padding**

**效果**

![15.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f33c81950bb847d9a3f0642fcfa85c3c~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<div class="box">
  <div class="img-container">
    <img src="https://i0.hippopx.com/photos/179/171/625/sparkler-holding-hands-firework-preview.jpg" alt="">
  </div>
</div>

<div class="box">
  <div class="img-container">
    <img src="https://i0.hippopx.com/photos/179/171/625/sparkler-holding-hands-firework-preview.jpg" alt="">
  </div>
</div>

<div class="box-vw">
  <div class="img-container">
    <img src="https://i0.hippopx.com/photos/179/171/625/sparkler-holding-hands-firework-preview.jpg" alt="">
  </div>
</div>
复制代码
```

**css**

```css
.box, .box-vw{
  background-color: #f5f6f9;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 15px;
}

.box:nth-of-type(2){
  width: 260px;
}
/* vw方案 */
.box-vw .img-container{
  width: 100vw;
  height: 66.620879vw;
  padding-bottom: inherit;
}
/* padding方案 */
.img-container{
  width: 100%;
  height: 0;
  /* 图片的高宽比 */
  padding-bottom: 66.620879%;
}

img{
width: 100%;
}
复制代码
```

## 13.隐藏滚动条

**第一个可以看到滚动条，第二个已隐藏了**

**效果**

![7.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1495b33a4371430d927116512710914a~tplv-k3u1fbpfcp-watermark.awebp)

> **注意**这里指的是容器可以滚动，但是滚动条仿佛透明一样被隐藏

**html**

```html
<div class="box">
  <div>
    爱情会离开，朋友会离开，快乐会离开，但是考试不会,因为你不会就不会
  </div>
</div>

<div class="box box-hide-scrollbar">
  <div>只是因为在人群中多看了你一眼，你就--问我游泳健身了解一下？</div>
</div>
复制代码
```

**css**

```css
.box {
  width: 375px;
  overflow: scroll;
}

/* 关键代码 */
.box-hide-scrollbar::-webkit-scrollbar {
  display: none; /* Chrome Safari */
}

.box > div {
  margin-bottom: 15px;
  padding: 10px;
  background-color: #f5f6f9;
  border-radius: 6px;
  font-size: 12px;
  width: 750px;
}

复制代码
```

## 14.自定义文本选中的样式

**第一个是默认选中状态，第二个是自定义选中状态**

**效果**

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/908a278fea2e441481501e3c71f77e67~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<div class="box">
  <p class="box-default">
    昨天遇见小学同学，没有想到他混的这么差--只放了一块钱到我的碗里
  </p>
  <p class="box--custom">
    今年情人节，不出意外的话，一个人过，出意外的话--去医院过
  </p>
</div>
复制代码
```

**css**

```css
.box-custom::selection {
  color: #ffffff;
  background-color: #ff4c9f;
}

复制代码
```

## 15.禁止选择文本

**第一个可以选中，第二个无法选中**

**效果**

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/851bf7d812e542868763c4be239c6815~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
 <div class="box">
  <p>好不容易习惯了自己的长相--去理了个发，又换了一种丑法</p>
  <p>国庆节放假，想跟女朋友去旅游，请大家帮忙推荐下--哪里有女朋友</p>
</div>
复制代码
```

**css**

```css
.box p:last-child{
  user-select: none;
}

复制代码
```

## 16.水平垂直居中

**效果**

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ea843f47b0fc42a394dd92d9d80d6767~tplv-k3u1fbpfcp-watermark.awebp) **html**

```html
<div class="parent">
  <p class="child">每次临时抱佛脚的时候--佛祖他总是给我一脚</p>
</div>
复制代码
```

**css**

```css
.parent{
  padding: 0 10px;
  background-color: #f5f6f9;
  height: 100px;
  border-radius: 6px;
  font-size: 14px;
  // 以下是水平垂直居中关键代码
  display: flex;
  align-items: center;
  justify-content: center;
}

复制代码
```

## 17.单行文本溢出显示省略号

> 这个点估计全世界的前端都知道如何写，所以还是看为您准备的笑话乐一乐更有价值。

**效果**

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0689528ade2e4de5af38428a1161d4a1~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<p class="one-line-ellipsis">不要轻易向命运低头，因为一低头就会看到赘肉 如果你愿意一层一层剥开我的心</p>
复制代码
```

**css**

```css
.one-line-ellipsis {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  /* 非必须，只是为了制造一行放不下的效果 */
  max-width: 375px; 
}

复制代码
```

## 18.多行文本溢出显示省略号

**示例**

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e5323c4d5824601aee9c55a3d4d30d0~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<p class="more-line-ellipsis">上帝对人都是公平的给了你丑外表--也会配给你低智商 如果你愿意一层一层剥开我的心，你会发现--我缺心眼啊！</p>
复制代码
```

**css**

```css
.more-line-ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  /* 设置n行，也包括1 */
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

复制代码
```

## 19.清除浮动

> 一个仿佛有年代感的布局方式😄。现在移动端应该大部分不采用该布局方式了。

**从图中可以看出，外层高度并未塌陷，就是使用了clearfix类的原因**

**效果**

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7965e187f9c44aa88d7ff2d3adc140d2~tplv-k3u1fbpfcp-watermark.awebp)

**html**

```html
<div class="box clearfix">
  <div class="float-left"></div>
  <div class="float-left float-left2"></div>
  <div class="float-left float-left3"></div>
</div>
复制代码
```

**css**

```css
body {
  padding: 15px;
  color: #324b64;
}
/* 关键代码 */
.clearfix{
  zoom: 1;
}
.clearfix::after{
  display: block;
  content: '';
  clear: both;
}

.box {
  padding: 10px;
  background-color: #f5f6f9;
  border-radius: 6px;
  font-size: 12px;
}

.box >div{
  width: 29%;
  height: 100px;
}

.float-left{
  background-color: #faa755;
  float: left;
  margin-right: 10px;
}

.float-left2{
  background-color: #7fb80e;
}

.float-left3{
  background-color: #b2d235;
}

复制代码
```

## 20. 使用filter:grayscale(1)使网页呈现哀悼模式

> 伟大的革命先烈们为我们祖国的诞生做出了巨大的牺牲，在相应节日里，我们的站点会呈现灰色哀悼模式，以此来纪念先烈们。

**效果**

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e221b97bf4e74aeab4a4c178c04555dd~tplv-k3u1fbpfcp-watermark.awebp?)

**css**

```css
body{
  filter: grayscale(1);
}
```

 