## amis改造之路

### 1.思路

​      鉴于amis是react写的，公司目前使用vue技术栈，目前采取的策略是：

- 修改源代码    amis源码下载https://github.com/aisuda/amis-editor-demo   执行 npm run  dev 生成 public文件
- 独立部署 public文件
- iframe嵌入

![image-20211103150058131](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211103150058131.png)



修改代码的位置 （压缩文件，vscode无法格式化，用的webstorm）

![image-20211103150524766](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211103150524766.png)



### 2.过程

- 空工程，引入sdk     

  1. github 的 [releases](https://github.com/baidu/amis/releases)，文件是 sdk.tar.gz。
  2. 使用 `npm i amis` 来下载，在 `node_modules\amis\sdk` 目录里就能找到。

- 文件结构

  ![image-20211103160645693](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211103160645693.png)

- 封装渲染器 （引入的sdk，调用其中的方法需要 加上全局window，否则找不到undefined）

  ![image-20211103160730429](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211103160730429.png)

- iframe 嵌入

  ![image-20211103163132745](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211103163132745.png)

  ```
  <template>
    <div style="width: 100%; height: 100%">
      <iframe
        ref="reportRef"
        frameborder="0"
        style="width: 100%; height: 100%"
      ></iframe>
    </div>
  </template>
  
  <script>
  export default {
    name: "FormConfig",
    components: {},
    data() {
      return {
        enterpriseId: "",
        reportHubUrl: "",
        reportPage: "/#/edit/0",
        amisType: "form",
      };
    },
    watch: {},
    created() {},
    mounted() {
      this.reportHubUrl = 'http://10.1.8.62:9098/' //process.env.AMIS_EDITOR_URL;
      this.init();
    },
    methods: {
      init() {
      //   if (sessionStorage.getItem("enterpriseId")) {
      //     this.enterpriseId = sessionStorage.getItem("enterpriseId");
      //   } else {
      //     window.location.href = process.env.BASE_UNION_LOGIN;
      //     return;
      //   }
  
        //业务主题ID
        let reportId = sessionStorage.getItem("reportId");
        //API
        let baseApi = 'http://10.1.8.62:9098/';//process.env.BASE_API_ROOT;
        //编辑器退出
        let goBackUrl = window.location.origin + "/#/businessSubject";
        if (reportId) {
          this.$refs["reportRef"].src =
            this.reportHubUrl +
            this.reportPage +
            "?reportId=" +
            reportId +
            "&enterpriseId=" +
            this.enterpriseId +
            "&token=" +
            this.getToken() +
            "&baseApi=" +
            baseApi +
            "&goBackUrl=" +
            goBackUrl +
            "&amisType=" +
            this.amisType;
        } else {
          this.$router.push({ path: "/businessSubject" });
        }
      },
    },
  };
  </script>
   
  ```

  注意点：

  ​    reportPage: "/#/edit/0",默认写死，只加载一个模板

     amisType: "form", 标识 区分表单和报表

     goBackUrl，退出的地址

     路由的跳转

- 

### 3.总结

