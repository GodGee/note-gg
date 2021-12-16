# 标准盒子模型和IE盒子模型

盒子模型是css中一个重要的概念，理解了盒子模型才能更好的排版。其实盒子模型有两种，分别是 **ie 盒子模型**和标准 w3c 盒子模型。他们对盒子模型的解释各不相同，先来看看我们熟知的标准盒子模型：

![标准盒子模型](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\标准和子模型.png)

从上图可以看到标准 ***\**\*W3C 盒子模型的范围包括 margin、border、padding、content，并且 content 部分不包含其他部分。\*\**\***

![IE盒子模型](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\IE盒子模型.png)

　**从上图可以看到 IE 盒子模型的范围也包括 margin、border、padding、content，和标准 W3C 盒子模型不同的是：IE 盒子模型的 content 部分包含了 border 和 pading。**





网页中的盒子模型；我们常常要控制盒子模型的宽度width:  

w3c中的盒子模型的宽:包括margin+border+padding+width;

  width:margin*2+border*2+padding*2+width;

  height:margin*2+border*2+padding*2+height;

iE中的盒子模型的width:也包括margin+border+padding+width;

上面的两个宽度相加的属性是一样的。不过在ie中content的宽度包括padding和border这两个属性；

例如一个盒子模型如下：margin:20px,border:10px,padding:10px;width:200px;height:50px;

如果用w3c盒子模型解释，那么这个盒子模型占用的

 宽度为：20*2+10*2+10*2+200=280px;

 高度：20*2+10*2+20*2+50=130px;

 盒子的实际宽度大小为:10*2+10*2+200=240px;

 实际高度：10*2+10*2+50=90px;

用ie的盒子模型解释 ：盒子在网页中占据的大小为20*2+200=240px; 高：20*2+50=90px;

盒子的实际大小为：宽度:200px, 高度:50px;

我们常常理解的盒子模型是w3c这样的盒子模型