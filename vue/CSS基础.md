**CSS基础**

## CSS构造块

**「1. HTML的局限性」**

- HTML满足不了设计者的需求，可以将网页结构与样式相分离，这样就可以在不更改网页结构的前提下，更换网站的样式。
- 操作html属性不方便
- HTML里面添加样式带来的是无尽的臃肿和繁琐

**「2. CSS网页的美容师」**

- 让我们的网页更加丰富多彩，布局更加灵活自如。
- CSS最大的贡献：让HTML从样式中脱离，实现了HTML专注去做结构呈现，样式交给CSS

**「3. CSS」**CSS(Cascading Style Sheets)通常称为CSS样式表或层叠样式表(级联样式表)。

- **作用**

- - 主要用于设置HTML页面中的文本内容(字体、大小、对齐方式等)\图片的外形(宽高、边框样式、边距等)以及版面的布局和外观显示样式。
  - CSS以HTML为基础，提供了丰富的功能，如字体、样式、背景的控制及整体排版等，而且可以针对不同的浏览器设置不同的样式。

**「4. CSS注释」**

```
/* 这是注释 */
```

### 引入CSS样式表

**「1.行内式(内联样式)」**

通过标签的style属性来设置元素的样式

- style其实就是标签的属性
- 样式属性和值中间是:
- 多组属性值直接用;隔开
- 只能控制当前的标签和以及嵌套在其中的字标签，造成代码冗余。
- **缺点:**没有实现样式和结构相分离。

```
<标签名 style="属性1:属性值1; 属性2:属性值2; 属性3:属性值3;"> 内容 </标签名>
例如：
<div style="color: red; font-size: 12px;">青春不常在，抓紧谈恋爱</div>
```

**「2.内部样式表(内嵌样式表)」**

也称为内嵌式，将CSS代码集中写在HTML文档的head头部标签中，并且用style标签定义。

- style标签一般位于head标签中，当然理论上他可以放在HTML文档的任何地方。
- type="text/css"  在html5中可以省略。
- 只能控制当前的页面
- **缺点:**没有彻底分离结构与样式

```
<head>
<style type="text/CSS">
    选择器（选择的标签） { 
      属性1: 属性值1;
      属性2: 属性值2; 
      属性3: 属性值3;
    }
</style>
</head>
```

**「3.外部样式表(外链式)」**

也称链入式，是将所有的样式放在一个或多个以.css为扩展名的外部样式表文件中，通过link标签将外部样式表文件链接到HTML文档中。

- `rel`:定义当前文档与被链接文档之间的关系，在这里需要指定为“stylesheet”，表示被链接的文档是一个样式表文件。
- `href`:定义所链接外部样式表文件的URL，可以是相对路径，也可以是绝对路径。

```
<link rel="stylesheet" href="index.css">
```

**「4.团队约定-代码风格」**

```
/*1.紧凑格式 (Compact)*/
h3 { color: deeppink;font-size: 20px;}
// 2.一种是展开格式（推荐）
h3 {
 color: deeppink;
    font-size: 20px;    
}

/* 团队约定-代码大小写*/
/* 样式选择器，属性名，属性值关键字全部使用小写字母书写，属性字符串允许使用大小写。*/
/* 推荐 */
h3{
 color: pink;
}
 
/* 不推荐 */
H3{
 COLOR: PINK;
}
```



------

## CSS基础选择器

#### CSS选择器作用

找到指定的HTML页面元素，选择标签。

### CSS基础选择器

**「1. 标签选择器」**

- 标签选择器（元素选择器）是指用HTML标签名称作为选择器，按标签名称分类，为页面中某一类标签指定统一的CSS样式。
- 作用：可以把某一类标签全部选择出来。
- 优点：快速为网页中同类型的标签统一样式
- 缺点：不能设计差异化样式。

```
标签名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; } 
```

**「2. 类选择器」**

- 类选择器使用"."(英文点号)进行标识，后面紧跟类名。
- 语法：类名选择器

```
.类名  {   
    属性1:属性值1; 
    属性2:属性值2; 
    属性3:属性值3;     
}
<p class='类名'></p>
```

- `优点`：可以为元素对象定义单独或相同的样式。可以选择一个或者多个标签。

- `注意`：类选择器使用“.”（英文点号）进行标识，后面紧跟类名(自定义，我们自己命名的)

- - 长名称或词组可以使用中横线来为选择器命名。
  - 不要纯数字、中文等命名， 尽量使用英文字母来表示。
  - 多类名选择器：各个类名中间用空格隔开。

**「3. id选择器」**id选择器使用`#`进行标识，后面紧跟id名

- 元素的id值是唯一的，只能对应于文档中某一个具体的元素。

```
#id名 {属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }
<p id="id名"></p>
```

**「4. 通配符选择器」**

通配符选择器用`*`号表示，`*` 就是选择所有的标签。它是所有选择器中作用范围最广的，能匹配页面中所有的元素。

- `注意`：会匹配页面所有的元素，降低页面响应速度，不建议随便使用

```
* { 属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }
```

例如下面代码，使用通配符选择器定义CSS样式，清除所有HTML标记的默认边距。

```
* {
  margin: 0;                    /* 定义外边距*/
  padding: 0;                   /* 定义内边距*/
}
```

**「5. 基础选择器总结」**

| 选择器       | 作用                          | 缺点                     | 使用情况   | 用法                 |
| :----------- | :---------------------------- | :----------------------- | :--------- | :------------------- |
| 标签选择器   | 可以选出所有相同的标签，比如p | 不能差异化选择           | 较多       | p { color：red;}     |
| 类选择器     | 可以选出1个或者多个标签       | 可以根据需求选择         | 非常多     | .nav { color: red; } |
| id选择器     | 一次只能选择器1个标签         | 只能使用一次             | 不推荐使用 | #nav {color: red;}   |
| 通配符选择器 | 选择所有的标签                | 选择的太多，有部分不需要 | 不推荐使用 | * {color: red;}      |

**「6. 团队约定-选择器」**

1. 尽量少用通配符选择器 `*`。
2. 尽量少用ID选择器
3. 不使用无具体语义定义的标签选择器。

```
/* 推荐 */
.jdc {}
li {}
p{}

/* 不推荐 */
*{}
#jdc {}
div{}   因为div 没有语义，我们尽量少用
```



### CSS复合选择器

复合选择器是由两个或多个基础选择器，通过不同的方式组合而成的

**「1. 后代选择器」**又称为包含选择器

- 用来选择元素或元素组的子孙后代
- 其写法就是把外层标签写在前面，内层标签写在后面，中间用**「空格」**分隔，先写父亲爷爷，再写儿子孙子。
- 子孙后代都可以这么选择。或者说，它能选择任何包含在内 的标签。

```
父级 子级{属性:属性值;属性:属性值;}

.class h3 {color:red;font-size:16px;}
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bc5pD3YIogL5EiblDcricNvp6WyicAjicIpxYbofDTPe7T66Q4Em2ibpQ8Fg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 当标签发生嵌套时，内层标签就成为外层标签的后代。
- 子孙后代都可以这么选择。或者说，它能选择任何包含在内的标签。

**「2. 子元素选择器」**

- 子元素选择器只能选择作为某元素子元素(亲儿子)的元素。
- 其写法就是把父级标签写在前面，子级标签写在后面，中间跟一个 `>` 进行连接
- 这里的子,指的是亲儿子。不包含孙子 重孙子之类。

```
.class>h3 {color:red;font-size:14px;}
```

**「3. 交集选择器」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bmB0Isp6lqsECib7wFZAo47SADSs6OZCgnjwsvJUnPRMBS6s1iamibZKYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 其中第一个为标签选择器，第二个为class选择器，两个选择器之间`不能有空格`，如h3.special。

```
交集选择器是并且的意思,即...又...的意思
比如：   p.one   选择的是： 类名为 .one 的段落标签。 
/*用的相对来说比较少，不建议使用。*/
```

**「4. 并集选择器」**如果某些选择器定义的相同样式，就可以利用并集选择器，可以让代码更简洁。并集选择器（CSS选择器分组）是各个选择器通过`,`连接而成的，通常用于集体声明。

- 任何形式的选择器（包括标签选择器、class类选择器 id选择器等），都可以作为并集选择器的一部分。
- 并集选择器通常用于集体声明 ，逗号隔开的，所有选择器都会执行后面样式，逗号可以理解为和的意思。

```
比如  
.one, 
p , 
#test {color: #F00;}  
表示   .one 和 p  和 #test 这三个选择器都会执行颜色为红色。 
通常用于集体声明。  
```

**「5. 链接伪类选择器」**

用于向某些选择器添加特殊的效果。写的时候，他们的顺序尽量不要颠倒,按照lvha的顺序。否则可能引起错误。

链接伪类，是利用交集选择器.

- `a:link` 未访问的链接
- `a:visited` 已访问的链接
- `a:hover` 鼠标移动到链接上
- `a:active` 选定的链接

##### 实际工作中，很少写全四个状态，一般写法如下：

```
a {   /* a是标签选择器  所有的链接 */
   font-weight: 700;
   font-size: 16px;
   color: gray;
      text-decoration: none; /* 清除链接默认的下划线*/
}
a:hover {   /* :hover 是链接伪类选择器 鼠标经过 */
   color: red; /*  鼠标经过的时候，由原来的 灰色 变成了红色 */
}
```

**「6. 复合选择器总结」**

| 选择器         | 作用                     | 特征                 | 使用情况 | 隔开符号及用法                          |
| :------------- | :----------------------- | :------------------- | :------- | :-------------------------------------- |
| 后代选择器     | 用来选择元素后代         | 是选择所有的子孙后代 | 较多     | 符号是`空格` .nav a                     |
| 子代选择器     | 选择 最近一级元素        | 只选亲儿子           | 较少     | 符号是`>`  .nav>p                       |
| 交集选择器     | 选择两个标签交集的部分   | 既是 又是            | 较少     | `没有符号` p.one                        |
| 并集选择器     | 选择某些相同样式的选择器 | 可以用于集体声明     | 较多     | 符号是`逗号` .nav, .header              |
| 链接伪类选择器 | 给链接更改状态           |                      | 较多     | 重点记住 a{} 和 a:hover  实际开发的写法 |

------

## CSS字体样式

### font字体

**「1. font-size」**

- font-size属性用于设置字号(字体大小)
- `谷歌浏览器`默认的文字大小为16px
- 不同浏览器可能默认显示的字号大小不一致，我们尽量给一个明确值大小，不要默认大小。一般给body指定整个页面文字的大小。

```
p { font-size:20px; }
```

#### 单位

- 相对长度单位、绝对长度单位

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bVSI8GFv6xicpGyAOicfYvoraJGCokvyicHS5zzlsx7tMPhFUpAPaqbasQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**「2. font-family」**

- font-family属性用于设置哪一种字体。

```
p { font-family:"微软雅黑";}
```

- 指定多个字体，如果浏览器不支持第一个字体就会尝试下一个直到找到合适的字体，如果都没有，以电脑默认字体为准。

```
p {font-family: Arial,"Microsoft Yahei", "微软雅黑";}
```

- CSS Unicode字体

- - 在 CSS 中设置字体名称，直接写中文是可以的。但是在文件编码（GB2312、UTF-8 等）不匹配时会产生乱码的错误。
  - xp 系统不支持 类似微软雅黑的中文。
  - 解决方案：英文来替代。比如`font-family:"Microsoft Yahei"`。在 CSS 直接使用 Unicode 编码来写字体名称可以避免这些错误。使用 Unicode 写中文字体名称，浏览器是可以正确的解析的。

```
font-family: "\5FAE\8F6F\96C5\9ED1";   表示设置字体为“微软雅黑”。
```

**「3. font-weight」**

| 属性值  | 描述                                                        |
| :------ | :---------------------------------------------------------- |
| normal  | 默认值（不加粗的）                                          |
| bold    | 定义粗体（加粗的）                                          |
| 100~900 | 400 等同于 normal，而 700 等同于 bold  (数字表示粗细用的多) |

**「4. font-weight」**

font-style属性用于定义字体风格，如设置斜体、倾斜或正常字体，其可用属性值如下：

| 属性   | 作用                                                    |
| :----- | :------------------------------------------------------ |
| normal | 默认值，浏览器会显示标准的字体样式  font-style: normal; |
| italic | 浏览器会显示斜体的字体样式。                            |

**「5. font:综合设置字体样式」**

```
选择器 { font: font-style  font-weight  font-size/line-height  font-family;}
```

- 注意：使用font属性时，必须按上面语法格式中的顺序书写，不能更换顺序，各个属性以`空格`隔开

- - 其中不需要设置的属性可以省略(取默认值),但必须保留`font-size`和`font-family`属性，否则font属性将不起作用。

**「6. font总结」**

| 属性        | 表示     | 注意点                                                       |
| :---------- | :------- | :----------------------------------------------------------- |
| font-size   | 字号     | 我们通常用的单位是px 像素，一定要跟上单位                    |
| font-family | 字体     | 实际工作中按照团队约定来写字体                               |
| font-weight | 字体粗细 | 记住加粗是 700 或者 bold  不加粗 是 normal 或者  400  记住数字不要跟单位 |
| font-style  | 字体样式 | 记住倾斜是 italic   不倾斜 是 normal  工作中我们最常用 normal |
| font        | 字体连写 | 1. 字体连写是有顺序的  不能随意换位置 2. 其中字号 和 字体 必须同时出现 |

### CSS外观属性

**「1. color」**

color属性用于定义文本的颜色
其取值方式有以下3种：

- 实际工作中，用16进制的写法是最多的，且我们更喜欢简写方式比如#f0代表红色。

| 表示表示       | 属性值                        |
| :------------- | :---------------------------- |
| 预定义的颜色值 | red，green，blue，pink        |
| 十六进制       | #FF0000，#FF6600，#29D794     |
| RGB代码        | rgb(255,0,0)或rgb(100%,0%,0%) |

**「2.text-align」**

text-align属性用于设置文本内容的水平对齐方式，相当于html中的align对齐属性。

- 注意：是让盒子里面的文本内容水平居中， 而不是让盒子居中对齐

其可用属性值如下：

| 属性   |       解释       |
| :----- | :--------------: |
| left   | 左对齐（默认值） |
| right  |      右对齐      |
| center |     居中对齐     |

**「3. line-height」**line-height属性用于设置行间距，就是行与行之间的距离，即字符的垂直间距，一般称为行高。

- line-height常用的属性值单位有三种，分别为像素px，相对值em和百分比%，实际工作中使用最多的是像素px

```
一般情况下，行距比字号大7--8像素左右就可以了。
line-height: 24px;
```

#### 行高测量

行高测量方法：![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8by3wLntBYAKUH3BtI30w2gibNAia6Z8urlF5ibiarb5d8XwjsG7aFuF20TA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b03vJ7gdfJaIXRQic9AWbA6fZDzfpHMiaxJLRn5zgFBMF34IbfaicVa8ew/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)`行高测量方法`行高我们利用最多的一个地方是：可以让单行文本在盒子中垂直居中对齐。

> **文字的行高等于盒子的高度。**行高  =  上距离 +  内容高度  + 下距离
> 上距离和下距离总是相等的，因此文字看上去是垂直居中的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bbSzvLR65klr1DmvribcsxLj5nrxV8GettRKl6hethAHmZtO18nEULAw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 行高与高度的三种关系

- 如果 行高 等 高度  文字会 垂直居中
- 如果行高 大于 高度  文字会 偏下
- 如果行高小于高度  文字会  偏上

```
  /*line-height 要设置在font属性下面，否则无效，例如：*/
  height: 80px;
  text-align: center;
  font: normal bold 30px "宋体";
  line-height: 80px;
```

可以使用display:flex;布局方式让文字水平垂直居中

```
  display: flex;
  align-items: center;     /* 侧轴对齐方式*/
  justify-content: center; /* 主轴对齐方式 */
```

**「4. text-indent」**

text-indent属性用于设置首行文本的缩进

- 其属性值可为不同单位的数值、em字符宽度的倍数、或相对于浏览器窗口宽度的百分比%，允许使用负值。
- 建议使用em作为设置单位。
- 1em 就是一个字的宽度。如果是汉字的段落，1em 就是一个汉字的宽度

```
p {
      /*行间距*/
      line-height: 25px;
      /*首行缩进2个字  em  1个em 就是1个字的大小*/
      text-indent: 2em;  
 }
```

**「5. text-decoration」**文本的装饰

text-decoration,通常我们用于给链接修改装饰效果

| 值           | 描述                                                  |
| :----------- | :---------------------------------------------------- |
| none         | 默认。定义标准的文本。取消下划线（最常用）            |
| underline    | 定义文本下的一条线。下划线 也是我们链接自带的（常用） |
| overline     | 定义文本上的一条线。（不用）                          |
| line-through | 定义穿过文本下的一条线。（不常用）                    |

**「6. CSS外观属性总结」**

| 属性            | 表示     | 注意点                                                 |
| :-------------- | :------- | :----------------------------------------------------- |
| color           | 颜色     | 我们通常用  十六进制  比如 而且是简写形式 #fff         |
| line-height     | 行高     | 控制行与行之间的距离                                   |
| text-align      | 水平对齐 | 可以设定文字水平的对齐方式                             |
| text-indent     | 首行缩进 | 通常我们用于段落首行缩进2个字的距离  text-indent: 2em; |
| text-decoration | 文本修饰 | 记住 添加 下划线  underline  取消下划线  none          |



------

## 标签显示模式(display)

`标签显示模式`是标签以什么方式进行显示。HTML标签一般分为块标签和行内标签两种类型，它们也称为块元素和行内元素。

#### 标签显示模式转换 display

- 块转行内：display:inline;
- 行内转块：display:block;
- 块、行内元素转换为行内块：display: inline-block;

**「1. 块级元素(block-level)」**

> 常见的块元素有<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等，其中<div>标签是最典型的块元素。

- ##### 块级元素的特点

- - 独占一行
  - 高度，宽度，外边距以及内边距都可以控制。
  - 宽度默认是容器(父级宽度)的100%
  - 是一个容器及盒子，里面可以放行内或者块级元素
  - **注意**：只有文字才能组成段落，因此p标签里面不能放块级元素，特别是p不能放div。同理，还有h1~h6，dt,它们都是文字类块级标签，里面不能放其他块级元素。

**「2. 行内元素(inline-level)」**

> 有的地方也称为`内联元素`
>
> 常见的行内元素有<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>等，其中<span>标签最典型的行内元素。

- ##### 行内元素的特点

- 1. 相邻行内元素在一行上，一行可以显示多个。
  2. 高度、宽度直接设置是无效的。
  3. 默认高度就是它本身内容的宽度。
  4. 行内元素只能容纳文本或其他行内元素。

###### 注意

- 链接里面不能再放链接
- 特殊情况a里面可以放块级元素，但是给a转换一下块级模式最安全。

**「3. 行内块元素(inline-block)」**

> 在行内元素中有几个特殊的标签——<img>、<input >、<td>，可以对它们设置宽高和对齐属性，有些资料可能会称它们为行内块元素。

- **行内块元素的特点**

- 1. 和相邻行内元素(行内块)在一行上，但是之间会有空白风险。一行可以显示多个
  2. 默认宽度就是它本身内容的宽度。
  3. 高度，行高，外边距以及内边距都可以控制。

#### 三种模式总结

| 元素模式   | 元素排列               | 设置样式               | 默认宽度         | 包含                     |
| :--------- | :--------------------- | :--------------------- | :--------------- | :----------------------- |
| 块级元素   | 一行只能放一个块级元素 | 可以设置宽度高度       | 容器的100%       | 容器级可以包含任何标签   |
| 行内元素   | 一行可以放多个行内元素 | 不可以直接设置宽度高度 | 它本身内容的宽度 | 容纳文本或则其他行内元素 |
| 行内块元素 | 一行放多个行内块元素   | 可以设置宽度和高度     | 它本身内容的宽度 |                          |



------

## CSS背景(background)

**「1. 背景颜色」**

```
background-color: 颜色值;   默认的值是 transparent  透明的
```

**「2. 背景图片(image)」**

```
语法：
background-image : none | url (url) ;
例如:
background-image: url(images/1.png);
```

**「3. 背景平铺（repeat）」**

```
background-repeat : repeat | no-repeat | repeat-x | repeat-y 
```

| 参数      |                 作用                 |
| :-------- | :----------------------------------: |
| repeat    | 背景图像在纵向和横向上平铺（默认的） |
| no-repeat |            背景图像不平铺            |
| repeat-x  |         背景图像在横向上平铺         |
| repeat-y  |          背景图像在纵向平铺          |

**「4. 背景位置(position)」**

```
background-position : length || length
background-position : position || position 
```

| 参数     |                              值                              |
| :------- | :----------------------------------------------------------: |
| length   |         百分数 \| 由浮点数字和单位标识符组成的长度值         |
| position | top \| center \| bottom \| left \| center \| right  方位名词 |

#### 注意：

- 必须先指定background-image属性
- position 后面是x坐标和y坐标。可以使用方位名词或者 精确单位。
- 如果指定两个值，两个值都是方位名字，则两个值前后顺序无关，比如left  top和top  left效果一致
- 如果只指定了一个方位名词，另一个值默认居中对齐。
- 如果position 后面是精确坐标， 那么第一个，肯定是 x 第二个一定是y
- 如果只指定一个数值,那该数值一定是x坐标，另一个默认垂直居中
- 如果指定的两个值是 精确单位和方位名字混合使用，则第一个值是x坐标，第二个值是y坐标

#### 背景简写：

- background：属性的值的书写顺序官方没有强制的标准。为了可读性，建议如下写：
- background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;

```
/* 有背景图片背景颜色可以不用写*/
background: transparent url(image.jpg) repeat-y  scroll center top ;
```

**「5. 背景半透明(CSS3)」**

```
background: rgba(0, 0, 0, 0.3);
background: rgba(0, 0, 0, .3);
```

- 等同于background-color: rgba(0, 0, 0, .3)
- 最后一个参数是alpha 透明度 取值范围 0~1之间
- 我们习惯把0.3 的 0 省略掉 这样写 background: rgba(0, 0, 0, .3);
- 注意：背景半透明是指盒子背景半透明，盒子里面的内容不受影响
- 低于IE 9的版本不支持

##### 盒子半透明 opacity

- 设置opacity元素的所有后代元素会随着一起具有透明性，一般用于调整图片或者模块的整体不透明度

```
opacity: .2;
```

**「6. 背景总结」**

| 属性                  | 作用             | 值                                                           |
| :-------------------- | :--------------- | :----------------------------------------------------------- |
| background-color      | 背景颜色         | 预定义的颜色值/十六进制/RGB代码                              |
| background-image      | 背景图片         | url(图片路径)                                                |
| background-repeat     | 是否平铺         | repeat/no-repeat/repeat-x/repeat-y                           |
| background-position   | 背景位置         | length/position   分别是x  和 y坐标， 切记 如果有 精确数值单位，则必须按照先X 后Y 的写法 |
| background-attachment | 背景固定还是滚动 | scroll/fixed                                                 |
| 背景简写              | 更简单           | 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;  他们没有顺序 |
| 背景透明              | 让盒子半透明     | background: rgba(0,0,0,0.3);  后面必须是 4个值               |



------

## CSS三大特性

**「1. CSS 层叠性」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bMdc6UDuvyOoic6olvqFUm4ug4lWznfZfhN6vVtE9LUtKr5zQk5EgtYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

-`概念`：

- 所谓层叠性是指多种CSS样式的叠加
- 是浏览器处理冲突的一个能力,如果一个属性通过两个相同选择器设置到同一个元素上，那么这个时候一个属性就会将另一个属性层叠掉

-`原则`：

- 样式冲突，遵循的原则是就近原则。 那个样式离着结构近，就执行那个样式。
- 样式不冲突，不会层叠。

**「2. CSS 继承性」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bQ0WCHTI80JbCGUSnPJQvvvFvaUjGLIY7phiacwicpcPiaBunJewuXOdIg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)-`概念`：

- 子标签会继承父标签的某些样式，如文本颜色和字号。
- 想要设置一个可继承的属性，只需将它应用于父元素即可。

-`注意`：

- 恰当地使用继承可以简化代码，降低CSS样式的复杂性。比如有很多子级孩子都需要某个样式，可以给父级指定一个，这些孩子继承过来就好了。
- 子元素可以继承父元素的样式（**text-，font-，line-这些元素开头的可以继承，以及color属性**）

**「3. CSS 优先级(CSS特殊性)」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bYhyc8Ticia3dGdKSsOkKbQRmkpsy6rS4nZmJG2ibvK6yfGcqeBbZYGyibw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)-`概念`：定义CSS样式时，经常出现两个或更多规则应用在同一元素上，此时，

- 选择器相同，则执行层叠性
- 选择器不同，就会出现优先级的问题。

-`权重计算公式`：

| 标签选择器               | 计算权重公式 |
| :----------------------- | :----------- |
| 继承或者 *               | 0,0,0,0      |
| 每个元素（标签选择器）   | 0,0,0,1      |
| 每个类，伪类             | 0,0,1,0      |
| 每个ID                   | 0,1,0,0      |
| 每个行内样式 style=""    | 1,0,0,0      |
| 每个!important  最重要的 | ∞ 无穷大     |

- 值从左到右，左面的最大，一级大于一级，数位之间没有进制，级别之间不可超越。
- 关于CSS权重，我们需要一套计算公式来去计算，这个就是 CSS Specificity（特殊性）
- div { color: pink !important; }

-`权重叠加`：

```
 div ul  li   ------>      0,0,0,3
 .nav ul li   ------>      0,0,1,2
 a:hover      -----—>      0,0,1,1
 .nav a       ------>      0,0,1,1
```

-`继承的权重是0`：

- 我们修改样式，一定要看该标签有没有被选中
- 如果选中了，那么以上面的公式来计权重。谁大听谁的。
- 如果没有选中，那么权重是0，因为继承的权重为0.



------

## 盒子模型

css学习三大重点： css 盒子模型 、 浮动 、 定位  

**网页布局的本质**

- 首先利用CSS设置好盒子的大小，然后摆放盒子的位置。
- 最后把网页元素比如文字图片等等，放入盒子里面。

### 1. 盒子模型(Box Model)

- 盒子模型就是把HTML页面中的布局元素看作是一个矩形的盒子，也就是一个盛装内容的容器。
- 盒子模型由元素的内容、边框（border）、内边距（padding）、和外边距（margin）组成。
- 盒子里面的文字和图片等元素是 内容区域
- 盒子的厚度 我们称为为盒子的边框
- 盒子内容与边框的距离是内边距
- 盒子与盒子之间的距离是外边距

**W3c标准盒子模型**

标准 w3c 盒子模型的范围包括 margin、border、padding、content

当设置为box-sizing: content-box;时，将采用标准模式解析计算，也是默认模式；

```
内盒尺寸计算(元素实际大小)
```

- 宽度：Element Height = content height + padding + border （Height为内容高度）
- 高度：Element  Width = content width + padding + border （Width为内容宽度）
- 盒子的实际大小：**内容的宽度和高度 +  内边距  +  边框**  ![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b3UoX2rImDIa63t2y9NfoobMuKib4I0Dhn0szXElibTw9YsdBDbPsavPA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**IE盒子模型**

IE 盒子模型的 content 部分包含了 border 和 pading

当设置为box-sizing: border-box时，将采用怪异模式解析计算；

### 2. 盒子边框(border)

| 属性         |          作用          |
| :----------- | :--------------------: |
| border-width | 定义边框粗细，单位是px |
| border-style |       边框的样式       |
| border-color |        边框颜色        |

**边框的样式：**

- none：没有边框即忽略所有边框的宽度（默认值）
- solid：边框为单实线(最为常用的)
- dashed：边框为虚线
- dotted：边框为点线

```
边框综合设置
border : border-width || border-style || border-color 

border: 1px solid red;  没有顺序要求  
```

**盒子边框写法总结表：**

很多情况下，我们不需要指定4个边框，我们是可以单独给4个边框分别指定的。

| 上边框                     | 下边框                        | 左边框                      | 右边框                       |
| :------------------------- | :---------------------------- | :-------------------------- | :--------------------------- |
| border-top-style:样式;     | border-bottom-style:样式;     | border-left-style:样式;     | border-right-style:样式;     |
| border-top-width:宽度;     | border- bottom-width:宽度;    | border-left-width:宽度;     | border-right-width:宽度;     |
| border-top-color:颜色;     | border- bottom-color:颜色;    | border-left-color:颜色;     | border-right-color:颜色;     |
| border-top:宽度 样式 颜色; | border-bottom:宽度 样式 颜色; | border-left:宽度 样式 颜色; | border-right:宽度 样式 颜色; |

**表格的细线边框：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bjM8eDXUbtfG62HugJaC84WZTjk08dNWiasE1nvCdX7OgnCoovIaKovg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 通过表格的`cellspacing="0"`,将单元格与单元格之间的距离设置为0，

- 但是两个单元格之间的边框会出现重叠，从而使边框变粗

- 通过css属性：table{ border-collapse:collapse; }  

- - `collapse` 单词是合并的意思,`border-collapse: collapse;`表示相邻边框合并在一起。

```
<style>
 table {
  width: 500px;
  height: 300px;
  border: 1px solid red;
 }
 td {
  border: 1px solid red;
  text-align: center;
 }
 table, td {
  border-collapse: collapse;  /*合并相邻边框*/
 }
</style>
```

### 2. 内边距(padding)

padding属性用于设置内边距。是指边框与内容之间的距离。

**设置**

| 属性           | 作用     |
| :------------- | :------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

**padding简写**

| 值的个数 | 表达意思                                        |
| :------- | :---------------------------------------------- |
| 1个值    | padding：上下左右内边距;                        |
| 2个值    | padding: 上下内边距   左右内边距 ；             |
| 3个值    | padding：上内边距  左右内边距  下内边距；       |
| 4个值    | padding: 上内边距 右内边距 下内边距 左内边距 ； |

当我们给盒子指定padding值之后， 发生了2件事情：

1. 内容和边框 有了距离，添加了内边距。
2. 盒子会变大

**解决措施：**通过给设置了宽高的盒子，减去相应的内边距的值，维持盒子原有的大小。

**padding不影响盒子大小情况：👉**如果没有给一个盒子指定宽度， 此时，如果给这个盒子指定padding， 则不会撑开盒子。

### 3. 外边距（margin）

margin属性用于设置外边距。margin就是控制`盒子和盒子之间的距离`

**设置**

| 属性          | 作用     |
| :------------ | :------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |

margin值的简写 （复合写法）代表意思  跟 padding 完全相同。

**块级盒子水平居中**

- 盒子必须指定宽度（width）
- 然后就给左右的外边距都设置为auto

实际工作中常用这种方式进行网页布局，示例代码如下：

```
.header  { width: 960px; margin: 0 auto;}
```

常见的写法，以下下三种都可以👇👇。

- margin-left: auto;  margin-right: auto;
- margin: auto;
- margin: 0 auto;

**文字居中和盒子居中区别👇👇**

1. 盒子内的文字水平居中是 text-align: center; 而且还可以让 行内元素和行内块居中对齐
2. 块级盒子水平居中  左右margin 改为 auto

**插入图片和背景图片区别👇👇**

1. `插入图片`我们用的最多 比如产品展示类  移动位置只能靠盒模型 padding margin
2. `背景图片`我们一般用于小图标背景或者超大背景图片、背景图片，移动位置只能通过  background-position

**清除元素的默认内外边距👇👇**

- 行内元素为了照顾兼容性,尽量只设置左右内外边距，不要设置上下内外边距。

```
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
```

### 4.外边距合并

使用margin定义块元素的**「垂直外边距」**时，可能会出现外边距的合并。

###### (1). 相邻块元素垂直外边距的合并

- 当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom
- 下面的元素有上外边距margin-top，则他们之间的垂直间距不是margin-bottom与margin-top之和
- **「取两个值中的较大者」**这种现象被称为相邻块元素垂直外边距的合并（也称外边距塌陷）。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b15wO9U7G1QHGZRB5OIsdJaCdPeg7MzVYtTsbhfGPusK7OCOwWtvs4A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**「解决方案：尽量给只给一个盒子添加margin值」**。

#### (2). 嵌套块元素垂直外边距的合并（塌陷）

- 对于两个嵌套关系的块元素，如果父元素没有上内边距及边框
- 父元素的上外边距会与子元素的上外边距发生合并
- 合并后的外边距为两者中的较大者

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8by8DvQuDYlXKtzlIWtltPGPW0HtWAqoaWKr6cy6jYzc4hrVmR6CFlDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**「解决方案：」**

1. 可以为父元素定义上边框。
2. 可以为父元素定义上内边距
3. 可以为父元素添加overflow: hidden。

还有其他方法，比如浮动、固定、绝对定位的盒子不会有问题，后面咱们再总结。。。

#### 盒子模型布局稳定性

优先使用  宽度 （width）  其次 使用内边距（padding）   再次  外边距（margin）

```
width >  padding  >   margin   
```

**原因：**

- margin 会有外边距合并 还有 ie6下面margin 加倍的bug（讨厌）所以最后使用。
- padding  会影响盒子大小， 需要进行加减计算（麻烦） 其次使用。
- width  没有问题（嗨皮）我们经常使用宽度剩余法 高度剩余法来做。

### 5. CSS3 新增

```
圆角边框：
border-radius:length;

border-top-left-radius   定义了左上角的弧度
border-top-right-radius   定义了右上角的弧度
border-bottom-right-radius   定义了右下角的弧度
border-bottom-left-radius   定义了左下角的弧度
```

- 其中每一个值可以为 数值或百分比的形式。
- 技巧：让一个正方形 变成圆圈

```
border-radius: 50%;
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8beaL3cUZXibAoj8ibW8c2cTVhXndJ1ELNMvttZxzRdTD15uqN0G72mM5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)如果要在四个角上一一指定，可以使用以下规则👇👇：

```
border-radius: 左上角 右上角  右下角  左下角;
```

1. 四个值: 第一个值为左上角，第二个值为右上角，第三个值为右下角，第四个值为左下角。
2. 三个值: 第一个值为左上角, 第二个值为右上角和左下角，第三个值为右下角
3. 两个值: 第一个值为左上角与右下角，第二个值为右上角与左下角
4. 一个值：四个圆角值相同

```
盒子阴影(box-shadow)：
box-shadow: offset-x offset-y [blur [spread]] [color] [inset]
```

| 值       | 描述                                           |
| :------- | :--------------------------------------------- |
| offset-x | 阴影的水平偏移量。正数向右偏移，负数向左偏移。 |
| offset-y | 阴影的垂直偏移量。正数向下偏移，负数向上偏移。 |
| blur     | 可选。阴影模糊距离，不能取负数。               |
| spread   | 可选。阴影大小                                 |
| color    | 可选。阴影的颜色                               |
| inset    | 可选。表示添加内阴影，默认为外阴影             |

```
div {
   width: 200px;
   height: 200px;
   border: 10px solid red;
   /* box-shadow: 5px 5px 3px 4px rgba(0, 0, 0, .4);  */
   /* box-shadow:水平位置 垂直位置 模糊距离 阴影尺寸（影子大小） 阴影颜色  内/外阴影； */
   box-shadow: 0 15px 30px  rgba(0, 0, 0, .4);   
}
```



------



## 浮动

### 浮动

**「1. CSS布局的三种机制」**

> 网页布局的核心——就是**用CSS来摆放盒子**。

CSS 提供了3种机制来设置盒子的摆放位置，分别是普通流（标准流）、浮动和定位，其中：

**A. 普通流（标准流）**

- 块级元素会独占一行，从上向下顺序排列；

- - 常用元素：div、hr、p、h1~h6、ul、ol、dl、form、table

- 行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行；

- - 常用元素：span、a、i、em等

**B. 浮动**

- 让盒子从普通流中浮起来,主要作用让多个块级盒子一行显示。

**C. 定位**

- 将盒子定在浏览器的某一个位置——CSS 离不开定位，特别是后面的 js 特效。

**「2. 什么是浮动」**元素的浮动是指设置了浮动属性的元素会

- 脱离标准普通流的控制,不占位置，脱标
- 移动到指定位置。

##### 作用

1. 让多个盒子(div)水平排列成一行，使得浮动称为布局的重要手段。
2. 可以实现盒子的左右对齐等等。
3. 浮动最早是用来控制图片，实现文字环绕图片效果。
4. float属性会改变元素的display属性，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。生成的块级框和我们前面的行内块极其相似。

##### 语法

```
选择器 { float: 属性值; }
```

| 属性值 | 描述                 |
| :----- | :------------------- |
| none   | 元素不浮动（默认值） |
| left   | 元素向左浮动         |
| right  | 元素向右浮动         |



> 浮动只会影响当前的或者是后面的标准流盒子，不会影响前面的标准流。
> **建议:**如果一个盒子里面有多个子盒子，如果其中一个盒子浮动了，其他兄弟也应该浮动。防止引起问题

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bnahuic1SXtFcVOiaDrcDGb8S21rrKf46W1p6ibqmFSqW6mlTzGodxBwyA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**浮动(float)小结**

| 特点 | 说明                                                         |
| :--- | :----------------------------------------------------------- |
| 浮   | 加了浮动的盒子**「是浮起来」**的，漂浮在其他标准流盒子的上面。 |
| 漏   | 加了浮动的盒子**「是不占位置的」**，它原来的位置**「漏给了标准流的盒子」**。 |
| 特   | **「特别注意」**：浮动元素会改变display属性， 类似转换为了行内块，但是元素之间没有空白缝隙 |

### 清除浮动

因为父级盒子很多情况下，不方便给高度，但是子盒子浮动就不占有位置，最后父级盒子高度为0，就影响了下面的标准流盒子。![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bMdS71HhXE4ZoSlJzhd7YhgiczkM9JgE3NeVkgs1V15CzQXdSKMCf81Q/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8baoekQGiaPwbIKRHticXfUA2rEM8grBmN1u6NcEvvb3tY44EQ9SiaZledA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**总结：**

- 由于浮动元素不再占用原文档流的位置，所以它会对后面的元素排版产生影响
- 准确地说，并不是清除浮动，而是清除浮动后造成的影响

**清除浮动本质**清除浮动主要为了解决父级元素因为子级浮动引起内部高度为0 的问题。清除浮动之后， 父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了

#### 清除浮动的方法

```
选择器 { clear: 属性值; }   clear 清除  
```

| 属性值 | 描述                                       |
| :----- | :----------------------------------------- |
| left   | 不允许左侧有浮动元素（清除左侧浮动的影响） |
| right  | 不允许右侧有浮动元素（清除右侧浮动的影响） |
| both   | 同时清除左右两侧浮动的影响                 |

实际工作中,几乎只用clear: both

**1).额外标签法(隔墙法)**

是W3C推荐的做法是通过在浮动元素末尾添加一个空的标签例如 <div style=”clear:both”></div>，或则其他标签br等亦可。

- 优点：通俗易懂，书写方便
- 缺点：添加许多无意义的标签，结构化较差。

**2).父级添加overflow属性方法**

```
可以给父级添加： overflow为 hidden| auto| scroll  都可以实现。
```

- 优点： 代码简洁
- 缺点： 内容增多时候容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素。

**3).使用after伪元素清除浮动**:after 方式为空元素额外标签法的升级版，好处是不用单独加标签了

```
    .clearfix:after {
        content: "";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }
  
    /* IE6、7 专有 */
    .clearfix {
        *zoom: 1;
    }
        
```

- 优点：符合闭合浮动思想  结构语义化正确
- 缺点：由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

**4).使用双伪元素清除浮动**

```
    .clearfix:before,
    .clearfix:after {
        content: "";
        display: table;
    }

    .clearfix:after {
        clear: both;
    }

    .clearfix {
       *zoom: 1;
    }
```

- 优点： 代码更简洁
- 缺点： 由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

#### 清除浮动总结

```
什么时候用清除浮动呢？
```

1. 父级没高度
2. 子盒子浮动了
3. 影响下面布局了，我们就应该清除浮动了。



| 清除浮动的方式       | 优点               | 缺点                               |
| :------------------- | :----------------- | :--------------------------------- |
| 额外标签法（隔墙法） | 通俗易懂，书写方便 | 添加许多无意义的标签，结构化较差。 |
| 父级overflow:hidden; | 书写简单           | 溢出隐藏                           |
| 父级after伪元素      | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |
| 父级双伪元素         | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |

### CSS属性书写顺序

建议遵循以下顺序：

1. 布局定位属性：display / position / float / clear / visibility / overflow（建议 display 第一个写，毕竟关系到模式）
2. 自身属性：width / height / margin / padding / border / background
3. 文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word
4. 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

```
.jdc {
    display: block;
    position: relative;
    float: left;
    width: 100px;
    height: 100px;
    margin: 0 10px;
    padding: 20px 0;
    font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
    color: #333;
    background: rgba(0,0,0,.5);
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    border-radius: 10px;
}
```



------

## 定位(position)

**「1. 定位详解」**

将盒子**「定」**在某一个**「位」**置  自由的漂浮在其他盒子(包括标准流和浮动)的上面。

所以，我们脑海应该有三种布局机制的上下顺序👇👇
标准流在最底层 (海底)  -------   浮动 的盒子 在 中间层  (海面)  -------  定位的盒子 在 最上层  （天空）

**定位**是用来布局的，它有两部分组成：定位 = 定位模式 + 边偏移在 CSS 中，通过 `top`、`bottom`、`left` 和 `right` 属性定义元素的**「边偏移」**：（方位名词）

| 边偏移属性 | 示例           | 描述                                                         |
| :--------- | :------------- | :----------------------------------------------------------- |
| `top`      | `top: 80px`    | **「顶端」**偏移量，定义元素相对于其父元素**「上边线的距离」**。 |
| `bottom`   | `bottom: 80px` | **「底部」**偏移量，定义元素相对于其父元素**「下边线的距离」**。 |
| `left`     | `left: 80px`   | **「左侧」**偏移量，定义元素相对于其父元素**「左边线的距离」**。 |
| `right`    | `right: 80px`  | **「右侧」**偏移量，定义元素相对于其父元素**「右边线的距离」** |

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b1MmIfz1wBVX3PXMGUVrAqRxdibY8EYy1q4xPibScXf5xQpicMYLJe71DQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**「2. 定位模式(position)」**在 CSS 中，通过 `position` 属性定义元素的**「定位模式」**，语法如下：

```
选择器 { position: 属性值; }
```

| 值         |       语义       |
| :--------- | :--------------: |
| `static`   | **「静态」**定位 |
| `relative` | **「相对」**定位 |
| `absolute` | **「绝对」**定位 |
| `fixed`    | **「固定」**定位 |

**「3. 静态定位(static)」**

- 静态定位是元素的默认定位方式，无定位的意思。它相当于border里面的none，不要定位的时候用。
- 静态定位 按照标准流特性摆放位置。它没有边偏移。
- 静态定位在布局时几乎不用

**「4. 相对定位(relative)」**

- 相对定位是元素相对于它原来在标准流中的位置来说的。![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bVfXeeVIdicb4akm3WYFGbwMqbs1ObZiaiauiaFkxSPX2NDmDkEH2BlY1uA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
- 相对于自己原来在标准流中位置来移动的
- 原来在标准流的区域继续占有，后面的盒子仍然以标准流的方式对待它。

**「5. 绝对定位(absolute)」**

绝对定位是元素以带有定位的父级元素来移动位置

- 完全脱表--完全不占位置；
- 父元素没有定位，则以浏览器为准定位(Document文档)。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b3kQIJPlKcMTgGicm7Hia18o1O538GDL2liaZBAnVXyhh1B142qx1zHO9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 父元素有定位

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bqCrQxW1Sb43kqPZ9BUyXoBYljAM6kFSAsC9sPGWIZWo904f6iaib3kPA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 定位口诀--子绝父相

**「6. 固定定位(fixed)」**

固定定位是绝对定位的一种特殊形式;

- 完全脱标--完全不占位置；

- 只认**浏览器的可视窗口**--浏览器可视窗口+边偏移属性来设置元素的位置

- - 跟父元素没有任何关系；单独使用
  - 不随滚动条滚动

### 定位(position)的扩展

#### 绝对定位的盒子居中

> 绝对定位/固定定位的盒子不能通过设置margin: auto设置水平居中 在使用绝对定位时要向实现水平居中，可以按照下面的方法：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bN6SCtF5tG61rscl5xZ0icicUDB8jtFtq5xibEqiba1WfUXOzkiadNjnX4nQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. left : 50%:让盒子的左侧移动到父级元素的水平中心位置；
2. margin-left: -100px;让盒子向左移动自身宽度的一半。
3. 同理垂直居中。

#### 堆叠顺序（z-index）

在使用**「定位」**布局时，可能会**「出现盒子重叠的情况」**。

加了定位的盒子，默认**「后来者居上」**， 后面的盒子会压住前面的盒子。

应用 `z-index` 层叠等级属性可以**「调整盒子的堆叠顺序」**。如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8beCsHfDxy5VBkWU3ET4s4qtBpHm9iajaajxo2GeWWb81a04PfG5rmH0A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)z-index的特性如下:

1. **属性值**：正整数、负整数或 0，默认值是 0，数值越大，盒子越靠上；
2. 如果属性值相同，则按照书写顺序，后来居上；
3. 数字后面不能加单位
4. z-index只能用于相对定位、绝对定位和固定定位的元素，其他标准流、浮动和静态定位无效。

#### 定位改变display属性

前面提过， display 是 显示模式， 可以通过以下方式改变显示模式:

- 可以用inline-block  转换为行内块
- 可以用浮动 float 默认转换为行内块（类似，并不完全一样，因为浮动是脱标的）
- 绝对定位和固定定位也和浮动类似， 默认转换的特性 转换为行内块。

所以说， 一个行内的盒子，如果加了**「浮动」**、**「固定定位」**和**「绝对定位」**，不用转换，就可以给这个盒子直接设置宽度和高度等。

#### 定位小结

| 定位模式         | 是否脱标占有位置     | 移动位置基准           | 模式转换（行内块） | 使用情况                 |
| :--------------- | :------------------- | :--------------------- | :----------------- | :----------------------- |
| 静态static       | 不脱标，正常模式     | 正常模式               | 不能               | 几乎不用                 |
| 相对定位relative | 不脱标，占有位置     | 相对自身位置移动       | 不能               | 基本单独使用             |
| 绝对定位absolute | 完全脱标，不占有位置 | 相对于定位父级移动位置 | 能                 | 要和定位父级元素搭配使用 |
| 固定定位fixed    | 完全脱标，不占有位置 | 相对于浏览器移动位置   | 能                 | 单独使用，不需要父级     |

**注意：**

1. `边偏移` 需要和 `定位模式` 联合使用，`单独使用无效`；
2. `top` 和 `bottom` 不要同时使用；
3. `left` 和 `right` 不要同时使用。



------

## CSS高级技巧

### 元素的显示与隐藏

- **目的:**让一个元素在页面中消失或者显示出来
- **场景:**类似网站广告，当我们点击关闭就不见了，但是我们重新刷新页面，会重新出现！

#### 1.1 display 显示（重点）

display设置或检索对象是否显示或如何显示。

- display: none 隐藏对象

- - 特点：隐藏之后，不再保留位置。

- display: block 除了转换为块级元素之外，同时还有显示元素的意思。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bIsqWiaIk6NLYYVTLwCWLThczx6srresNQxzaqanNPia06CDlCoibib4JiaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)实际开发场景：配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛

#### 1.2 visibility 可见性

设置或检索是否显示对象

```
visibility：visible ;  对象可视

visibility：hidden;    对象隐藏
```

- 特点：隐藏之后，继续保留原有位置。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bWR8zicEo3L5npkKeSd74ibl4D2ZseBNAH4PviaAQmbdq9XRL0ef7ee3aA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 1.3 overflow 溢出

检索或设置当对象的内容超过其指定高度及宽度时如何管理内容。

| 属性值  | 描述                                       |
| :------ | :----------------------------------------- |
| visible | 不剪切内容也不添加滚动条                   |
| hidden  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| scroll  | 不管超出内容否，总是显示滚动条             |
| auto    | 超出自动显示滚动条，不超出不显示滚动条     |

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bEFUTYKtKQ3Dm0dDAGKsSAw5QaSqPLgETx84IuU20677gOjya8BAR7A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)`实际开发场景`：

1. 清除浮动
2. 隐藏超出内容，隐藏掉,  不允许内容超过父盒子。

#### 1.4 显示与隐藏总结

| 属性       | 区别                   | 用途                                                         |
| :--------- | :--------------------- | :----------------------------------------------------------- |
| display    | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
| visibility | 隐藏对象，保留位置     | 使用较少                                                     |
| overflow   | 只是隐藏超出大小的部分 | 1. 可以清除浮动 2. 保证盒子里面的内容不会超出该盒子范围      |

### CSS用户界面样式

所谓的界面样式， 就是更改一些用户操作样式，以便提高更好的用户体验。

- 更改用户的鼠标样式
- 表单轮廓等。
- 防止表单域拖拽

#### 2.1 鼠标样式

设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。

| 属性值      | 描述       |
| :---------- | :--------- |
| default     | 小白  默认 |
| pointer     | 小手       |
| move        | 移动       |
| text        | 文本       |
| not-allowed | 禁止       |

```
<ul>
  <li style="cursor:default">我是小白</li>
  <li style="cursor:pointer">我是小手</li>
  <li style="cursor:move">我是移动</li>
  <li style="cursor:text">我是文本</li>
  <li style="cursor:not-allowed">我是文本</li>
</ul>
```

#### 2.2 轮廓线 outline

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bdicbvJ2vdWvVm9PuHt5WLFn7XqYAx60k0El53qx9JfgLB734At4Ru9w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

```
outline : outline-color ||outline-style || outline-width 
```

但是我们都不关心可以设置多少，我们平时都是去掉的。
最直接的写法是 ： outline: 0;  或者  outline: none;

#### 2.3 防止拖拽文本域resize

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bHX3RwpjHtdppaZKxrEhq5QjZB339Zn5b7xMvy2XsrPblC9V7Z478JQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
<textarea  style="resize: none;"></textarea>
```

#### 2.4 用户界面样式总结

| 属性     | 用途                 | 用途                                                         |
| :------- | :------------------- | :----------------------------------------------------------- |
| 鼠标样式 | 更改鼠标样式cursor   | 样式很多，重点记住 pointer                                   |
| 轮廓线   | 表单默认outline      | outline 轮廓线，我们一般直接去掉，border是边框，我们会经常用 |
| 防止拖拽 | 主要针对文本域resize | 防止用户随意拖拽文本域，造成页面布局混乱，我们resize:none    |

### vertical-align 垂直对齐

- 有宽度的块级元素居中对齐，是margin: 0 auto;
- 让文字居中对齐，是 text-align: center;

vertical-align 垂直对齐，它只针对于**「行内元素」**或者**「行内块元素」**

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bdibQ1oDm9HIxTxiclw7Fq54j5nmKRK04kmDMrPsg6VZiaL6kDcKsBdibSw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
设置或检索对象内容的垂直对其方式。
vertical-align : baseline |top |middle |bottom 
```

注意：

vertical-align 不影响块级元素中的内容对齐，它只针对于**「行内元素」**或者**「行内块元素」**，

特别是行内块元素， 通常用来控制图片/表单与文字的对齐。

#### 3.1 图片、表单和文字对齐

我们可以通过`vertical-align` 控制图片和文字的垂直关系了。默认的图片会和文字基线对齐。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bfWPgKVgBq5JoRLkTdcjQxPG1eUAL8g1N9iaxOUHfD7ZN6otR5Kh4NAw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bX32nStNY16aY9odqroA3Mpia6nia2fuh9DYmYCszG7V1to2VsNwibY8Sg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 3.2 去除图片底侧空白缝隙

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8boGWdjhQUvkicibGiaRYd0KDL2Y1kmqWLl75piaeMlRTMVh4go056Pj1OLA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**原因：**图片或者表单等行内块元素，他的底线会和父级盒子的基线对齐。

就是图片底侧会有一个空白缝隙。

**解决方法：**

- 给img vertical-align:middle | top| bottom等等。 让图片不要和基线对齐。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bNO8sogwyKtqHT6Bg0iaDeAkWqlbbSWqKJIGqtt8As1oFz17wBkQYb4g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 给img 添加 display：block; 转换为块级元素就不会存在问题了。

### 溢出的文字省略号显示

#### 4.1 white-space

- white-space设置或检索对象内文本显示方式。通常我们使用于强制一行显示内容

```
white-space:normal ；默认处理方式

white-space:nowrap ； 强制在同一行内显示所有文本，直到文本结束或者遭遇br标签对象才换行。
```

#### 4.2 text-overflow 文字溢出

- 设置或检索是否使用一个省略标记（...）标示对象内文本的溢出

```
text-overflow : clip ；不显示省略标记（...），而是简单的裁切 

text-overflow：ellipsis ； 当对象内文本溢出时显示省略标记（...）
```

**「注意」**：

一定要首先强制一行内显示，再次和overflow属性  搭配使用

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bJMSFBl0zS6bbLaYbicsptgr6KC0uTOEBhZViaFYNY96FibxUF3Xp3fReQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 4.3 总结三步曲

```
  /*1. 先强制一行内显示文本*/
      white-space: nowrap;
  /*2. 超出的部分隐藏*/
      overflow: hidden;
  /*3. 文字用省略号替代超出的部分*/
      text-overflow: ellipsis;
```

### CSS精灵技术（sprite)

CSS精灵技术（也称CSS Sprites、CSS雪碧）。![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bRib7fONvzIh0zt2skNON8Fvj2NF43KNDc7BwY7vcItRGHiapGiczcg85Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 图所示为网页的请求原理图，当用户访问一个网站时，需要向服务器发送请求，网页上的每张图像都要经过一次请求才能展现给用户。
- 然而，一个网页中往往会应用很多小的背景图像作为修饰，当网页中的图像过多时，服务器就会频繁地接受和发送请求，这将大大降低页面的加载速度。

**为什么需要精灵技术**：为了有效地减少服务器接受和发送请求的次数，提高页面的加载速度。

#### 5.1 精灵技术讲解

CSS 精灵其实是将网页中的一些背景图像整合到一张大图中（精灵图），然而，各个网页元素通常只需要精灵图中不同位置的某个小图，要想精确定位到精灵图中的某个小图。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8blELib9Dme0DKEby545viabCfDcwxcytyxMquZINDK5VjqQibmTFKXobRA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)这样，当用户访问该页面时，只需向服务发送一次请求，网页中的背景图像即可全部展示出来。

我们需要使用CSS的:

- background-image、
- background-repeat
- background-position属性进行背景定位，
- 其中最关键的是使用`background-position` 属性精确地定位。

#### 5.2 精灵技术使用的核心总结

首先我们知道，css精灵技术主要针对于背景图片，插入的图片img 是不需要这个技术的。

1. 精确测量，每个小背景图片的大小和 位置。
2. 给盒子指定小背景图片时， 背景定位基本都是 负值。

### 滑动门

![图片](https://mmbiz.qpic.cn/mmbiz_gif/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bqoVDG5FKf7XNSxwXQhjYYGQgrWSZVfkOcialL3ticPnDy25pCpL4bNCg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

#### 6.1 滑动门出现的背景

制作网页时，为了美观，常常需要为网页元素设置特殊形状的背景，比如微信导航栏，有凸起和凹下去的感觉，最大的问题是里面的字数不一样多，咋办？

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bIKmUvSuib6aFic4Seia4jaorfGmEib2sf2V0eibBmF4r3yichjQzVMBOeibbA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)为了使各种特殊形状的背景能够自适应元素中文本内容的多少，出现了CSS滑动门技术。它从新的角度构建页面，使各种特殊形状的背景能够自由拉伸滑动，以适应元素内部的文本内容，可用性更强。最常见于各种导航栏的滑动门。

#### 6.2 核心技术

核心技术就是利用`CSS精灵`（主要是背景位置）和 `盒子padding`撑开宽度, 以便能适应不同字数的导航栏。

一般的经典布局都是这样的：

```
<li>
  <a href="#">
    <span>导航栏内容</span>
  </a>
</li>
* {
    padding:0;
    margin:0;

   }
    body{
      background: url(images/wx.jpg) repeat-x;
    }
    .father {
      padding-top:20px;
    }
    li {
      padding-left: 16px;
      height: 33px;
      float: left;
      line-height: 33px;
      margin:0  10px;
      background: url(./images/to.png) no-repeat left ;
    }
    a {
      padding-right: 16px;
      height: 33px;
      display: inline-block;
      color:#fff;
      background: url(./images/to.png) no-repeat right ;
      text-decoration: none;
    }
    li:hover,
    li:hover a {
      background-image:url(./images/ao.png);
    }
```

总结：

1. a 设置 背景左侧，padding撑开合适宽度。
2. span 设置背景右侧， padding撑开合适宽度 剩下由文字继续撑开宽度。
3. 之所以a包含span就是因为 整个导航都是可以点击的。

### CSS 三角形

```
div {

    width: 0; 

    height: 0;
    line-height:0；
    font-size: 0;
   border-top: 10px solid red;

   border-right: 10px solid green;

   border-bottom: 10px solid blue;

   border-left: 10px solid #000; 

     }
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bMZvtNQGxxYDG6SvNX35cbQ6TaDfdMYoBxHVLwNHujC7dg4WzBD5TVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. 我们用css 边框可以模拟三角效果
2. 宽度高度为0
3. 我们4个边框都要写， 只保留需要的边框颜色，其余的不能省略，都改为 transparent 透明就好了
4. 为了照顾兼容性 低版本的浏览器，加上 font-size: 0;  line-height: 0;

## H5新增内容

**「1. 什么是HTML5」**

- 定义：**HTML5**定义了**HTML**标准的最新版本，是对**HTML**的第五次重大修改，号称下一代的HTML。

- 两个概念：

- - 是一个新版本的**HTML**语言，定义了新的标签、特性和属性
  - 拥有一个强大的技术集，这些技术集是指：**HTML5、CSS3、JavaScript**,这也是广义上的HTML5。

**「2. HTML5拓展了哪些内容」**

- 语义化标签
- 本地存储
- 兼容特性
- 2D、3D
- 动画、过渡
- CSS3特性
- 性能与集成

**「3. HTML5的现状」**

绝大多数新的属性，都已经被浏览器所支持，最新版本的浏览器已经开始陆续支持最新的特性，总的来说：HTML5已经是大势所趋。

### HTML5新增标签

**「1. 什么是语义化」**

语义化是指用HTML写出符合**内容的结构化**（内容语义化），选择**合适的标签**（代码语义化），能够便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。

**「2. 新增了哪些语义化标签」**

- `header`  ---  头部标签
- `nav`     ---  导航标签
- `article` ---  内容标签
- `section` ---  块级标签
- `aside`   ---  侧边栏标签
- `footer`  ---  尾部标签

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5XFFBGvo602ZcJv2AHoia0rq714Jpn6VZm09kb9Fib2vyO2m7kic0SOfOA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**「3. 新增多媒体音频标签」**

- 多媒体标签有两个，分别是音频 **audio**和视频**video**。

- `audio 标签说明`

- - 可以在不使用标签的情况下，也能够原生的支持音频格式文件的播放，
  - 但是：播放的格式是**有限**的。

- `audio支持的音频格式`

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5hWV9FYrrOVN88n5PmicgNGZvib1KKlBlrtzStcOGhJnlZDWqGj5ggQ4Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- audio 的参数

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic56HnNx77icRXPlGaComyxZpk5UiaWBAgujx3tnGN2aicGa33aVia9w4I8VA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
<audio controls>
    <!-- 注意：在 chrome 浏览器中已经禁用了 autoplay 属性 -->
    <!-- <audio src="./media/snow.mp3" controls autoplay></audio> -->
    <!-- 
    因为不同浏览器支持不同的格式，所以我们采取的方案是这个音频准备多个文件 -->                             
  <source src="myAudio.mp3" type="audio/mpeg">
  <source src="myAudio.ogg" type="audio/ogg">
  <p>Your browser doesn't support HTML5 audio. Here is
     a <a href="myAudio.mp4">link to the audio</a> instead.</p>
</audio>
```

**「4. 新增多媒体视频标签」**

- video视频标签目前支持三种格式

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5QRCCeRWzAAaOkQJhRib7lqA94loGchGN3R1tz4h37g2SRkVHDHErb8A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 语法格式

```
<video src="./media/video.mp4" controls="controls"></video>
```

- video的参数

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5OM9KED9Mlx55vprYUic5LUXIibWSTtGjXRgvbxUCH6lrNaZ8ID2e0eTg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- video代码演示

```
<body>
  <!-- <video src="./media/video.mp4" controls="controls"></video> -->

  <!-- 谷歌浏览器禁用了自动播放功能，如果想自动播放，需要添加 muted 属性 -->
  <video controls="controls" autoplay muted loop poster="./media/pig.jpg">
    <source src="./media/video.mp4" type="video/mp4">
    <source src="./media/video.ogg" type="video/ogg">
  </video>
</body>
```

- 多媒体标签总结

- - 音频标签和视频标签使用基本一致
  - 多媒体标签在不同浏览器下情况不同，存在兼容性问题
  - 谷歌浏览器把音频和视频标签的自动播放都**禁止**了
  - 谷歌浏览器中视频添加**muted**属性就可以自己播放了
  - 注意：重点记住使用方法及自动播放即可，其他属性在使用时查找对应的手册

**「5. 新增input标签」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5R5CVlFOmg1cDsHkmTJu5Hq5OvOTREjcTK96kshjuDa3ic0s1wvibomnw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**「6. 新增表单属性」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5NnvIDpqxdcEiciapIfX1Fia8jd0TwboCqr27nNB4yesa3Wukj1rMRknnA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## CSS3新增

**「1. CSS3属性选择器」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic53TZgoX9QYEFLYhibia3sD5SmyrzUQVVC2PjZIOBy7FDSBv1crX6gE9bQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
  button {
    cursor: pointer;
  }
  button[disabled] {
    cursor: default;
  }
  input[type=search] {
    color: skyblue;
  }

  span[class^=black] {
    color: lightgreen;
  }

  span[class$=black] {
    color: lightsalmon;
  }

  span[class*=black] {
    color: lightseagreen;
  }
```

**「2. 结构伪类选择器」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic56bfm2wTK7VicPpLaLkrqGZotqicFZeenTTe17XBibz0IWtib06kltTGC1Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
ul li:first-child {
  background-color: lightseagreen;
}

ul li:last-child {
  background-color: lightcoral;
}

ul li:nth-child(3) {
  background-color: aqua;
}
```

- **nth-child(n)**参数n详解

- - 注意：本质上就是选中第几个子元素
  - n 可以是数字、关键字、公式
  - n 如果是数字，就是选中第几个
  - 常见的关键字有 `even` 偶数、`odd` 奇数
  - 常见的公式如下(如果 n 是公式，则从 0 开始计算)
  - 但是第 0 个元素或者超出了元素的个数会被忽略

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5WD9GjYUo26W4pOicqanFytOc05c2Qd6XhIcMvJkxUibCia65gibw1buAuQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
<style>
  /* 偶数 */
  ul li:nth-child(even) {
    background-color: aquamarine;
  }

  /* 奇数 */
  ul li:nth-child(odd) {
    background-color: blueviolet;
  }

  /*n 是公式，从 0 开始计算 */
  ul li:nth-child(n) {
    background-color: lightcoral;
  }

  /* 偶数 */
  ul li:nth-child(2n) {
    background-color: lightskyblue;
  }

  /* 奇数 */
  ul li:nth-child(2n + 1) {
    background-color: lightsalmon;
  }

  /* 选择第 0 5 10 15, 应该怎么选 */
  ul li:nth-child(5n) {
    background-color: orangered;
  }

  /* n + 5 就是从第5个开始往后选择 */
  ul li:nth-child(n + 5) {
    background-color: peru;
  }

  /* -n + 5 前五个 */
  ul li:nth-child(-n + 5) {
    background-color: tan;
  }
</style>
```

- `nth-child与nth-of-type区别`

- - `nth-child` 选择父元素里面的第几个子元素，不管是第几个类型
  - `nth-of-type` 选择指定类型的元素

```
<style>
  div :nth-child(1) {
    background-color: lightblue;
  }

  div :nth-child(2) {
    background-color: lightpink;
  }

  div span:nth-of-type(2) {
    background-color: lightseagreen;
  }

  div span:nth-of-type(3) {
    background-color: #fff;
  }
</style>
```

**「3. 伪元素选择器」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic56TniaRkAzvNSaibeuEUW6c9UTNBN65guMhWwa9xKCGlaNh21a6AiaIH9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 伪元素选择器注意事项

- - `before` 和 `after` 必须有 `content` 属性
  - `before` 在内容前面，after 在内容后面
  - `before` 和 `after` 创建的是一个元素，但是属于行内元素
  - 创建出来的元素在 `Dom` 中查找不到，所以称为伪元素
  - 伪元素和标签选择器一样，权重为 1

```
<style>
    div {
      width: 100px;
      height: 100px;
      border: 1px solid lightcoral;
    }

    div::after,
    div::before {
      width: 20px;
      height: 50px;
      text-align: center;
      display: inline-block;
    }
    div::after {
      content: '德';
      background-color: lightskyblue;
    }

    div::before {
      content: '道';
      background-color: mediumaquamarine;
    }
  </style>
```

##### 伪元素字体图标

```
p {
   position: relative;
   width: 220px;
   height: 22px;
   border: 1px solid lightseagreen;
   margin: 60px;

}
p::after {
  content: '\ea50';
  font-family: 'icomoon';
  position: absolute;
  top: -1px;
  right: 10px;
}
```

**「4. 2D 转换之translate」**

- **2D转换**

- - **2D**转换是改变标签在二维平面上的位置和形状
  - 移动：**translate**
  - 旋转：**rotate**
  - 缩放：**scale**

- translate语法

- - x就是X轴上水平移动
  - y就是y轴上水平移动

```
  transform: translate(x, y)
  transform: translateX(n)
  transfrom: translateY(n)  
```

- 重点知识点

- - 2D的移动主要是指水平、垂直方向上的移动
  - translate最大的优点就是**不影响**其他元素的位置
  - translate中的100%单位，是相对于**本身**的宽度和高度来进行计算的
  - 行内标签没有效果

```
div {
  background-color: lightseagreen;
  width: 200px;
  height: 100px;
  /* 平移 */
  /* 水平垂直移动 100px */
  /* transform: translate(100px, 100px); */

  /* 水平移动 100px */
  /* transform: translate(100px, 0) */

  /* 垂直移动 100px */
  /* transform: translate(0, 100px) */

  /* 水平移动 100px */
  /* transform: translateX(100px); */

  /* 垂直移动 100px */
  transform: translateY(100px);
  /*百分比用法*/
  transform: translateY(100%);   
}
```

##### 让一个盒子水平垂直居中

```
div {
    position: relative;
    width: 500px;
    height: 500px;
    background-color: pink;
    /* 1. 我们tranlate里面的参数是可以用 % */
    /* 2. 如果里面的参数是 % 移动的距离是 盒子自身的宽度或者高度来对比的 */
    /* 这里的 50% 就是 50px 因为盒子的宽度是 100px */
    /* transform: translateX(50%); */
    }
        
p {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 200px;
    height: 200px;
    background-color: purple;
    /1.* margin-top: -100px;
    margin-left: -100px; */
  
    /2.* translate(-50%, -50%)  
      盒子往上走自己高度的一半   */
    transform: translate(-50%, -50%);
  }
        
span {
    /* translate 对于行内元素是无效的 */
    transform: translate(300px, 300px);
     }
```

**「5. 2D 转换之rotate」**

- **rotate**旋转

- - 2D旋转指的是让元素在二维平面内顺时针或者逆时针旋转

```
/* 单位是：deg */
img:hover {
  transform: rotate(360deg)
}
```

- rotate语法

- - `rotate` 里面跟度数，单位是 `deg`
  - 角度为**正**时，顺时针，角度为负时，逆时针
  - 默认旋转的中心点是元素的中心点

- **设置元素旋转的中心的(transform-origin)**

```
  transform-origin: x y;
```

- `注意`

- - 后面的参数 x 和 y 用空格隔开
  - x y 默认旋转的中心点是元素的中心(50% 50%),等价于**center center**
  - 还可以给x y 设置像素或者方位名词(top、bottom、left、right、center)

**「6. 2D 转换之scale」**

- **scale**的作用：用来控制元素的放大与缩小

```
transform: scale(x, y)
```

- `知识要点：`

- - 注意，x与y之间用逗号进行分隔
  - `transform: scale(1, 1)`: 宽高都放大一倍，相当于没有放大
  - `transform: scale(2, 2)`: 宽和高都放大了二倍
  - `transform: scale(2)`: 如果只写了一个参数，第二个参数就和第一个参数一致
  - `transform:scale(0.5, 0.5)`: 缩小
  - `scale` 最大的优势：可以设置转换中心点缩放，默认以中心点缩放，而且不影响其他盒子

```
   div:hover {
    /* 注意，数字是倍数的含义，所以不需要加单位 */
    /* transform: scale(2, 2) */
   
    /* 实现等比缩放，同时修改宽与高 */
    /* transform: scale(2) */
   
    /* 小于 1 就等于缩放*/
    transform: scale(0.5, 0.5)
   }
```

**「7. 2D 转换综合写法以及顺序问题」**

##### 知识要点

- 同时使用多个转换，其格式为 `transform: translate() rotate() scale()`
- 顺序会影响到转换的效果(先旋转会改变坐标轴方向)
- 当我们同时有位置或者其他属性的时候，要将位移放到最前面

```
div:hover {
  transform: translate(200px, 0) rotate(360deg) scale(1.2)
}
```

### 动画(animation)

**「动画」**是CSS3中最具颠覆性的特征之一，可通过设置多个节点来精确的控制一个或者一组动画，从而实现复杂的动画效果。

**「动画的使用」**

1. 先**定义**动画
2. 再**调用**定义好的动画

```
/*1. 定义动画*/
@keyframes 动画名称 {
    0% {
        width: 100px;
    }
    100% {
        width: 200px
    }
}
div {
 /* 调用动画 */
  animation-name: 动画名称;
  /* 持续时间 */
  animation-duration: 持续时间；
}
```

**「动画序列」**

- 0% 是动画的开始，100 % 是动画的完成，这样的规则就是**动画序列**
- 在 **@keyframs**中规定某项 CSS 样式，就由创建当前样式**逐渐**改为新样式的动画效果
- 动画是使元素从一个样式逐渐变化为另一个样式的效果，可以改变任意多的样式任意多的次数
- 用百分比来规定变化发生的时间，或用 `from` 和 `to`，等同于 0% 和 100%

```
<style>
    div {
      width: 100px;
      height: 100px;
      background-color: aquamarine;
      animation-name: move;
      animation-duration: 0.5s;
    }

    @keyframes move{
      0% {
        transform: translate(0px)
      }
      100% {
        transform: translate(500px, 0)
      }
    }
  </style>
```

**「动画常见属性」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic53XukGdp1jVdFz3gszMHHCcx382Rz7GiamHYDR4fVHia6RS16yIhvQhKg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
div {
  width: 100px;
  height: 100px;
  background-color: aquamarine;
  /* 动画名称 */
  animation-name: move;
  /* 动画花费时长 */
  animation-duration: 2s;
  /* 动画速度曲线 */
  animation-timing-function: ease-in-out;
  /* 动画等待多长时间执行 */
  animation-delay: 2s;
  /* 规定动画播放次数 infinite: 无限循环 */
  animation-iteration-count: infinite;
  /* 是否逆行播放 */
  animation-direction: alternate;
  /* 动画结束之后的状态 */
  animation-fill-mode: forwards;
}

div:hover {
  /* 规定动画是否暂停或者播放 */
  animation-play-state: paused;
}
```

**「动画简写方式」**

```
/* animation: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 起始与结束状态 */
animation: name duration timing-function delay iteration-count direction fill-mode
```

**知识要点**

- 简写属性里面不包含 `animation-paly-state`
- 暂停动画 `animation-paly-state: paused`; 经常和鼠标经过等其他配合使用
- 要想动画走回来，而不是直接调回来：`animation-direction: alternate`
- 盒子动画结束后，停在结束位置：`animation-fill-mode: forwards`

```
animation: move 2s linear 1s infinite alternate forwards;
```

**「速度曲线细节」**

`animation-timing-function`: 规定动画的速度曲线，默认是**ease**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5ofA0S4z12Uhc1pkAoLSrclZTokhosNiaKIDsic6b7S8uv0n8rcFn2ODQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
/*打字机效果*/
div {
  width: 0px;
  height: 50px;
  line-height: 50px;
  white-space: nowrap;
  overflow: hidden;
  background-color: aquamarine;
  animation: move 4s steps(24) forwards;
}

@keyframes move {
  0% {
    width: 0px;
  }

  100% {
    width: 480px;
  }
}
```



### CSS 过渡transition

通过过渡**transition**，可以让web前端开发人员不需要javascript就可以实现简单的动画交互效果。

> `深入理解CSS过渡transition`
> https://www.cnblogs.com/xiaohuochai/p/5347930.html

**「定义」**过渡transition是一个复合属性，包括**transition-property**、**transition-duration**、**transition-timing-function**、**transition-delay**这四个子属性。通过这四个子属性的配合来完成一个完整的过渡效果。

```
transition-property: 过渡属性(默认值为all)
transition-duration: 过渡持续时间(默认值为0s)
transiton-timing-function: 过渡函数(默认值为ease函数)
transition-delay: 过渡延迟时间(默认值为0s)
.test{
    height: 100px;
    width: 100px;
    background-color: pink;
    transition-duration: 3s;
/*     以下三值为默认值，稍后会详细介绍 */
    transition-property: all;
    transition-timing-function: ease;
    transition-delay: 0s;
}    
.test:hover{
    width: 500px;
}
~~~html
<div class="test"></div>
```

![图片](https://mmbiz.qpic.cn/mmbiz_gif/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic57tTB4aokkeXTqmgr3vBWdsdeicgA0nXzBmpJKIGK9ZCibdkE5pcwKhicg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

**「复合属性」**过渡transition的这四个子属性只有<transition-duration>是必需且不能为0。其中，<transition-duration>和<transition-delay>都是时间。当两个时间同时出现时，第一个是<transition-duration>，第二个是<transition-delay>；当只有一个时间时，它是<transition-duration>，而<transition-delay>为默认值0s

- **注意:**

- - transition的这四个子属性之间不能用逗号隔开，只能用空格隔开。因为逗号隔开的代表不同的属性(transition属性支持多值，多值部分稍后介绍)；而空格隔开的代表不同属性的四个关于过渡的子属性。

```
.test{
    height: 100px;
    width: 100px;
    background-color: pink;
/*代表持续时间为2s，延迟时间为默认值0s*/
    transition；2s;
}    
.test:hover{
    width: 500px;
}
<div class="test"></div>
```

![图片](https://mmbiz.qpic.cn/mmbiz_gif/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic57tTB4aokkeXTqmgr3vBWdsdeicgA0nXzBmpJKIGK9ZCibdkE5pcwKhicg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)`延迟时间delay 案例`

```
.test{
    height: 100px;
    width: 100px;
    background-color: pink;
    /*代表持续时间为1s，延迟时间为2s*/
    transition: 1s 2s;
}    
.test:hover{
    width: 500px;
}
<div class="test"></div>
```

![图片](https://mmbiz.qpic.cn/mmbiz_gif/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic519JP8gHBUXF94cpJM9PlRJ3UX4Z2Clsu8aQMGqHEc7z4vQZpicdSLWg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

**「过渡属性」**

- 值: none | all | <transition-property>[,<transition-property>]
- 初始值: all
- 应用于: 所有元素
- 继承性: 无

```
  none: 没有指定任何样式
  all: 默认值，表示指定元素所有支持transition-property属性的样式
  <transition-property>: 可过渡的样式，可用逗号分开写多个样式
```

![图片](https://mmbiz.qpic.cn/mmbiz_gif/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5AT1v9ypgZlXsBicDsic0pfffJ6MC23n6d7YURD5tuJYTXgS0JYGtJfGw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

**「过渡持续时间」**

- 值: <time>[,<time>]*
- 初始值: 0s
- 应用于: 所有元素
- 继承性: 无
- [注意]该属性不能为负值
- [注意]若该属性为0s则为默认值，若为0则为无效值。所以必须**带单位**
- [注意]该值为单值时，即所有过渡属性都对应同样时间；该值为多值时，过渡属性按照顺序对应持续时间

```
/*DEMO中的过渡属性值*/
transition-property: width,background;
```

![图片](https://mmbiz.qpic.cn/mmbiz_gif/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5PEiaR3W5X9iaI0kHiaic4oynPHhxLJ4MhXdibmdv9K4oxDMNPvZicfxr9Gag/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)**「过渡时间函数」**

**过渡时间函数**用于定义元素过渡属性随时间变化的过渡速度变化效果

- 值: <timing-function>[,<timing-function>]*
- 初始值: **ease**
- 应用于: 所有元素
- 继承性: 无

**「取值」** 过渡时间函数共三种取值，分别是**关键字**、**steps函数**和**bezier函数**

**「关键字」**其实是bezier函数或steps函数的特殊值

```
ease: 开始和结束慢，中间快。
linear: 匀速。
ease-in: 开始慢。
ease-out: 结束慢。
ease-in-out: 和ease类似，但比ease幅度大。
```

------

## 3D转换

### 认识3D转换

**「3D的特点」**近大远小，物体和面遮挡不可见

**「三维坐标系」**

- x 轴：水平向右  -- `注意：x 轴右边是正值，左边是负值`
- y 轴：垂直向下  -- `注意：y 轴下面是正值，上面是负值`
- z 轴：垂直屏幕  --  `注意：往外边的是正值，往里面的是负值`

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5h6SJicHWE6Y7CRGT7MliaOWoVhpOB8GDaegxNqyEdgLx98pNG3dmIYxw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 3D转换

**1. 3D 转换知识要点**

- `3D` 位移：`translate3d(x, y, z)`
- `3D` 旋转：`rotate3d(x, y, z)`
- `透视` ：`perspctive`
- `3D`呈现 `transfrom-style`

**2. 3D 移动translate3d**

- `3D` 移动就是在 `2D` 移动的基础上多加了一个可以移动的方向，就是 z 轴方向
- `transform: translateX(100px)`：仅仅是在 x 轴上移动
- `transform: translateY(100px)`：仅仅是在 y 轴上移动
- `transform: translateZ(100px)`：仅仅是在 z 轴上移动
- `transform: translate3d(x, y, z)`：其中x、y、z 分别指要移动的轴的方向的距离
- `注意：x, y, z 对应的值不能省略，不需要填写用 0 进行填充`

```
  transform: translate3d(100px, 100px, 100px)
  /* 注意：x, y, z 对应的值不能省略，不需要填写用 0 进行填充 */
  transform: translate3d(100px, 100px, 0)
```

### 透视perspective

- 知识点讲解

- - 如果想要网页产生 `3D` 效果需要透视(理解成 `3D` 物体投影的 `2D` 平面上)
  - 实际上模仿人类的视觉位置，可视为安排一只眼睛去看
  - 透视也称为视距，所谓的视距就是人的眼睛到屏幕的距离
  - 距离视觉点越近的在电脑平面成像越大，越远成像越小
  - 透视的单位是像素

- 知识要点

- - **透视需要写在被视察元素的父盒子上面**
  - 注意下方图片
  - d：就是视距，视距就是指人的眼睛到屏幕的距离
  - z：就是 z 轴，z 轴越大(正值)，我们看到的物体就越大

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5Sfoj6AYfic4Uh2zXZ0QQKvGPzfO5wbibojvleQ2uuIexRG4U0qjldFTg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)代码演示

```
body {
  /*透视需要写在被视察元素的父盒子上面 */
  perspective: 1000px;
}
translateZ与perspective的区别
```

- `perspecitve` 给父级进行设置视距的，`translateZ` 给 子元素进行设置不同的大小

### 3D 旋转rotateX

**3D 旋转**指可以让元素在三维平面内沿着 x 轴、y 轴、z 轴 或者自定义轴进行旋转

- `语法：`

- - **transform: rotateX(45deg)** -- 沿着 x 轴正方向旋转 45 度
  - **transform: rotateY(45deg)** -- 沿着 y 轴正方向旋转 45 度
  - **transform: rotateZ(45deg)** -- 沿着 z 轴正方向旋转 45 度
  - **transform: rotate3d(x, y, z, 45deg)** -- 沿着自定义轴旋转 45 deg 为角度

- `左手法则：`

- - 左手的手拇指指向 x 轴的正方向
  - 其余手指的弯曲方向就是该元素沿着 x 轴旋转的方向

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5ScYaw7OGbdUEIRhFroCViavMb08LXozqSyicIvytvt9edpAgmZk6tsdA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
div {
  /*透视写在被视察元素的父盒子上面 */
  perspective: 300px;
}
/*被观察元素*/
img {
  display: block;
  margin: 100px auto;
  transition: all 1s;
}

img:hover {
  transform: rotateX(-45deg)
}
```

### 3D 旋转rotateY

- `左手法则：`

- - 左手的拇指指向 y 轴的正方向
  - 其余的手指弯曲方向就是该元素沿着 y 轴旋转的方向(正值)

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmpOOT8MBs9Ge1VUwKs31Ic5XfJ17BIrFbwuH2997vpCFDRkibjAL96mpRW47cY9SzuSVbBiasma9u9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
div {
  perspective: 500px;
}

img {
  display: block;
  margin: 100px auto;
  transition: all 1s;
}

img:hover {
  transform: rotateY(180deg)
}
```

### 3D 旋转rotateZ

```
div {
  perspective: 500px;
}

img {
  display: block;
  margin: 100px auto;
  transition: all 1s;
}

img:hover {
  transform: rotateZ(180deg)
}
```

**「rotate3d」**

- **transform: rotate3d(x, y, z, deg)** -- 沿着自定义轴旋转 deg 为角度

- x, y, z 表示旋转轴的矢量，是标识你是否希望沿着该轴进行旋转，最后一个标识旋转的角度

- - **transform: rotate3d(1, 1, 0, 180deg)** -- 沿着对角线旋转 45deg
  - **transform: rotate3d(1, 0, 0, 180deg)** -- 沿着 x 轴旋转 45deg

```
div {
  perspective: 500px;
}

img {
  display: block;
  margin: 100px auto;
  transition: all 1s;
}

img:hover {
  transform: rotate3d(1, 1, 0, 180deg)
}
```

### 3D呈现transform-style

- 控制子元素是否开启三维立体环境
- `transform-style: flat` 代表子元素不开启 `3D` 立体空间，默认的
- `transform-style: preserve-3d` 子元素开启立体空间
- 代码写给父级，但是影响的是子盒子

```
<body>
    <div class="box">
        <div></div>
        <div></div>
    </div>
</body>
<style>
    body {
        perspective: 500px;
        }
        
    .box {
        position: relative;
        width: 200px;
        height: 200px;
        margin: 100px auto;
        transition: all 2s;
        /* 让子元素保持3d立体空间环境 */
        transform-style: preserve-3d;
        }
        
    .box:hover {
        transform: rotateY(60deg);
    }
        
    .box div {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: pink;
    }
        
    .box div:last-child {
        background-color: purple;
        transform: rotateX(60deg);
    }
</style>
```