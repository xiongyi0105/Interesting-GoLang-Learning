# Git 初始化仓库并做最简单的配置

1. git init  - 本地初始化一个空的仓库

2. git config --global user.name ""  - 配置全局的用户名和email 

   （xiongyi0105）

3. git config --global user.email ""（1019487236@qq.com）

4. git config --global --list - 看配置信息

   >在git中，我们使用git config 命令用来配置git的配置文件，git配置级别主要有以下3类：
   >1、仓库级别 local 【优先级最高】
   >2、用户级别 global【优先级次之】
   >3、系统级别 system【优先级最低】

# 完成一个最简单的Git操作流程

1. git status 
>On branch master
>No commits yet
>Untracked files:
>(use "git add <file>..." to include in what will be committed)
>   .idea/
>   go.mod
>nothing added to commit but untracked files present (use "git add" to track)

2. git add ...(git add .表示添加所有文件)
3. git commit -m "注释"
4. git log 显示提交历史

>git add前git仓库里的文件未被跟踪，属于工作区
>
>在git add后文件被git跟踪后便属于了暂存区
>
>git commit 后文件便被提交到了git仓库。-m后跟此次提交的注释

🎲<!--！！以上都是在本地git仓库-->

# 将本地仓库同步到远程GitHub仓库

1. git remote add origin git@github.com:xiongyi0105/Interesting-GoLang-Learning.git

   意思是给本地仓库添加一个远程仓库，origin远程仓库的一个代号，也可以自己取名字哦

   <!--在此之前需要先添加ssh公钥到github上就可以每次ssh免密拉代码，配置ssh的方式详情见-->

   [github 配置ssh]: https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

   

2. ssh -T git@github.com 验证ssh的可用性

   

3. git push -u origin master(or main) 推送到远端

# 扩展及工作中遇到的

> git 拉取远程分支到本地
>
> 步骤：
>
> 1,   git init  (git安装后初次使用需要git init)
>
> 2，git clone git@git.n.xxx.com:xxx/xxx.git
>
> ​     （git@git.n.xxx.com:xxx/xxx.git这个地址在gitlab上找）
>
> ​    根据提示输入git账户用户名
>
> ​    根据提示输入密码
>
> 3,   cd 进入clone下来的工程的主目录
>
> 4，git branch -a                           查看所有分支名称
>
> 5，git fetch origin dev      把远程dev分支拉到远程的当前（这里的dev是远程分支名，假设存在。）
>
> 6，git checkout -b dev origin/dev    在本地创建分支dev关联远程的dev分支，同时作为本地的当前分支（此时代码已经拉下来了）
>
> 7，git pull origin dev                    把远程dev分支上的内容都拉取到本地
>
> 8，git switch 切换分支
>
> 注意：(在git clone之后，git branch -a之前如果有新增分支，要git pull一下才能获取新分支,查看到新分支)
>
> ————————————————
>
> 版权声明：本文为CSDN博主「乐乐Gold」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
>
> 原文链接：https://blog.csdn.net/leminfei/article/details/108850792

1.本地创建分支 ： git checkout -b xxx

2.本地切换分支： git switch xxx

3.远端还不存在分支创建一个新分支： git fetch origin xxx

4.远端创建新分支后，本地若要push代码时遇到 

> λ git push origin dev -u
> To github.com:xiongyi0105/Interesting-GoLang-Learning.git
>  ! [rejected]        dev -> dev (fetch first)
> error: failed to push some refs to 'github.com:xiongyi0105/Interesting-GoLang-Learning.git'

是遇到了冲突

> 问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：
>
> 1，强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容
>
> git push -f
>
> 2，先把git的东西fetch到你本地然后merge后再push
>
> $ git fetch
>
> $ git merge
>
> 这2句命令等价于
>
> ```python
> $ git pull  
> ```
>
> 可是，这时候又出现了如下的问题：
>
> ![img](https://img-blog.csdn.net/20170216141449767)
>
> 上面出现的 [branch "master"]是需要明确(.git/config)如下的内容
> [branch "master"]
>   remote = origin
>
>   merge = refs/heads/master
>
> 这等于告诉git2件事:
>
> 1，当你处于master branch, 默认的remote就是origin。
>
> 2，当你在master branch上使用git pull时，没有指定remote和branch，那么git就会采用默认的remote（也就是origin）来merge在master branch上所有的改变
>
> 如果不想或者不会编辑config文件的话，可以在bush上输入如下命令行：
>
> ```python
> $ git config branch.master.remote origin  
> 
> 
> 
> $ git config branch.master.merge refs/heads/master  
> ```
>
> 之后再重新git pull下。最后git push你的代码吧。it works now~
>
> 原文链接：https://blog.csdn.net/sinat_25306771/article/details/55257901

