# JavaScript引擎运行原理解析

#### 什么是JavaScript解析引擎？

简单地说，JavaScript解析引擎就是能够“读懂”JavaScript代码，并准确地给出代码运行结果的一段程序。比方说，当你写了 var a = 1 + 1; 这样一段代码，JavaScript引擎做的事情就是看懂（解析）你这段代码，并且将a的值变为2。

JavaScript 引擎”通常被称作一种虚拟机。

JavaScript 虚拟机是一种进程虚拟机，专门设计来解释和执行的 JavaScript代码。

JavaScript 引擎的基本工作是把开发人员写的 JavaScript代码转换成高效、优化的代码，这样就可以通过浏览器进行解释甚至嵌入到应用中。每个JavaScript引擎都实现了一个版本的ECMAScript，JavaScript是它的一个分支。随着ECMAScript的不断发展，JavaScript引擎也不断改进。之所以有这么多不同的引擎，是因为它们每个都被设计运行在不同的web浏览器、headless浏览器、或者像Node.js那样的运行时环境中，它的唯一的目的就是读取和编译JavaScript代码。

常见的解析器：

![在这里插入图片描述](https://img-blog.csdn.net/20180930100125126?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E0MTk0MTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1.解析引擎就是根据ECMAScript定义的语言标准来动态执行JavaScript字符串;
2.解析JS的过程分成两个阶段：**语法检查阶段和运行阶段**;
3.语法检查包括**词法分析和语法分析**，运行阶段又包括**预解析和运行阶段**（V8引擎会将JavaScript字符串编译成二进制代码，此过程应该归到语法检查过程中）;
4.在JavaScript解析过程中，如遇错误就直接跳出当前代码块，直接执行下一个script代码段。所以在同一个script内的代码段有错误的话就不会执行下去，但是不会影响下一个script内的代码段。

#### 第一阶段：语法检查

语法检查也是JavaScript解析器的工作之一，包括词法分析 和 语法分析，过程大致如下：

##### 一：词法分析

JavaScript解释器先把JavaScript代码（字符串）的字符流按照ECMAScript标准转换为记号流

a = (b - c);

```
NAME "a"
EQUALS
OPEN_PARENTHESIS
 NAME "b"
MINUS 
NAME "c"
CLOSE_PARENTHESIS
SEMICOLON

```

##### 二：语法分析

JavaScript语法分析器在经过词法分析后，将记号流按照ECMAScript标准把词法分析所产生的记号**生成语法树**。通俗地说就是把从程序中收集的信息存储到数据结构中，每取一个词法记号，就送入语法分析器进行分析。

语法分析不做的事：去掉注释，自动生成文档，提供错误位置（可以通过记录行号来提供）。
ECMAScript标准如下：

```
var，if，else，break，continue等是JavaScript的关键词
怎么样算是数字、怎么样算是字符串等等
定义了操作符（+，-，=）等操作符
定义了JavaScript的语法
定义了对表达式，语句等标准的处理算法，比如遇到==该如何处理

```


当语法检查正确无误之后，就可以进入运行阶段了。

#### 第二阶段：运行阶段

##### 一：预解析

第一步：创建执行上下文。解析器将语法检查正确后生成的语法树复制到当前执行上下文中。
第二步：属性填充。解析器会对语法树当中的变量声明、函数声明以及函数的形参进行属性填充。
预解析阶段创建的执行上下文包括：变量对象、作用域链、this
变量对象（Variable Object）：由vardeclaration、function declaration（变量声明、函数声明）、arguments（参数）构成。变量对象是以单例形式存在。
作用域链（Scope Chain）：variableobject + all parent scopes（变量对象以及所有父级作用域）构成。
this值：（thisValue）：contentobject。this值在进入上下文阶段就确定了。一旦进入执行代码阶段，this值就不会变了。

栗子:

```js
<script>
    console.log(a);
    var a = 1;
    function a(){console.log(2);}
    console.log(a);
    var a = 3;
    console.log(a);
    function a(){console.log(4);}
    console.log(a)
</script>

```

第二行：会弹出functiona(){console.log(4);} ，因为预解析完成之后，被存进内存的a的值就是functiona(){console.log(4);}
第三行：第三行里有表达式，a被赋了一个新的值1表达式会改变变量的值。表达式可以改变预解析的值。
第四行：只是函数的声明，并没有用到表达式，而且也没有函数的调用，所以不会改变a的值。
第五行：因为a的值没有变化，所以还是1
第六行：使用了表达式，a被赋了一个新的值3
第七行：会弹出3
第八行：函数的声明，不会改变a的值。
第九行：a的值没有改变，所以还是3

### 为什么JavaScript是单线程？

JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。那么，为什么JavaScript不能有多个线程呢？这样能提高效率啊。

JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

### 任务队列

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。

如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。

JavaScript语言的设计者意识到，这时主线程完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。

于是，所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

具体来说，异步执行的运行机制如下。（同步执行也是如此，因为它可以被视为没有异步任务的异步执行。）

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。

 

![在这里插入图片描述](https://img-blog.csdn.net/20180930101557237?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E0MTk0MTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)