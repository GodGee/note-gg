# git提交忽略文件或文件夹



### 一、 模式

.gitignore文件过滤有两种模式，开放模式和保守模式

1. 开放模式负责设置过滤哪些文件和文件夹

例如： /target/ 表示项目根目录下的target文件夹里面所有的内容都会被过滤，不被GIT 跟踪

​           .classpath 表示项目根目录下的.classpath文件会被过滤，不被GIT跟踪

 

2. 保守模式负责设置哪些文件不被过滤，也就是哪些文件要被跟踪。

例如：!/target/*.h 表示target文件夹目录下所有的.h文件将被跟踪

### 二、 规则

##### **1. 在已忽略文件夹中不忽略指定文件夹**

```
/node_modules/*

!/node_modules/layer/
```

##### **2. 在已忽略文件夹中不忽略指定文件**

```
/node_modules/*

!/node_modules/layer/layer.js
```

【注意项】注意写法 要忽略的文件夹一定要结尾 /* ，否则不忽略规则将无法生效

##### **3. 其他规则写法 (附)**

- 　 以斜杠“/”开头表示目录；
- 　以星号“*”通配多个字符；
- 
  　以问号“?”通配单个字符
- 
     以方括号“[]”包含单个字符的匹配列表；
- 
     以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

1、忽略文件

```
*.bak        # 忽略所有扩展名为.bak的文件

!keep.bak   # 但keep.bak文件除外（不会被忽略）

temp/test.txt # 忽略temp目录下的test.txt文件

temp/*.txt    # 忽略temp目录下所有扩展名为.txt的文件
```

 

2、忽略目录

```
temp/    # 忽略temp目录下的所有目录和文件

temp/*/  # 忽略temp目录下的所有目录，但不会忽略该目录下的文件
```

### 三、**Git忽略规则(.gitignore配置）不生效原因和解决**

```
第一种方法:
.gitignore中已经标明忽略的文件目录下的文件，git push的时候还会出现在push的目录中，或者用git status查看状态，想要忽略的文件还是显示被追踪状态。
原因是因为在git忽略目录中，新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的，
这时候我们就应该先把本地缓存删除，然后再进行git的提交，这样就不会出现忽略的文件了。
 
解决方法: git清除本地缓存（改变成未track状态），然后再提交:
[root@kevin ~]``# git rm -r --cached .
[root@kevin ~]``# git add .
[root@kevin ~]``# git commit -m 'update .gitignore'
[root@kevin ~]``# git push -u origin master
```

 

```
需要特别注意的是：
1）.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
2）想要.gitignore起作用，必须要在这些文件不在暂存区中才可以，.gitignore文件只是忽略没有被staged(cached)文件，
  ``对于已经被staged文件，加入ignore文件时一定要先从staged移除，才可以忽略。
```

 

```
第二种方法:（推荐）
在每个clone下来的仓库中手动设置不要检查特定文件的更改情况。
[root@kevin ~]``# git update-index --assume-unchanged PATH         //在PATH处输入要忽略的文件
```