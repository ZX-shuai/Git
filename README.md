[toc]


-----

### 1. git 简介

git的两大特点：

- 版本控制：可以解决多人同时开发的代码问题，也可以解决找回历史代码的问题。

- 分布式：Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。首先找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。可以自己搭建这台服务器，也可以使用GitHub网站。

### 2. 安装与配置

（1）安装

- CentOS使用yum命令安装

```shell
sudo yum install -y git
```

- Ubuntu使用apt-get命令安装

```shell
sudo apt-get install git
```

（2）查看Git版本

```shell
git --version
```

（3）配置

```shell
git config --global user.name "用户名"
git config --global user.email "邮箱地址"
```

### 3. 创建一个版本库

在需要的位置创建一个裸仓库（可作为远程仓库）

```shell
git init --bare git_test
```

可以使用`git init`在当前目录创建 git 仓库，也可使用`git init <path>`在 path 路径下创建目录并创建 git 仓库。注意`--bare`参数可以生成一般作为远程仓库的裸仓库，其不包含工作区，可以参考:[git init与git init --bare](https://www.jianshu.com/p/5b7ff91c5338)。

我们使用`git init`创建本地裸仓库git_test

```shell
git init git_test
```

执行上面命令会出现仓库的地址，git_test就是一个工作区，工作区中有一个隐藏目录.git，是git的版本库，具体参见工作区和暂存区。

```shell
[shuai@localhost ~]$ git init git_test
初始化空的 Git 版本库于 /home/shuai/git_test/.git/
[shuai@localhost ~]$ ls
git_test  公共  模板  视频  图片  文档  下载  音乐  桌面
[shuai@localhost ~]$ 
```

### 4. 版本创建与回退

（1）进入到工作区，即git_test，使用`touch code.txt`命令在git_test目录下创建一个文件code.txt，然后使用`vi code.txt`编辑文本，最后可使用`cat code.txt`查看文本内容，具体实现如下：

```shell
[shuai@localhost git_test]$ touch code.txt
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
```

Vim命令参考：退出时按Esc+`:wq`，:一定要是英文冒号。

（2）使用如下两条命令可以创建一个版本：`git add code.txt`将工作区的code.txt文件提交到暂存区，`git commit -m '版本1'`将暂存区的code.txt提交到当前分支，命名为版本1。具体实现如下：

```shell
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m '版本1'
[master（根提交） d8d7d7f] 版本1
 1 file changed, 1 insertion(+)
 create mode 100644 code.txt
```

（3）使用`git log`命令可以查看版本记录，实现如下：

```shell
[shuai@localhost git_test]$ git log
commit d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca
Author: shuai <1335656785@qq.com>
Date:   Fri Apr 29 11:47:26 2022 +0800

    版本1
[shuai@localhost git_test]$ 
```

其中包含五项信息： 1. 提交的索引值（哈希值）commit 2. 提交的作者Author，这与最开始配置时的git config信息相同 3. 提交时间Date 4. HEAD指针指向当前的版本

（4）继续编辑code.txt，在里面增加一行，实现如下：

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
[shuai@localhost git_test]$ 
```

（5）将改动后的code.txt提交版本库并查看版本记录，实现如下：

```shell
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m '版本2'
[master fc29a48] 版本2
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$ git log
commit fc29a485c7c4835dea81e354e447a9805322c0d3
Author: shuai <1335656785@qq.com>
Date:   Fri Apr 29 12:21:12 2022 +0800

    版本2

commit d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca
Author: shuai <1335656785@qq.com>
Date:   Fri Apr 29 11:47:26 2022 +0800

    版本1
[shuai@localhost git_test]$ 
```

当版本记录过多时，会发生折叠，可按q退出，或者使用`git log --pretty=oneline`让版本记录变简短。

```shell
[shuai@localhost git_test]$ git log --pretty=oneline
fc29a485c7c4835dea81e354e447a9805322c0d3 版本2
d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca 版本1
```

此时只包含三项信息：1. 索引值（哈希值） 2. 提交信息 3. HEAD指针指向的记录

（6）现在若想回到上一个版本，可以使用命令`git reset --hard HEAD^`，其中HEAD表示当前最新版本，HEAD\^表示当前版本的前一个版本,HEAD^^表示当前版本的前前个版本，也可以使用HEAD\~1表示当前版本的前一个版本,HEAD~100表示当前版本的前100版本。

退回到版本1的具体实现如下：

```shell
[shuai@localhost git_test]$ git reset --hard HEAD^
HEAD 现在位于 d8d7d7f 版本1
[shuai@localhost git_test]$ git log --pretty=oneline
d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca 版本1
```

执行命令后使用git log查看版本记录，发现现在只能看到版本1的记录，cat code.txt查看文件内容，现在只有一行，也就是第一个版本中code.txt的内容。

（7）假如我们现在又想回到版本2，这个时候怎么办？可以使用命令`git reset --hard 版本号`，之前执行`git log`命令得到的版本记录中有版本号，我们可以在（5）中查到版本2的版本号为fc29a485c7c4835dea81e354e447a9805322c0d3，我们只用取前几位即可，具体实现如下：

```shell
[shuai@localhost git_test]$ git reset --hard fc29a485
HEAD 现在位于 fc29a48 版本2
[shuai@localhost git_test]$ git log --pretty=oneline
fc29a485c7c4835dea81e354e447a9805322c0d3 版本2
d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca 版本1
```

（8）假如说上面的终端已经关闭，看不到版本2的版本号怎么办？`git reflog`命令可以查看操作记录。

### 5. 工作区与暂存区

#### 5.1 工作区

电脑中的learngit.git，就是一个工作区。

#### 5.2 版本库

工作区有一个隐藏目录.git，这个不是工作区，而是git的版本库。git的版本库里面存着很多东西，其中最重要的就是成为stage的暂存区，还有git为我们创建的第一个分支master，以及指向master的一个指针叫HEAD。

因为我们创建git版本库时，git自动为我们创建了唯一一个master分支，所以现在git commit就是往master分支上提交更改。

可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

![image-20220426224406262](https://github.com/ZX-shuai/Git/blob/main/img/image-20220426224406262.png)

把文件往git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

（1）下面在git_test目录下再创建一个文件code2.txt，然后编辑内容如下：

```shell
[shuai@localhost git_test]$ touch code2.txt
[shuai@localhost git_test]$ vi code2.txt
[shuai@localhost git_test]$ cat code2.txt
add one line
```

（2）然后再次编辑code.txt内容，在其中加入一行，编辑后内容如下：

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
```

（3）使用`git status`命令查看当前工作树的状态：

```shell
[shuai@localhost git_test]$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
# 未跟踪的文件:
#   （使用 "git add <file>..." 以包含要提交的内容）
#
#	code2.txt
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

上面提示我们code.txt被修改，而code2.txt没有被跟踪。`git status`命令用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被Git tracked到。`git status`不显示已经`commit`到项目历史中去的信息，看项目历史的信息要使用`git log`。

（4）我们使用如下命令把code.txt和code2.txt加入到暂存区，然后再执行`git status`命令，结果如下：

```shell
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git add code2.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      code.txt
#	新文件：    code2.txt
#
[shuai@localhost git_test]$ 
```

git add命令是把所有提交的修改存放到暂存区。

（5）然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支创建一个版本，命名版本3，操作如下：

```shell
[shuai@localhost git_test]$ git commit -m '版本3'
[master d8962fe] 版本3
 2 files changed, 2 insertions(+)
 create mode 100644 code2.txt
[shuai@localhost git_test]$ 
```

（6）提交后，没有对工作区做任何修改，那么此时工作区是“干净的”

```shell
[shuai@localhost git_test]$ git status
# 位于分支 master
无文件要提交，干净的工作区
[shuai@localhost git_test]$
```

现在我们的版本库变成了这样：![image-20220427164406622](https://github.com/ZX-shuai/Git/blob/main/img/image-20220427164406622.png)

#### 5.3 管理修改

git管理文件的修改，它只会提交暂存区的修改来创建版本。

（1）编辑code.txt，并使用`git add`命令将其添加到暂存区中。

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ 
```

（2）继续编辑code.txt，并在其中添加一行，我们不提交到版本库中的暂存区

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
this is the new line
[shuai@localhost git_test]$ 
```

（3）使用`git commit` 创建一个版本，并用`git status` 查看，发现对于第二次修改code.txt的内容，创建版本的时候并没有被提交，工作区未干净。

```shell
[shuai@localhost git_test]$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      code.txt
#
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
[shuai@localhost git_test]$ git commit -m '版本4'
[master 80856fd] 版本4
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[shuai@localhost git_test]$
```

#### 5.4 撤销修改

（1）继续上面的操作，提示我们可以使用 `git checkout -- <文件>` 来丢弃工作区的改动。执行如下命令，发现工作区干净了，第二次的改动内容也没了。

```shell
[shuai@localhost git_test]$ git checkout -- code.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
无文件要提交，干净的工作区
[shuai@localhost git_test]$ 
```

（2）我们继续编辑code.txt，并在其中添加如下内容，并将其添加的暂存区。

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
the new line
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      code.txt
#
[shuai@localhost git_test]$ 
```

（3）用命令`git reset HEAD file`可以把暂存区的修改撤销掉，重新放回工作区。

```shell
[shuai@localhost git_test]$ git reset HEAD code.txt
重置后撤出暂存区的变更：
M	code.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[shuai@localhost git_test]$
```

（4）重新放回工作区后，若想丢弃code.txt的修改，执行`git checkout -- code.txt`命令即可。

``` shell
[shuai@localhost git_test]$ git checkout -- code.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
无文件要提交，干净的工作区
[shuai@localhost git_test]$
```

如果，现在改错了东西，还从暂存区提交到了版本库，则需要进行版本回退。

**小结：**

场景1：当改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节。

#### 5.5 对比文件的不同

**对比工作区和某个版本中文件的不同：**

（1）继续编辑文件 code.txt，在其中添加一行内容。

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
the new line
[shuai@localhost git_test]$ 
```

（2）现在要对比工作区中code.txt 和HEAD版本中 code.txt 的不同。使用`git diff HEAD -- code.txt`命令：

```shell
[shuai@localhost git_test]$ git diff HEAD -- code.txt
diff --git a/code.txt b/code.txt
index 66f9219..324317f 100644
--- a/code.txt
+++ b/code.txt
@@ -2,3 +2,4 @@ this is the first line
 this is the second line
 this is the third line
 this is the forth line
+the new line
[shuai@localhost git_test]$
```

---代表HEAD版本中code.txt的内容，+++代表工作区中code.txt的内容，`+the new line`表示工作区code.txt比HEAD版本code.txt多了一行。

![image-20220427203705603](https://github.com/ZX-shuai/Git/blob/main/img/image-20220427203705603.png)


（3）使用如下命令丢弃工作区的改动

```shell
[shuai@localhost git_test]$ git checkout -- code.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
无文件要提交，干净的工作区
[shuai@localhost git_test]$
```

**对比两个版本间文件的不同：**

（1）现在要对比HEAD和HEAD^版本中code.txt的不同，使用如下命令：

```shell
[shuai@localhost git_test]$ git diff HEAD HEAD^ -- code.txt
diff --git a/code.txt b/code.txt
index 66f9219..01e1274 100644
--- a/code.txt
+++ b/code.txt
@@ -1,4 +1,3 @@
 this is the first line
 this is the second line
 this is the third line
-this is the forth line
[shuai@localhost git_test]$
```

可以看到`diff --git a/code.txt b/code.txt`。显示HEAD版本code.txt比HEAD^版本code.txt多了一行。

![image-20220427205111412](https://github.com/ZX-shuai/Git/blob/main/img/image-20220427205111412.png)

#### 5.6 删除文件

（1）我们把目录中的code2.txt删除

```shell
[shuai@localhost git_test]$ rm code2.txt
[shuai@localhost git_test]$ 
```

这个时候git知道删除了文件，因此，工作区和版本库就不一致了，`git status` 命令会立刻提示哪些文件被删除了。

``` shell
[shuai@localhost git_test]$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add/rm <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	删除：      code2.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[shuai@localhost git_test]$
```

（2）如果确定要从版本库中删除该文件，那就用命令` git rm file`或者`git add file`将删除改动放进暂存区，注意删除文件也属于工作区的改动。然后使用`git commit -m '文件名' `提交到版本库。

```shell
[shuai@localhost git_test]$ git rm code2.txt
rm 'code2.txt'
[shuai@localhost git_test]$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	删除：      code2.txt
#
[shuai@localhost git_test]$ git commit -m '删除code2.txt'
[master fb546dc] 删除code2.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 code2.txt
[shuai@localhost git_test]$ git status
# 位于分支 master
无文件要提交，干净的工作区
[shuai@localhost git_test]$
```

如果误删文件code2.txt，此时还没有提交到暂存区和版本库，可使用`git checkout -- file`撤销删除操作。如果提交到暂存区，参考**5.4撤销修改**；如果删除操作提交到版本库，参考**5.4撤销修改**。

### 6. 分支管理

#### 6.1 概念

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了git又学会了SVN！

![image-20220429144004458](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429144004458.png)

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

#### 6.2 创建和合并分支

git把我们之前每次提交的版本串成一条时间线，这条时间线就是一个分支。截止到目前只有一条时间线，在git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

(1) 一开始的时候，master分支是一条线，git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：![image-20220429144430189](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429144430189.png)

每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。

(2)当我们创建新的分支，例如dev时，git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：![image-20220429144446025](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429144446025.png)

git创建一个分支很快，因为除了增加一个dev指针，改变HEAD的指向，工作区的文件都没有任何变化。

(3)不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：![image-20220429144520783](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429144520783.png)

(4)假如我们在dev上的工作完成了，就可以把dev合并到master上。git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：![image-20220429144543212](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429144543212.png)

git合并分支也很快，就改改指针，工作区内容也不变。

(5)合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：![image-20220429144610552](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429144610552.png)

**案例：**

（1）执行`git branch`命令可以查看当前有几个分支，并且看到在哪个分支下工作。

```shell
[shuai@localhost git_test]$ git branch
* master
[shuai@localhost git_test]$
```

（2）`git checkout -b 分支名`创建分支并且切换其上进行工作

```shell
[shuai@localhost git_test]$ git checkout -b dev
切换到一个新分支 'dev'
[shuai@localhost git_test]$ git branch
* dev
  master
[shuai@localhost git_test]$ 
```

![image-20220429145836817](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429145836817.png)

（3）在dev分支下，我们修改code.txt内容，在里面添加一行，并进行提交。

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m 'dev分支提交'
[dev e50c2a8] dev分支提交
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$ 
```

![image-20220429150107896](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429150107896.png)

（4）使用`git checkout 分支名`切换分支

```shell
[shuai@localhost git_test]$ git checkout master
切换到分支 'master'
[shuai@localhost git_test]$ git branch
  dev
* master
[shuai@localhost git_test]$ 
```

![image-20220429153156041](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429153156041.png)

查看code.txt，发现添加的内容没有了。因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：

````shell
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
[shuai@localhost git_test]$
````

（5）现在使用`git merge 要合并的分支名`把dev分支的工作成果合并到master分支上：

```shell
[shuai@localhost git_test]$ git merge dev
更新 fb546dc..e50c2a8
Fast-forward
 code.txt | 1 +
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$
```

git merge命令用于合并指定分支到当前分支。合并后，再查看code.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。

```shell
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
[shuai@localhost git_test]$
```

![image-20220429153523572](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429153523572.png)

注意到上面的Fast-forward信息，git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。

（6）合并完成后，就可以放心地删除dev分支了，使用`git branch -d 分支名`删除后，使用`git branch`查看branch，就只剩下master分支了。

```shell
[shuai@localhost git_test]$ git branch -d dev
已删除分支 dev（曾为 e50c2a8）。
[shuai@localhost git_test]$ git branch 
* master
[shuai@localhost git_test]$
```

![image-20220429153752063](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429153752063.png)

**小结：**

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

####  6.3 解决冲突

（1）再创建一个新分支dev并切换：

```shell
[shuai@localhost git_test]$ git checkout -b dev
切换到一个新分支 'dev'
[shuai@localhost git_test]$ 
```

（2）修改code.txt内容，并进行提交：

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
add one more line
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m 'dev分支提交2'
[dev ca3b761] dev分支提交2
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$
```

（3）切换回master分支：

```shell
[shuai@localhost git_test]$ git checkout master
切换到分支 'master'
[shuai@localhost git_test]$
```

（4）在master的code.txt添加一行内容并进行提交：

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
add more line in master
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m 'master提交'
[master 58a7c69] master提交
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$
```

现在，master分支和dev分支各自都分别有新的提交，变成了这样：

![image-20220429155347306](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429155347306.png)

这种情况下，git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突。

（6）执行`git merge`命令尝试将dev分支合并到master分支上来：

```shell
[shuai@localhost git_test]$ git merge dev
自动合并 code.txt
冲突（内容）：合并冲突于 code.txt
自动合并失败，修正冲突然后提交修正的结果。
[shuai@localhost git_test]$
```

git告诉我们，code.txt文件存在冲突，必须手动解决冲突后再提交。

（6）git status也可以告诉我们冲突的文件：

```shell
[shuai@localhost git_test]$ git status
# 位于分支 master
# 您有尚未合并的路径。
#   （解决冲突并运行 "git commit"）
#
# 未合并的路径：
#   （使用 "git add <file>..." 标记解决方案）
#
#	双方修改：     code.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[shuai@localhost git_test]$
```

（7）我们手动解决冲突，首先查看code.txt的内容，git用<<<<<<<，\==\=\===，>>>>>>>标记出不同分支的内容，我们修改如下后保存：

```shell
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
<<<<<<< HEAD
add more line in master
=======
add one more line
>>>>>>> dev
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
add more line in master
add one more line
[shuai@localhost git_test]$
```

（8）再提交：

```shell
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m '解决冲突'
[master ed998d5] 解决冲突
[shuai@localhost git_test]$ 
```

现在，master分支和dev分支变成了下图所示：

![image-20220429160308284](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429160308284.png)

（9）用`git log --graph --pretty=oneline`也可以看到分支的合并情况：

```shell
[shuai@localhost git_test]$ git log --graph --pretty=oneline
*   ed998d575df5279d26e363147c89ecc0f96576e3 解决冲突
|\  
| * ca3b7615965a56cfaa3c8f0cce650967baf79eb5 dev分支提交2
* | 58a7c69289e69a567b57f3e6be4bf8f30f2beb22 master提交
|/  
* e50c2a8654e1ef4e6124aa2660890dafbffd67cc dev分支提交
* fb546dc37abe21339096dfae9ec229378e042f34 删除code2.txt
* 80856fd946a6c34854eb8d1429a5d1f9f6b0e301 版本4
* d8962fe61b05888dccc14427a750ffeaf053ffa6 版本3
* fc29a485c7c4835dea81e354e447a9805322c0d3 版本2
* d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca 版本1
[shuai@localhost git_test]$
```

（10）最后工作完成，可以删除dev分支：

```shell
[shuai@localhost git_test]$ git branch -d dev
已删除分支 dev（曾为 ca3b761）。
[shuai@localhost git_test]$ git branch
* master
[shuai@localhost git_test]$
```

####  6.4 分支管理策略

通常，合并分支时，如果可能，git会用fast forward模式，但是有些快速合并不能成而且合并时没有冲突，这个时候会合并之后并做一次新的提交。但这种模式下，删除分支后，会丢掉分支信息。

（1）创建一个分支dev并切换到其下工作：

```shell
[shuai@localhost git_test]$ git checkout -b dev
切换到一个新分支 'dev'
[shuai@localhost git_test]$
```

（2）新建一个文件code3.txt编辑内容如下，并提交一个commit：

```shell
[shuai@localhost git_test]$ touch code3.txt
[shuai@localhost git_test]$ vi code3.txt
[shuai@localhost git_test]$ cat code3.txt
a line
[shuai@localhost git_test]$ git add code3.txt
[shuai@localhost git_test]$ git commit -m '创建code3.txt'
[dev 930653f] 创建code3.txt
 1 file changed, 1 insertion(+)
 create mode 100644 code3.txt
[shuai@localhost git_test]$
```

（3）切换回master分支，编辑code.txt并进行一个提交：

```shell
[shuai@localhost git_test]$ git checkout master
切换到分支 'master'
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m '添加新行'
[master 4e8a04e] 添加新行
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$
```

（4）使用`git merge dev`合并dev分支的内容到master分支，出现如下提时，这是因为这次不能进行快速合并，所以git提示输入合并说明信息，输入之后合并内容之后git会自动创建一次新的提交：

```shell
Merge branch 'dev'

# 请输入一个提交信息以解释此合并的必要性，尤其是将一个更新后的上游分支
# 合并到主题分支。
#
# 以 '#' 开头的行将被忽略，而且空提交说明将会终止提交。
~                                                                                             
                                                                                          
".git/MERGE_MSG" 6L, 234C

```

（5）使用`git log --graph --pretty=oneline`命令查看分支信息：

```shell
[shuai@localhost git_test]$ git log --graph --pretty=oneline
*   4a355ea8f5b3955d39c16d898242c5e8eb6e80f3 合并分支 dev
|\  
| * 930653f72623047fc251318781731e745c68ecdd 创建code3.txt
* | 4e8a04ed381210d84475856a466c1c4b18a1d99e 添加新行
|/  
*   ed998d575df5279d26e363147c89ecc0f96576e3 解决冲突
|\  
| * ca3b7615965a56cfaa3c8f0cce650967baf79eb5 dev分支提交2
* | 58a7c69289e69a567b57f3e6be4bf8f30f2beb22 master提交
|/  
* e50c2a8654e1ef4e6124aa2660890dafbffd67cc dev分支提交
* fb546dc37abe21339096dfae9ec229378e042f34 删除code2.txt
* 80856fd946a6c34854eb8d1429a5d1f9f6b0e301 版本4
* d8962fe61b05888dccc14427a750ffeaf053ffa6 版本3
* fc29a485c7c4835dea81e354e447a9805322c0d3 版本2
* d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca 版本1
[shuai@localhost git_test]$
```

（6）使用`git branch -d 分支名`删除分支dev：

```shell
[shuai@localhost git_test]$ git branch -d dev
已删除分支 dev（曾为 930653f）。
[shuai@localhost git_test]$
```

如果要强制禁用fast forward模式，git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

（1）创建并切换dev分支：

```shell
[shuai@localhost git_test]$ git checkout -b dev
切换到一个新分支 'dev'
[shuai@localhost git_test]$
```

（2）修改code.txt内容，并提交一个commit：

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m '添加新行'
[dev dc9a520] 添加新行
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$
```

（3）切换回master分支：

```shell
[shuai@localhost git_test]$ git checkout master
切换到分支 'master'
[shuai@localhost git_test]$ 
```

（4）准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：

```shell
[shuai@localhost git_test]$ git merge --no-ff -m '禁用fast-forward合并' dev
Merge made by the 'recursive' strategy.
 code.txt | 1 +
 1 file changed, 1 insertion(+)
[shuai@localhost git_test]$ 
```

因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

（5）使用`git log --graph --pretty=oneline`命令查看分支信息。可以看到，不使用Fast forward模式，merge后就像这样：

```shell
[shuai@localhost git_test]$ git log --graph --pretty=oneline
*   b7ac7295e48cb583c4b7abe48232cb6bbf4e2055 禁用fast-forward合并
|\  
| * dc9a5209064b2b7c4b8f0722ea9853fc2b15665b 添加新行
|/  
*   4a355ea8f5b3955d39c16d898242c5e8eb6e80f3 合并分支 dev
|\  
| * 930653f72623047fc251318781731e745c68ecdd 创建code3.txt
* | 4e8a04ed381210d84475856a466c1c4b18a1d99e 添加新行
|/  
*   ed998d575df5279d26e363147c89ecc0f96576e3 解决冲突
|\  
| * ca3b7615965a56cfaa3c8f0cce650967baf79eb5 dev分支提交2
* | 58a7c69289e69a567b57f3e6be4bf8f30f2beb22 master提交
|/  
* e50c2a8654e1ef4e6124aa2660890dafbffd67cc dev分支提交
* fb546dc37abe21339096dfae9ec229378e042f34 删除code2.txt
* 80856fd946a6c34854eb8d1429a5d1f9f6b0e301 版本4
* d8962fe61b05888dccc14427a750ffeaf053ffa6 版本3
* fc29a485c7c4835dea81e354e447a9805322c0d3 版本2
* d8d7d7f7f4cf97273fd4fe0ba00b22e79262c1ca 版本1
[shuai@localhost git_test]$
```

![image-20220429163229210](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429163229210.png)

#### 6.5 Bug分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在git中，由于分支是如此的强大，所以，**每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。**

（1）当你接到一个修复一个代号001的bug的任务时，很自然地，你想创建一个分支bug-001来修复它，但是等等，当前正在dev上进行的工作还没有提交：

```shell
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ git status
# 位于分支 dev
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[shuai@localhost git_test]$ 
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

（2）git还提供了一个stash功能，执行`git stash`可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```shell
[shuai@localhost git_test]$ git stash
Saved working directory and index state WIP on dev: dc9a520 添加新行
HEAD 现在位于 dc9a520 添加新行
[shuai@localhost git_test]$ 
```

（3）首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

```shell
[shuai@localhost git_test]$ git checkout master
切换到分支 'master'
[shuai@localhost git_test]$ git checkout -b bug-001
切换到一个新分支 'bug-001'
[shuai@localhost git_test]$
```

（4）现在修复bug，假如bug就是把code.txt最后一行 add new line删掉，然后提交：

```shell
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
add more line in master
add one more line
a line in master
add new line
[shuai@localhost git_test]$ vi code.txt
[shuai@localhost git_test]$ cat code.txt
this is the first line
this is the second line
this is the third line
this is the forth line
add one line
add more line in master
add one more line
a line in master
[shuai@localhost git_test]$ git add code.txt
[shuai@localhost git_test]$ git commit -m '修复bug-001'
[bug-001 c1e91b8] 修复bug-001
 1 file changed, 1 deletion(-)
[shuai@localhost git_test]$
```

（5）修复完成后，切换到master分支，并完成合并，最后删除bug-001分支：

```shell
[shuai@localhost git_test]$ git checkout master
切换到分支 'master'
[shuai@localhost git_test]$ git merge --no-ff -m '修复bug-001' bug-001
Merge made by the 'recursive' strategy.
 code.txt | 1 -
 1 file changed, 1 deletion(-)
[shuai@localhost git_test]$ 
```

（6）现在bug-001修复完成，是时候接着回到dev分支干活了：

```shell
[shuai@localhost git_test]$ git branch -d bug-001
已删除分支 bug-001（曾为 c1e91b8）。
[shuai@localhost git_test]$ git checkout dev
切换到分支 'dev'
[shuai@localhost git_test]$ git status
# 位于分支 dev
无文件要提交，干净的工作区
[shuai@localhost git_test]$
```

（7）工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```shell
[shuai@localhost git_test]$ git stash list
stash@{0}: WIP on dev: dc9a520 添加新行
[shuai@localhost git_test]$
```

（8）作现场还在，git把stash内容存在某个地方了，但是需要恢复一下，执行`git stash pop`我们发现之前的工作区又回来了：

```shell
[shuai@localhost git_test]$ git stash pop
# 位于分支 dev
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
丢弃了 refs/stash@{0} (a01c72240af30a1e421a4590c68f0e3c236676c7)
[shuai@localhost git_test]$ git status
# 位于分支 dev
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      code.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
[shuai@localhost git_test]$
```

**小结：**

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，恢复工作现场。

###  7. 使用Github作为服务器

####  7.1 创建仓库

注册github账户，登录后，点击"New respository "

####  7.2 添加SSH账户

（1）点击账户头像后的下拉三角，选择'settings'

![image-20220429171743920](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429171743920.png)

（2）点击'SSH and GPG keys'，添加ssh公钥。

![image-20220429171751211](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429171751211.png)

（3）在CentOS的命令行中，回到用户的主目录下，编辑文件.gitconfig，修改机器的git配置，在2-（3）时已经配置过。

```shell
[shuai@localhost git_test]$ cd
[shuai@localhost ~]$ vi .gitconfig
```

（3）修改为注册github时的邮箱，填写用户名（可随意填写）。

```shell
[user]
        name = shuai
        email = 1335656785@qq.com
```

（4）使用`ssh-keygen -t rsa -C "邮箱地址"`命令生成ssh密钥：

```shell
[shuai@localhost ~]$ ssh-keygen -t rsa -C "1335656785@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/shuai/.ssh/id_rsa): 
/home/shuai/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/shuai/.ssh/id_rsa.
Your public key has been saved in /home/shuai/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:i1W9fcP/UUNdfnGbaUvZSE6hdFuRkk8cQCC08Y12TNE 1335656785@qq.com
The key's randomart image is:
+---[RSA 2048]----+
|        .+ .+=@=B|
|          =o=O E@|
|         ..+o+BO=|
|         .. .o=.o|
|        S   . .=o|
|       o .     .=|
|      . .      ..|
|                o|
|                .|
+----[SHA256]-----+
[shuai@localhost ~]$ 

```

（5）进入主目录下的.ssh文件，下面有两个文件:

- 公钥为id_rsa.pub

- 私钥为id_rsa

查看公钥内容，复制此内容：

```shell
[shuai@localhost ~]$ cd .ssh/
[shuai@localhost .ssh]$ ls
id_rsa  id_rsa.pub  known_hosts
[shuai@localhost .ssh]$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDtLI748gYryVGahrOgm+j/tkUr3s+pXpCxCagtlvpRG3SteG9LBFnrlv8dNvjXHjjxSi5Z4cJ0WVtDHVqS3bY2v2Ak7NkcKUsjPI0wwsU8l6Lh7Km8/77EzUJCbRXjmL1ie0GG7EVyDNc+3HTk+GmEAJ5q4BFXNW8Sdv5d2O46xoLHyScw6jesyQYNmXA8awCbMDddHTYmm1Ss8fW9nCZPaad7T+j8vzqcoVcVyB5sv4xb8JIKlAzGrxz0F5ijwF4FcG3gpJbaJ9nUGJYg63ax3TUJrc3q5k9ShCwjKO571kJSPNEBuSG+/glFR/lRH1TMzaz+SBJfP0+ShcEkI3Dl 1335656785@qq.com
[shuai@localhost .ssh]$ 
```

（）回到浏览器中，填写标题，粘贴公钥

![image-20220429175124246](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429175124246.png)

####  7.3 克隆项目

（1）选中项目，复制git地址，注意选SSH

![image-20220429175625970](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429175625970.png)

（2）再命令行执行`git clone <git地址>`

```shell
[shuai@localhost ~]$ ls
git_test  公共  模板  视频  图片  文档  下载  音乐  桌面
[shuai@localhost ~]$ git clone git@github.com:ZX-shuai/Git.git
正克隆到 'Git'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
接收对象中: 100% (6/6), done.
[shuai@localhost ~]$ ls
Git  git_test  公共  模板  视频  图片  文档  下载  音乐  桌面
[shuai@localhost ~]$ 
```

可以看见Git仓库已经被克隆到本地。

####  7.4 上传分支

（1）项目克隆到本地之后，进入Git仓库后执行`git checkout -b smart`命令创建分支smart。

```shell
[shuai@localhost ~]$ cd Git
[shuai@localhost Git]$ git checkout -b smart
切换到一个新分支 'smart'
[shuai@localhost Git]$ git branch
  main
* smart
[shuai@localhost Git]$ 
```

（2）删除README.md中部分的内容，并提交一个版本：

```shell
[shuai@localhost Git]$ ls
README.md
[shuai@localhost Git]$ vi README.md
[shuai@localhost Git]$ cat README.md

[shuai@localhost Git]$ git add README.md
[shuai@localhost Git]$ git commit -m '版本1'
[smart 2d2a69d] 版本1
 1 file changed, 1 insertion(+), 51 deletions(-)
 rewrite README.md (99%)
[shuai@localhost Git]$
```

（3）推送前github分支列表如下：

![image-20220429182716965](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429182716965.png)

（4）推送分支，就是把该分支上的所有本地提交推送到远程库，推送时要指定本地分支，这样，git就会把该分支推送到远程库对应的远程分支上

```
git push origin 分支名称
例：
git push origin smart
```

```shell
[shuai@localhost Git]$ git push origin smart
Counting objects: 5, done.
Compressing objects: 100% (1/1), done.
Writing objects: 100% (3/3), 242 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'smart' on GitHub by visiting:
remote:      https://github.com/ZX-shuai/Git/pull/new/smart
remote: 
To git@github.com:ZX-shuai/Git.git
 * [new branch]      smart -> smart
[shuai@localhost Git]$ 
```

推送后变化如下：

![image-20220429183404440](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429183404440.png)

点击Compare&pull request可以看到提交的内容，点击Create pull request，推送和合并是在github上操作的。

![image-20220429183513818](https://github.com/ZX-shuai/Git/blob/main/img/image-20220429183513818.png)

####  7.5 将本地分支跟踪服务器分支

```
git branch --set-upstream-to=origin/远程分支名称 本地分支名称
例：
git branch --set-upstream-to=origin/smart smart
```

####  7.6 从远程分支上拉取代码

```shell
git pull orgin 分支名称
例：
git pull orgin smart
```

使用上述命令会把远程分支smart上的代码下载并合并到本地所在分支。

####  7.7  删除分支

- 查找本地分支，并删除除主分支之外的本地分支

```shell
[shuai@localhost Git]$ git branch
  main
* smart
[shuai@localhost Git]$ git checkout main
切换到分支 'main'
[shuai@localhost Git]$ git branch -d smart
warning: 将要删除的分支 'smart' 已经被合并到
         'refs/remotes/origin/smart'，但未合并到 HEAD。
已删除分支 smart（曾为 2d2a69d）。
[shuai@localhost Git]$ 
```

- 查找远程分支，并删除除主分支之外的本地分支

```shell
[shuai@localhost Git]$ git branch -r
  origin/HEAD -> origin/main
  origin/main
  origin/smart
[shuai@localhost Git]$ git push origin --delete smart
To git@github.com:ZX-shuai/Git.git
 - [deleted]         smart
[shuai@localhost Git]$ 
```

**小结：**

查看远程分支`git branch -r`；

查看本地分支`git branch `；

查看当前所有的分支`git branch -a`包括本地分支和远程分支；

删除远程分支：`git push <主机名> --delete <分支名> `，注意github上的主机名为origin，分支名前面没有remote/origin，--delete不能用-d代替。

删除本地分支：`git branch -d <分支名>`会在删除前检查merge状态，如果分支包含未合并的更改和未推送的提交，则该-d标志将不允许删除本地分支。 `git branch -D <分支名> `是git branch --delete --force的简写，它会直接删除。

删除本地分支与删除远程分支命令相互独立，想要本地和远程都删除，必须运行两个命令。

### 8. 工作使用Git

**项目经理：**

(1)   项目经理搭建项目的框架。

(2)   搭建完项目框架之后，项目经理把项目框架代码放到服务器。

**普通员工：**

(1)  在自己的电脑上，生成ssh公钥，然后把公钥给项目经理，项目经理把它添加的服务器上面。

(2)  项目经理会给每个组员的项目代码的地址，组员把代码下载到自己的电脑上。

(3)  创建本地的分支dev,在dev分支中进行每天的开发。

(4)  每一个员工开发完自己的代码之后，都需要将代码发布远程的dev分支上。

 

Master:用户保存发布的项目代码。V1.0,V2.0

Dev:保存开发过程中的代码。



### 参考资料：

[Git(linux)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19J411j7SZ?p=7)  
[OneSizeFitsQuorum/git-tips: 33 条常用 git 命令详解 ](https://github.com/OneSizeFitsQuorum/git-tips)  
[Githug](https://github.com/Gazler/githug)  
[learnGitBranching](https://github.com/pcottle/learnGitBranching)  
[Git原理入门解析 ](https://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247489338&idx=2&sn=6fdd8968003b8f59d43c09b72894e877&chksm=ebd62816dca1a1003dcefb6dae8296dd08924426fa6f12d09b8695922007fcb9bfa342c35bd1&scene=21#wechat_redirect)  
[Git 从入门到精通，这一篇就够了](https://mp.weixin.qq.com/s/b8bQW2N5VC-qmGD4dTiwKQ)  
[Git - Book ](https://git-scm.com/book/zh/v2)  
[vim常用命令总结](https://www.cnblogs.com/yangjig/p/6014198.html)  
