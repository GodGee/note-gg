# CSS关系选择器

### 1.包含选择器（E F）

选择所有被E元素包含的F元素，中间用空格隔开

```
ul li{color:green;}

<ul>
  <li>宝马</li>
  <li>奔驰</li>
</ul>
<ol>
  <li>奥迪</li>
</ol>
```

![这里写图片描述](https://img-blog.csdn.net/20180123204813859?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VubWluZzcwOTQyNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 2.子选择器（E>F）

选择所有作为E元素的直接子元素F，对更深一层的元素不起作用，用>表示

```
div>a{color:red}

<div>
    <a href="#">子元素1</a>
    <p>
        <a href="#">孙元素</a>
    </p>
    <a href="#">子元素2</a>
</div>
```

![这里写图片描述](https://img-blog.csdn.net/20180124100952198?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VubWluZzcwOTQyNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 3.相邻选择器（E+F）

选择紧跟E元素后的F元素，用加好表示，选择相邻的第一个兄弟元素。

```
 h1+p{color:red;}

<h1>h1元素</h1>
<p>第一个元素</p>
<p>第二个元素</p>
```

![相邻选择器](https://img-blog.csdn.net/20180124102920611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VubWluZzcwOTQyNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 4.兄弟选择器（E~F）

选择E元素之后的所有兄弟元素F，作用于多个元素，用~隔开

```
 h1~p{color:red;}

<h1>h1元素</h1>
<p>第一个元素</p>
<p>第二个元素</p>
```

![这里写图片描述](https://img-blog.csdn.net/20180124103325687?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VubWluZzcwOTQyNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)