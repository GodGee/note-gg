# CSS 特性

## 简介

如果您有关注过这两年的 [**CSS 发展状态报告**](https://stateofcss.com/)（[2019年](https://2019.stateofcss.com/en-US/features/)和[2020年](https://2020.stateofcss.com/en-US/features/)）的话，不难发现，在报告中有专门关于 [**CSS 新特性**](https://2020.stateofcss.com/en-us/features/)一项的介绍： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/166e328625cd49f1bd9f169cafa2a04a~tplv-k3u1fbpfcp-zoom-1.image)

> 上图截取 [2020 年 CSS 发展状态报告](https://2020.stateofcss.com/en-us/features/)！



是不是有种似有相识的感觉，或者有一些又是那么的陌生。对于我而言，这些都不陌生。不过我想说的是上图（或者报告）中提到的只是冰山一角，有很多是没有出现在调查问卷和报告中。比如接下来内容中提到的伪类选择器，内容可见性，容器查询等等。 

嗯！先从 CSS 选择器开始！(^_^)

## CSS 伪类选择器

CSS 选择发展到今日，可以说是一个庞大的体系了： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e36da97059674ff7a9a51c68c18b7257~tplv-k3u1fbpfcp-zoom-1.image)

> 特别声明，上图来自于 [@linxz 的博客](https://github.com/linxz/blog/tree/gh-pages/img/2021-05)，[如果上图看不清楚可以点击这里获取原图](https://raw.githubusercontent.com/linxz/blog/gh-pages/img/2021-05/css-selector.png)。

现在描述 CSS 选择器的规范主要有 [CSS （指的是 CSS 2.1）](https://www.w3.org/TR/CSS22/selector.html)，后面分为选择器 [**Level 3**](https://www.w3.org/TR/selectors-3/) 和 [**Level 4**](https://www.w3.org/TR/selectors-4/)。伪类选择器在这三部分规范中都有出现过，他们是一个递进的过程，特别是在 [**Level 3**](https://www.w3.org/TR/selectors-3/) 和 [**Level 4**](https://www.w3.org/TR/selectors-4/)，在原有的基础上新增了不少优秀的选择器。今天要提到的几个现代伪类选择器就是出于 Levele 4: 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f49751371b44b90907424d6f33fde23~tplv-k3u1fbpfcp-zoom-1.image)

### :is() 和 :where()



先来看 `:is()` 和 `:where()` 。 [@Elad Shechter](https://twitter.com/eladsc) 曾在推特上发了一个关于 `:is()` 和 `:where()` [选择器的测试题](https://twitter.com/eladsc/status/1371824429009866752?ref_src=twsrc^tfw|twcamp^tweetembed|twterm^1371824429009866752|twgr^|twcon^s1_c10&ref_url=https%3A%2F%2Fpublish.twitter.com%2F%3Fquery%3Dhttps3A2F2Ftwitter.com2Feladsc2Fstatus2F1371824429009866752widget%3DTweet)： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c691896828242a49ff95a9bbb589732~tplv-k3u1fbpfcp-zoom-1.image)

如果你是第一次看到这样的测试题，请先自测一下，如果是你，你会选择哪个答案（`green`，`purple`，`red` 还是 `blue`）？如果你选择的是 `purple` ，那么要恭喜你，你答对了。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f2f891c0dc5c4fccba09b58d6daddb55~tplv-k3u1fbpfcp-zoom-1.image)

> Demo 地址：[codepen.io/airen/full/…](https://codepen.io/airen/full/ExWxbKe)



示例中出现了 `:is()` 和 `:where()` 两个可能你从未接触过的伪类选择器： 

```
:is(.header, .main) .p {
	color: purple
}

:where(.header, .main) .p {
	color: red;
}


```



其实这两个选择器等同于： 

```
.header .p,
.main .p {
	color: purple;
}

.header .p,
.main .p {
	color: red;
}


```

他们唯一不同之处，就是选择器权重不同。`:where()` 的优先级总是为 `0` ，但是 `:is()` 的优先级是由它的选择器列表中优先级最高的选择器决定的。那么上面的示例中，我们可以用下图来清晰的描述他们之间的关系： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1e694057be344989e6457c704fdbba1~tplv-k3u1fbpfcp-zoom-1.image)

> 上图地址来自： [twitter.com/eladsc/stat…](https://twitter.com/eladsc/status/1372445466709868545)



即，下面三个选择器选中的相同的元素： 

```
.header .p,
.main .p {
	// ...
}

:is(.header, .main) .p {
	// ...
}

:where(.header, .main) .p {
	// ...
}


```



不同的是他们的权重不同，其中： 

```
.header .p,
.main .p {
	// ....
}

:is(.header, .main) .p {
	// ...
}


```

示例中的两种选择器具有相同的权重（即`020`）;其中： 

```
:where(.header, .main) .p {
	// ...
}

.p {
	// ...
}


```

上面示例代码中的选择器具有相同权重（即 `010`）： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6726febf6d864289bebffe27395585fd~tplv-k3u1fbpfcp-zoom-1.image)

`:is()` 和 `:where()` 伪类选择器的出现，将会让我们的选择器变得更简洁，比如下成这个 `:is()` 的示例： 

```
// Level 0
h1 {
  font-size: 30px;
}
//  Level 1 
section h1, article h1, aside h1, nav h1 {
  font-size: 25px;
}
// Level 2 
section section h1, section article h1, section aside h1, section nav h1,
article section h1, article article h1, article aside h1, article nav h1,
aside section h1, aside article h1, aside aside h1, aside nav h1,
nav section h1, nav article h1, nav aside h1, nav nav h1, {
  font-size: 20px;
}


```

使用 `:is()` 可以像下面这样来描述：

```
// Level 0
h1 {
  font-size: 30px;
}
// Level 1
:is(section, article, aside, nav) h1 {
  font-size: 25px;
}
// Level 2 
:is(section, article, aside, nav)
:is(section, article, aside, nav) h1 {
  font-size: 20px;
}


```

回过头来，请大家再看看下面这个测试题，请问 `p` 文本的颜色是什么？ 

```
<!-- HTML -->
<main class="main">
	<p class="p">What is the text color?</p>
</main>

// CSS
:where(.header, .main) .p {
	color: red;
}

.p {
	color: blue;
}

:is(.header, .main) .p {
	color: purple;
}

.header .p,
.main .p {
	color: green;
}


```

> 可以点击这里查看答案： [codepen.io/airen/full/…](https://codepen.io/airen/full/dyvyJmW)



### :not() 和 :has()

或许大家平时在开发前端页面的时候，碰到类似下图这样的需求： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a81aba0f89c5459e955a7b1e8b29e762~tplv-k3u1fbpfcp-zoom-1.image)

希望最后一张卡片没有`margin-bottom` （或第一张卡片没有 `margin-top`）。针对于这样的场景，`:not()` 选择器就非常有优势。为什么这么说呢？ 

在没有 `:not()` 选择器的时候，你可能会想到下面这样的方式： 

```
.card + .card {
	margin-top: 20px;
}	

// 或

.card {
	margin-bottom: 20px;
}

.card.card--last { // 也可能会使用 .card:last-child
	margin-bottom: 0;
}


```

如果换成 `:not()` 选择器，可以这要来实现： 

```
.card:not(:last-child) {
	margin-bottom: 20px
}


```

效果如下：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e8f0ab34e5814cabbcd27e053e281ffe~tplv-k3u1fbpfcp-zoom-1.image)

> Demo URL: [codepen.io/airen/full/…](https://codepen.io/airen/full/MPNLEo)



虽然 CSS 选择器已经非常强大了，但一直以来，在 CSS 中没有从子元素选到父元素的样的选择器（父选择器）： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5fa5a98a19a64bbc8729cbc6993295e9~tplv-k3u1fbpfcp-zoom-1.image)

> 有人提出希望有一个 `:parents()` 这样的选择器！



虽然直到目前为止还没有 `:parents()` 选择器，但庆幸的是，`:has()` 选择器即将到来，它可以用来选择父级元素。目前 Igalia 公司正在为 Chrome 实现该选择器，其团队成员 Brian Kardell 新发表的《[**Can I :has()**](https://bkardell.com/blog/canihas.html)》文章中对 `:has()` 选择器进行了详细的阐述。 

```
<section><!-- section 边框颜色是 blue,margin-bottom是 30px --> 
  <h1>H1 Level Title</h1>
</section>  

<section><!-- section 边框颜色是 #09f,margin-bottom是 30px --> 
  <h1>H2 Level Title</h1>
</section>  

<section><!-- section 边框颜色是 red --> 
  <p>Text Paragraphs</p>
</section>  

/* CSS */

// 将匹配含有h1子元素的 section元素
section:has(h1) {
	border-color: blue;
}
// 将匹配含有h2子元素的 section元素
section:has(h2) {
	border-color: #09f;
}
// 将匹配含有p子元素的 section元素
section:has(p) {
	border-color: red;
}
// 将匹配除了含有p子元素的 section元素
section:not(:has(p)) {
	margin-bottom: 30px;
}


```



在支持`:has()`的浏览器中，你将看到下图这样的效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fdc090bde68c4929b6b6735fe4d4f941~tplv-k3u1fbpfcp-zoom-1.image)

示例中还演示了 `:not()` 和 `:has()` 组合在一起使用的，但两者组合在一起所达到的意思却完全不一样。其中 `:not(:has(selector))` 匹配不含有 `selector` 元素的父元素，而 `:has(:not(selector))` 匹配含有的不是 `selector` 子元素的元素。两者主要区别在于，`:has(:not(selector))` 写法必须要含有一个子元素，而 `:not(:has())` 可以不含有元素也会被匹配。 

有意思的是，[Github 中有一个 Issue](https://github.com/w3c/csswg-drafts/issues/4903) 在讨论** **`\**:has-child()\**`** 选择器**，或许哪一天，我们在组件中就可以这样使用： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/022233a0d35b453c91965ba369badcc4~tplv-k3u1fbpfcp-zoom-1.image)

### :empty 和 :blank

在现代 Web 的开发的过程中，总是无法避免数据吐出为空的情况，此时往往会给我们的 UI 带来额外的麻烦。比如说，在同一类的 UI 元件中编写了一定的样式规则，但数据为空，此时在页面上可能会出现这样的场景： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ca7b6519b95471bbee6928d4ef2489a~tplv-k3u1fbpfcp-zoom-1.image)

 CSS 的 `:empty` 和 `:blank` 两个伪类选择器可以帮助我们避免这种现象。这两个选择器都很有用： 

- 给空元素添加样式
- 创建空的状态



既然都是可以为空元素添加样式，那何为空元素，他们之间的差异又是什么？先来回答第一个问题，何为空元素？空元素是指元素没有任何子元素或子节点，比如： 

```
<!-- 空元素 -->
<div class="error"></div>
<div class="error"><!-- 注释 --></div>

<!-- 非空元素 -->
<div class="error"> </div><!-- 中间有一个空格符 -->
<div class="error">
</div><!-- 断行 -->
<div class="error">
	<!-- 注释 -->
</div><!-- 注释断行排列 -->
<div class="error"><span></span></div>


```



`:empty` 和 `:blank` 相比，`:empty` 只能选中没有子元素的元素。子元素只可以是元素节点或文本（包括空格）。注释或处理指令都不会产生影响。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f75adb60972047f6a906cdb4d39524dd~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/yLMLKyo)



注意，在空元素上即使使用伪元素 `::before` 或 `::after` 创建内容，也能被`:empty` 识别。 

`:blank` 要比 `:empty` 灵活地多，只要该元素中无任何子元素都能被识别。不过 W3C 规范对该伪类选择器的定义更趋向于作用到表单控件中，比如用户没有在 `input` 或 `textarea` 中输入内容时，提交表单能被识别到。有点类似于表单验证的功能。 

早在 2018 年 Zell Liew 在推特上就针对 `:empty` 和 `:blank` 做过相关的讨论，并发表一了篇有关于这方面的博文《[**:empty and :blank**](https://zellwk.com/blog/empty-and-blank/)》：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/17d0fdbb478b491ba1d8579a93e28961~tplv-k3u1fbpfcp-zoom-1.image)

到目前为止，`:empty` 已得到主流浏览器支持，可以用于实际生产中，但 `:blank` 伪类选择器还是存在一定的争议的，[有关于这方面的讨论可以点击这里查阅](https://github.com/w3c/csswg-drafts/issues/1967)。

### :focus-visible 和 :focus-within

CSSer 对于 `:focus` 应该不会感到陌生，早期对于可聚焦元素可以使用 `:focus` 来给元素设置焦点状态下的 UI 风格，即焦点环样式： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c5e403e13be646dfa1777b350a70bd65~tplv-k3u1fbpfcp-zoom-1.image)

Chrome 86 开始，另外引入了 `:focus-visible` 和 `:focus-within` 两种伪类选择器来帮助我们更好的控制 UI 焦点环的样式： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43c52ff3c8e34eefa8166b5c7cc147e8~tplv-k3u1fbpfcp-zoom-1.image)

虽然 `:focus` 、`:focus-within` 和 `:focus-visible` 都可以用来设置焦点环的样式，但他们之间还是有一定的差异： 

- `:focus` ：当用户使用鼠标点击焦点元素或使用键盘的 `Tab` 键（或快捷键）触发焦点元素焦点环的样式
- `:focus-visible` ：只有使用键盘的 `Tab` 键（或快捷键）触发焦点元素焦点环的样式。如果仅使用 `:focus-visible` 设置焦点环样式的话，那么用户使用鼠标点击焦点元素时不会触发焦点环样式
- `:focus-within`：表示一个元素获得焦点，或该元素的后代元素获得焦点。这也意味着，它或它的后代获得焦点，都可以触发 `:focus-within`

来简单的看一个示例： 

```
button:focus { 
  outline: 2px dotted #09f; 
  outline-offset: 2px; 
} 

button:focus-visible { 
  outline: 2px solid #f36; 
  outline-offset: 2px; 
}


```

你会发现，分别使用鼠标点击按钮和按`Tab` 让按钮获得焦点时焦点环样式效果不同： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2673e952d82145b4a420fa3717fc8f03~tplv-k3u1fbpfcp-zoom-1.image)

不过需要注意的是，`:focus` 和 `:focus-visible` 也会涉及到选择器权重的问题，就上面的示例来说，如果我们把 `:focus` 选择器对应的样式放置到 `:focus-visible` 之后： 

```
button:focus-visible { 
  outline: 2px solid #f36; 
  outline-offset: 2px; 
}

button:focus { 
  outline: 2px dotted #09f; 
  outline-offset: 2px; 
} 


```

这个时候，你会发现不管用户使用键盘 `Tab` 键还是鼠标让 `<button>` 获得焦点时，焦点样式都会采用 `:focus` 对应的样式： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5eabfce03ae043a882c4d41e43618283~tplv-k3u1fbpfcp-zoom-1.image)

如果我们要让 `:focus` 和 `:focus-visible` 可以有独自的样式，可以借助CSS选择器中的 `:not()` 来处理： 

```
button:focus:not(:focus-visible) {
  outline: 2px dotted #416dea;
  outline-offset: 2px;
  box-shadow: 0px 1px 1px #416dea;
}

button:focus-visible {
  outline: 2px solid #416dea;
  outline-offset: 2px;
  box-shadow: 0px 1px 1px #416dea;
}


```



这个时候按 `Tab` 键盘和鼠标点击时焦点环样式就有相应的差异： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/baf7f721a2844c3b83a1c1d8c9470623~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/YzGdoLq)



或许你想到这样的场景了，如果你在做 A11Y 方面的优化，希望在移动端和PC端上对焦点元素设置不同的焦点环样式，那用上面这种方案来说就会非常的灵活。 

对于 `:focus-within` 而言，它其实有点类似于 `:has()` 伪类选择器，可以在子元素得到焦点时，触发父元素： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/60c91ae63b0441cca02fbc894358a26a~tplv-k3u1fbpfcp-zoom-1.image)

上图描述了 `:focus-within` 和 `:focus` 之间的差异。更简单的说，它有点类似于 JavaScript 的事件冒泡，从可获得焦点元素开始一直冒泡到HTML的根元素 `<html>` ，都可以接收触发 `:focus-within` 事件。比如： 

```
.box:focus-within,
.container:focus-within {
    box-shadow: 0 0 5px 3px rgb(255 255 255 / 0.65);
}

body:focus-within {
    background-color: #2a0f5bde;
}

html:focus-within {
    border: 5px solid #09f;
}


```



![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0a2bb31f3fd941768713cdd1ceeff8c8~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/JjRxoLE)



借助于 `:focus-within` 伪类选择器的特性，我们就可以让一些交互效果变得更为简单，比如下面这个示例，使用 `:focus-within` 就可以实现，不再需要任何 JavaScript 代码： 

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/KrPLmV)



有了 `:focus` 、`:focus-visible` 和 `:focus-within` 会让我们更好的管理焦点元素在焦点状态下焦点环的 UI 效果。 

在 CSS 的世界中，除了这里提到的伪类选择器之外，还有很多其他的伪类选择器（或伪元素），比如 `::marker` 、`:in-range` 、`:out-of-range` 。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/605f812f42bf43d18c5b6c2d6ece2041~tplv-k3u1fbpfcp-zoom-1.image)

如果你感兴趣的话，可以尝试着使用 `:in-range` 、`:out-of-range` 给一些 `input` (比如 `type` 为 `number`、`range` 的 `input` 元素，他们都有 `min` 和 `max` 属性的设置) 在用户输入范围值和非范围值时，[**提供不同的 UI 效果**](https://codepen.io/collection/DgYaMj/)。 

## CSS 颜色

[**CSS 颜色模块自从 Level 4 开始**](https://www.w3.org/TR/css-color-4/)，除了新增了一些新的函数值，比如 `hwb()`、`lch()` 、`lab()` 、`color-mix()` 、 `color-contrast()` 和 `color()` 之外，对原本的 `rgb()` 、`hsl()` 以及 `#rrggbb` 等颜色值定义语法规则也做出调整。 

比如，原本我们熟悉的描述颜色值的方式：

```
#09f 
#90eafe
rgb(123, 123, 123)
rgba(123, 123, 123, .5)
hsl(220, 50%, 50%)
hsla(220, 50%, 50%, .5)


```

都有了新的语法规则，特别是对于 `rgb()`、`rgba()` 、`hsl()` 和 `hsla()` ，以往用逗号（`,`）作为分割符，现在可以直接使用空格做分割符，并且`rgb()` 和 `hsl()` 函数在第三个值和第四个值之间加上 `/` 可以取替 `rgba()` 和 `hsla()` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9aa962672e2e4d0eb6cf34ae5255cabb~tplv-k3u1fbpfcp-zoom-1.image)



另外，十六进制描述颜色，也可以在原本的语法规则中最后两位添加新的位值来描述透度明。比如 `#rrggbbaa` 或 `#rgba` 。在Chrome 浏览器代码审查器中，已经可以看到这种语法规则的身影了： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/386d3875833141b9b60f4b20133f6469~tplv-k3u1fbpfcp-zoom-1.image)

也就是说，我们现在可以这样来描述颜色值： 

```
#hex-with-alpha {
  color: #0f08;
  color: #00ff0088;
}

#functional-notation {
  color: hsl(0deg 0% 0%);
  color: hsl(2rad 50% 50% / 80%);
  color: rgb(0 0 0);
  color: rgb(255 255 255 / .25);
}

#lab-and-lch {
  --ux-gray: lch(50% 0 0);
  --rad-pink: lch(50% 200 230);
  --rad-pink: lab(150% 160 0);
  --pale-purple: lab(75% 50 -50);
}


```



另外，我们现在描述颜色都是在`sRBG` 色值空间，而颜色色值空间是一个复杂的体系，除了 `sRGB` 之外还有其他的色值空间，比如说 `LCH`： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d1ac4d669e7d4b098ac57a56056888b4~tplv-k3u1fbpfcp-zoom-1.image)

正如上图所示，`LCH` 颜色空间的颜色数量要比 `sRGB` 颜色空间的多，而且描述的颜色更为细腻。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9dfc3ba65cae44f2868b1ab698e5f705~tplv-k3u1fbpfcp-zoom-1.image)

如上图所示，可以在`color()` 函数中指定颜色空间： 

```
#color-function {
  --rad-pink: color(display-p3 1 0 1);
  --rad-pink: color(lab 50% 150 -50);
  --rad-pink: color(srgb 100% 0% 50%);
}


```

注意，使用了`color()` 函数指定颜色空间除了要考虑该函数支持度（浏览器的兼容性）还需要考虑硬件设备对颜色空间的支持度。 

在 CSS 中可以借助媒体查询 `@media` 来做相应的判断，比如下面的示例，如果终端设备支持的话，就会采用指定颜色空间的色值： 

```
@media (dynamic-range: high) {
  .neon-pink {
    --neon-glow: color(display-p3 1 0 1);
  }
  
  .neon-green {
  	--neon-grow: color(display-p3 0 1 0);
  }
}



```

更为强大的是 [**CSS 颜色模块 Level 5 版本**](https://www.w3.org/TR/css-color-5/)，对颜色函数能力做了进一步的扩展。比如，可以在 `rgb()`、`hsl()` 、`hwb()` 、`lab()` 和 `lch()` 函数基础上添加 `from` 关键词，实现基于一个颜色的基础上，只对某个参数做调整： 

```
rgb() = rgb([from <color>]? <percentage>{3} [ / <alpha-value> ]? ) | rgb([from <color>]? <number>{3} [ / <alpha-value> ]? ) 
hsl() = hsl([from <color>]? <hue> <percentage> <percentage> [ / <alpha-value> ]? ) 
hwb() = hwb([from <color>]? <hue> <percentage> <percentage> [ / <alpha-value> ]? ) 
lab() = lab([from <color>]? <percentage> <number> <number> [ / <alpha-value> ]? ) 
lch() = lch([from <color>]? <percentage> <number> <hue> [ / <alpha-value> ]? )


```

来看一个 `hsl()` 的示例： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7179196080694803a71c0db47151a76b~tplv-k3u1fbpfcp-zoom-1.image)







上图展示的是，基于 `--theme-primary` （原色，即 `hsl(274, 61%, 50%)`）颜色只对`l` 参数做调整，从 `50%` 调整到 `30%` ，从而获得一个新的颜色，即 `hsl(274, 61%, 30%)` 。使用这样的方式，我们就可以很轻易的获取基于某个颜色参数改变得来的颜色面板： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a8c150390fb2420ca336e2b87a598d52~tplv-k3u1fbpfcp-zoom-1.image)



除此之外， 颜色模块 Level 5 版本还新增了一些新的函数用来描述颜色，比如 `color-mix()` 、`color-contrast()` 、`color-adjust()` 等： 

```
.color-mix {
  --pink: color-mix(red, white);

  --brand: #0af;
  --text1: color-mix(var(--brand) 25%, black);
  --text2: color-mix(var(--brand) 40%, black);
}

.color-contrast {
  color: color-contrast(
    var(--bg)
    vs
    black, white
  );
}

--text-on-bg: color-contrast(
  var(--bg-blue-1)
  vs
  var(--text-subdued),
  var(--text-light),
  var(--text-lightest)
);

.color-adjust {
  --brand: #0af;
  --darker: color-adjust(var(--brand) lightness -50%);
  --lighter: color-adjust(var(--brand) lightness +50%);
}


```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/57b7f856d3c84ccebe6da66a777ee0e4~tplv-k3u1fbpfcp-zoom-1.image)

简单介绍一下这几个颜色函数。其中 `color-mix()` 函数接受两个 `<color>` 值，并在给定的颜色空间中以指定的数量返回它们混合的结果。如果没有特殊说明，否则在 `lch()` 颜色空间中进行混合。比如下图所展示的就是 在`lch()` 颜色空间（默认）中将 `red` 和 `yellow` 混合，每个 `lch` 通道的红色值占`65%`，黄色值占 `35%`，即 `color-mix(red yellow 65%)` ：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f67835be7e934ded92f40b638ca7216c~tplv-k3u1fbpfcp-zoom-1.image)

`color-contrast()` 函数很有意思，它可以帮助我们提高Web可访问性方面的能力。其主要作用是获取一个颜色值，并将其与其他颜色值的列表进行比较，从列表中选择对比度最高的一个。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a087b128e59b432abc79b9ca60875fc3~tplv-k3u1fbpfcp-zoom-1.image)

比如 `color-contrast(white vs red, white, green)` ，分别会拿 `red` 、`white` 和 `green` 与 `white` 对比，其中 `green` 与 `white` 对比度最高，最终会取 `green` 颜色： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/36d799f992ea421f96d7752d913aa8ff~tplv-k3u1fbpfcp-zoom-1.image)

`color-adjust()` 函数可用来在给定颜色空间中通过指定的变换函数对该颜色进行调整，除非另有规定，否则调整是在 `lch()` 色彩空间中进行的。比如 `color-adjust(#0af lightness +50%)` 是颜色 `#0af` 在 `lch` 颜色空间中高度增加 `50%` 。

也就是说，在不久的未来，这些颜色函数可以很好的帮助我们控制Web中的颜色值。因为这些颜色函数都已成为主流浏览器的实验性属性，特别是 Safari 浏览器，对这方面的支持度要高于 Chrome 和 Firefox 浏览器。 

## CSS 背景

CSS 的 `background` 属性是一个简写属性，这个对于大家来说再熟悉不过了。不过在这里着重想把 `background-position` 和 `background-repeat` 单独拿出来和大家聊聊。你可能会问，这两个属性又不是什么新属性了，有什么值得聊的呢？其实未必，这两个属性虽然老，但有几个小细节对于很多开发者来说还是会感到陌生的。咱们先从 `backgroundd-position` 开始。 

### background-position

`background-position` 主要是用来指定背景图片在容器中的位置。大家较为熟悉的是，背景图片左上角（顶点）相对于容器左上角计算，如下图所示： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7581ddea6ae43b497d51605edb447a8~tplv-k3u1fbpfcp-zoom-1.image)

不过，有的时候，希望背景图片能相对于容器右侧边缘或底部边缘计算，比如下图： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/950da2fe97824f1f9f6a9efb8135d907~tplv-k3u1fbpfcp-zoom-1.image)

要让背景图片距离容器右侧边缘和底部边缘都是 `50px` 。针对这样的场景，你或许首先会想到使用容器大小和背景图片大小进行计算，得出距离顶部和左侧边缘的距离，然后将计算出来的值运用于 `background-position` 中。当然，熟悉 CSS 的同学或许会想到使用 `calc()` 函数： 

```
:root {
	--xPosition: 50px;
  --yPosition: 50px;
}

.container {
	background-position: calc(100% - var(--xPosition)  calc(100% - var(--yPosition)))
}


```

使用 `calc()` 动态计算要比通过容器和背景图片尺寸大小计算方便得多。事实上呢？`background-position` 提供了另一种特性，我们可以通过关键词 `top` 、`right` 、`bottom` 和 `left` 来指定方向。比如我们熟悉的用法 ： 

```
background-position: 50px 50px;

// 相当于
background-position: top 50px  left 50px;


```

那么，我们要实现上图的效果，就可以使用 `right` 和 `bottom` 关键词，让事情变得非常简单： 

```
:root {
	--xPosition: 50px;
  --yPosition: 50px;
}

.container {
	background-position: right var(--xPosition)  bottom var(--yPosition);
}


```

最终效果如下：

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/mdWyryr)



注，示例中左侧是使用 `calc()` 计算的效果，右侧使用的是关键词实现的效果。 

`background-position` 使用还有一个小细节需要注意，即 `background-position` 采用百分比值。因为使用百分比值的 `background-position` 计算会相对于其他单位值复杂： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a63bc70e88c3447ca6d8cd4ec602fe6b~tplv-k3u1fbpfcp-zoom-1.image)

正如上图所示，背景图片原始尺寸是 `100px x 100px` ，容器的尺寸（使用该背景图片的元素）是 `410px x 210px` ，如果使用 `background-position: 75% 50%` 时，它的计算如下： 

```
// background-position的x轴坐标
x = (容器宽度 - 背景图片宽度) x background-position的x坐标的百分比值 = (410 - 100) x 75% = 232.5px

// background-position的y轴坐标
y = (容器高度 - 背景图片高度) x background-position的y坐标的百分比值 = (210 - 100) x 50% = 55px


```

就该示例而言，`background-position: 75% 50%` 就相当于 `bacckground-position: 232.5px 55px`。 

注意，如果我们把 `background-size` 和 `background-position` 取百分比值场景结合起来使用的时候，会让事情变得更为复杂，特别是`background-size`取值为`cover` 或`contain` 更会让你感到蛋疼。为什么会这样呢？这已经超出来这篇文章所要探讨的话题，如果你感兴趣的话，可以以此为课题，深入探讨一下。 

### background-repeat

在 `background-repeat` 属性上除了可以使用我们熟悉的 `no-repeat` 和 `repeat` (或者 `repeat-x` 和 `repeat-y`) 之外还可以使用 `round` 和 `space` 。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9836d1bd9d0b457ba84221f9eca5fb1e~tplv-k3u1fbpfcp-zoom-1.image)

我们都知道，使用 `repeat` 的时候，有可能会造成背景图片在平铺的时候被裁剪。如果希望背景图片在平铺时不被剪切，那么可以使用 `round` 来替代，它的最大特色就是背景图片在平铺的时候会根据容器的宽高对背景图片做相应的尺寸调整。而 `space` 会留出相应的空间，即在保证背景图片不被裁剪的情况之下，对多出来的空间以空白的方式在背景图片之间留出。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7fb2e55ee2024cfd959089e3acbd4371~tplv-k3u1fbpfcp-zoom-1.image)

针对于这些值，我录制了一个简单的视频，从视频的展示效果中可以更好的看出它们之间的差异： 



[![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1215aed28cb14e4d833f1835aa402082~tplv-k3u1fbpfcp-zoom-1.image)](https://yuque-api.antfin-inc.com/f2e-ai-group/d2c/ar18ce?_lake_card={"status"%3A"done"%2C"name"%3A"iShot2021-05-27+15.40.44.mp4"%2C"size"%3A"24919903"%2C"taskId"%3A"ue4a08950-515b-4815-87c6-2b1d96b0968"%2C"taskType"%3A"upload"%2C"url"%3Anull%2C"cover"%3Anull%2C"videoId"%3A"inputs%2Fprod%2Flark%2F2021%2F34229%2Fmp4%2F1622101348345-44f225ca-55b1-47e7-a11f-d222e5e862ab.mp4"%2C"download"%3Afalse%2C"__spacing"%3A"both"%2C"id"%3A"S7a4g"%2C"margin"%3A{"top"%3Atrue%2C"bottom"%3Atrue}%2C"card"%3A"video"}#S7a4g)

[
 ](https://yuque-api.antfin-inc.com/f2e-ai-group/d2c/ar18ce?_lake_card={"status"%3A"done"%2C"name"%3A"iShot2021-05-27+15.40.44.mp4"%2C"size"%3A"24919903"%2C"taskId"%3A"ue4a08950-515b-4815-87c6-2b1d96b0968"%2C"taskType"%3A"upload"%2C"url"%3Anull%2C"cover"%3Anull%2C"videoId"%3A"inputs%2Fprod%2Flark%2F2021%2F34229%2Fmp4%2F1622101348345-44f225ca-55b1-47e7-a11f-d222e5e862ab.mp4"%2C"download"%3Afalse%2C"__spacing"%3A"both"%2C"id"%3A"S7a4g"%2C"margin"%3A{"top"%3Atrue%2C"bottom"%3Atrue}%2C"card"%3A"video"}#S7a4g)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/mdWyOOj)

## CSS 蒙层和剪切

如果你对设计或设计软件较为熟悉的话，对于蒙层和剪切不会感到陌生。设计师在做一些设计稿的时候，时常也会用到蒙层和剪切的能力。随着 CSS 的发展，在 CSS 的世界中也有了这两个特性，它们在 W3C 的 《[**CSS Masking Module Level 1**](https://www.w3.org/TR/css-masking-1/)》规范中定义，主要的作用如下图所示： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9e585bc2ec34e629d2f275e2bbcdef8~tplv-k3u1fbpfcp-zoom-1.image)

可以灵活的控制内容的显示区域。 

蒙层和剪切对应的 CSS 属性就是 `mask` 和 `clip-path` ，其中 `mask` 是一个简写属性，它的使用规则和 `background` 非常的相似。我将通过简单的示例来向大家展示它们能帮我们做些什么。 

先看 `mask` ： 

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/MdQrvR)

视频中的 emoji 和文本残缺不全（看上去被啃了一样）。在没有`mask` 的能力之前，如果我们要实现这样的效果几乎是不太可能，现在有了之后，实现起来就非常的简单。我们只需要像下面这样的一张图片（用于`mask-image` 上的图片），即蒙层图： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f18fe73d63b447778495fda0975fddd9~tplv-k3u1fbpfcp-zoom-1.image)



```
h1 {
	mask-image: url(mask.png);
}


```



而且我们还可以借助 `mask` 的合成能力，让多个蒙层做合成运算： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/789c60bb1b944f4db59afe33d8ec1ff9~tplv-k3u1fbpfcp-zoom-1.image)

运用蒙层相关的能力，可以快速帮助我们实现一些业务场景所需的效果：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba513fd42745453ca77f0beb2bd0690f~tplv-k3u1fbpfcp-zoom-1.image)



除此之外还可以实现换肤的效果：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5b94211b8ad241bea602ba13098d085c~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/yWvzYy)



再来看剪切。 在 CSS 中使用 `clip-path` 的能力，除了可以帮助我们控制好所要展示的区域之外，还可以结合不同的路径和函数值实现不规则的图形展示。比如 [**Clippy**](https://bennettfeely.com/clippy/) 所展示的效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4f978ed0206a4e35b0837e7e891f8dad~tplv-k3u1fbpfcp-zoom-1.image)

比如在实际需求中，需要实现一些不规则，又是多状态下的 UI 效果，那么 `clip-path` 就会很方便： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7f5c1a19cb18482f997f8ebf08a8d6e9~tplv-k3u1fbpfcp-zoom-1.image)

而且使用`clip-path` 结合 `transition` 或 `animation` 可以在交互上做一些更好的动效： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6f39ed1ae35341c7997767fe00bfe386~tplv-k3u1fbpfcp-zoom-1.image)

左侧是原状态下效果，右侧是鼠标悬浮状态下效果： 

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/zYqbgRK)



特别是，当`clip-path` 支持 `path()` 函数时，可以做得事情更多了，比如我们想要绘制一个心形的 UI 效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/450d43d03e9646188824c031151bebff~tplv-k3u1fbpfcp-zoom-1.image)

另外就是把 `clip-path` 、`float` 和 CSS 的 Shapes 结合可以实现一些不规则的图文混排的布局效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b27e5786cb146818d4dce83677f760b~tplv-k3u1fbpfcp-zoom-1.image)





## CSS 混合模式



CSS 混合模式是个很有意思的特性，目前主要有 `mix-blend-mode` 和 `background-blend-mode` 两个属性，前者是用于多个元素的合成，后者是用于多个背景的合成。使用它们可以实现一些特殊的效果，比如类似 Photoshop 中的滤镜效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1796264b9de24bf68c212123465a2518~tplv-k3u1fbpfcp-zoom-1.image)

采用混合模式特性，我们可以轻易的实现产品图换色的效果： 

> Demo: [codepen.io/kylewetton/…](https://codepen.io/kylewetton/full/OJLmJoV)



这里简单地介绍一下，这个效果是怎么实现的。 

首先我们有一张类似下图的产品图： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f6df13a9d4a44573a45e9fc5cbcf012d~tplv-k3u1fbpfcp-zoom-1.image)

通过 SVG 的能力，我们描绘图一个纯黑色的 SVG 形状，形状和上图的沙发是相吻合的： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2ae79256f06d4d65900646d5d7b16064~tplv-k3u1fbpfcp-zoom-1.image)



描绘出来的 SVG 代码并不复杂： 

```
<svg id="js-couch" class="couch__overlay" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="none" width="1000" height="394"> 
  <defs> 
    <path d="M996.35 77.55q-1.85-1.95-8.65-3.75l-62.4-17.1q-9.3-2.75-12.15-2.5-1.8.15-2.85.45l-.75.3q2.25-16.3 3.75-22.05 1.15-4.4 1.4-10.8.2-6.6-.7-10.85-1.25-5.65-3.1-7.8-2.95-3.35-9.65-2.7-5.95.6-39.3 1.7-38.3 1.25-39.45 1.3-10.25.5-126.75.5-5.05 0-54.2 1.3-45.8 1.25-54.05.95-19.45-.45-30.4-.7-20.2-.55-23.1-1.3-22.3-5.85-26.5 1.25-2.65 4.55-3.85 7.9-.6 1.7-.7 2.5-.65-2.2-2.05-4.55-2.75-4.65-6.45-5.2-3.85-.55-13.65-.4-7.4.1-12 .4-.4.05-18.7.9-16.55.8-19.15 1.1-3.4.4-14.6 1.1-11.3.75-13.05.65h-9.8q-8.65-.05-11.45-.4-2.85-.35-9.25-.6-6.7-.15-8.5-.25-2.7-.1-27.75-.1-25.1 0-29.6.1-92.35 1.15-99 1.65-5.15.4-20 0-15.3-.4-24.4-1.25-6.75-.6-21-1.55-12.95-.9-14.85-1.1-6.45-1.05-11.05-1.5-8.7-.85-12.85.5-5.45 1.75-8.1 4.65-3.2 3.4-2.9 8.6.25 4.65 2.1 11.8 1 3.8 2.55 9.1 1 3.85 2.35 10.1-.1 1-1.5 1-1.75 0-7.7.85-7.1 1-9.8 2.05-2.4.9-23 4.75-21.2 3.9-22.05 4.15-8.2 1.85-15.05 3.35Q7.4 69.1 5.65 70.3 2.5 72.45 2 73.1.6 75 .75 79.2q.15 4.15 1.3 12.75.9 6.85 1.45 10 .5 2.75 8.55 54 6.65 42.15 7.35 46.85 1.15 7.65 4.9 28.55 4.55 25.2 6.35 31.2 2.45 8.15 3.8 11.75 1.85 4.9 3.2 5.75 1.25.8 6.85.65 2.75-.05 5.3-.25l23.85.35q.1 0 1 .95t2 .95q1.9 0 3.4-1.4l23.1-.25 43.65.4q135.05 2.15 137.9 1.9 1.25-.1 72.9.5 72.45.65 76.85.45 8.1-.35 64 .4 143.35.95 146 1.1.55.05 75.3.3 74.7.3 79.8.6 8.65.5 68.25-.35l51.75.5 1.6.4q1.95.35 3.8.05 1.45-.25 3.5-.2 1.9 0 3.35-.3 2.1-.45 8.25-.8 6.25-.3 8.75-.05 1.7.2 8 1 5.75.3 7.4-1.75 1.75-2.2 4.95-10.85 2.8-7.55 4.05-12.4.65-2.5 3.6-17.2 2.75-13.75 3.15-14.8.45-1.25 4.45-22.85 4.05-22.4 4.4-24.4.3-1.45 3.75-25.2 3.35-23.2 4-26.3 1.15-5.5 2.35-18.8 1.4-15.7.8-23.7-.6-8.35-3.35-11.15z" id="a" /> 
	</defs> 
  <use xlink:href="#a"/> 
</svg>


```



使用 CSS 自定义属性，配合少许的 JavaScript代码即可完成产品图换色的效果： 

```
:root {
	--fill: #f36;
}

svg {
	fill: var(--fill);
  mix-blend-mode: multiply;
}


```



我们使用 `mix-blend-mode` 的 `multiply` 效果。另外，JavaScript 动态改变 `--fill` 的值代码也简单： 

```
const inputEle = document.querySelector('input') 

inputEle.addEventListener('change', (e)=>{ 
  document.documentElement.style.setProperty('--fill', e.target.value); 
})


```



很简单吧。而且你可以发挥你的想象空间，可以实现很多具有创意的效果。 

## CSS 自定义属性

CSS 自定义属性可以说是 CSS 的标配了，正如 W3C 规范所描述的，[**CSS 自定义属性又被称为 CSS 变量**](https://www.w3.org/TR/css-variables-1/#cycles)： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b0fc923b6f354d8d941a69e6f2c2a0dc~tplv-k3u1fbpfcp-zoom-1.image)

不过在这里要说的不是 CSS 自定义如何使用？要和大家说的是 CSS 自定义属性中的无效变量。CSS 自定义属性中的无效变量是很有用的一个特性，它可以实现 `1` （真）和 `0` （假）的开关切换效果。换句话说，可以很好的实现不同状态的 UI 效果。 

同样地，在 W3C 规范中对无效变量有详细描述，但不仔细的话，还是会忽略该特性。先来看[**规范是怎么描述无效变量**](https://www.w3.org/TR/css-variables-1/#invalid-variables)的： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e9ddd6f8b95c49babce865b554517934~tplv-k3u1fbpfcp-zoom-1.image)

它的意思是： 

> **当一个自定义属性的值是 \**`\*\*initial\*\*`\** 时，**`**var()**`** 函数不能使用它进行替换。除非 指定了一个有效的回退值，否则会使声明在计算值时无效。 **



在 CSS 中注册自定义属性时是使用 `--` 来注册的，可以给已注册的自定义属性赋值，包括空字符串，但其中有一个细节非常有意思： 

```
:root {
	--invalid:; // 注意冒号和分号之间无空格符，也无任何字符
  --valid: ;  // 注意冒号和分号之间只有一个空格符
}


```

其中，`--invalid` 自定义属性被称为无效变量，而 `--valid` 自定义属性是一个有效变量。使用上面这种方式来区分有效和无效变量对于开发者而言，可读性极差，而且有些文本编辑器可能会对代码按自己配置的规格格式化，有可能会造成 `--invalid:;` 变成 `--invalid: ;` （有空格）。为此，一般使用关键词 `initial`来显式声明一个自定义属性为无效变量：

```
:root {
	--invalid: initial; // 无效变量
  --valid: ;					// 有效变量
}


```

如果不使用 `var()` 函数调用已注册的自定义属性的话，那么对于已注册的自定义属性而言，不会起任何作用。而 `var()` 函数有两个参数，第一个参数就是自定义属性，第二个参数是备用值。当第一个参数是个无效值时，会采用第二个参数。正因如此，对于已注册的无效自定义属性（即无效变量），比如 上面代码中的 `--invalid` 。那么 `var()` 没有提供备用值（第二个参数），则会使 CSS 样式规则（声明）在计算值时无效： 

```
:root {
	--invalid: initial; // 无效变量
  --valid: ;          // 有效变量
}

foo {
	background-color: var(--invalid); // 未提供备用值，则background-color 计算值无效
  color: var(--invalid, red)        // 提供了备用值，--invalid是无效变量，则会采用备用值 red
}


```

先来看一个简单的示例： 

```
<div class="element">
  <i>Element</i>
</div>

<i>Element</i>

<style>
  .element {
  	--color: red;  // 只作用于 .element 元素及其后代元素
	}

	i {
  	--foo: initial;                         // 无效变量
  	--color: var(--foo);										// 无效变量
  	background-color: var(--color, orange); // --color是无效变量，采用备用值 orange
	}
</style>


```

运行上面的代码，你将看到的效果如下图所示： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9cdfd75da904c838b5453e916ed6177~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/GRWJGvx)

再来看另一个示例： 

```
<section>Element</section>

<style>
  :root {
    --initial: initial; // 无效变量
    --valid: ;					// 有效变量
	}
  section {
  	background-color: var(--initial, red); // 无效变量，会采用备用值 red
    color: var(--valid, red);							 //	有效变量，备用值被忽略
  }
</style>  


```

因此，你看到的效果如下： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc85ce59d51049c9af57674e17f7be37~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/poeJKpM)



这个示例中 `color` 属性相当于设置了 `color: ;` 值，客户端在计算该值时将被忽略，因此会继承其祖先元素的 `color` 值，该例继承了 `body` 元素的 `color` 值 `black` 。 

你可能对 CSS 自定义属性中的无效变量有了一定的认知。我们继续往下。 

为了在使用 CSS 自定义属性中的无效变量让开发者更易于理解，[@Lea Verou](https://twitter.com/LeaVerou)在《[**The --var: ; hack to toggle multiple values with one custom property**](https://lea.verou.me/2020/10/the-var-space-hack-to-toggle-multiple-values-with-one-custom-property/)》引入了类似于Switch的概念，即： 

```
:root {
	--ON: initial; // 无效变量,相当于开启 var()的备用值
  --OFF:;        // 有效变量,相当于关闭 var()的备用值
}


```

这样一来，我们在做 UI 不同状态切换时，就只需要对 `--ON` 和 `--OFF` 的切换。比如 @Lea Verou 在文章中提供的示例：

```
:root {
  --ON: initial;
  --OFF: ;
}

button {
  --is-raised: var(--OFF);
  
  border: 1px solid var(--is-raised, rgb(0 0 0 / 0.1));
  background: var(
      --is-raised,
      linear-gradient(hsl(0 0% 100% / 0.3), transparent)
    )
    hsl(200 100% 50%);
  box-shadow: var(
    --is-raised,
    0 1px hsl(0 0% 100% / 0.8) inset,
    0 0.1em 0.1em -0.1em rgb(0 0 0 / 0.2)
  );
  text-shadow: var(--is-raised, 0 -1px 1px rgb(0 0 0 / 0.3));
}

button:hover {
  --is-raised: var(--ON);
}

button:active {
  box-shadow: var(--is-raised, 0 1px 0.2em black inset);
}


```

最终效果如下： 

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/XWNYRga)



上面这个示例算是简单的了。不过我们可以利用该特性实现更复杂的 UI 效果。比如下面这个多种换肤效果： 

```
label {
  --box-shadow: var(--ON);
  --box-shadow-active: var(--OFF);
  box-shadow: 0 0 0 3px var(--box-shadow, rgba(0, 0, 0, 0.05))
    var(--box-shadow-active, #2196f3);
  cursor: pointer;
}

label.dark {
  background-color: var(--dark-bgcolor);
}

label.light {
  background-color: var(--light-bgcolor);
}

label.blue {
  background-color: var(--blue-bgcolor);
}

#dark:checked ~ div .dark,
#light:checked ~ div .light,
#blue:checked ~ div .blue {
  --box-shadow: var(--OFF);
  --box-shadow-active: var(--ON);
}

.nav {
  color: var(--light, var(--light-color)) var(--dark, var(--dark-color))
    var(--blue, var(--blue-color));
  background-color: var(--light, var(--light-bgcolor))
    var(--dark, var(--dark-bgcolor)) var(--blue, var(--blue-bgcolor));
}

a.active,
a:hover {
  background-color: var(--light, var(--light-active-bgcolor))
    var(--dark, var(--dark-active-bgcolor))
    var(--blue, var(--blue-active-bgcolor));
}

/* 设置切换开关 */
:root {
  --ON: initial;
  --OFF: ;

  /* Dark */
  --dark-color: rgba(156, 163, 175, 1);
  --dark-bgcolor: rgba(17, 24, 39, 1);
  --dark-active-bgcolor: rgba(55, 65, 81, 1);

  /* Light */
  --light-color: rgba(55, 65, 81, 1);
  --light-bgcolor: rgba(243, 244, 246, 1);
  --light-active-bgcolor: rgba(209, 213, 219, 1);

  /* Blue */
  --blue-color: rgba(165, 180, 252, 1);
  --blue-bgcolor: rgba(49, 46, 129, 1);
  --blue-active-bgcolor: rgba(67, 56, 202, 1);
}

#dark:checked ~ .nav {
  --light: var(--OFF);
  --dark: var(--ON);
  --blue: var(--OFF);
}

/* 默认为Light */
#light:checked ~ .nav {
  --light: var(--ON);
  --dark: var(--OFF);
  --blue: var(--OFF);
}

#blue:checked ~ .nav {
  --light: var(--OFF);
  --dark: var(--OFF);
  --blue: var(--ON);
}


```

效果如下： 

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/OJbEzzw)

##  CSS 等比缩放

CSS 等比缩放一般指的是 “容器高度按比例根据宽度改变”，很多时候也称为宽高比或纵宽比。 众所周知，我们开发 Web 页面要面对的终端更复杂的了，而这些终端的宽高比都不一样。常见的比例有： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9b4b9f82d52b4b3cb46850ac0e29e24b~tplv-k3u1fbpfcp-zoom-1.image)

特别是在做媒体相关开发的同学，比如视频、图像等，这方面的需求会更多，比如 Facebook 上的图片，视频展示： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/af2fd1a26e7e4d77a9df108c725b05b1~tplv-k3u1fbpfcp-zoom-1.image)

CSS 在还没有 `_aspect-ratio_` 之前，常使用一些 Hacck 手段来实现实类似的效果，即使用 `padding-top` 或 `padding-bottom` 来实现： 

```
<aspectratio-container> 
  <aspectratio-content></aspectratio-content> 
</aspectratio-container> 

<style>
	.aspectratio-container { 
  	width: 50vmin; /* 用户根据自己需要设置相应的值 */ 
  
  	/* 布局方案可以调整 */ 
  	display: flex; 
  	justify-content: center; 
  	align-items: center; 
  } 
  
  /* 用来撑开aspectratio-container高度 */ 
  .aspectratio-container::after { 
    	content: ""; 
    	width: 1px; 
    	padding-bottom: 56.25%; 
    	
    	/*元素的宽高比*/ 
    	margin: -1px; 
    	z-index: -1; 
  }
</style>  


```



有了 CSS 自定义属性之后，可以结合 `calc()` 函数来实现容器等比缩放的效果： 

```
.container {
	--ratio: 16/9;
  height: calc(var(--width) * 1 / (var(--ratio)));
  width: 100%;
}


```



虽然比`padding-top` 这样的Hack 手段简单，但相比原生的`aspect-ratio`还是要复杂的多。即: 

```
.container {
	width: 100%;
  aspect-ratio: 16 / 9;
}


```

下面这个示例演示了这三种不同方案实现宽比的效果：

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/ExWjeZr)



还可以通过 `@media` 让元素在不同的终端上按不同的比例进行缩放： 

```
.transition-it {
  aspect-ratio: 1/1;
  transition: aspect-ratio .5s ease;

  @media (orientation: landscape) { & {
    aspect-ratio: 16/9;
  }}
}


```

## CSS 滚动捕捉

在 Web 布局中，时常会碰到内容溢出容器的现状，如果 `overflow` 设置为 `auto` 或 `scroll` 时容器会出现水平或垂直滚动条： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/252bca72f43a4d3eb71bc453c5bcabb9~tplv-k3u1fbpfcp-zoom-1.image)

为了给用户提供更好的滚动体验，CSS 提供了一些优化滚动体验的 CSS 特性，其中滚动捕捉就是其中之一。CSS 的滚动捕捉有点类似于 Flexbox 和 Grid 布局的特性，分类可用于滚动容器的属性和滚动项目的属性： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/81428188792b45458a176ee69f6609e8~tplv-k3u1fbpfcp-zoom-1.image)

有了滚动捕捉特性，我们要实现类似下图的效果就可以不需要依赖任何 JavaScript 库或脚本： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cc89fce86aef4efaafbb8fb4862c179e~tplv-k3u1fbpfcp-zoom-1.image)

就是每次滚动时，图片的中心位置和容器中心位置对齐（想象一下 Swiper 的效果）。关键代码就下面这几行： 

```
.container {
  scroll-behavior: smooth;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x mandatory;
  scroll-padding: 20px;
}

img {
  scroll-snap-align: center;
  scroll-snap-stop: always;
}



```

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/mdRpboo)

利用该特性，还可以实现类似 iOS的一些原生交互效果：

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/PoWQPvN)

要是再利用一点点JavaScript脚本，还可以实现沉浸式讲故事的交互效果： 

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/qBRxNOo)

## CSS Gap（沟槽）

CSS 的 `gap` 属性的出现，帮助我们解决了以前一直比较麻烦的布局效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/295e2852c22d48bbadd3584764f73681~tplv-k3u1fbpfcp-zoom-1.image)

正如上图所示，设计师期望的一个效果是，紧邻容器边缘没有任何间距，但相邻项目之间（水平或垂直方向）都有一定的间距。在没有 `gap` 属性之前使用 `margin` 是很烦人的，特别是多行多列的时候更麻烦。有了 `gap` 仅需要一行代码即可。 

CSS 的 `gap` 属性是一个简写属性，分为 `row-gap` 和 `column-gap` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba289b4eb54845cf986cb4474c31d1f3~tplv-k3u1fbpfcp-zoom-1.image)

该属性 `gap` 到目前为止只能运用于多列布局，Flexbox布局和网格布局的容器上： 

```
// 多列布局 
.multi__column { 
  gap: 5ch 
} 

// Flexbox布局 
.flexbox { 
  display: flex; 
  gap: 20px 
} 

// Grid布局 
.grid { 
  display: grid; 
  gap: 10vh 20% 
}


```

`gap` 属性可以是一个值，也可以是两个值： 

```
.gap { 
  gap: 10px; 
} 
// 等同于 
.gap { 
  row-gap: 10px; 
  column-gap: 10px 
} 

.gap { 
  gap: 20px 30px; 
} 
// 等同于 
.gap { 
  row-gap: 20px; 
  column-gap: 30px; 
}


```

如果 `gap` 仅有一个值时，表示 `row-gap` 和 `column-gap` 相同。 

## CSS 逻辑属性

国内大多数 Web 开发者面对的场景相对来说比较单一，这里所说的场景指的是书写模式或排版的阅读模式。一般都是 `LTR` (Left To Right)。但有开发过国际业务的，比如阿拉伯国家的业务，就会碰到 `RTL` (Right To Left) 的场景。比如你打开 Facebook ，查看中文和阿拉伯文两种语言下的 UI 效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cca27d740f914d08ba635882f55d754d~tplv-k3u1fbpfcp-zoom-1.image)

在没见有逻辑属性之前，一般都会在 `<html>` 或 `<body>` 上设置 `dir` 属性，中文是 `ltr` ，阿拉伯语是 `rtl` ，然后针对不同的场景运用不同的 CSS 样式： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b397d344f0974e3da696b8f639293ee2~tplv-k3u1fbpfcp-zoom-1.image)

其实，阅读方式除了水平方向（`ltr` 或 `trl`）之外，还会有垂直方向的阅读方式： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1cbffd75fae476f8127687d68be7ce7~tplv-k3u1fbpfcp-zoom-1.image)



为了让 Web 开发者能更好的针对不同的阅读模式提供不同的排版效果，在CSS新增逻辑属性。有了逻辑属性之后，以前很多概念都有所变化了。比如我们以前熟悉的坐标轴，`x` 轴和 `y` 轴就变成了 `inline` 轴 和 `block` 轴，而且这两个轴也会随着书写模式做出调整： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0109edbe56c9439da28f5fe7df1448cd~tplv-k3u1fbpfcp-zoom-1.image)

除此之外，我们熟悉的 CSS 盒模型也分物理盒模型和逻辑盒模型： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/85d563bc5ffa44e5bd950201b6b04ca5~tplv-k3u1fbpfcp-zoom-1.image)

你可能感知到了，只要是以前带有 `top`、`right` 、`bottom` 和 `left` 方向的物理属性都有了相应的 `inline-start` 、 `inline-end` 、`block-start` 和 `block-end` 的逻辑属性： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa2c5673f41744d0ab764151b89e0dfe~tplv-k3u1fbpfcp-zoom-1.image)

我根据 W3C 规范，把物理属性和逻辑属性映射关系整了一份更详细的表： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f27e7d446c284c8daa9500d93996c9d9~tplv-k3u1fbpfcp-zoom-1.image)



回到实际生产中来： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c238712bcfec482aa0ce7c04db7b73e6~tplv-k3u1fbpfcp-zoom-1.image)

如果不使用逻辑属性的话，要实现类似上图这样的效果，我们需要这样来编写 CSS： 

```
.avatar {
  margin-right: 1rem;
}

html[dir="rtl"] .avatar {
  margin-right: 0;
  margin-left: 1rem;
}


```

有了 CSS 逻辑属性之后，仅一行 CSS 代码即可实现： 

```
.avatar {
  margin-inline-end: 1rem;
}


```

简单多了吧，特别是有国际化需求的开发者，简直就是一种福音。

## CSS 媒体查询

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43a2b6b721254fa8b12b434ec359d655~tplv-k3u1fbpfcp-zoom-1.image)

CSS 媒体查询 `@media` 又称为 CSS 条件查询。在 [**Level 5 版本中提供了一些新的媒体查询特性，**](https://drafts.csswg.org/mediaqueries-5/#at-ruledef-custom-media)可以查询到用户在设备上的喜好设置：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d8fc954fa41d42788ddbefc5edd39b5e~tplv-k3u1fbpfcp-zoom-1.image)

比如： 

- `prefers-reduced-motion`
- `prefers-contrast`
- `prefers-reduced-transparency`
- `prefers-color-scheme`
- `inverted-colors`



使用的方式和以往我们熟悉的 `@media` 是相似。比如 `prefers-color-scheme` 实现暗黑查式的皮肤切换效果： 

```
// 代码源于： https://codepen.io/airen/full/ProgLL
// dark & light mode
:root {
  /* Light theme */
  --c-text: #333;
  --c-background: #fff;
}

body {
  color: var(--c-text);
  background-color: var(--c-background);
}

@media (prefers-color-scheme: dark) {
  :root {
    /* Dark theme */
    --c-text: #fff;
    --c-background: #333;
  } 
}


```



还可以根据网格数据设置来控制资源的加载： 

```
@media (prefers-reduced-data: reduce) {
  header {
    background-image: url(/grunge.avif);
  }
}

@media (prefers-reduced-data: no-preference) {
  @font-face {
    font-family: 'Radness';
    src: url(megafile.woff2);
  }
}


```

其他的使用方式和效果就不一一演示了。不过在未来，CSS 的 `@media` 在编写方式上会变得更简单： 

```
@media (width <= 320px) {
  body {
    padding-block: var(--sm-space);
  }
}

@custom-media --motionOK (prefers-reduced-motion: no-preference);

@media (--motionOK) {
  .card {
    transition: transform .2s ease;
  }
}

.card {
  @media (--motionOK) { & {
    transition: transform .2s ease;
  }}
}

@media (1024px >= width >= 320px) {
  body {
    padding-block: 1rem;
  }
}



```



> 特别声明，[该示例代码来自于 @argyleink 的 PPT](https://2021-hover-conf-new-in-css.netlify.app/#custom-media-queries) 。



自从折叠屏设备的出现，给 Web 开发者带来新的挑战： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e448a628972a4efa87d048480bc074ad~tplv-k3u1fbpfcp-zoom-1.image)

值得庆幸的是，微软和三星的团队就针对折叠屏幕设备提供了不同的 媒体查询判断。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f1b4087407164f2eb899c0bd8370e743~tplv-k3u1fbpfcp-zoom-1.image)

上图是带有物理分隔线的双屏幕设备：

```
main {
  display: grid;
  gap: env(fold-width);
  grid-template-columns: env(fold-left) 1fr;
}

@media (spanning: single-fold-vertical) {
  aside {
    flex: 1 1 env(fold-left);
  }
}


```

无缝的折叠设备： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/484887cb5084438fbec87f511e572894~tplv-k3u1fbpfcp-zoom-1.image)

```
@media (screen-fold-posture: laptop){ 
  body { 
    display: flex; 
    flex-flow: column nowrap; 
  } 
  .videocall-area, 
  .videocall-controls { 
    	flex: 1 1 env(fold-bottom); 
  } 
}


```



## CSS 比较函数

CSS 的比较函数是指 `min()` 、`max()` 和 `clamp()` ，我们可以给这几个函数传入值（多个）或表达式，它们会对传入的值进行比较，然后返回最合适的值。另外，这几个和我们熟悉的 `calc()` 类似，也可以帮助我们在 CSS 中做动态计算。 

### min() 和 max()

先看 `min()` 和 `max()` ，它们之间的差异只是返回值的不同： 

- `min()` 函数会从多个参数（或表达式）中返回一个最小值作为CSS属性的值，即 使用 `min()` 设置最大值，等同于 `max-width`
- `max()` 函数会从多个参数（或表达式）中返回一个最大值作为CSS属性的值，即 使用`max()`设置最小值，等同于 `min-width`

下图展示了 `min(50vw, 500px)` 在浏览器视窗宽度改变时，返回的值的变化： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/615c76f68fdf4b8ca59a09152bf450e3~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/mdeLMoZ)



把上面的示例的 `min()` 换成 `max()` 函数，即 `max(50vw, 500px)`，它的返回值是： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c2a71241801941cd82b8590ef5093528~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/oNjdGyv)

### clamp()

`clamp()` 和 `min()` 以及 `max()`略有不同，它将返回一个区间值，即 在定义的最小值和最大值之间的数值范围内的一个中间值。该函数接受三个参数： 

- 最小值（`MIN`）
- 中间值（`VAL`），也称首选值
- 最大值（`MAX`）



`clamp(MIN, VAL, MAX)`，这三个值之间的关系（或者说取值的方式）： 

- 如果 `VAL` 在 `MIN` 和 `MAX` 之间，则使用 `VAL` 作为函数的返回值
- 如果 `VAL` 大于 `MAX` ，则使用 `MAX` 作为函数的返回值
- 如果 `VAL` 小于 `MIN` ，则使用 `MIN` 作为函数的返回值



比如下面这个示例： 

```
.element { 
    /** 
    * MIN = 100px 
    * VAL = 50vw ➜ 根据视窗的宽度计算 
    * MAX = 500px 
    **/ 
    width: clamp(100px, 50vw, 500px); 
}


```



就这个示例而言，`clamp()` 函数的计算会经历以下几个步骤： 

```
.element { 
    width: clamp(100px, 50vw, 500px); 

    /* 50vw相当于视窗宽度的一半，如果视窗宽度是760px的话，那么50vw相当等于380px*/ 
    width: clamp(100px, 380px, 500px); 

    /* 用min()和max()描述*/ 
    width: max(100px, min(380px, 500px)) 

    /*min(380px, 500px)返回的值是380px*/ 
    width: max(100px, 380px) 

    /*max(100px, 380px)返回的值是380px*/ 
    width: 380px; 
}


```

示例效果如下：

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/pojVpJv)



简单地说，`clamp()` 、`min()` 和 `max()` 函数都可以随着浏览器视窗宽度的缩放对值进行调整，但它们的计算的值取决于上下文。

我们来看一个比较函数中 `clamp()` 的典型案例。假设我们需要在不同的屏幕（或者说终端场景）运用不同大小的 `font-size` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/321e512f1d9d426c953248b18d3640c3~tplv-k3u1fbpfcp-zoom-1.image)

在还没有 CSS 比较函数之前，使用了一个叫 CSS 锁（CSS Locks）的概念来实现类似的效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e0b9225240a4851b5f9151b47dcc7fc~tplv-k3u1fbpfcp-zoom-1.image)

需要做一些数学计算： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23ac0cda0664442f91c3252bf83d8954~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/bGqdOma)



使用 `clamp()` 之后，只需要一行代码就可以实现： 

```
/** minf: 20px (min font-size)
 * maxf: 40px (max font-size)
 * current vw: 100vw
 * minw: 320px (min viewport's width)
 * maxw: 960px (max viewport's width)
*/
h1 {
  font-size: clamp(20px, 1rem + 3vw, 40px);
}


```



使用这方面的技术，我们就可以轻易实现类似下图这样的效果： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/98dee56cdee943979857aba98c11065d~tplv-k3u1fbpfcp-zoom-1.image)

> 注，上图来自《[Use CSS Clamp to create a more flexible wrapper utility](https://piccalil.li/quick-tip/use-css-clamp-to-create-a-more-flexible-wrapper-utility/)》一文。

## CSS 内容可见性

CSS 内容可见性，说要是指 `content-visibilit` 和 `contain-intrinsic-size` 两个属性，目前隶属于 [**W3C 的 CSS Containment Module Level 2 模块**](https://www.w3.org/TR/css-contain-2/#content-visibility)，主要功能是可以用来提高页面的渲染性能。

一般来说，大多数Web应用都有复杂的UI元素，而且有的内容会在设备可视区域之外（内容超出了用户浏览器可视区域），比如下图中红色区域就在手机设备屏幕可视区域之外： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5e21388a75d4883b85333aaf3bed073~tplv-k3u1fbpfcp-zoom-1.image)

在这种场合下，我们可以使用CSS的 `content-visibility` 来跳过屏幕外的内容渲染。也就是说，如果你有大量的离屏内容（Off-screen Content），这将会大幅减少页面渲染时间。 

Google Chrome 团队有工程师对 `content-visibility` 做过相关的测试： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8e3a1204efc0496487cc02321d32257d~tplv-k3u1fbpfcp-zoom-1.image)

使用了 CSS 的 `content-visibility` 属性，浏览器的渲染过程就变得更加容易。本质上，这个属性 改变了一个元素的可见性，并管理其渲染状态。 

而 `contain-intrinsic-size` 属性控制由 `content-visibility` 指定的元素的自然尺寸。它的意思是，`content-visibility` 会将分配给它的元素的高度（`height`）视为 `0`，浏览器在渲染之前会将这个元素的高度变为 `0`，从而使我们的页面高度和滚动变得混乱。但如果已经为元素或其子元素显式设置了高度，那这种行为就会被覆盖。如果你的元素中没显式设置高度，并且因为显式设置 `height`可能会带来一定的副作用而没设置，那么我们可以使用`contain-intrinsic-size`来确保元素的正确渲染，同时也保留延迟渲染的好处。 

换句话说，`contain-intrinsic-size` 和 `content-visibility` 是一般是形影不离的出现： 

```
section {
  content-visibility: auto;
  contain-intrinsic-size: 1000px;
}


```

## CSS 内在尺寸

如果你使用浏览器开发者工具审查代码时，将鼠标放到一个 `<img>` 标签上，你会看到类似下图这样的： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/420d329e63e44c34beef33069cf48f3e~tplv-k3u1fbpfcp-zoom-1.image)

<img> 的 src 路径上浮出来的图片底下有一行对图像的尺寸的描述，即252 x 158 px (intrinsic: 800 x 533 px) ，其实现这表述图片尺寸中两个重要信息： ​

- 外在尺寸： `252 x 158 px` ，开发者给 `img` 设置的尺寸
- 内在尺寸： `800 x 533 px` ，图片原始尺寸



![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2aa4aa275a0843aab1b0565fec2d1def~tplv-k3u1fbpfcp-zoom-1.image)

其实在 CSS 中给一个元素框设置大小时，有的是根据**元素框内在的内容来决定，有的是根据上下文来决定的**。根据该特性，CSS的尺寸也分为内部(内在)尺寸和外部（外在）尺寸。 

- 内部尺寸：指的是元素根据自身的内容（包括其后代元素）决定大小，而不需要考虑其上下文；而其中 `min-content` 、 `max-content` 和 `fit-content` 能根据元素内容来决定元素大小，因此它们统称为内部尺寸。
- 外部尺寸：指的是元素不会考虑自身的内容，而是根据上下文来决定大小。最为典型的案例，就是 `width` 、`min-width` 、`max-width` 等属性使用了 `%` 单位的值。



通地一个简单的示例来向大家演示 CSS 内在尺寸的特性，即 `min-content` 、`max-content` 和 `fit-content` 的特性。 

```
<h1>CSS is Awesome</h1>

<style>
  h1 {
    width: auto;
  }
</style>  


```



先来看 `h1` 的 `width` 取值为 `auto` 和 `min-content` 的差异： 

```
// 外在尺寸
h1 {
	width: auto; // 根据容器变化
}

// 内在尺寸
h1 {
	width: min-content; // 根据内容变化
}


```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ca291c48ded846ea8935c0ac985f23df~tplv-k3u1fbpfcp-zoom-1.image)

> Demo： [codepen.io/airen/full/…](https://codepen.io/airen/full/zYZvGrY)



从上图中不难发现，`width` 取值为 `min-content` 时，`h1` 的宽度始终是单词“Awesome”长度（大约是`144.52px`）。它的宽度和容器宽度变化并无任何关系，但它受排版内相关的属性的影响，比如`font-size`、`font-family` 等。 

再来看`max-content` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1af2530a4b14ff2a073d757d21bc43f~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/zYZvGrY)



当`h1` 的 `width` 取值为 `max-content` 时，它的宽度是`h1` 所在行所有内容的宽度。最后来看 `fit-content` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1212a5a6bc4f49e49dc70e4cdf115afc~tplv-k3u1fbpfcp-zoom-1.image)



> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/zYZvGrY)



相对而言，`fit-content` 要比 `min-content` 和 `max-content` 复杂地多： 

```
h1 {
	width: fit-content;
}

// 等同于 
h1 {
	width: auto;
  min-width: min-content;
  max-width: max-content;
}


```

简单地说，`fit-content` 相当于 `min-content` 和 `max-content`，其 取值: 

- 如果元素的可用空间(Available)充足，`fit-content` 将使 用 `max-content`
- 如果元素的可用空间(Available)不够充足，比 `max-content` 小点，那就是用可用空间的值，不会导致内容溢出
- 如果元素的可用空间(Available)很小，比 `min-content`还小,那就使用 `min-content`



使用下图来描述它们之间的关系： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/99680561b42546a18d0255417d4e5481~tplv-k3u1fbpfcp-zoom-1.image)

`min-content`、`max-content` 和 `fit-content` 被称之个内在尺寸，它可以运用于设置容器尺寸的属性上，比如`width` 、`height` 以及 `inline-size` 和 `block-size` 等。但有些属性上使用的话则会无效： 

- `min-content`、`max-content` 和 `fit-content` 用于 `flex-basis` 无效
- `fit-content` 用于设置网格轨道尺寸的属性上无效
- 网格项目或Flex项目上显式设置 `width:fit-content`也无效,因为它们的默认宽度是 `min-content`
- 最好不要在 `min-width` 或 `max-width` 上使用 `fit-content`，易造成混乱，建议在 `width` 上使用 `fit-content`



在布局上使用 `min-content` 、`max-content` 或 `fit-content` 可以帮助我们设计内在布局，另外在构建一些自适应的布局也非常灵活。特别是和 CSS 的 Grid 和 Shapes 相关特性结合，还能构建一些具有创意的布局。 

最后有一点需要特别声明，`fit-content` 和 `fit-content()`函数不是相同的两个东东，使用的时候需要区别对待。 

## CSS 的 display

`display` 对于大家而言并不陌生，主要用来格式化上下文，这里特别拿出来和大家说的是因为 `display` 也有一些变化。其中之一就是 `display` 未来可以支持多个值： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59f9929a5d734841bc013814b5966115~tplv-k3u1fbpfcp-zoom-1.image)

据最新的消息，Sarafi 浏览器已经把`display` 设置两个值已进入实验性属性。`display` 取两个值的含义大致如下： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/53da18acd7904aee9ce89c870bae57a5~tplv-k3u1fbpfcp-zoom-1.image)

另外单独要说的是，`display` 新增了 `contennts` 属性值，[** W3C规范是这样描述的**](https://www.w3.org/TR/css-display-3/#box-generation)： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8e7fe82e04144101af1e13b66d7ec193~tplv-k3u1fbpfcp-zoom-1.image)

大致意思是说： 

> 设置了 `display: contents` 的元素自身将不会产生任何盒子，但是它的子元素能正常展示。



比如： 

```
<div class="outer">
  I'm, some content
  <div class="inner">I'm some inner content </div>
</div>

<style>
  .outer {
  	border: 2px solid lightcoral;
    background-color: lightpink;
    padding: 20px;
  }
  
  .innter {
  	background-color: #ffdb3a;
    padding: 20px;
  }
</style>


```



上面这个简单地示例代码，你将看到的效果如下： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48c42c50a20b4dea8ce8b94535e9eee5~tplv-k3u1fbpfcp-zoom-1.image)

如果我们在`.outer` 元素上显式设置 `display: contents` ，该元素本身不会被渲染，但子元素能够正常渲染： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3d36c6ed3e8e4443b99cfb9a27af3838~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/abJvyoj)



在某些布局中，特别是不希望调整 HTML 的结构的时候，我们就可以使用该特性。比如在 Flexbox 或 Grid 中，希望把其他后代元素变成网格项目或 Flex项目，那就可以这样做： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa7843a2532240fb8180b0b80f25aacf~tplv-k3u1fbpfcp-zoom-1.image)

> Demo: [codepen.io/airen/full/…](https://codepen.io/airen/full/zYZvdJb)



`display: contents` 在规范讨论阶段和 `display: subgrid` 的讨论中是非常的激烈，最终是 `display： contents` 获胜了。你现在在Grid的布局中，也没有单独的`display: subgrid` ，而是将`subgrid` 移入到 `grid-template-columns` 和 `grid-template-rows` 中。 

另外还有一个比较大的争执就是 `display: contents` 和 Web 可访问性方面的。有关于这方面的讨论，你要是感兴趣的话，可以阅读： 

- [More accessible markup with display: contents](https://hiddedevries.nl/en/blog/2018-04-21-more-accessible-markup-with-display-contents)
- [Display: Contents Is Not a CSS Reset](https://adrianroselli.com/2018/05/display-contents-is-not-a-css-reset.html)



## CSS @规则

CSS 中的 `@` 规则有很多种，但大家比较熟悉的应该是 `@import` 、`@media` 和 `@supports` 之类的。今天给大家简单的提几个不常见的，比如： 

- 用于嵌套的 `@nest` 和 `@apply`
- 用于注册自定义属性的 `@property`
- 最近讨论比较多的容器查询 `@container`
- [@argyleink](https://twitter.com/argyleink) 在新分享的PPT提到的 `@scope` 和 `@layer`



### CSS 的嵌套

使用过 CSS 处理器的同学，应该用过嵌套来组织自己的代码，比如 SCSS: 

```
// SCSS
foo {
	color: red;
  
  & bar {
  	color: green;
  }
}


```

上面的代码经过编译之后： 

```
// CSS
foo {
	color: red;
}

foo bar {
	color: green;
}


```

庆幸的是，[W3C 也在讨论和定义CSS中的嵌套规则](https://drafts.csswg.org/css-nesting-1/#nest-selector)。目前两种规则： 

```
foo {
	color: red;
  
  @nest bar {
  	color: green;
  }
}

// 或者 
foo {
	color: red;
  
  & bar {
  	color: green;
  }
}

// 都等同于
foo {
	color: red;
}

foo bar {
	color: green;
}


```

也可以和媒体查询 `@media` 相互嵌套： 

```
article {
  color: darkgray;

  & > a {
    color: var(--link-color);
  }
}

code > pre {
  @media (hover) {
    &:hover {
      color: hotpink;
    }
  }
}

code > pre {
  @media (hover) {
    @nest &:hover {
      color: hotpink;
    }
  }
}

article {
  @nest section:focus-within > & {
    color: hotpink;
  }
}

main {
  padding: var(--space-sm);

  @media (width >= 540px) { & {
    padding: var(--space-lg);
  }}
}


```

除了 `@nest` 之外还有 `@apply` 。你可能在一些前端的框架或构建器中看到过 `@apply`： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/11f7654b0e02464a9f45f30411a3a983~tplv-k3u1fbpfcp-zoom-1.image)

如果你在 Chrome Canary 浏览器“实验性属性” 就可以直接体验 `@apply` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20ae10459d9f47d2a59fd4083495baac~tplv-k3u1fbpfcp-zoom-1.image)



简单地说，它有点类似于 SCSS 中的混合宏 `@mixin` 和 `@extend` : 

```
:root {
	--brand-color: red;
  --heading-style: {
  	color: var(--brand-color);
    font-family: cursive;
    font-weight: 700;
  }
}

h1 {
	--brand-color: green;
  @apply --heading-style;
}


```

### CSS Houdini 变量 @property

`@property` 是用来注册一个变量的，该变量是一个 CSS Houdini 中的变量，但它的使用和 CSS 中的自定义属性（CSS变量）是一样的，不同的是注册方式： 

```
// Chrome 78+
// 需要在 JavaScript脚本中注册
CSS.registerProperty({
	'name': '--custom-property-name',
  'syntax': '<color>',
  'initialValue': 'black',
  'inherits': false
})

// Chrome 85+
// 在CSS文件中注册
@property --custom-property-name {
	'syntax': '<color>',
  'initialValue': 'black',
  'inherits': false
}


```

他的最大特色之一就是可以指定已注册的 CSS 变量的类型、初始值，是否可继承： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1099862d48d2485685ca20425904393b~tplv-k3u1fbpfcp-zoom-1.image)

> 上图截取于 [Maxi 在推特上发的推文](https://publish.twitter.com/?query=https%3A%2F%2Ftwitter.com%2Fdiverent2%2Fstatus%2F1278302746668609536&widget=Tweet)。



虽然它的使用方式和 CSS 的自定义属性相似，但它要更强大，特别是在动效方面的使用，能增强 CSS 的动效能力，甚至实现一些以前 CSS 无法实现的动效。比如 

```
@property --hue {
  initial-value: 0;
  inherits: false;
  syntax: '<number>';
}

@keyframes rainbow {
  to {
    --hue: 360;
  }
}


@property --milliseconds {
  syntax: '<integer>';
  initial-value: 0;
  inherits: false;
}
.counter {
  counter-reset: ms var(--milliseconds);
  animation: count 1s steps(100) infinite;
}

@keyframes count { to {
  --milliseconds: 100;
}}



```

把它和 CSS Houdini 的 Paint API 结合起来，可做的事情更多： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5307e326ce384bc586cbb8d473a5e471~tplv-k3u1fbpfcp-zoom-1.image)

[更多这方向的效果可以在 houdini.how 网站上查阅](https://houdini.how/)：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/18e0658ffd0d426bae39634b77372ab4~tplv-k3u1fbpfcp-zoom-1.image)

### 容器查询 @container

[Una Kravets 在 Google I/O 开发大会上就分享了容器查询](https://web.dev/new-responsive/) `@container` ，她把它称为新的响式布局所需特性之一： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff573b4f8d374e409e75d48ee0828063~tplv-k3u1fbpfcp-zoom-1.image)

那么容器查询 `@container` 可以做什么呢？假设你的设计师给你提供了一份像下图这样的设计稿： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b16dc0f84924db096e9a8515e5a3df2~tplv-k3u1fbpfcp-zoom-1.image)

你可能首先会想到的是 `@media` (在没有容器查询之前，似乎也只有这样的方式)，而有了`@container` 之后，就可以换过一种姿势： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/75c6aeb2bae7478a9b08bf915b376e3c~tplv-k3u1fbpfcp-zoom-1.image)

> 这两张图上来自于 [@shadeed9](https://twitter.com/shadeed9) 的 《[CSS Container Queries For Designers](https://ishadeed.com/article/container-queries-for-designers/)》一文，他的另一篇文章《[Say Hello To CSS Container Queries](https://ishadeed.com/article/say-hello-to-css-container-queries/)》也是介绍容器查询的。

看上去非常强大，事实上也很强大，并且它的使用和 `@meida` 非常相似： 

```
// Demo: https://codepen.io/una/pen/mdOgyVL
.product {
  contain: layout inline-size;
}

@container (min-width: 350px) {
  .card-container {
    padding: 0.5rem 0 0;
    display: flex;
  }

  .card-container button {
    /* ... */
  }
}


```

> Demo: [codepen.io/una/pen/mdO…](https://codepen.io/una/pen/mdOgyVL)



对于 `@container` 特性，有叫好的，也有不同的，比如 [Kenton de Jong](https://kentondejong.medium.com/?source=post_page-----f8c2ba77afca--------------------------------) 在他的新博文《[Why I am not a fan of CSS container queries](https://kentondejong.medium.com/why-i-am-not-a-fan-of-css-container-queries-f8c2ba77afca)》阐述了自己不喜欢该t特性： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e858aa8d76b249ff8830ba9bfc3dfaaf~tplv-k3u1fbpfcp-zoom-1.image)

就我个人而言，我是很喜欢这个特性，后面会花一定的时间深入了解和学习 `@container`。当然有讨论是一件好事，这样会让该特性更成熟，[如果你也想参与进来讨论的话，可以点击这里加入](https://github.com/oddbird/css-sandbox/blob/main/src/rwd/query/explainer.md)。 

### @layer 和 @scope

我以前只看到过 `@scope` 规则，[主要是用来处理 CSS 中样式规则作用域相关的](https://css.oddbird.net/scope/)，但并没有深入了解过。[Una Kravets 在 Google I/O 开发大会](https://web.dev/new-responsive/)分享上再次看到了 `@scope` ： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a322edddacbb41338a09ce46e6975d4b~tplv-k3u1fbpfcp-zoom-1.image)

> 上图是 [Miriam Suzanne](https://css.oddbird.net/) 绘制的！



`@scope` 内的样式允许穿透和特定组件的样式，以避免命名冲突，许多框架和插件（如CSS模块）已经使我们能够在框架内做到这一点。现在，这个规范将允许我们为我们的组件编写具有可读性的CSS的本地封装样式，而不需要调整标记。 

```
/* @scope (<root>#) [to (<boundary>#)]? { … } */

@scope (.tabs) to (.panel) {
  :scope { /* targeting the scope root */ }
  .light-theme :scope .tab { /* contextual styles */ }
}


```

怎么看上去和 Web Componed中的 Scope 那么的相似呢？ 

对于 `@layer` ，我第一见： 

```
@layer reset {
  * { box-sizing: border-box; }
  body { margin: 0; }
}
// ...
@layer reset { /* add more later */ }

@import url(headings.css) layer(default);
@import url(links.css) layer(default);

@layer default;
@layer theme;
@layer components;

@import url(theme.css) layer(theme);

@layer default, theme, components;

@import url(theme.css) layer(theme);

@layer framework.theme {
  p {
    color: rebeccapurple;
  }
}

@layer framework {
  @layer theme {
    p { color: cyan; }
  }
}


```

上面代码表示啥意思，我也还没整明白，只知道 `@layer` 被称为层叠层（Cascade Layers）。该特性是 什么[** W3C 层叠和继承规范 Level5**](https://www.w3.org/TR/css-cascade-5/#cascade-layers) 中新提出来的。 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/984274ec7bb64b5e99b5300a62664d9c~tplv-k3u1fbpfcp-zoom-1.image)

## 其他...

在我分享结束没多久，正在整理这篇文章的时候，发现我的偶像 [@argyleink](https://twitter.com/argyleink) 也分享了一个相似的话题《[Hover:CSS! What's New in 2021?](https://2021-hover-conf-new-in-css.netlify.app/#)》，分享了 31 个 CSS 相关的特性，并且按风险级别分为高、中、低三档： 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba5f115b18fd49aaaeef10a8cf1c3cd0~tplv-k3u1fbpfcp-zoom-1.image)

你会发现，和我整理的特性有很多吻合之处。如果你听过他去年在伦敦CSS大会分享的《[**London CSS: What‘s New in 2020?**](https://london-css-2020.netlify.app/)》，你会发现 2021 年的是 2020 年的升级版。 

> 在 2020 年听完 分享之后，我也整理了一份中文版本的《[2020年你不应该错过的 CSS 新特性](https://mp.weixin.qq.com/s/HbfThzav79GFS-sMirAINA)》，并在淘系前端团队 微信公众号发过。

 