# JavaScript防抖和节流

原创 coderwhy [coderwhy](javascript:void(0);) *2020-04-29 09:12*

收录于话题#前端面试精讲10个

## 一. 认识防抖和节流

### 1.1. 对防抖和节流的认识

防抖和节流的概念其实最早并不是出现在软件工程中，防抖是出现在电子元件中，节流出现在流体流动中

- 而JavaScript是事件驱动的，大量的操作会触发事件，加入到事件队列中处理。
- 而对于某些频繁的事件处理会造成性能的损耗，我们就可以通过防抖和节流来限制事件频繁的发生；

防抖和节流函数目前已经是前端实际开发中两个非常重要的函数，也是面试经常被问到的面试题。

但是很多前端开发者面对这两个功能，有点摸不着头脑：

- 某些开发者根本无法区分防抖和节流有什么区别（面试经常会被问到）；
- 某些开发者可以区分，但是不知道如何应用；
- 某些开发者会通过一些第三方库来使用，但是不知道内部原理，更不会编写；

而这篇文章的重点就是解决上面的问题：

- 我们不仅仅要区分清楚防抖和节流两者的区别，也要明白在实际工作中哪些场景会用到；
- 并且我会带着大家一点点来编写一个自己的防抖和节流的函数，不仅理解原理，也学会自己来编写；

### 1.2. 为什么需要防抖节流

#### 1.2.1. 为什么需要防抖

我先举一个生活中的例子：

- 比如说有一天我上完课，我说大家有什么问题来问我，我会等待一分钟的时间。

- 如果在一分钟的时间内，没有同学问我问题，那么我就下课了；

- - 在此期间，a同学过来问问题，并且帮他解答，解答完后，我会再次等待一分钟的时间看有没有其他同学问问题；
  - 如果我等待超过了1分钟，就点击了下课（才真正执行这个时间）

我们都遇到过这样的场景，在某个搜索框中输入自己想要搜索的内容：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/O8xWXzAqXuslmreF5tpamNRJvB9hP96ibtrHTLCqR1329jeC0AeAiat3l1n5XvjAzXgu00pb6YgfHRkpM7uj8nUA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)搜索界面

比如想要搜索一个MacBook：

- 当我输入m时，为了更好的用户体验，通常会出现对应的联想内容，这些联想内容通常是保存在服务器的，所以需要一次网络请求；
- 当继续输入ma时，再次发送网络请求；
- 那么macbook一共需要发送7次网络请求；
- 这大大损耗我们整个系统的性能，无论是前端的事件处理，还是对于服务器的压力；

但是我们需要这么多次的网络请求吗？

- 不需要，正确的做法应该是在合适的情况下再发送网络请求；
- 比如如果用户快速的输入一个macbook，那么只是发送一次网络请求；
- 比如如果用户是输入一个m想了一会儿，这个时候m确实应该发送一次网络请求；
- 也就是我们应该监听用户在某个时间，比如500ms内，没有再次触发时间时，再发送网络请求；

这就是防抖的操作：

- 只有在某个时间内，没有再次触发某个函数时，才真正的调用这个函数；

我们用一副图来理解一下它的过程；

- 当事件触发时，相应的函数并不会立即触发，而是会等待一定的时间；
- 当事件密集触发时，函数的触发会被频繁的推迟；
- 只有等待了一段时间也没有事件触发，才会真正的执行响应函数；

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)防抖函数

防抖的应用场景很多：

- 输入框中频繁的输入内容，搜索或者提交信息；
- 频繁的点击按钮，触发某个事件；
- 监听浏览器滚动事件，完成某些特定操作；
- 用户缩放浏览器的resize事件；

总之，密集的事件触发，我们只希望触发比较靠后发生的事件，就可以使用防抖函数；

#### 1.2.2. 为什么需要节流

我先举一个生活中的例子：

- 在有课的情况下，我每天都会在晚上8点钟开始上课，这是一个固定频率；
- 无论中间有多少学生和我说，老师我们早点上课吧？不行，晚上8点钟准时开讲；

很多人都玩过类似于飞机大战的游戏（当然，这里只是举一个例子，为了方便大家理解，虽然大部分前端程序员并不做游戏，但是这个例子可以很好的解释节流函数）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)飞机大战

在飞机大战的游戏中，我们按下空格会发射一个子弹：

- 很多飞机大战的游戏中会有这样的设定，即使按下的频率非常快，子弹也会保持一定的频率来发射；（其实很多的街机游戏都是这样的，普通攻击有一个最高的频率）；
- 比如1秒钟只能发射一次，即使用户在这1秒钟按下了10次，子弹会保持发射一颗的频率来发射；
- 但是事件是触发了10次的，响应的函数只触发了一次；

这就是节流的操作：

- 在某个时间内（比如500ms），某个函数只能被触发一次；

我们还是用一副图来表示：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)节流函数

节流的应用场景：

- 监听页面的滚动事件；
- 鼠标移动事件；
- 用户频繁点击按钮操作；
- 游戏中的一些设计；

总之，依然是密集的事件触发，但是这次密集事件触发的过程，不会等待最后一次才进行函数调用，而是会按照一定的频率进行调用；

## 二. 防抖函数的实现

### 2.1. 防抖案例准备

我们通过一个搜索框来延迟防抖函数的实现过程：

- 监听input的输入，通过打印模拟网络请求

```
<body>
  <input class="search" type="text">

  <script>
    // 1.获取输入框
    var search = document.querySelector(".search");

    // 2.监听输入内容,发送ajax请求
    // 2.1.定义一个监听函数
    var counter = 0;
    function searchChange() {
      counter++;
      console.log("发送"+ counter +"网络请求");
    }

    // 绑定oninput
    search.oninput = searchChange
  </script>
</body>
```

测试发现快速输入一个macbook共发送了7次请求，显示我们需要对它进行防抖操作

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)image-20200423105918838

### 2.2. 第三方库实现

事实上我们可以通过一些第三方库来实现防抖操作：

- lodash
- underscore

**我们这里以lodash为例，我们可以理解成lodash是underscore的升级版**

#### 2.2.1. 安装lodash

lodash的官方：https://lodash.com/

lodash的安装有很多种方式：

- 下载lodash，本地引入；
- 通过CDN直接引入；
- 通过包管理工具（npm）管理安装；

我们这里以CDN为例：

```
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.15/lodash.min.js"></script>
```

#### 2.2.2. 使用lodash

下面我们通过lodash中的debounce函数对searchChange函数进行处理：

- debounce函数要求我们传入一个需要处理的函数，并且传入一个delay的时间

- - 在delay的时间内没有再次触发事件，才会真正执行函数

- debounce返回一个新的函数，将新的函数设置到oninput事件中

```
// 对searchChange处理
var _searchChange = _.debounce(searchChange, 500);

// 绑定oninput
search.oninput = _searchChange
```

测试结果如下：

- 当我快速输入macbook时，只会发送一次请求

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)测试lodash处理后

### 2.3. 自定义防抖函数

作为学习，我们肯定不满足只会用第三方的库，成为一个API程序员。另外为了一个防抖函数引用整个库对最终打包项目的大小也会有影响。

所以，我们要实现一个自己的防抖函数。

- 注意，在整个实现过程中，我尽可能**不采用ES6的语法**；
- 因为如果我们不babel来处理我们的代码，ES6以上的语法会存在兼容性问题；
- 我们封装的防抖和节流函数就不太具备通用性；

#### 2.3.1. 防抖基本功能

防抖函数的核心思路如下：

- 当触发一个函数时，并不会立即执行这个函数，而是会延迟（通过定时器来延迟函数的执行）

- - 如果在延迟时间内，有重新触发函数，那么取消上一次的函数执行（取消定时器）；
  - 如果在延迟时间内，没有重新触发函数，那么这个函数就正常执行（执行传入的函数）；

接下来，就是将思路转成代码即可：

- 定义debounce函数要求传入两个参数

- - 需要处理的函数fn；
  - 延迟时间；

- 通过定时器来延迟传入函数fn的执行

- - 如果在此期间有再次触发这个函数，那么clearTimeout取消这个定时器；
  - 如果没有触发，那么在定时器的回调函数中执行即可；

```
function debounce(fn, delay) {
  var timer = null;
  return function() {
    if (timer) clearTimeout(timer);
    timer = setTimeout(function() {
      fn();
    }, delay);
  }
}
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)测试自己的debounce

#### 2.3.2. 优化参数和this

我们知道在oninput事件触发时会有参数传递，并且触发的函数中this是指向当前的元素节点的

- 目前我们fn的执行是一个独立函数调用，它里面的this是window

- - 我们需要将其修改为对应的节点对象，而返回的function中的this指向的是节点对象；

- 目前我们的fn在执行时是没有传递任何的参数的，它需要将触发事件时传递的参数传递给fn

- - 而我们返回的function中的arguments正是我们需要的参数；

所以我们的代码可以进行如下的优化：

```
function debounce(fn, delay) {
  var timer = null;
  return function() {
    if (timer) clearTimeout(timer);
    // 获取this和argument
    var _this = this;
    var _arguments = arguments;
    timer = setTimeout(function() {
      // 在执行时，通过apply来使用_this和_arguments
      fn.apply(_this, _arguments);
    }, delay);
  }
}
```

我们打印一下searchChange中的this和event参数：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)测试优化后的debounce

#### 2.3.3. 优化取消功能

有时候，在等待执行的过程中，可能需要取消之前的操作：

- 比如用户进行了搜索，但是还没有来得及发送搜索的情况下，退出了界面；
- 当用户退出时，之前的操作就可以取消掉；

我们这里将delay时间改长，并且在下方增加一个按钮：

- 在延迟时间内，我们点击按钮，就取消之前的函数执行；

这一次我给出完整的代码结构：

- HTML代码；
- 第一个script标签中封装的是debounce函数；
- 第二个script标签中是业务逻辑js代码；

```
<body>
  
  <input class="search" type="text">
  <button class="cancel-btn">取消事件</button>

  <script>
    function debounce(fn, delay) {
      var timer = null;
      var handleFn = function() {
        if (timer) clearTimeout(timer);
        // 获取this和argument
        var _this = this;
        var _arguments = arguments;
        timer = setTimeout(function() {
          // 在执行时，通过apply来使用_this和_arguments
          fn.apply(_this, _arguments);
        }, delay);
      }

      // 取消处理
      handleFn.cancel = function() {
        if (timer) clearTimeout(timer);
      }

      return handleFn;
    }
  </script>
  <script>
    // 1.获取输入框
    var search = document.querySelector(".search");

    // 2.监听输入内容,发送ajax请求
    // 2.1.定义一个监听函数
    var counter = 0;
    function searchChange(e) {
      counter++;
      console.log("发送"+ counter +"网络请求");
      console.log(this);
      console.log(e.target.value);
    }

    // 对searchChange处理
    var _searchChange = debounce(searchChange, 3000);

    // 绑定oninput
    search.oninput = _searchChange;

    // 3.取消事件
    var cancelBtn = document.querySelector(".cancel-btn");
    cancelBtn.onclick = function(event) {
      _searchChange.cancel();
    }

  </script>
</body>
```

#### 2.3.4. 优化立即执行

目前我们的事件触发都要等到delay时间，但是某些场景是用户开始输入时的第一次是立即执行的，后续的输入才需要等待，我们可以如何优化呢？

- 我们可以让用户多传入一个参数：leading

- - 那么第一次就立即执行
  - 后来的事件需要等待delay时间执行
  - leading为false，或者不传，那么按照上面的防抖进行操作
  - leading为true

- 我们可以根据是否传入leading进行不同的处理方式

这个代码会一些复杂，在立即执行的地方需要进行更多的操作：

```
function debounce(fn, delay, leading) {
  var timer = null;
  leading = leading || false;
  var handleFn = function() {
    if (timer) clearTimeout(timer);
    // 获取this和argument
    var _this = this;
    var _arguments = arguments;

    if (leading) {
      // 通过一个变量来记录是否立即执行
      var isInvoke = false;
      if (!timer) {
        fn.apply(_this, _arguments);
        isInvoke = true;
      }
      // 定时器通过修改timer来修改instant
      timer = setTimeout(function() {
        timer = null;
        if (!isInvoke) {
          fn.apply(_this, _arguments);
        }
      }, delay);
    } else {
      timer = setTimeout(function() {
        // 在执行时，通过apply来使用_this和_arguments
        fn.apply(_this, _arguments);
      }, delay);
    }
  }

  // 取消处理
  handleFn.cancel = function() {
    if (timer) clearTimeout(timer);
  }

  return handleFn;
}
```

#### 2.3.5. 优化返回值

有时候fn函数执行结束后还有返回值，如果我们希望拿到这个返回值应该怎么办呢？

先明确一个操作：

- 内部执行fn函数大多数情况是异步执行的（在setTimeout中执行）
- 所以通过return是无法拿到返回值的

异步的操作如何获取返回值呢？

- ES6中通过Promise
- ES6之前通过回调函数

第一版：Promise的版本：

- 这里我给出一版Promise，但是我们希望这个防抖函数尽可能具备通用性
- 所以暂时不建议使用Promise

```
function debounce(fn, delay, leading) {
  var timer = null;
  leading = leading || false;
  var handleFn = function () {
    return new Promise((resovle, reject) => {
      if (timer) clearTimeout(timer);
      // 获取this和argument
      var _this = this;
      var _arguments = arguments;

      if (leading) {
        // 通过一个变量来记录是否立即执行
        var isInvoke = false;
        if (!timer) {
          resovle(fn.apply(_this, _arguments));
          isInvoke = true;
        }
        // 定时器通过修改timer来修改instant
        timer = setTimeout(function () {
          timer = null;
          if (!isInvoke) {
            resovle(fn.apply(_this, _arguments)); 
          }
        }, delay);
      } else {
        timer = setTimeout(function () {
          // 在执行时，通过apply来使用_this和_arguments
          resovle(fn.apply(_this, _arguments));
        }, delay);
      }
    })
  }

  // 取消处理
  handleFn.cancel = function () {
    if (timer) clearTimeout(timer);
  }

  return handleFn;
}
```

第二版：回调函数版本

- 因为这一次我们有多个可选参数，所以我们让调用者传入一个option

- - leading：是否开始的回调直接执行一次
  - result：函数类型，通过它来将结果回调出去

```
function debounce(fn, delay, option) {
  var timer = null;
  if (!option) option = {};
  leading = option.leading || false;
  result = option.result || null;
  var handleFn = function () {
    if (timer) clearTimeout(timer);
    // 获取this和argument
    var _this = this;
    var _arguments = arguments;

    if (leading) {
      // 通过一个变量来记录是否立即执行
      var isInvoke = false;
      if (!timer) {
        callFn(_this, _arguments);
        isInvoke = true;
      }
      // 定时器通过修改timer来修改instant
      timer = setTimeout(function () {
        timer = null;
        if (!isInvoke) {
          callFn(_this, _arguments);
        }
      }, delay);
    } else {
      timer = setTimeout(function () {
        // 在执行时，通过apply来使用_this和_arguments
        callFn(_this, _arguments);
      }, delay);
    }
  }

  function callFn(context, argument) {
    var res = fn.apply(context, argument);
    if (result) {
      result(res);
    }
  }

  // 取消处理
  handleFn.cancel = function () {
    if (timer) clearTimeout(timer);
  }

  return handleFn;
}
```

## 三. 节流函数的实现

> 因为防抖和节流函数都是对频繁事件的处理，所以我们可以使用相同的案例来演练，另外对应的优化操作也是比较相似的，所以这里不再进行细分，某些代码在实现过程中直接编写。

### 3.1. 案例和第三方库

我们还是使用之前的案例，在输入框中不断的输入内容，但是回调函数会以固定的频率来执行：

```
<body>
  <input class="search" type="text">
  
  <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.15/lodash.min.js"></script>
  <script>
    // 1.获取输入框
    var search = document.querySelector(".search");

    // 2.监听输入内容,发送ajax请求
    // 2.1.定义一个监听函数
    var counter = 0;
    function searchChange() {
      counter++;
      console.log("发送"+ counter +"网络请求");
    }

    var _lodashSearchChange = _.throttle(searchChange, 1000)

    // 绑定oninput
    search.oninput = _lodashSearchChange
  </script>
</body>
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)实现节流效果

### 3.2. 自定义防抖函数

#### 3.2.1. 节流基本功能

节流函数的默认实现思路我们采用时间戳的方式来完成：

- 我们使用一个last来记录上一次执行的时间

- - 每次准备执行前，获取一下当前的时间now：`now - last > interval`
  - 那么函数执行，并且将now赋值给last即可

```
function throttle(fn, interval) {
  var last = 0;

  return function() {
    // this和argument
    var _this = this;
    var _arguments = arguments;

    var now = new Date().getTime();
    if (now - last > interval) {
      fn.apply(_this, _arguments);
      last = now;
    }
  }
}
```

#### 3.2.2. 优化最后执行

默认情况下，我们的防抖函数最后一次是不会执行的

- 因为没有达到最终的时间，也就是条件`now - last > interval`满足不了的
- 但是，如果我们希望它最后一次是可以执行的，那么我们可以让其传入对应的参数来控制

我们来看一下代码如何实现：

- 我们增加了else语句：

- - 所以我们可以使用timer变量来记录定时器是否已经开启
  - 已经开启的情况下，不需要开启另外一个定时器了
  - else语句表示没有立即执行的情况下，就会开启定时器；
  - 但是定时器不需要频繁的开启，开启一次即可

- 如果固定的频率中执行了回调函数

- - 因为刚刚执行过回调函数，所以定时器到时间时不需要执行；
  - 所以我们需要取消定时器，并且将timer赋值为null，这样的话可以开启下一次定时器；

- 如果定时器最后执行了，那么timer需要赋值为null

- - 因为下一次重新开启时，只有定时器为null，才能进行下一次的定时操作；

```
function throttle(fn, interval) {
  var last = 0;
  var timer = null;

  return function() {
    // this和argument
    var _this = this;
    var _arguments = arguments;

    var now = new Date().getTime();
    if (now - last > interval) {
      if (timer) {
        clearTimeout(timer);
        timer = null;
      }
      fn.apply(_this, _arguments);
      last = now;
    } else if (timer === null) { // 只是最后一次
      timer = setTimeout(function() {
        timer = null;
        fn.apply(_this, _arguments);
      }, interval);
    }
  }
}
```

我们可以传入一个变量让来确定是否需要最后执行一次：

```
function throttle(fn, interval, option) {
  var last = 0;
  var timer = null;
  if (!option) option = {};

  var trailing = option.trailing || false;

  return function() {
    // this和argument
    var _this = this;
    var _arguments = arguments;

    var now = new Date().getTime();
    if (now - last > interval) {
      if (timer) {
        clearTimeout(timer);
        timer = null;
      }
      fn.apply(_this, _arguments);
      last = now;
    } else if (timer === null && trailing) { // 只是最后一次
      timer = setTimeout(function() {
        timer = null;
        fn.apply(_this, _arguments);
      }, interval);
    }
  }
}
```

#### 3.2.3. 优化取消功能

取消功能和防抖函数类似：

```
function throttle(fn, interval) {
  var last = 0;
  var timer = null;

  var handleFn = function() {
    // this和argument
    var _this = this;
    var _arguments = arguments;

    var now = new Date().getTime();
    if (now - last > interval) {
      if (timer) {
        clearTimeout(timer);
        timer = null;
      }
      fn.apply(_this, _arguments);
      last = now;
    } else if (timer === null) { // 只是最后一次
      timer = setTimeout(function() {
        timer = null;
        fn.apply(_this, _arguments);
      }, interval);
    }
  }

  handleFn.cancel = function() {
    clearTimeout(timer);
    timer = null;
  }

  return handleFn;
}
```

#### 3.2.4. 优化返回值

和防抖函数类似，使用Promise或者回调函数，这里我还是采用回调函数：

```
function throttle(fn, interval, option) {
  var last = 0;
  var timer = null;
  if (!option) option = {};

  var trailing = option.trailing || false;
  var result = option.result || null;

  var handleFn = function() {
    // this和argument
    var _this = this;
    var _arguments = arguments;

    var now = new Date().getTime();
    if (now - last > interval) {
      if (timer) {
        clearTimeout(timer);
        timer = null;
      }
      callFn(_this, _arguments);
      last = now;
    } else if (timer === null && trailing) { // 只是最后一次
      timer = setTimeout(function() {
        timer = null;
        callFn(_this, _arguments);
      }, interval);
    }
  }

  handleFn.cancel = function() {
    clearTimeout(timer);
    timer = null;
  }

  function callFn(context, argument) {
    var res = fn.apply(context, argument);
    if (result) {
      result(res);
    }
  }

  return handleFn;
}
```



 

 