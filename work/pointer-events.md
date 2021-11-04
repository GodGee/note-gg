# pointer-events

### 实例一：pointer-events 可以禁用 HTML 元素的 hover/focus/active 等动态效果

 禁用a标签链接或按钮的完美组合是：`pointer-events:none & without href `  

```
<a class="tab_a tab_on" style="pointer-events:none;">年终奖</a>
```

### 案例二：切换开/关按钮状态

点击提交按钮的时候，为了防止用户一直点击按钮，发送请求，当请求未返回结果之前，给按钮增加 pointer-events: none，可以防止这种情况，这种情况在业务中也十分常见。

```
    <!--CSS-->
    .j-pro{ pointer-events: none; }
    <!--HTML-->
    <button r-model={this.submit()} r-class={{"j-pro": flag}}>提交</button>
    <!--JS-->
    submit: function(){
    　　this.data.flag = true;
    　　this.$request(url, {
    　　　     onload: function(json){
    　　　　　　　　if(json.retCode == 200){
    　　　　　　　　　　this.data.flag = false;
    　　　　　　　　} }.bind(this)
    　　　

　　});
}
```

### 案例三：防止透明元素和可点击元素重叠不能点击

一些内容的展示区域，为了实现一些好看的 css 效果，当元素上方有其他元素遮盖，为了不影响下方元素的事件，给被遮盖的元素增加 pointer-events: none; 可以解决。

```
 <!--CSS-->
.layer{ backround: linear-gradient(180deg, #fff, transparent); }
.j-pro{ poninter-events: none; }
<!--HTML-->
<ul>
　　<li class="layer j-pro"></li>
　　<li class="item"></li>
　　<li class="item"></li>
　　<li class="item"></li>
</ul>
```

### 案例四：白色半透明渐变覆盖

![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\807471-20200826171632241-1785313774.png)

如图所示：在某个项目中我做了一个时间选择器，对未选中的时间上方做一个渐变的蒙版遮罩层，而又不能影响时间滑动，实现方法如下

```
.picker__mask {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    width: 100%;
    height: 100%;
    background-image: -webkit-linear-gradient(top,hsla(0,0%,100%,.9),hsla(0,0%,100%,.4)),-webkit-linear-gradient(bottom,hsla(0,0%,100%,.9),hsla(0,0%,100%,.4));
    background-image: linear-gradient(180deg,hsla(0,0%,100%,.9),hsla(0,0%,100%,.4)),linear-gradient(0deg,hsla(0,0%,100%,.9),hsla(0,0%,100%,.4));
    background-repeat: no-repeat;
    background-position: top,bottom;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    pointer-events: none;
}
```

主要就是用pointer-events: none;属性，来穿透当前层，才能在蒙版在上方的基础上，又不影响蒙版下的操作



### 案例五：穿透当前层

首先看到 `pointer-events: auto`，就是我们一般常见的，一个div被另外一个div遮住，就无法进行点击或hover的动作，如下图：

![img](https://img2018.cnblogs.com/blog/1204023/201906/1204023-20190605152659442-2145269390.gif)

 

HTML：

```
<div class="ybox"></div>
<div class="gbox"></div>
```

CSS：

```
.ybox {
  background: rgba(255, 200, 0, .8);
  margin: 20px;
  z-index: 3;
}
.gbox {
  background: rgba(0, 220, 170, .8);
  margin: -80px 40px 20px;
  z-index: 2;
}
.gbox:hover{
  background: rgba(255, 50, 50, .8);
}
```

这时候如果我们把ybox增加 `pointer-events: none;`，就会发现底下的gbox可以hover了！

![img](https://img2018.cnblogs.com/blog/1204023/201906/1204023-20190605153016848-761216779.gif)

 

