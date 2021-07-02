Vue3.0学习笔记

![image-20210621161037902](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621161037902.png)

![image-20210621161138951](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621161138951.png)

![image-20210621162227026](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621162227026.png)

告诉vue 将vnode ->node  虚拟dom 转换成个真实dom   跨平台 脱离源码之外，保留api入口，扩展手段，自定义custom renderer 用特殊方式在不同平台做特殊事情

 ![image-20210621171012037](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621171012037.png)

![image-20210621171553286](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621171553286.png)

![image-20210621171915099](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621171915099.png)

![image-20210621172342305](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621172342305.png)

![image-20210621172712169](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621172712169.png)

![image-20210621172731948](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621172731948.png)

![image-20210621174053349](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210621174053349.png)

![image-20210622103129849](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622103129849.png)

![image-20210622104406179](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622104406179.png)

![image-20210622104435579](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622104435579.png)

![image-20210622104511492](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622104511492.png)

![image-20210622104930824](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622104930824.png)

![image-20210622105038766](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105038766.png)

vue.config.js

![image-20210622105158866](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105158866.png)

vite.config.js

![image-20210622105220427](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105220427.png)

![image-20210622105457421](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105457421.png)

![image-20210622105659029](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105659029.png)

![image-20210622105718504](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105718504.png)

![image-20210622105759543](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105759543.png)

![image-20210622105815367](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105815367.png)

![image-20210622105903233](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105903233.png)

![image-20210622105923800](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622105923800.png)

属性没有值，undefined,null 默认行为，哪些属性不做处理，哪些行为处理 ，处理结果true，flase

 行为上变化，主要是底层

![image-20210622113157676](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622113157676.png)

![image-20210622114002726](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622114002726.png)

![image-20210622114058856](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622114058856.png)

![image-20210622114322410](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622114322410.png)

![image-20210622160029657](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622160029657.png)

组件注册 全局，局部 全局注册到当前App实例上，局部没变



封装组件，理清输入，输出是什么

![image-20210622175029485](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210622175029485.png)

![image-20210623111431768](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623111431768.png)

![image-20210623111441071](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623111441071.png)

![image-20210623112444309](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623112444309.png)

![image-20210623113302559](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623113302559.png)

​             router-link 默认a标签，自定义标签，更纯粹

![image-20210623113623952](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623113623952.png)

exact  确切匹配 以前的完全匹配 有很多问题解决不了，别名alias跳转到，不是完全匹配。

让用户自己处理exact匹配。用户输入的和定义的路由完美一样，其他的自行处理

![image-20210623113838132](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623113838132.png)

mixins 以前混入会写路由守卫

![image-20210623114033515](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623114033515.png)

![image-20210623114201330](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623114201330.png)



首屏加载动画等待首屏加载完成使用，

服务端渲染不想等待app实例导航加载完成，可以不用等router就绪，直接挂载

![image-20210623114549193](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623114549193.png)

![image-20210623114700399](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623114700399.png)

![image-20210623114821735](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623114821735.png)

![image-20210623115007971](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623115007971.png)



![image-20210623115227333](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623115227333.png)

![image-20210623115309170](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623115309170.png)

![image-20210623115453506](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623115453506.png)

匹配不到/dashboard/home

![image-20210623115512300](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623115512300.png)

匹配/dashboard/home



编码 加号减号  浏览器一致性，可以直接拷贝浏览器地址，直接push

![image-20210623142557267](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623142557267.png)

![image-20210623142608634](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210623142608634.png)
