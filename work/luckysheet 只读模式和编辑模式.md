##### luckysheet 只读模式和编辑模式

只读模式配置：

```
var options = {
  container: "luckysheet", //容器的ID
  title: "bi", // 工作簿名称
  lang: "zh", // 设定表格语言 国际化设置，允许设置表格的语言，支持中文("zh")和英文("en")
  allowCopy: false, // 是否允许拷贝
  showtoolbar: false, // 是否显示工具栏
  showinfobar: false, // 是否显示顶部信息栏
  showsheetbar: false, // 是否显示底部sheet页按钮
  showstatisticBar: false, // 是否显示底部计数栏
  sheetBottomConfig: false, // sheet页下方的添加行按钮和回到顶部按钮配置
  allowEdit: false, // 是否允许前台编辑
  enableAddRow: false, // 允许增加行
  enableAddCol: false, // 允许增加列
  userInfo: false, // 右上角的用户信息展示样式
  showRowBar: false, // 是否显示行号区域
  showColumnBar: false, // 是否显示列号区域
  sheetFormulaBar: false, // 是否显示公式栏
  enableAddBackTop: false,//返回头部按钮
  rowHeaderWidth: 0,//纵坐标
  columnHeaderHeight: 0,//横坐标
  showstatisticBarConfig: {
    count:false,
    view:false,
    zoom:false,
  },
  showsheetbarConfig: {
    add: false, //新增sheet
    menu: false, //sheet管理菜单
    sheet: false, //sheet页显示
  },
  hook: {
    cellMousedown:this.cellMousedown,//绑定鼠标事件
  },
  forceCalculation: true,//强制计算公式
  data: sheetData
};
```

编辑模式：

```
container: 'luckysheet', //luckysheet为容器id
lang:'zh',
showGridLines:true,
allowEdit:true,
showinfobar: false, // 是否显示顶部信息栏
showsheetbar: false, // 是否显示底部sheet页按钮
showstatisticBar: false, // 是否显示底部计数栏
sheetBottomConfig: false, // sheet页下方的添加行按钮和回到顶部按钮配置
userInfo: false, // 右上角的用户信息展示样式
plugins: ['chart'],
showstatisticBarConfig: {
  count:false,
  view:false,
  zoom:false,
},
showsheetbarConfig: {
  add: false, //新增sheet
  menu: false, //sheet管理菜单
  sheet: false, //sheet页显示
},
hook: {
  cellMousedown:this.cellMousedown,
}
```

编辑模式下格式数据保存到服务端，到只读模式下展示时 需要 数据转换：

```
let sheetfile = luckysheet.getLuckysheetfile();
sheetfile[0].celldata = luckysheet.transToCellData(sheetfile[0].data);
```

由于是集成到vue  keep-alive 的router-view下 每个路由下的页面都显示一个luckysheet 只读界面，但是luckysheet集成到项目中是全局的（只有一个）,所有在切换路径时需要 切换显示luckysheet的index

目前采用的是修改扩展luckysheet  每次切换路由 都重新设置显示内容setStore 和绑定 鼠标单击事件 

```
//重新绑定事件
  luckysheet$1.setHook = function(hook){
    luckysheetConfigsetting.hook = hook;
  }

//重新设置文档设置和数据
  luckysheet$1.setStore = function(Store_out){
    Store = Store_out;
  }

//获取设置文档设置和数据
  luckysheet$1.getStore = function(){
    return Store;
  }
```

 