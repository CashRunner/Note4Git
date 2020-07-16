# 4.10 Git 托管服务

创建GitHub项目git@github.com:CashRunner/Note4Git.git，以及本地仓库/d/doc/note4Git/Note4Git，将当前笔记放在本地仓库分支Buk_gitPro下。

在GitHub下创建仓库需要创建SSH秘钥，将公共秘钥复制到GitHub上，这样在本地可以通过git指令pull或者push分支到远程仓库。GitHub上邀请协作开发的功能如下

![image-20200716210204538](D:\doc\note4Git\Note4Git\note4GitPro.assets\image-20200716210204538.png)

## 将GitHub上的分支拉倒本地

在不同的电脑上将同一个仓库中的分支拉倒本地，如果使用`git clone `只有master分支。
`git clone [远程分支名] origin/[远程分支名]`在本地创建一个同名分支，追踪远程仓库的分支。使用`checkout`切换分支时可以看到分支上的文件被拉倒本地了。`https://blog.csdn.net/weixin_41287260/article/details/98987135`将远程仓库的分支都拉倒本地。

需要解决的问题，md文件使用相对路径，不同文件系统路径路径不同，无法加载图片。