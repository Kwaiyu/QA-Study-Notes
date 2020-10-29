### 安装

**Linux**

```
$ git
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入：./config，make，sudo make install
```

**MacOS**

1. 安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：http://brew.sh/。
2. 从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

**Windows**

Git官网直接[下载安装程序](https://git-scm.com/downloads)

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

### 版本库

```
$ mkdir repository
$ cd repository
$ pwd
# 目录不能包含中文
$ git init
# 编写文件README.MD,添加到暂存区
$ git add README.MD
# 把文件提交到仓库
$ git commit -m "wrote a readme file"
```

### 修改状态

```
# 修改文件查看状态
$ git status
# 查看修改内容
$ git diff
```

### 版本回退

```
# 查看从最近到最远的提交日志
$ git log
# 只查版本号日志，HEAD表示当前版本，HEAD^上一版，HEAD^^上上版，HEAD~100
$ git log --pretty=oneline
# 回退上个版本
$ git reset --hard HEAD^
# 记录每一次命令
$ git reflog
```

### 工作区和暂存区



### git gui

**GitKraken**

### 命令脑图

![img](https://cdn.jsdelivr.net/gh/Kwaiyu/SQA-Study-Notes@master/docs/_media/git.png)