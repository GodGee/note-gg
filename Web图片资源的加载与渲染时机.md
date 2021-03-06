# Web图片资源的加载与渲染时机

此文研究页面中的图片资源的加载和渲染时机，使得我们能更好的管理图片资源，避免不必要的流量和提高用户体验。

## 浏览器的工作流程

要研究图片资源的加载和渲染，我们先要了解浏览器的工作原理。以**Webkit**引擎的工作流程为例：

![webkitflow](https://segmentfault.com/img/remote/1460000010032506)

从上图可看出，浏览器加载一个HTML页面后进行如下操作：

- 解析HTML —> 构建DOM树
- 加载样式 —> 解析样式 —> 构建样式规则树
- 加载javascript —> 执行javascript代码
- 把DOM树和样式规则树匹配构建渲染树
- 计算元素位置进行布局
- 绘制

从上图我们不能很直观的看出图片资源从什么时候开始加载，下图标出图片加载和渲染的时机：

- 解析HTML【遇到`<img>`标签加载图片】 —> 构建DOM树
- 加载样式 —> 解析样式【遇到背景图片链接不加载】 —> 构建样式规则树
- 加载javascript —> 执行javascript代码
- 把DOM树和样式规则树匹配构建渲染树【遍历DOM树时加载对应样式规则上的背景图片】
- 计算元素位置进行布局
- 绘制【开始渲染图片】

## 图片加载与渲染规则

页面中不是所有的`<img>`标签图片和样式表背景图片都会加载。

### **display:none**

```
<style>
.img-purple {
    background-image: url(../image/purple.png);
}
</style>
<img src="../image/pink.png" style="display:none">
<div class="img-purple" style="display:none"></div>
```

图片资源请求如下：
![display-none](https://segmentfault.com/img/remote/1460000010032507)

设置了`display:none`属性的元素，图片不会渲染出来，但会加载。

**原理**

把DOM树和样式规则树匹配构建渲染树时，只会把可见元素和它对应的样式规则结合一起产出到渲染树，这就意味有不可见元素，当匹配DOM树和样式规则树时，若发现一个元素的对应的样式规则上有`display:none`，浏览器会认为该元素是不可见的，因此不会把该元素产出到渲染树上。

上面代码中，当解析HTML时会加载`<img>`标签元素上的图片。

当把DOM树和样式规则树匹配构建渲染树时，遍历DOM树上的元素，发现元素对应的样式规则上有`background-image`属性时会加载背景图片，但是因为这个元素是不可见元素（对应的样式规则上有`diaplay:none`），不会把该元素和它对应的样式规则产出到渲染树。

当绘制时因为渲染树上没有该元素，因此不会绘制该元素的背景图片。

```
<style>
.img-yellow {
    background-image: url(../image/yellow.png);
}
</style>
<div style="display:none">
    <img src="../image/red.png">
    <div class="img-yellow"></div>
</div>
```

图片资源请求如下：
![display-none](https://segmentfault.com/img/remote/1460000010032508)

设置了`display:none`属性元素的子元素，样式表中的背景图片不会渲染出来，也不会加载；而`<img>`标签的图片不会渲染出来，但会加载。

**原理**

正如上面所说的，当匹配DOM树和样式规则树时，若发现元素的对应的样式规则上有`display:none`，浏览器会认为该元素的子元素是不可见的，因此不会把该元素的子元素产出到渲染树上。

当构建渲染树遇到了设置了`display:none`属性的不可见元素时，不会继续遍历不可见元素的子元素，因此不会加载该元素中子元素的背景图片。

当绘制时也因为渲染树上没有设置了`display:none`属性元素，也没有改元素的子元素，因此该元素中子元素的背景图片不会渲染出来。

### 重复图片

```
.img-blue {
    background-image: url(../image/blue.png);
}
<div class="img-blue"></div>
<img src="../image/blue.png">
<img src="../image/blue.png">
```

图片资源请求如下：
![repeat-image](https://segmentfault.com/img/remote/1460000010032509)

页面中多个`<img>`标签或样式表中的背景图片图片路径是同一个，图片只加载一次。

**原理**

浏览器请求资源时，都会先判断是否有缓存，若有缓存且未过期则会从缓存中读取，不会再次请求。先加载的图片会存储到浏览器缓存中，后面再次请求同路径图片时会直接读取缓存中的图片。

### 不存在元素的背景图片

```
.img-blue {
    background-image: url(../image/blue.png);
}
.img-orange{
    background-image: url(../image/orange.png);
}
<div class="img-orange"></div>
```

图片资源请求如下：
![no-image](https://segmentfault.com/img/remote/1460000010032510)

不存在元素的背景图片不会加载。

**原理**

不存在的元素不会产出到DOM树上，构建渲染树过程中遍历DOM树时无法遍历不存在的元素，因此不会加载图片，也不会产出到渲染树上。当解析渲染树时无法解析不存在的元素，不存在的元素自然也不会渲染。

### 伪类的背景图片

```
.img-green {
    background-image: url(../image/green.png);
}
.img-green:hover{
    background-image: url(../image/red.png);
}
<div class="img-green"></div>
```

触发hover前的图片资源请求如下：
![class-image](https://segmentfault.com/img/remote/1460000010032511)

触发hover后的图片资源请求如下：
![class-image](https://segmentfault.com/img/remote/1460000010032512)

当触发伪类的时候，伪类样式上的背景图片才会加载。

**原理**

触发hover前，构建渲染树过程中，遍历DOM树时，该元素匹配的样式规则是无hover状态选择器`.img-green`的样式，因此加载无hover状态选择器`.img-green`的样式上*green.png*图片。该元素是可见元素，因此会被产出到渲染树上，绘制时渲染的也是*green.png*。

触发hover后，因为`.img-green:hover`的优先级比较高，构建新的渲染树过程中，该元素匹配的是有hover状态选择器，因此加载有hover状态选择器`.img-green:hover`的样式上的*red.png*图片。该元素是可见元素，因此会被产出到渲染树上，绘制时渲染的也是*red.png*。

## 应用

### 占位图

当使用样式表中的背景图片作为占位符时，要把背景图片转为base64格式。这是因为背景图片加载的顺序在`<img>`标签后面，背景图片可能会在`<img>`标签图片加载完成后才开始加载，达不到想要的效果。

### 预加载

很多场景里图片是在改变或触发状态后才显示出来的，例如点击一个Tab后，一个设置`display:none`隐藏的父元素变为显示，这个父元素里的子元素图片会在父元素显示后才开始加载；又如当鼠标hover到图标后，改变图标图片，图片会在hover上去后才开始加载，导致出现闪一下这种不友好的体验。

在这种场景下，我们就需要把图片预加载，预加载有很多种方式:

1. 若是小图标，可以合并成雪碧图，在改变状态前就把所有图标都一起加载了。
2. 使用上文讲到的，设置了display:none属性的元素，图片不会渲染出来，但会加载。把要预加载的图片加到设置了`display:none`的元素背景图或`<img>`标签里。
3. 在javascript创建img对象，把图片url设置到img对象的src属性里。

