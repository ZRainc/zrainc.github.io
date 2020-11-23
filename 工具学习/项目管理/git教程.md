# git教程



## 1、git简介

git是一个开源的分布式版本控制系统，与常用的版本控制工具CVS、Subversion等不同，它采用了分布式版本库的方式，不必服务器端版本控制

## 2、git和svn的对比

git不仅仅是个版本控制系统，它也是个内容管理系统（CMS），工作管理系统等

- **1、git是分布式的，svn不是**：这是git和其他非分布式版本控制系统，最核心的区别
- **2、git把数据按元数据方式存储，而svn按文件**：所有的资源控制系统都是把文件的元信息隐藏在一个类似.svn、.cvs等的文件夹里
- **3、git分支和svn分支不同**：分支在svn中一点都不特别，它其实就是版本库中的另外一个目录
- **4、git没有一个全局的版本号，而svn有：**目前位置这是跟svn相比git缺少的最大的一个特征
- **5、git的内容完整性要由于svn：**Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

![img](https://www.runoob.com/wp-content/uploads/2015/02/0D32F290-80B0-4EA4-9836-CA58E22569B3.jpg)

## 3、git工作流程

一般工作流程如下：

1．从远程仓库中克隆 Git 资源作为本地仓库。

2．从本地仓库中checkout代码然后进行代码修改

3．在提交前先将代码提交到暂存区。

4．提交修改。提交到本地仓库。本地仓库中保存修改的各个历史版本。

5．在修改完成后，需要和团队成员共享代码时，可以将代码push到远程仓库。

下图展示了 Git 的工作流程：

![img](file:///C:\Users\DELL\AppData\Local\Temp\ksohtml17216\wps1.jpg)

## 4、git工作区、缓存区和版本库

- 工作区：就是你在电脑中能看到的目录
- 暂存区：英文叫stage或index。一般存在`.git`目录下的index文件中，所以我们把缓存区有时也叫索引（index）
- 版本库：工作区有一个隐藏目录`.git`，这个不算工作区，而是git的版本库

## 5、创建仓库

- 使用GitBash或使用TortoiseGit
- git init 用来初始化一个git仓库

## 6、git基本操作

git的工作就是创建和保存你项目的快照与之后的快照进行对比

常用命令：**git colne、git push、git add、git commit、git checkout、git pull**

![img](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)

**说明**：

- worksapce：工作区
- staging area：暂存区/缓冲区
- local repository：本地仓库
- remote repositroy：远程仓库

### 创建仓库命令

下表列出了 git 创建仓库的命令：

| 命令        | 说明                                   |
| :---------- | :------------------------------------- |
| `git init`  | 初始化仓库                             |
| `git clone` | 拷贝一份远程仓库，也就是下载一个项目。 |

### 提交与修改

Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

下表列出了有关创建与提交你的项目的快照的命令：

| 命令         | 说明                                     |
| :----------- | :--------------------------------------- |
| `git add`    | 添加文件到仓库                           |
| `git status` | 查看仓库当前的状态，显示有变更的文件。   |
| `git diff`   | 比较文件的不同，即暂存区和工作区的差异。 |
| `git commit` | 提交暂存区到本地仓库。                   |
|              |                                          |
| `git reset`  | 回退版本。                               |
| `git rm`     | 删除工作区文件。                         |
| `git mv`     | 移动或重命名工作区文件。                 |

### 提交日志

| 命令         | 说明                                 |
| :----------- | :----------------------------------- |
| `git log`    | 查看历史提交记录                     |
| `git blame ` | 以列表形式查看指定文件的历史修改记录 |

### 远程操作

| 命令         | 说明               |
| :----------- | :----------------- |
| `git remote` | 远程仓库操作       |
| `git fetch`  | 从远程获取代码库   |
| `git pull`   | 下载远程代码并合并 |
| `git push`   | 上传远程代码并合并 |

> git pull = git fetch 和 git merge

## 7、分支管理

​	在我们每次的提交，git都会把它串成一条时间线，这条时间线就是一个分支。截至到目前位置，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD指针严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向就是当前分支。

​	一开始的时候，master分支是一条线，git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点

![img](file:///C:\Users\DELL\AppData\Local\Temp\ksohtml17216\wps2.jpg)

​	每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长

​	当我们创建新的分支时，例如dev分支，git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上。

![img](file:///C:\Users\DELL\AppData\Local\Temp\ksohtml17216\wps3.jpg)

你看，创建一个新的分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件没有任何变化。

不过，从现在开始，对工作去的修改和提交就针对dev分支了，比如新提交一次后，dev指针向前移动一步，而master指针不变

![img](file:///C:\Users\DELL\AppData\Local\Temp\ksohtml17216\wps4.jpg)

假如我们在dev上的工作完成了，就可以把dev合并到master上，git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：

![img](file:///C:\Users\DELL\AppData\Local\Temp\ksohtml17216\wps5.jpg)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：

![img](file:///C:\Users\DELL\AppData\Local\Temp\ksohtml17216\wps6.jpg)

> 分支切换以及解决冲突等

## 8、远程仓库

github 、gitee











