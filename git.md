# Git 版本控制

[BiliBili 课程链接](https://www.bilibili.com/video/BV1vy4y1s7k6?p=1&vd_source=8f3e3e3380e76ee)

## 0. 笔记大纲

[git 官网](https://git-scm.com/) 		|[git 下载](https://git-scm.com/downloads)		|[git 文档](https://git-scm.com/docs)		|[git 官方书籍 《Pro Git》](https://git-scm.com/book/zh/v2)

- Git 常用命令
- Git基本操作与使用
  - Git 介绍  分布式版本控制工具 & 集中式版本控制工具
  - Git 安装  基于官网发布的最新版本2.31.1安装讲解
  - Git 命令  基于开发案例详细讲解了git的常用命令
  - Git 分支  分支特性 分支创建 分支转换 分支合并 代码合并冲突解决
- Github
  - 创建远程库
  - 代码推送 Push
  - 代码拉取 Pull
  - 代码克隆 Clone
  - SSH 免密登录
  - IDEA 集成 Github
- Gitee码云
  - 码云创建远程库
  - Idea集成Gitee码云
  - 码云连接GitHub进行代码的复制和迁移
- GitLab
  - GitLab服务器的搭建和部署
  - Idea集成GitLab


## 1. Git
### 1.1 Git
Git 是一个免费的、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的各种项目。
Git 易于学习，占地面积小，性能极快。 它具有廉价的本地库，方便的暂存区域和多个工作流分支等特性。 其性能优于 `Subversion`、`CVS`、 `Perforce` 和 `ClearCase` 等版本控制工具。

### 1.2 版本控制介绍
**版本控制**是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。
**版本控制**其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，方便版本切换。
通过使用版本控制可以从个人开发过渡到团队合作。

### 1.3 版本控制工具
#### 1.3.1 集中式版本控制工具
集中化的版本控制系统诸如`CVS`、 `SVN` 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。

这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而管理员也可以轻松掌控每个开发者的权限，并且管理一个集中化的版本控制系统， 要远比在各个客户端上维护本地数据库来得轻松容易。

事分两面，有好有坏。这么做显而易见的缺点是中央服务器的单点故障。如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。

svn因为每次存的都是差异 需要的硬盘空间会相对的小一点  可是回滚的速度会很慢

- 优点: 

代码存放在单一的服务器上 便于项目的管理

- 缺点: 

服务器宕机: 员工写的代码得不到保障

服务器炸了: 整个项目的历史记录都会丢失

![img](.\gitmd_img\f0cc7f6f1eb57316f7f8fce8e2f03f45.png)


#### 1.3.2 分布式版本控制工具
工具： `Git`、 `Mercurial`、 `Bazaar`、 `Darcs`等等
像 Git 这种分布式版本控制工具，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用其他客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。
分布式的版本控制系统出现之后,解决了集中式版本控制系统的缺陷：

1. 服务器断网的情况下也可以进行开发（因为版本控制是在本地进行的）
2. 每个客户端保存的也都是整个完整的项目（包含历史记录， 更加安全）

![img](.\gitmd_img\340e9b3bc50013b61f0e83e96b99f86a.png)

### 1.4 发展历史

![img](.\gitmd_img\3f25cba01665ab8dcb63fcc79d05f04f.png)

## 2. Git 常用命令

### 2.1 底层命令

- git对象
  - `git hash-object -w <fileUrl>` :  生成一个key(hash值):val(压缩后的文件内容)键值对存到.git/objects

- tree对象
  - `git update-index --add --cacheinfo 100644 hash test.txt `: 往暂存区添加一条记录(让git对象 对应 上文件名)存到.git/index
  - `git write-tree` : 生成树对象存到.git/objects

- commit对象
  - `echo 'first commit' | git commit-tree treehash` : 生成一个提交对象存到.git/objects

- 对以上对象的查询
  - `git cat-file -p hash`    : 拿对应对象的内容
  - `git cat-file -t hash`    : 拿对应对象的类型

- `git ls-files -s`  查看暂存区

### 2.2 高层命令

- 安装 `git --version`
- 初始化配置
	- `git config --global user.name "xxx"`
	- `git config --global user.email xxx@example.com  `  
	- `git config --list`
- 初始化仓库 `git init`
- **C(新增)**
	- 在工作目录中新增文件
	- `git status`
	- `git add ./`
	- `git commit -m "xxx" `   
- **U(修改)**
	- 在工作目录中修改文件
	- `git status`
	- `git add ./`
	- `git commit -m "xxx"`     
- **D(删除 & 重命名)**
	- `git rm 要删除的文件`  /  `git mv 老文件 新文件`
	- `git  status` 
	- `git commit -m "xxx`" 
- **R(查询)**
	- `git  status`   :  查看工作目录中文件的状态(已跟踪(已提交 已暂存 已修改) 未跟踪)
	- `git  diff`     :  查看未暂存的修改
	- `git  diff --cache` : 查看未提交的暂存
	- `git  log --oneline` : 查看提交记录（缩略版）

### 2.3 Git 分支命令

分支的本质其实就是一个提交对象!!!
HEAD: 
1. 是一个指针 它默认指向master分支 切换分支时其实就是让HEAD指向不同的分支
2. 每次有新的提交时 HEAD都会带着当前指向的分支 一起往前移动



#### `git branch`
- `git  log --oneline --decorate --graph --all` : 查看整个项目的分支图  
- `git branch` : 查看分支列表
- `git branch -v`: 查看分支指向的最新的提交
- `git branch <branchname>` : 在当前提交对象上创建新的分支
- `git branch <branchname> commit <hash>`: 在指定的提交对象上创建新的分支
- `git branch -d <branchname>` : 删除空的分支 删除已经被合并的分支
- `git branch -D <branchname>` : 强制删除分支 

#### `git merge`
- `git merge <branchname>`  合并分支
	- 快进合并 --> 不会产生冲突
	- 典型合并 --> 有机会产生冲突
	- 解决冲突 --> 打开冲突的文件 进行修改 `add commit` 

- `git branch --merged` 查看合并到当前分支的分支列表: 
	- 一旦出现在这个列表中 就应该删除
- `git branch --no-merged`  查看没有合并到当前分支的分支列表
	- 一旦出现在这个列表中 就应该观察一下是否需要合并

- git分支的注意点:在切换的时候 一定要保证当前分支是干净的
- 允许切换分支: 
	- 分支上所有的内容处于 已提交状态    
	- (避免)分支上的内容是初始化创建 处于未跟踪状态
	- (避免)分支上的内容是初始化创建 第一次处于已暂存状态
- 不允许切分支:
	- 分支上所有的内容处于 已修改状态  或 第二次以后的已暂存状态  

#### `git checkout`
- `git checkout <branchname>` :     切换分支
- `git checkout -b <branchnam>e` 创建&切换分支


- `git  checkout <branchname>`  跟   `git reset --hard commit <hash>`特别像
- 共同点
	- 都需要重置 HEAD   暂存区   工作目录
- 区别
	- checkout对工作目录是安全的    reset --hard是强制覆盖
	- checkout动HEAD时不会带着分支走而是切换分支
	- reset --hard时是带着分支走
  
- checkout + 路径
	- `git checkout commit <hash> <filename> `还原某个特定的文件到之前的版本
	- `git checkout --<filename>` 把filename文件在工作区的修改撤销到最近一次git add 或 git commit时的内容

#### `git stash`
- 在分支上的工作做到一半时 如果有切换分支的需求, 我们应该将现有的工作存储起来
- `git stash` : 会将当前分支上的工作推到一个栈中(分支切换  进行其他工作 完成其他工作后 切回原分支)
	- `git stash apply `: 将栈顶的工作内容还原 但不让任何内容出栈 
	- `git stash drop`  : 取出栈顶的工作内容后 就应该将其删除(出栈)
	- `git stash pop` :      git stash apply +  git stash drop 
	- `git stash lis`t : 查看存储

#### `git reset`
- `git reset --soft commit <hash>`     用commithash的内容重置HEAD内容
- `git reset [--mixed] commit <hash>`   用commithash的内容重置HEAD内容 重置暂存区
- `git reset --hard commit <hash>`     用commithash的内容重置HEAD内容 重置暂存区 重置工作目录

所有的路径reset都要省略第一步!!!
第一步是重置HEAD内容  我们知道HEAD本质指向一个分支 分支的本质是一个提交对象 
提交对象 指向一个树对象 树对象又很有可能指向多个git对象 一个git对象代表一个文件!!!
HEAD可以代表一系列文件的状态!

####  后悔药们
- `git checkout --filename`  撤销工作目录的修改
-  ` git reset HEAD  filename`   撤销暂存区的修改
- `git commit --amend`   撤销提交

## 3. Git 基本操作及基本命令使用

### 3.0 Git 工作顺序
```mermaid
flowchart LR

A[工作区-写代码] -->|git add| B(暂存区-临时存储)
B[暂存区-临时存储] -->|git commit| C(本地库-生成历史版本)
```
#### 代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为**远程库**。

- 局域网
  - GitLab
- 互联网
  - GitHub（外网）
  - Gitee 码云（国内网站）

|               命令名称               |       作用        |
| :----------------------------------: | :---------------: |
| git config --global user.name 用户名 |   设置用户签名    |
| git config --global user.email 邮箱  | 设置用户email地址 |
|               git init               |   初始化本地库    |
|              git status              |  查看本地库状态   |
|            git add 文件名            |   添加到暂存区    |
|   git commit -m “日志信息” 文件名    |   提交到本地库    |
|              git reflog              |   查看历史记录    |
|       git reset --hard 版本号        |     版本穿梭      |

### 3.1 设置用户签名

```bash
git config --global user.name fufu
git config --global user.email liusu1889@foxmail.com
```

说明：**签名的作用是区分不同操作者身份**。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。 **Git 首次安装必须设置一下用户签名，否则无法提交代码**。

**注意**： 这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。

```bash
44845@Tiny MINGW64 ~/Desktop
$ cat ~/.gitconfig
[filter "lfs"]
        required = true
        clean = git-lfs clean -- %f
        smudge = git-lfs smudge -- %f
        process = git-lfs filter-process
[user]
        name = fufu
        email = liusu1889@foxmail.com
```

### 3.2 初始化本地库

语法：`git init`

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo
$ git init
Initialized empty Git repository in E:/study/cs/github/git-space/git-demo/.git/

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ ll
total 0

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ ll -a
total 4
drwxr-xr-x 1 44845 197609 0 Feb 21 18:14 ./
drwxr-xr-x 1 44845 197609 0 Feb 21 18:13 ../
drwxr-xr-x 1 44845 197609 0 Feb 21 18:14 .git/

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ cd .git

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo/.git (GIT_DIR!)
$ ll -a
total 11
drwxr-xr-x 1 44845 197609   0 Feb 21 18:14 ./
drwxr-xr-x 1 44845 197609   0 Feb 21 18:14 ../
-rw-r--r-- 1 44845 197609  23 Feb 21 18:14 HEAD
-rw-r--r-- 1 44845 197609 130 Feb 21 18:14 config
-rw-r--r-- 1 44845 197609  73 Feb 21 18:14 description
drwxr-xr-x 1 44845 197609   0 Feb 21 18:14 hooks/
drwxr-xr-x 1 44845 197609   0 Feb 21 18:14 info/
drwxr-xr-x 1 44845 197609   0 Feb 21 18:14 objects/
drwxr-xr-x 1 44845 197609   0 Feb 21 18:14 refs/

```

### 3.3 查看本地库状态

基本语法：`git status`

#### 3.3.1 首次查看（工作区没有任何文件）

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

#### 3.3.2 新增文件

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ cat hello.txt
Hello its my first time to study Git!
```

#### 3.3.3 再次查看状态（检测到了未追踪文件）

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.txt
```

### 3.4 将文件添加进缓存区

基本语法：`git add 文件名`

#### 3.4.1 将工作的文件添加至缓存区

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git add hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
```

#### 3.4.2 查看状态（检测到暂存区有新文件）

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt
```

#### 3.4.3 使用`git rm`移除文件后再查看缓存区

``` bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git rm --cached hello.txt
rm 'hello.txt'

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```

### 3.5 文件提交本地库

基本语法：`git commit -m "日志信息" 文件名`

#### 3.5.1 将暂存区的文件提交到本地库

``` bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git commit -m "first commit" hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
[master (root-commit) 71f7e16] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

### 3.5.2 查看状态（没有文件需要提交）

````bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git status
On branch master
nothing to commit, working tree clean
````

### 3.5.3 查看版本号和日志

查看版本信息：`git reflog`

查看日志：`git log`

- `git log`命令可以显示所有提交过的版本信息，如果感觉内容太繁琐可以加上参数`--pretty=oneline`，只显示版本号和提交时的备注
- `git reflog`可以查看所有分支的所有操作记录（包括被删除的commit记录和reset操作），可以看到被删除的commit并恢复到被删除的版本。

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git reflog
71f7e16 (HEAD -> master) HEAD@{0}: commit (initial): first commit

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git log
commit 71f7e16b730d8e72b0714b781bf3d355baacbf11 (HEAD -> master)
Author: fufu <liusu1889@foxmail.com>
Date:   Tue Feb 21 19:09:21 2023 +0800

    first commit
```

### 3.6 修改文件

修改后使用`git status`查看状态，可以看到`modified:   hello.txt`文件状态被修改。可以使用`git add`再次将修改后的文件提交进缓存区，使用`git commit -m`提交版本，即可提交代码并生成版本。

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git commit -m "second commit" hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
[master e386332] second commit
 1 file changed, 1 insertion(+), 1 deletion(-)
 
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git reflog
e386332 (HEAD -> master) HEAD@{0}: commit: second commit
71f7e16 HEAD@{1}: commit (initial): first commit
```

### 3.7 历史版本&版本穿梭

#### 3.7.1 查看历史版本

查看版本信息 `git reflog` 

查看版本详细信息 `git log`

`git log` 命令可以显示所有提交过的版本信息，如果感觉太繁琐，可以加上参数 `--pretty=oneline`，只会显示版本号和提交时的备注信息。

`git reflog` 可以查看所有分支的所有操作记录（包括已经被删除的 `commit` 记录和 `reset` 的操作）。例如，执行 `git reset --hard HEAD~1`，退回到上一个版本，用`git log`则是看不出来被删除的`commitid`，用`git reflog`则可以看到被删除的`commitid`，我们就可以买后悔药，恢复到被删除的那个版本。

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git log
commit e386332119c8ed2e41d81dc33ee366034efa1d58 (HEAD -> master)
Author: fufu <liusu1889@foxmail.com>
Date:   Tue Feb 21 19:15:10 2023 +0800

    second commit

commit 71f7e16b730d8e72b0714b781bf3d355baacbf11
Author: fufu <liusu1889@foxmail.com>
Date:   Tue Feb 21 19:09:21 2023 +0800

    first commit
```

#### 3.7.2 版本穿梭

基本语法：`git reset --hard 版本号`

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git reset --hard 71f7e16
HEAD is now at 71f7e16 first commit
```

可以往前穿也可以往后穿，可以在 `~/.git/refs/heads/master` 里看到master文件指向我们指定的版本号（指针）

**Git 切换版本， 底层其实是移动的 HEAD 指针。**

## 4. Git 分支操作

![img](.\gitmd_img\bcad650a512a72097b3391e00ecb8bbe.png)

### 3.1 什么是分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来， 开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）

![img](.\gitmd_img\f1d0659ed000e9dfa295fc696a58cf74.png)

#### 分支的好处

同时并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

### 3.2 分支的操作

|      命令名称       |             作用             |
| :-----------------: | :--------------------------: |
|  git branch 分支名  |           创建分支           |
|    git branch -v    |           查看分支           |
| git checkout 分支名 |           切换分支           |
|  git merge 分支名   | 把指定的分支合并到当前分支上 |

#### 3.2.1 查看分支

基本语法：`git branch -v`

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git branch -v
* master 71f7e16 first commit
```

#### 3.2.2 创建分支

基本语法：`git branch 分支名`

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git branch hot-fix

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git branch -v
  hot-fix 71f7e16 first commit
* master  71f7e16 first commit
```

#### 3.2.3 切换分支

基本语法：`git checkout 分支名`

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git checkout hot-fix
Switched to branch 'hot-fix'

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (hot-fix)
$ git branch -v
* hot-fix 71f7e16 first commit
  master  71f7e16 first commit
```

#### 3.2.3 修改分支的文件

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (hot-fix)
$ vim hello.txt

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (hot-fix)
$ git status
On branch hot-fix
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (hot-fix)
$ git commit -m "hot-fix commit" hello.txt
[hot-fix abde1c2] hot-fix commit
 1 file changed, 1 insertion(+), 1 deletion(-)
 
 44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (hot-fix)
$ git reflog
abde1c2 (HEAD -> hot-fix) HEAD@{0}: commit: hot-fix commit
71f7e16 (master) HEAD@{1}: checkout: moving from master to hot-fix
71f7e16 (master) HEAD@{2}: reset: moving to 71f7e16
e386332 HEAD@{3}: commit: second commit
71f7e16 (master) HEAD@{4}: commit (initial): first commit
```

#### 3.2.4 合并分支

使用`git checkout master`回到`master`分支，再使用`cat`命令查看`hello.txt`文件，可以看到文件内容还是修改前的，并没有将内容合并，需要使用合并分支。

基本语法：`git merge 分支名`

#### 3.2.5 产生冲突

冲突产生的表现：命令行最后状态为 **MERGING**。

冲突产生的原因：并分支时，两个分支在同一个文件的**同一个位置有**两套完全不同的修改。 Git 无法替我们决定使用哪一个，因此，必须**人为决定**新代码内容。

- HEAD 如果指向 master，那么我们现在就在 master 分支上。
- HEAD 如果执行 hot-fix，那么我们现在就在 hot-fix 分支上。
- 所以切换分支的本质就是移动 HEAD 指针。

master、 hot-fix 其实都是指向具体版本记录的指针。当前所在的分支，其实是由 HEAD决定的。所以创建分支的本质就是多创建一个指针。



产生冲突并解解决冲突的步骤：

1. 在分支1中修改文件中的一个位置，上传缓存区并提交
2. 在分支2相同文件同样的位置做不同的修改，上传缓存区并提交
3. 将两个分支进行`git merge xx`，可以看到merge失败，并得到提示：merge conflict，需要在文件指定位置进行修改
4. 将文件冲突区域修正完毕后，输入命令`git commit -m "merge xxx"`显示合并成功。（**注意**：因冲突导致merge失败并重新提交commit指令时，指令末尾应该为空，不可以添加文件名）

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master|MERGING)
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master|MERGING)
$  vim hello.txt

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master|MERGING)
$ git add hello.txt

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master|MERGING)
$ git commit -m "merge hot-fix"
[master 0a04d5b] merge hot-fix

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
```



## 5. 团队协作

### 5.1 团队内协作

![img](.\gitmd_img\c397bde00d728c4e41eca79f578d25c3.png)

### 5.2 跨团队协作

![img](.\gitmd_img\e3069f865cc2d9760801b7a06c9d213b.png)



## 6. Github操作

### 6.1 创建远程仓库

Github页面点击New repository创建新的仓库，按需求对选项进行选择。

### 6.2 远程仓库操作

| 命令名称                           | 作用                                                      |
| ---------------------------------- | --------------------------------------------------------- |
| git remote -v                      | 查看当前所有远程地址别名                                  |
| git remote add 别名 远程地址       | 起别名                                                    |
| git push 别名 分支                 | 推送本地分支上的内容到远程仓库                            |
| git clone 远程地址                 | 将远程仓库的内容克隆到本地                                |
| git pull 远程库地址别名 远程分支名 | 将远程仓库对于分支最新内容拉下来后与 当前本地分支直接合并 |

#### 6.2.1创建别名

基本语法：

- `git remote -v` 查看当前所有远程地址别名
- `git remote add 别名 远程地址`

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git remote -v

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git remote add git-demo https://github.com/fuzikfu/git-demo.git

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git remote -v
git-demo        https://github.com/fuzikfu/git-demo.git (fetch)
git-demo        https://github.com/fuzikfu/git-demo.git (push)
```

#### 6.2.2 推动本地分支到远程仓库

基本语法：`git push 别名 分支`

git push后需要登录，如果推送失败就再push一次，登录成功后可以在Github上看到提交成功的代码

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git push git-demo master
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 16 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (15/15), 1.17 KiB | 300.00 KiB/s, done.
Total 15 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/fuzikfu/git-demo.git
 * [new branch]      master -> master
```

#### 6.2.3 拉取远程库到本地库

基本语法：`git pull 别名 分支`

在GitHub上手动修改文件并提交，远程库和本地库内容不一样了。

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git pull git-demo master
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 651 bytes | 43.00 KiB/s, done.
From https://github.com/fuzikfu/git-demo
 * branch            master     -> FETCH_HEAD
   0a04d5b..20123a9  master     -> git-demo/master
Updating 0a04d5b..20123a9
Fast-forward
 hello.txt | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ git status
On branch master
nothing to commit, working tree clean

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo (master)
$ cat hello.txt
Hello its my first time to study Git!1234567
qweqweqweqe
Hello its my first time to study Git!1234567
test pull
```

#### 6.2.4 克隆仓库到本地

基本语法：`git clone 远程地址`

从GitHub页面获取需要克隆的仓库的链接，将链接粘贴进命令里即可（克隆代码不需要登录账号）。

clone 会做如下操作：

1. 拉取代码。
2. 初始化本地仓库。
3. 创建别名。

```bash
44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo-clone
$ git clone https://github.com/fuzikfu/git-demo.git
Cloning into 'git-demo'...
remote: Enumerating objects: 18, done.
remote: Counting objects: 100% (18/18), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 18 (delta 3), reused 14 (delta 2), pack-reused 0
Receiving objects: 100% (18/18), done.
Resolving deltas: 100% (3/3), done.

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo-clone
$ cd git-demo

44845@Tiny MINGW64 /e/study/cs/github/git-space/git-demo-clone/git-demo (master)
$ git remote -v
origin  https://github.com/fuzikfu/git-demo.git (fetch)
origin  https://github.com/fuzikfu/git-demo.git (push)
```

#### 6.2.4 团队内协作

邀请链接发送给团队成员即可。

#### 6.2.5 跨团队协作

团队外成员 fork 仓库后，修改文件并提交到本地仓库，通过 Pull Requests 向原仓库提交代码修改请求。

## 7. GitHub设置

### 7.1 SSH免密登录

1. 在用户主页位置运行命令`ssh-keygen`生成.ssh目录：
```bash
44845@Tiny MINGW64 ~
$ ssh-keygen -t rsa -C 123@qq.com
```
2. 在用户主页位置就可以看到名为`.ssh`的新文件夹，将文件夹打开，打开`id_rsa.pub`文件，将文件内容复制
3. GitHub主页 - 头像 - Settings - SSH and GPG keys - New SSH key
4. Title随便填写，用于备注；将前面复制的内容贴近Key里
5. 点击 Add SSH Key 表明 SSH 密钥关联成功
6. 接下来通过SSH方式提交文件，在仓库主页点击Code，复制SSH链接




### 7.2 集成环境设置

配置 Git 忽略文件
与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能够屏蔽 IDE 工具之
间的差异。例如，Maven工程根据src生成的target。

创建忽略规则文件 xxxx.ignore（前缀名随便起，建议是 git.ignore），这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig 文件引用，建议也放在用户家目录下。

git.ignore 文件模版内容如下
```bash
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml
```

在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）

```bash
[user]
    name = Layne
    email = Layne@atguigu.com
[core]
	excludesfile = C:/Users/asus/git.ignore
```

### 7.3 IDE设置

课程讲得是IDEA，其它IDE应该差不多，就不记笔记了，主要涉及到IDE与git的配置。

1. **IDE 配置 git**（IDEA - File - Setting - 搜Git，配置Git的安装路径）
2. **IDE 项目初始化 git** （IDEA - VCS - Import into Version Control - Create Git Reaposity）
3. **add，commit 操作的方式** （右键文件，选择add/commit）
4. **切换版本**（Version Control - log 可以查看本地历史版本， 右键想切换的版本选择 Checkout Reversion “xxx” 即可，可以往前穿越也可以往后穿）
5. **创建分支** （Version Control - Git - Repository - Branches - +New Branch）
6. **切换分支** (点 IDE 状态栏的 Branch 名，点击想切换的分支；或者在 log 窗口，右键点击分支，选择checkout)
7. **合并分支** （点分支名 - Merge into Current， 若代码没有冲突，则合并成功）
8. **合并分支 - 冲突合并**（点分支名 - Merge into Current， 代码有冲突，需要人工解决，将代码提交本地库即可，此时 log 会有一些变化）
9. **设置 GitHub 账号** （设置搜索 GitHub，可以找到登录页面，登录即可，如果网络不好可以通过Token登录）
10. **GitHub设置 Token**（GitHub 页面 - 头像 - Settings - Developer Seetings - Personal access tokens - 取名&Generate token - 在 IDE 通过 Token登录即可）
11. **IDE分享项目到** GitHub （IDEA - VCS - Import into Version Control - Share Project on Github）
12. **IDE 推送代码到远程库**（Version Control - Git - Repository - Push）
13. **拉取远程库代码合并到本地库**（同上，选择Pull）
14. **克隆代码到本地**（File - Close Project - Get from Version Control - 填写URL； 或者VCS - Get from Version Control）



**注意1**： push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，
push 的操作是会被拒绝的。也就是说， 要想 push 成功，一定要保证本地库的版本要比远程库的版本高！ 因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地代码的区别！如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！

**注意2**： pull 是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动
合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

## 8 码云 Gitee

[码云 Gitee](https://gitee.com/)使用：

1. 码云可以从Github和GitLab中导入仓库
2. 码云私有库需要收费
3. 在部分IDE中需要安装Gitee插件，功能和使用方式等与Github插件类似

## 9 GitLab

[GitLab](https://about.gitlab.com/) 是由 GitLab Inc.开发，使用 MIT 许可证的基于网络的 Git 仓库管理工具，且具有wiki 和 issue 跟踪功能。使用 Git 作为代码管理工具，并在此基础上搭建起来的 web 服务。（可搭建局域网Git仓库）。

GitLab 由乌克兰程序员 DmitriyZaporozhets 和 ValerySizov 开发，它使用 Ruby 语言写成。后来，一些部分用 Go 语言重写。截止 2018 年 5 月，该公司约有 290 名团队成员，以及 2000 多名开源贡献者。 GitLab 被 IBM， Sony， JülichResearchCenter， NASA， Alibaba，Invincea， O’ReillyMedia， Leibniz-Rechenzentrum(LRZ)， CERN， SpaceX 等组织使用。

安装方式在[安装说明](https://about.gitlab.com/install/)里有详细解释，若需要集成进 IDE，操作方式同 Github 与 Gitee 一样。













