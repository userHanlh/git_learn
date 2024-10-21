

## 一 什么是Git

Git是一个分布式版本控制系统工具，主要用于管理开发过程中的源代码文件。

### 1 Git的作用

代码回溯，版本切换，多人协作，远程备份

### 2 Git的相关概念

- 版本库：.git隐藏文件夹就是版本库，存储了许多配置信息，文件版本信息，日志信息等
- 工作区：包含.git文件夹的目录就是工作区，主要用于存放开发的代码
- 暂存区：.git文件夹中有很多文件，有一个index文件就是暂存区，也可以叫做stage。暂存区是一个临时保存修改文件的地方

### 3 Git工作区文件状态

1. untracked 未跟踪（未被纳入版本控制，如工作区新添加了一个目录，但没有添加到暂存区）
2. tracked 已跟踪（被纳入版本控制，之前已加入到暂存区）	

- ​	Unmodified 未修改状态
- ​	Modified 已修改状态（已经提交的文件修改后但未为添加到暂存区）
- ​	Staged 已暂存状态	（已添加到暂存区）

**检查当前仓库状态**：`git status`

## 二 用 SSH  进行与 GitHub 的通信，配置 SSH 步骤：

确保你已安装 Git，并打开终端（Linux/macOS）或 Git Bash（Windows）

### 1 **生成 SSH 密钥**（如果你还没有的话）：

```
ssh-keygen -t rsa -b 4096 -C "GitHub注册的邮箱地址"
```

这将生成一个 SSH 密钥对，并提示你保存到 `~/.ssh/id_rsa`（默认位置）。

### 2 **将 SSH 公钥添加到 GitHub**：

- 打开 `/.ssh/id_rsa.pub`，复制里面的内容。（或者执行命令`cat ~/.ssh/id_rsa.pub`复制输出的内容）
- 登录 GitHub，进入 [SSH and GPG keys 页面](https://github.com/settings/keys)。
- 添加新的 SSH 密钥，并将复制的公钥粘贴进去。

### 3 **测试连接**：

​    如果你是第一次使用 SSH 连接到 GitHub，系统可能会询问你是否要继续连接，输入 `yes` 并回车

```
ssh -T git@github.com
```



## 三 使用git上传github项目

###  1 切换到项目目录

**在任意目录点击鼠标右键，通过Git Bash Here打开Git命令行（常用）**

或者 在 Git Bash 中，切换到某一目录。路径应使用正斜杠。例如，要进入 `D:\python-learn\basiclearn`，可以使用以下命令：

```
cd /d/python-learn/basiclearn
```

- `pwd`可查看当前目录

###  2 获取git仓库

**从远程仓库克隆，要么克隆一个空仓库要么克隆一个已有仓库（常用）（相当于既建立了一个仓库又连接了远程仓库）**

或者在本地初始化一个Git仓库，如果采用这种方式还需要添加远程仓库

```
git init
```

```
git remote add origin git@github.com:userHanlh/11111-.git
```

- 查看当前设置的远程仓库`git remote -v`

- 如果需要删除远程仓库`git remote remove origin`

###  3  添加文件到暂存区并提交到版本库

使用 `git add .` 命令时，Git 会递归地扫描当前目录及其所有子目录中的所有文件，并将它们的更改添加到暂存区，`git add <file>`, 是工作区下的文件名

`git commit -m " "`将暂存区的文件提交到版本库。m的意思是message

```
git add .
git commit -m "first commit"
```

**查看提交的历史记录**：`git log`

```
$ git log
commit ae8b1c0f5f8a8a74cd7aed54f1bebb7428f51ae3 (HEAD -> main)
Author: userHanlh <hanl_h@163.com>
Date:   Mon Oct 21 19:09:18 2024 +0800

    提交3.txt文件到版本库

commit c76a7c06bada7b48c5b5c0231a0dbffe0d4654ee
Author: userHanlh <hanl_h@163.com>
Date:   Mon Oct 21 19:08:19 2024 +0800

    添加了1.txt和2.txt文件到版本库

```

### 4 取消暂存或切换到指定版本

git reset 命令的作用是将暂存区的文件取消暂存或者切换到指定版本

1 git reset filename 将暂存区的文件取消暂存

```
hanlonghui@LAPTOP-8ORHBDL2 MINGW64 /d/documents_learning/learn/1212 (main)
$ git add 4.txt

hanlonghui@LAPTOP-8ORHBDL2 MINGW64 /d/documents_learning/learn/1212 (main)
$ git reset 4.txt

```

2 git reset --【hard,soft,mixed】 版本号 切换到指定版本

如果为hard，则stage区和工作目录的内容会完全重置到指定版本对应状态

如果为soft，会保留工作目录和暂存区的内容，并将重置HEAD所带来的新的差异放进暂存区

```
hanlonghui@LAPTOP-8ORHBDL2 MINGW64 /d/documents_learning/learn/1212 (main)
$ git reset --soft c76a7c06bada7b48c5b5c0231a0dbffe0d4654ee

hanlonghui@LAPTOP-8ORHBDL2 MINGW64 /d/documents_learning/learn/1212 (main)
$ git status
On branch main
Your branch is based on 'origin/main', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   3.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        4.txt

```

如果为mixed，会保留工作目录，并且清空缓存区，工作目录的修改，暂存区的修改以及reset导致的新的文件差异都会放进工作目录



###  5 推送代码到远程仓库

git push [remote-name] [branch-name]

```
git push origin main 
```

### 6 拉取远程仓库最新版本合并到本地仓库

git pull [remote-name] [branch-name]



## 四 分支操作

分支是Git使用过程中非常重要的概念，使用分支可以把你的工作从开发主线分离出来，以免影响开发主线。例如当某一版本出现bug时，可在此版本的基础上开一个分支来解决该bug，原主线继续进行，到合适的为止在将该分支合并到主线。

同一个仓库可以有多个分支，各个分支相互独立互不干扰。

git branch查看当前分支

git branch[name] 创建分支

git checkout [name] 切换到name分支

git push [remote-name] [branch-name] 推送至远程仓库分支

git merge [name] 合并分支

git branch -d [name] 删除分支

## 五 标签操作

Git 中的标签，指的是某个分支某个特定时间点的状态。通过标签可以很方便的切换到标记时的状态。

git tag 列出已有的标签

git tag [name]创建标签

git push [shortName] [name] 将标签推送到远程仓库

git checkout -b [branch] [name]检出标签