#### git如何查看远程仓库地址------git remote -v

#### git查看远程项目所有分支--------git branch -a

#### 更改git远程仓库

* 通过命令直接修改远程仓库地址

  ~~~
  git remote 查看所有远程仓库
  git remote xxx 查看指定远程仓库地址
  git remote set-url origin 你新的远程仓库地址
  ~~~

* 先删除再添加你的远程仓库

  ~~~
  git remote rm origin
  git remote add origin 你的新远程仓库地址
  ~~~

* 直接修改你本地的.git文件

![1567607984097](C:\Users\whichone\AppData\Roaming\Typora\typora-user-images\1567607984097.png)

进入.git文件编辑.git文件中的config文件修改config文件中的url路径为你的新远程仓库地址路径。

![1567608054369](C:\Users\whichone\AppData\Roaming\Typora\typora-user-images\1567608054369.png)

#### 上传本地文件到github

~~~
git init   //初始化后项目中多了一个隐藏文件夹.git
git add .  //将所有文件添加到仓库
git commit -m "注释" //提交文件
git remote add origin 远程仓库地址  //关联远程仓库
git pull --rebase origin master //代码合并
git push -u origin master  //上传本地代码
~~~

