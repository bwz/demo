#Git 学习方法
1. 基本使用方法
2. gitHub 或 gitLab的基本使用方法
3. git 与 gitLab（gitHub）在场景中的使用节点
##难点理解
###基础命令
1. git 仓库与目录关系 
2. git **操作** 与 **分区（工作区→暂存区→仓库）** 关系
3. git 切换**分支**的方式与特点
4. git **HEAD** 与 **分支** 与 **分区** 的特点
5. git 各分支的特点与生产代码过程中的意义
6. git 各分支的共有祖父节点
7. git 合并分支的意义

###如何正确使用 git 版本控制
正确的版本控制系统的使用方法是，一次提交只干一件事：完成一个新功能、修改了一个Bug、或是写完了一节的内容、或是添加了一幅图片，就执行一次提交。而不要在下班时才想起来要提交，那样的话版本控制系统就被降格为文件备份系统了。

但有时在同一个工作区中可能同时在做两件事情，一个是尚未完成的新功能，另外一个是解决刚刚发现的Bug。很多版本控制系统没有提交列表的概念，或者要在命令行指定要提交的文件，或者默认把所有修改内容全部提交，破坏了一个提交干一件事的原则。

###gitHub 或 gitLab
gitHub（gitLab）的意义所在
###git 与 gitLab(gitHub)在不同角色的应用场景
运维对gitLab（gitHub）注重的地方

开发对gitLab（gitHub）注重的地方 

***

#Git 权威指南第二版 学习笔记
## 爱上 Git 的理由
###每日的工作备份
习惯：一般完成一节内容，则执行下面的命令提交一次：

	git add -u #提交更新的文件
	git commit
	git push #下班后，执行一次推送操作，将今天的保存推送到 Git 服务器上

##1.Git初始化 2018/6/8 12:24:43
###创建版本及第一次提交
版本查看：`git --version` 

设置使用人：`git config --global user.name "lbc"` 

设置邮件： `git config --global user.email "eyofx@163.com"`
 
设置颜色： `git config --global color.ui true`

设置别名：`git config --system alias.ci "commit -s"` ; `git config --system alias.co checkout`

初始化版本库：`git init`

1. 创建一个新的工作目录： `cd /path/workspace`
2. 创建需要对该目录进行版本管理：`mkdir demo`
3. `cd demo`
4. `git init`
5. 初始化空的 Git 版本库 /path/workspace/demo/.git，目录 .git 就是 Git 的版本库（叫仓库,repository）。

为工作区/path/workspace/demo 新增文件readme.txt 及向 git **添加文件**及**提交**

新增文件：`echo "this is test" > readme.txt`

添加文件：`git add readme.txt`

提交文件：`git commit -m "init"`

###思考 工作区下有一个 .git的目录
Git 的这种设计，将版本库放在工作区根目录下，所有的版本控制操作都在本地完成。在 git 工作区目录下执行操作时候，会对目录依次递归向上查找 **.git** 目录，有**.git**目录的就是工作区对应的版本库，也是工作区的根目录。

提供对文件内容搜索：`git grep "this is test"`

显示**.git**所在的目录：`git rev-parse --git-dir`

显示工作区的根目录：`git rev-parse --show-toplevel`

显示当前相对于工作区相对的路径:`git rev-parse --show-prefix`

显示从当前目录到工作区根的深度：`git rev-parse --show-cdup`

###思考 `git config` 命令参数区别
版本库配置文件：`git config -e`

全局配置文件（用户主目录下）：`git config -e --global`

系统配置文件（/etc下）：`git config -e --system`

使用 git config <section>.<key> <value> 对上述的文件的选项进行取值或赋值

	[root@www01 Data]# cat .git/config
	[core]
	repositoryformatversion = 0
   	filemode = true
   	bare = false
  	logallrefupdates = true
	[root@www01 Data]# git config  core.bare
	false
	[root@www01 Data]# git config  core.bare true
	[root@www01 Data]# cat .git/config  
	[core]
	repositoryformatversion = 0
	filemode = true
	bare = true
	logallrefupdates = true
	[root@www01 Data]# git config  core.bare false
	[root@www01 Data]# git config  core.bare 
	false
	[root@www01 Data]#

	[root@www01 Data]# git config --global color.ui
	true
	[root@www01 Data]# 

向某配置文件ini中添加配置或读取配置（利用这一技术读取git-svn专有配置文件）

	[root@www01 Data]# ls
	a  readme.txt
	[root@www01 Data]# GIT_CONFIG=test.ini git config a.b.c.d "hello,world"
	[root@www01 Data]# GIT_CONFIG=test.ini git config a.b.c.d 
	hello,world
	[root@www01 Data]# GIT_CONFIG=test.ini git config e.f "hello,girl"     
	[root@www01 Data]# GIT_CONFIG=test.ini git config e.f 
	hello,girl
	[root@www01 Data]# cat test.ini 
	[a "b.c"]
        d = hello,world
	[e]
	    f = hello,girl
	[root@www01 Data]# ls
	a  readme.txt  test.ini
	[root@www01 Data]# 

###思考 是谁完成提交