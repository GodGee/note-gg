### vue3 + vite + ts  开发笔记

##### 1.问题

- shims-vue.d.ts文件的作用

  shims-vue.d.ts是为了 typescript 做的适配定义文件，因为.vue 文件不是一个常规的文件类型，ts 是不能理解 vue 文件是干嘛的，
  加这一段是是告诉 ts，vue 文件是这种类型的。这一段删除，会发现 import 的所有 vue 类型的文件都会报错。declare module '*.vue' { //declare声明宣告， 声明一个ambient module(即:没有内部实现的 module声明) 

  ```
  declare module '*.vue' { //declare声明宣告， 声明一个ambient module(即:没有内部实现的 module声明) 
    import Vue from 'vue'
    export default Vue
  }
  
  declare module 'vue-echarts'  // 引入vue-echarts
  
  
  <script lang="ts">
      /* eslint-disable @typescript-eslint/camelcase */
      import { Vue, Component, Watch } from 'vue-property-decorator'
      import ECharts from 'vue-echarts' //报错,按上面的方法在shims-vue.d.ts文件中引入即可
      import 'echarts/lib/chart/line'
      import 'echarts/lib/chart/pie'
      import 'echarts/lib/component/tooltip'
  </script>
  ```

   

- env.d.ts

  TypeScript 的智能提示

  默认情况下，Vite 在 [`vite/client.d.ts`](https://github.com/vitejs/vite/blob/main/packages/vite/client.d.ts) 中为 `import.meta.env` 提供了类型定义。随着在 `.env[mode]` 文件中自定义了越来越多的环境变量，你可能想要在代码中获取这些以 `VITE_` 为前缀的用户自定义环境变量的 TypeScript 智能提示。

  要想做到这一点，你可以在 `src` 目录下创建一个 `env.d.ts` 文件，接着按下面这样增加 `ImportMetaEnv` 的定义：

  ```
  /// <reference types="vite/client" />
  
  interface ImportMetaEnv {
    readonly VITE_APP_TITLE: string
    // 更多环境变量...
  }
  
  interface ImportMeta {
    readonly env: ImportMetaEnv
  }
  ```

- TypeScript报错--找不到模块“path”或其相应的类型声明

  ```
  npm install @types/node --save-dev
  ```

  

- Type ‘HTMLElement | null‘ is not assignable to type ‘HTMLElement‘

  在typescript3.9中，以下代码编译时会提示错误：

  ```
  const elem : HTMLElement = document.getElementById('someid');
  // Type 'HTMLElement | null' is not assignable to type 'HTMLElement'
   
  解决方法1： 禁用strict模式
  修改tsconfig.ts文件，
  "strict": true,  ---> "strict": false,
   
  解决方法2: 严格模式下，加个判断
  let elem: HTMLElement;
  const temp = document.getElementById('someid');
  if (temp) {
  	elem = temp;
     // ...
  }
  解决方法3：使用类型断言(Type Assertion)
  const elem : HTMLElement = document.getElementById('someid') as HTMLElement;
  ```

   

- 

##### 2.步骤

- 在vue3 中使用echarts

  1.安装

  ```
  npm install echarts --save
  ```

  2.在main.js中导入

  ```
  import { createApp } from 'vue'
  import App from './App.vue'
  import * as echarts from 'echarts'
  
  const app = createApp(App).mount('#app')
  app.echarts=echarts
  ```

  3,在需要使用的页面,定义<font color='red'>div宽高</font>

  ```
  <div id="myChart"
       :style="{ width: '300px', height: '300px' }">
  </div>
  ```

  4,在mounted中init

  ```
  mounted() {
      //this.$root => app
      let myChart = this.$root.echarts.init(
        document.getElementById("myChart")
      );
      // 绘制图表
      myChart.setOption({
        title: { text: "总用户量" },
        tooltip: {},
        xAxis: {
          data: ["12-3", "12-4", "12-5", "12-6", "12-7", "12-8"],
        },
        yAxis: {},
        series: [
          {
            name: "用户量",
            type: "line",
            data: [5, 20, 36, 10, 10, 20],
          },
        ],
      });
    },
  ```

  这里最重要的是import * as echarts from 'echarts', 不能 import echarts from 'echarts',这样会报错,因为5,0版本的echarts的接口已经变成了下面这样

  ```
  export { EChartsFullOption as EChartsOption, connect, disConnect, dispose, getInstanceByDom, getInstanceById, getMap, init, registerLocale, registerMap, registerTheme };
  ```

  


  在SetUp中使用echarts


  因为setup中没有this,而且这时候还没有渲染,所以在setup中 ,可以使用provide/inject来把echart引入进来 

  在根组件里引入echart,一般是App.vue

  ```
  App.vue:
  
  import * as echarts from 'echarts'
  import { provide } from 'vue'
  
  export default {
    name: 'App',
    setup(){
      provide('ec',echarts)//provide
    },
    components: {
    }
  }
  ```

  之后在需要的页面中inject

   data_page.vue:

  ```
  import { inject, onMounted } from "vue";
  
  export default {
    name: "data_page",
    setup() {
      let echarts = inject("ec");//引入
      onMounted(() => {//需要获取到element,所以是onMounted的Hook
        let myChart = echarts.init(document.getElementById("customerChart"));
        // 绘制图表
        myChart.setOption({
          title: { text: "总用户量" },
          tooltip: {},
          xAxis: {
            data: ["12-3", "12-4", "12-5", "12-6", "12-7", "12-8"],
          },
          yAxis: {},
          series: [
            {
              name: "用户量",
              type: "line",
              data: [5, 20, 36, 10, 10, 20],
            },
          ],
        });
        window.onresize = function () {//自适应大小
          myChart.resize();
        };
      });
    },
    components: {},
    mounted() {},
  };
  ```

   setup这一块，我在需要用到echarts的组件上直接引入import * as echarts from 'echarts'也是可以直接使用echarts来指向当前插件，感觉不需要用到provide和inject

- vue3-json-viewer 的使用

  需要依赖clipboard，先安装clipboard

  ```
  $ npm install clipboard --save
  ```

  再安装vue3-json-viewer

  ```
  $ npm install vue3-json-viewer --save
  ```

  mian.js

  ```
  import { createApp } from 'vue'
  import App from './App.vue'
  import JsonViewer from "vue3-json-viewer"
  const app=createApp(App)
  app.use(JsonViewer)
  app.mount('#app')
  ```

  App.vue

  ```
  <template>
    <json-viewer :value="jsonData" copyable boxed sort />
  </template>
  
  <script setup>
  import { reactive, ref } from "vue";
  import HelloWorld from "./components/HelloWorld.vue";
  let obj = {
    name: "qiu",//字符串
    age: 18,//数组
    isMan:false,//布尔值
    date:new Date(),
    fn:()=>{},
    arr:[1,2,5]
  };
  const jsonData = reactive(obj);
  const strData = ref("http://www.baidu.com");
  </script>
  
  <style></style>
  ```

- echarts图表二次渲染残留第一次数据怎么搞

  <font color='red'>mychart.clear();</font>

  ```
  const renderEchart = () => {
    //需要获取到element,所以是onMounted的Hook
    let myChart = echarts.init(
      document.getElementById("customerChart") as HTMLElement
    );
    //echarts图表二次渲染残留第一次数据
    myChart.clear();
    // 绘制图表
    myChart.setOption(jsonData.json);
    window.onresize = function () {
      //自适应大小
      myChart.resize();
    };
  };
  ```

  

- vue3更改setup中定义的值不渲染到视图？

  了解到需要ref()、.value来更新数据到视图：赋值的时候就需要加上.value

  ```
  let info: any = ref({}) // 用ref() 定义
  ```

  复杂类型可用reactive() ，此处注意，reactive() 定义的字段没法直接赋值，所以需要像以下：

  ```
  const info = reactive({
        detail: {}, // 活动信息
        list: [] 
      });
  
  // 赋值
  info.detail = {id: 1,name: "玛卡巴卡"} 
  info.list = [{id: 1,name: "玛卡巴卡"}]
  ```

   

- 加载雷达图报错，json格式有问题

  ![image-20211223104619580](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211223104619580.png)

  ```
  {
        title: {
          text: 'Basic Radar Chart'
        },
        tooltip: {
          confine: true,
          enterable: true,
        },
        legend: {
          data: ["Allocated Budget", "Actual Spending"],
        },
        toolbox: {
          show: true,
          feature: {
            mark: {
              show: true,
            },
            dataView: {
              show: true,
              readOnly: true,
            },
            magicType: {
              show: false,
              type: ["line", "bar"],
            },
            restore: {
              show: true,
            },
            saveAsImage: {
              show: true,
            },
          },
        },
        radar: [
          {
            indicator: [
              { text: "Sales", max: 6500 },
              { text: "Administration", max: 16000 },
              { text: "Information Technology", max: 30000 },
              { text: "Customer Support", max: 38000 },
              { text: "Development", max: 52000 },
              { text: "Marketing", max: 25000 },
            ],
            radius: 90,
            splitNumber: 7,
            name: {
              textStyle: {
                color: "#838D9E",
              },
            },
            axisLine: {
              lineStyle: {
                color: "rgba(0,0,0,0.15)",
              },
            },
            splitArea: {
              show: false,
              areaStyle: {
                color: "rgba(255,255,255,0)",
              },
            },
            splitLine: {
              show: true,
              lineStyle: {
                width: 1,
                color: "rgba(0,0,0,0.15)",
              },
            },
          },
        ],
        series: [
          {
            type: "radar",
            grid: {
              containLabel: true,
            },
            tooltip: {
              trigger: "item",
            },
            areaStyle: {
              normal: {
                width: 1,
                opacity: 0.2,
              },
            },
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              normal: {
                areaStyle: {
                  type: "default",
                },
                color: ["#1FC48D", "3F8FFF"],
              },
            },
            data: [
              {
                value: [4200, 3000, 20000, 35000, 50000, 18000],
                name: "Allocated Budget",
                itemStyle: {
                  normal: {
                    color: "#1FC48D",
                    lineStyle: {
                      color: "#1FC48D",
                    },
                  },
                },
              },
              {
                value: [5000, 14000, 28000, 26000, 42000, 21000],
                name: "Actual Spending",
                itemStyle: {
                  normal: {
                    color: "#3F8FFF",
                    lineStyle: {
                      color: "#3F8FFF",
                    },
                  },
                },
              },
            ],
          },
        ],
      }
  ```

  

- 

##### 3.总结