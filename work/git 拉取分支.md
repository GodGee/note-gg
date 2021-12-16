从master分支copy一个开发分支DEV：

1.切换到master分支

git checkout master

2.获取最新代码

git pull origin master

3.从当前分支拉copy开发分支dev：(新建了一个和master一样的分支Dev)

git checkout -b dev

4.把新建的分支push到远端

git push origin dev

5.关联

git branch --set-upstream-to=origin/dev

6.再次拉取验证

git pull origin dev

7.就可以在新的分支上开发了

将Dev的修改再合并到master

1.切换到master

git checkout master

2.获取最新代码

git pull

3.合并：

git merge dev
 