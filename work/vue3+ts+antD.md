### vue3+ts+antD

1.环境搭建

```
vue create vue3-gg

npm install ant-design-vue@next --save

npm install babel-plugin-import --save-dev
```

- 新建文件保存按需加载的控件

![image-20211008150219586](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211008150219586.png)

- 引入

  ![image-20211008150318697](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211008150318697.png)

- 配置  只需从 ant-design-vue 引入模块即可，无需单独引入样式。等同于下面手动引入的方式。

  ![image-20211008150345718](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\image-20211008150345718.png)