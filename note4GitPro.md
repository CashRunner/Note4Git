# 1、入门

## 1、环境搭建

[下载git 的windows版本，](https://gitforwindows.org/)

## 配置文件

Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。 具体位置为git安装目录
- `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。 
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。使用git init命令的目录

在 Windows 系统上，Git 会找寻用户主目录下的 .gitconfig 文件。主目录即 $HOME 变量指定的目录，一般都是 C:\Users\USER。如果当前项目配置与用户级别配置冲突，以当前项目配置为准。

```shell
$ git config --global user.name "runoob"  # 配置用户名
$ git config --global user.email test@runoob.com  # 配置电子邮件
$ git config --global core.editor emacs # 配置默认文本编辑器
```

# Git 工作流程

![img](https://www.runoob.com/wp-content/uploads/2015/02/git-process.png)

# Git 工作区、暂存区和版本库

[回来再看一遍])(https://www.runoob.com/git/git-workspace-index-repo.html)

# git 常用命令

## 1、init

将指定目录初始化为仓库，默认为当前目录

## add

将文件添加进缓存区，常用命令格式如下

```shell 
git add . # 将当前目录下的所有文件，以及递归所有文件夹中的文件到缓存区
git add 文件名 # 将指定文件添加进缓存区
git add * # 同第一个
git status -s # 查看工作目录中哪些文件被追踪状态
```

这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“精确地将内容添加到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。

## 2、clone

```shell
git clone <repo> <directory> # 将指定仓库克隆到本地指定目录
git clone <repo> # 将指定仓库克隆到当前目录  
# 上述操作将复制该项目的全部记录，让你本地拥有这些。并且该操作将拷贝该项目的主分支， 使你能够查看代码，或编辑、修改。
```

git本身是版本管理软件，当需要将远程仓库克隆到本地，会涉及网络数据传输，为了保证数据传输安全git集成了SSH客户端，用于保证数据安全性。另外使用SSH方式传输不用每次都进行身份验证，并且速度稍快。

## git status

```shell
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

新添加的未跟踪文件前面有 `??` 标记，新添加到暂存区中的文件前面有 `A` 标记，修改过的文件前面有 `M` 标记。 输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。例如，上面的状态报告显示： `README` 文件在工作区已修改但尚未暂存，而 `lib/simplegit.rb` 文件已修改且已暂存。 `Rakefile` 文件已修，暂存后又作了修改，因此该文件的修改中既有已暂存的部分，又有未暂存的部分。

## git  diff

当前做的哪些更新尚未暂存？ 有哪些更新已暂存并准备好下次提交？ 虽然 `git status` 已经通过在相应栏下列出文件名的方式回答了这个问题，但 `git diff` 能通过文件补丁的格式更加具体地显示哪些行发生了改变。git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件，运行 `git diff` 后却什么也没有，就是这个原因。

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --staged` 命令。 这条命令将比对已暂存文件与最后一次提交的文件差异。

使用 `git difftool --tool-help` 命令来看你的系统支持哪些 Git Diff 插件。

## git commit

提交缓存区快照，可以在之后的任意时刻回溯到当前时间节点。可以使用`git -a -m "message" `一次性将跟踪的文件提交，不用每个文件使用add命令。

## git cached

不再跟踪文件

## git rm

add命令是将文件放入缓存区并且跟踪文件，rm是告诉git不要跟踪文件，并将文件移除缓存区并且删除文件。所以移动到缓存区就意味着是被git跟踪。如果在工作区单纯的删除文件，使用status命令会看到修改未提交的问题，所以git的版本控制有两个方面，一个是文件本身，另外一个是文件的修改记录。在第一个将文件add到缓存区时就git就开始跟踪该文件，记录该文件的状态，之后的在该文件上所有的操作都会被git跟踪，这样实现文件的版本管理。也就说明了工作区删除文件对于git来说只是增加了一个修改记录，并没有真正的删除，需要的话任然可以通过版本回溯恢复到原来的状态。

```shell 
git rm 文件名  # 不在追踪文件，并从工作区删除
git rm --cached 文件名 # 将文件从缓存区删除但是保留在工作区
```

需要注意反斜线

## git mv

给文件改名字。

## git log

查看提交历史

## git commit --amend

撤销刚才的提交，可以修改commit时候的语句。同时如果当缓存区有新的添加，会一并作为一次快照提交。

## git reset

如果文件在工作区修改了，并提交到缓存区，使用该命令将文件从缓存区中移除。但是该文件必须已经使用add命令添加到缓存区后并进行修改。

```shell
git reset HEAD filename 
git checkout -- filename # 文件在工作区的修改回溯到上一次add状态。危险，确定是否真的需要删除文件的修改操作，不可恢复。
```

# 2 常用命令

## 2.6 远程仓库

```shell
git remote # 查看远程仓库，显示远程仓库的别名
git remote -v #  查看远程仓库，显示远程仓库的别名同时显示远程仓库的URL
git remote add [远程仓库别名] [远程仓库地址] # 给远程仓库指定一个别名，仅仅如此，在本地仓库文件夹中不做更改
git fetch [远程仓库别名] # 到远程仓库中拉取所有你本地仓库中还没有的数据，运行完成后，你就可以在本地访问该远程仓库中的所有分支。fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并。
```

如果设置了某个分支用于跟踪某个远端仓库的分支（参见下节及第三章的内容），可以使用 `git pull` 命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支。在日常工作中我们经常这么用，既快且好。实际上，默认情况下 `git clone` 命令本质上就是自动创建了本地的 master 分支用于跟踪远程仓库中的 master 分支（假设远程仓库确实有 master 分支）。所以一般我们运行 `git pull`，目的都是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。

```shell
git push [remote-name] [branch-name] # 将本地仓库中的数据推送到远程仓库。
$ git push origin master  # 把本地的 master 分支推送到 origin 服务器上（再次说明下，克隆操作会自动使用默认的 master 和 origin 名字）
```

只有在所克隆的服务器上有写权限，或者同一时刻没有其他人在推数据，这条命令才会如期完成任务。如果在你推数据前，已经有其他人推送了若干更新，那你的推送操作就会被驳回。你必须先把他们的更新抓取到本地，合并到自己的项目中，然后才可以再次推送。

```shell
git remote show [remote-name]  # 查看传远程仓库的信息（很多信息）
git remote rename pb paul # 更多远程仓库在本地的命名
git remote rm paul # 碰到远端仓库服务器迁移，或者原来的克隆镜像不再使用，又或者某个参与者不再贡献代码，那么需要移除对应的远端仓库
```

## 2.6 标签

```shell
git tag # 列出现有标签
git tag -l [模式匹配字符串] # 用特定的搜索模式列出符合条件的标签
git tag -a [标签名字] -m '[标签注解]' # 添加带有注解的标签
```

轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。而含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 GNU Privacy Guard (GPG) 来签署或验证。一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签

```shell
git show # 查看所有标签信息
git show [标签名字] # 查看指定标签的信息
git tag -s [标签名字] -m '[标签注解]' # 用 GPG 来签署标签
git tag [标签名字] # 添加轻级标签，轻量级标签实际上就是一个保存着对应提交对象的校验和信息的文件。
git tag -v [tag-name] # 验证已经签署的标签，此命令会调用 GPG 来验证签名，所以你需要有签署者的公钥，存放在 keyring 中
git tag -a '标签名' [commit本地的某次版本号] # 给已经提交的版本大标签需
git log --pretty=oneline # 以简要信息显示commit的版本号和commit内容，结合上面的指令可以给提交的版本打标签。
```

个人理解，标签功能就是给版本添加一个容易记住的标记。标签在本地才有用，别人是看不到的。

```shell
git push origin [标签名] # 将本地标签提交的远程仓库
git push origin --tags # 一次推送所有本地新增的标签到远程仓库
```

## 2.7 小技巧···

```shell
git config --global	alias.co checkout # 给checkout命令去别名
git co # 相当于git checkout
$ git config --global alias.unstage 'reset HEAD --' # 取消暂存文件时的输入
$ git unstage fileA
$ git reset HEAD fileA  # 上面两个命令相同
```





# 3 Git分支

管理分支操作

```shell
git branch -v # 查看各个分支最后一个提交对象的信息
git branch --merged # 筛选出你已经与当前分支合并的分支
git branch --no-merged #筛选出你尚未与当前分支合并的分支
git branch -d [分支名字] # 删除分支，建议合并后再删除分支
git branch -D [分支名字] # 直接删除不管是否已经合并
```

## 远程分支

本地仓库有一个指向远程仓库分支的锚点，格式为远程仓库地址/分支名字。当第一次从远程仓库clone仓库到本地，git在本地创建origin作为远程仓库的别名，比如clone远程仓库的master分支，这样在本地仓库可以通过origin/master指向远程仓库master上的一个提交节点，无法使用本地commit方式向origin/master分支提交节点对象。远程服务器上的master分支可以继续往前移动，但是本地的origin/master分支不会移动，任然指向之前的节点。可以使用`git fetch origin`指令获取远程服务器master分支上与本地不用的部分。

git 支持多个远程仓库的分支同时开发

```shell
git remote add [远程分支在本地的别名] [远程仓库地址]
git push [远程仓库名] [分支名] # 将当前分支推送到远程仓库下的指定分支
git merge [远程仓库别名]/[远程分支名] # 在本地将指定的远程仓库分支合并到当前分支
```

在本地不能提交到远程分支，远程分支就是一个只读的指向远程仓库具体分支的一个只读快捷键。

git远程分支跟踪特性，在执行以下指令创建的分支会和远程分支建立联系。

```shell
git clone -b [远程仓库中分支名字] [远程仓库地址] # 克隆远程仓库指定分支，并同名，并关联
git checkout -b [本地分支名] [远程仓库别名/远程仓库中的分支] # 创建并关联远程分支
git checkout --track [远程仓库别名/远程仓库中的分支] # 从指定远程仓库分支创建本地分支并同名，并关联
```

关联分支后在本地分支上的push和pull会指向关联的远程仓库分支。

```shell
git push [远程仓库别名:远程仓库分支] # 删除远程仓库指定分支
```



## 分支的衍合

有两种方式将分支合并为一个分支，使用merge和rebase方式。merge会把两个分支最新的快照（C3 和 C4）以及二者最新的共同祖先（C2）进行三方合并，合并的结果是产生一个新的提交对象（C5）。rebase		在 C3 里产生的变化补丁在 C4 的基础上重新打一遍。如下图

​	![img](https://gitee.com/progit/figures/18333fig0329-tn.png)

rebase原理是回到两个分支最近的共同祖先，根据当前分支（也就是要进行衍合的分支 `experiment`）后续的历次提交对象（这里只有一个 C3），生成一系列文件补丁，然后以基底分支（也就是主干分支 `master`）最后一个提交对象（C4）为新的出发点，逐个应用之前准备好的补丁文件，最后会生成一个新的合并提交对象（C3'），从而改写 `experiment` 的提交历史，使它成为 `master` 分支的直接下游。执行的命令如下

```shell
git checkout testing 
git rebase master	# 先切换到testing分支然后将当前分支rebase到目标分支
```

使用rebase最终master分支如下图所示	

![img](https://gitee.com/progit/figures/18333fig0330-tn.png)

需要注意的是rebase指令涉及的两个分支是有最短路径公共父节点，如果将client合并到master分支如下图需要重新制定base指令如下

```shell
git rebase --onto master server client
```

结果如下图所示

![img](https://gitee.com/progit/figures/18333fig0332-tn.png)

需要注意的是master任然在原来的位置，需要切换到master然后fast  forward到client。

```shell
$ git checkout master
    $ git merge client
```

```shell
git rebase master server # 当前分支为master分支上执行该指令，将server分支rebase到当前分支，不用切换到server分支
```

# 服务器的git

## 协议

服务器上的仓库与本地计算进行数据传输可以使用4中协议：本地传输，SSH 协议，Git 协议和 HTTP 协议。除了 HTTP 协议外，其他所有协议都要求在服务器端安装并运行 Git。

本地传输协议，没搞懂，貌似与共享文件系统有关。在项目中推送时候控制台输出的命令似乎有这样的格式。

SSH协议，唯一一个同时支持读写操作的网络协议。另外两个网络协议（HTTP 和 Git）通常都是只读的，所以虽然二者对大多数人都可用，但执行写操作时还是需要 SSH。SSH 同时也是一个验证授权的网络协议；

```shell
git clone ssh://user@server/project.git # 通过 SSH 克隆一个 Git 仓库
git clone user@server:project.git # 不指明某个协议 — 这时 Git 会默认使用 SSH,如果不指明用户，Git 会默认使用当前登录的用户名连接服务器。
```

git协议相比SSH缺少授权和加密，速度快。通常用于只读的仓库。在服务器需要维护一个git协议线程用于数据传输。可以设置为仓库可写，这样所有人都可以写该仓库。

当前进度4-10

问题：git rebase --onto 指令三个参数的含义。

git rebase有风险。没搞懂这个风险表现在哪里？





[菜鸟教程-git](https://www.runoob.com/git/git-workflow.html)

[git参考手册](http://gitref.justjavac.com/basic/)

# 4.10 Git 托管服务

创建GitHub项目git@github.com:CashRunner/Note4Git.git，以及本地仓库/d/doc/note4Git/Note4Git，将当前笔记放在本地仓库分支Buk_gitPro下。

在GitHub下创建仓库需要创建SSH秘钥，将公共秘钥复制到GitHub上，这样在本地可以通过git指令pull或者push分支到远程仓库。GitHub上邀请协作开发的功能如下

![image-20200716210204538](D:\doc\note4Git\Note4Git\note4GitPro.assets\image-20200716210204538.png)

在github上添加协作者

![image-20200717224836644](E:\git\gitLearn\Note4Git\note4GitPro.assets\image-20200717224836644.png)

对于将来的项目，你可以从现有项目复制协作者名单，或者直接借用协作者群组。





## 将GitHub上的分支拉倒本地

在不同的电脑上将同一个仓库中的分支拉倒本地，如果使用`git clone `只有master分支。
`git clone [远程分支名] origin/[远程分支名]`在本地创建一个同名分支，追踪远程仓库的分支。使用`checkout`切换分支时可以看到分支上的文件被拉倒本地了。`https://blog.csdn.net/weixin_41287260/article/details/98987135`将远程仓库的分支都拉倒本地。

# 5  分布式Git

## 5.1 分布式工作流程

关于使用git如何让各个参与者协作。

![img](https://gitee.com/progit/figures/18333fig0501-tn.png)

### 集中式工作流

服务器上搭建项目，各个开发者拉去项目到本地修改，上传。其它开发者要上传必须修改版本冲突。适合小型开发。

### 集成管理员工作流

![img](https://gitee.com/progit/figures/18333fig0502-tn.png)

红色开发者维护唯一的blessed 版本，其它开发者将bless拉去到本地，进行开发，然后让manager将开发者的仓库添加为远程仓库，并拉取到本地进行合并，然后push到bless。贡献者推送数据到自己的公共仓库 developer public。贡献者给维护者发送邮件，请求拉取自己的最新修订。维护者在自己本地的 integration manger 仓库中，将贡献者的仓库加为远程仓库，合并更新并做测试。	

### 司令官与副官工作流

![img](https://gitee.com/progit/figures/18333fig0503-tn.png)

## 5.2 作为项目贡献者

关于参与项目的一些建议

提交规范:

![image-20200717231959448](E:\git\gitLearn\Note4Git\note4GitPro.assets\image-20200717231959448.png)

建议多次提交而不是多次改动一次提交。如果针对两个问题改动的是同一个文件，可以试试看 `git add --patch` 的方式将部分内容置入暂存区域。

## 私有小团队之间的合作

私有仓库下小团队之间的协作流程如下图：

 ![img](https://gitee.com/progit/figures/18333fig0511-tn.png)

各自在本都有远程仓库的副本并在本地独自开发，每次推送远程仓库时候需要在本地合并分支解决冲突。不同成员也是这样相同操作。

## 团队私有项目合作

上面的工作模式可以是每个人负责一个特性分支，如果在比较大的项目中，某个特性分支可以由多个人负责开发，将从事同一feature开发的成员为一个小组。值得注意的是在远程仓库上master分支是稳定的分支，由具有合并权限的管理员将不同，这样做的理由是让特性开发的小组分支与稳定分支相互隔离互补影响。远程仓库上同时维护者多个特性分支由小组成员维护，开发过程与上面的开发方式相当。大致开发流程如下：

![img](https://gitee.com/progit/figures/18333fig0515-tn.png)

与上面的区别在于特性分支与master合并由专员管理，小组成员没有在仓库合并分支的权限。

### 公开的小型项目 

看鸡巴不懂，没几把说清楚。

# 6 Git 小技巧

查看指定commit的提交的内容

```shell
git log # 查看commit历史，显示每次commit是的message和修改内容
git log --abbrev-commit pretty=oneline #n 唯一SHA码 commit历史查看
git show [SHA1码] # 显示指定某次提交
git show master # 显示master分支最近一次提交，提交的内容和git show [master指向提交的SHA1码]效果相同
git log --pretty=format:'%h %s' --graph # 图形化查看提交历史
git reflog # 查看历史HEAD指针指向分支的提交
git show HEAD@{num} # 查看从当前HEAD开始向之前回溯第num个版本的提交内容。
git show HEAD^ # 当前分支当前提交的前一个提交，注意是当前分支。会忽略其他分支与当前分支的合并提交历史。区别 git reflog指令，该指令显示HEAD曾今到过的分支，以及在上面的提交。	
git show master^ # master分支上一次提交
git show master^2 # master 分支上两次提交
```

当前进度 提交范围 感觉后面没啥必要看的了。







# 需要解决的问题：

需要解决的问题，md文件使用相对路径，不同文件系统路径路径不同，无法加载图片。

github上的协作者群组是怎么回事？如何操作？

百度打车多少钱。平摊

# 当前进度 公开的小型项目r·