# Git



## 1.创建版本库（repository）

### （1）创建空目录

```pascal
$ mkdir learngit                //创建一个名叫learngit的空目录
$ cd learngit                   //把learngit设置为当前目录
$ pwd                           //查看当前目录
```

### （2）建立仓库

```pascal
$ git init
```



## 2.文件的创建与修改

### （1）添加文件

1.在 learngit 目录下编写一个 read.txt 文件，内容如下：

```pascal
ray is a teenager.
ray likes to play LOL.
```



2.用 git add 命令，把文件添加至仓库：

```pascal
$ git add read.txt               //执行完后无显示
```

3.用 git commit 命令，把文件提交到仓库：

```pascal
$ git commit -m "creat"          //-m后面输入的是本次提交的说明，可以输入任意内容。
[master 63d6425] creat
 1 file changed, 2 insertions(+)
                                 //1 file changed：1个文件被改动（新添加的read.txt文件）；
                                 //2 insertions：插入了两行内容（read.txt内有两行内容）
 create mode 100644 read.txt
```

### （2）修改文件

修改 read.txt 为：

```pascal
ray is a teenager.
ray really likes to play LOL.
```

- 运行 git status 命令来查看仓库当前的状态：


```pascal
$ git status                      //查看仓库当前的状态
On branch master
Changes not staged for commit:    //没有文件将要被提交
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
 
    modified:   read.txt          //read.txt文件被修改过了
 
no changes added to commit (use "git add" and/or "git commit -a")
```

- 运行 git diff 命令来得知文件被如何修改：


```pascal
$ git diff read.txt
diff --git a/read.txt b/read.txt
index c524634..fa9a37a 100644
--- a/read.txt
+++ b/read.txt
@@ -1,2 +1,2 @@
 ray is a teenager.
-ray likes to play LOL.           //删除的句子
\ No newline at end of file       
+ray really likes to play LOL.    //新增的句子
\ No newline at end of file
```

重复之前的 git add xxx.xx 与 git commit -m "xxx" 指令，便可更改

再用 git status 查看一下当前仓库状态：

```pascal
$ git status
On branch master
nothing to commit, working tree clean //当前没有需要提交的修改，而且，工作目录是干净的。
```

git commit 命令只负责把 暂存区/index (stage) 的修改提交

再次修改 read.txt

可以用 git diff HEAD -- read.txt 命令去查看 工作区/Working Directory (learngit) 和 版本库/Repository (.git) 里面最新版本的区别：
```pascal
$ git diff HEAD -- read.txt
diff --git a/read.txt b/read.txt
index df7409c..057d7fe 100644
--- a/read.txt
+++ b/read.txt
@@ -1,2 +1,3 @@
 ray is a teenager.
-ray really likes to play LOL and 2077.
\ No newline at end of file
+ray really likes to play LOL and 2077.
+ray is handsome.
\ No newline at end of file
```


### （3）版本回退

修改 read.txt 为：

```pascal
ray is a teenager.
ray really likes to play LOL and 2077.
```

添加并提交：

```pascal
25471@LAPTOP-T6P6JMD7 MINGW64 ~/learngit (master)
$ git add read.txt

25471@LAPTOP-T6P6JMD7 MINGW64 ~/learngit (master)
$ git commit -m "2077"
[master 463be6d] 2077
 1 file changed, 1 insertion(+), 1 deletion(-)
```

- 运行 git log：


```pascal
$ git log //查看历史记录
commit 463be6d78f7676172b53e0c75dfa979f68c4f81c (HEAD -> master)
Author: ray <254719244@qq.com>
Date:   Mon Nov 30 09:37:23 2020 +0800

    2077

commit 771736d11bd580db5283f85505d9acd7f3927018
Author: ray <254719244@qq.com>
Date:   Mon Nov 30 09:27:06 2020 +0800

    really

commit 63d64250335efe2a04c76f18c7df8b32411a4780
Author: ray <254719244@qq.com>
Date:   Mon Nov 30 08:55:06 2020 +0800

    creat
```

- 如果加上了 --pretty-oneline 参数：

```pascal
$ git log --pretty=oneline
463be6d78f7676172b53e0c75dfa979f68c4f81c (HEAD -> master) 2077
771736d11bd580db5283f85505d9acd7f3927018 really
63d64250335efe2a04c76f18c7df8b32411a4780 creat
//一大串数字是 commit id ，而且每个人的都不一样。
```

若想将文件退回上一个版本，就可以使用 git reset 命令：

```pascal
$ git reset --hard HEAD^      //HEAD表示当前版本，则HEAD^表示上一个版本，那么上上版本就是HEAD^^
HEAD is now at 771736d really
```

这时使用 cat 命令查看 read.txt

```pascal
$ cat read.txt
ray is a teenager.
ray really likes to play LOL.
```

若想回到最新版本，继续使用 git reset 命令

```pascal
$ git reset --hard 463be6d   //这里使用commit id（的一部分）
HEAD is now at 463be6d 2077
```

这时再查看文件内容：

```pascal
$ cat read.txt
ray is a teenager.
ray really likes to play LOL and 2077.
```

### （4）撤销修改

目前 read.txt 内容为：

```pascal
ray is a teenager.
ray really likes to play LOL and 2077.
ray is handsome.
```

想要删除最后一行的话

#### (i)   还未 git add 

1.手动直接修改文件内容

2.使用 git checkout -- file 命令

```pascal
$ git checkout -- read.txt  //把read.txt文件在工作区的修改全部撤销。
```

#### (ii)  暂未 git commit

此时的修改在暂存区（stage）：

```pascal
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   read.txt
```
此时使用 git reset HEAD file 指令
```pascal
$ git reset HEAD read.txt
//git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区，HEAD表示最新版本。
Unstaged changes after reset:
M       read.txt
```

#### (iii) git commit 之后

见 版本回退

### （5）文件删除

创建一个 deletet.txt 文件，并添加和提交到git.

```pascal
$ git add delete.txt

$ git commit -m "add"
[master 1c15773] add
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 delete.txt
```

使用 rm 命令将文件从工作区移除：

```pascal
$ rm delete.txt
```

如果删错了，就用 git checkout 还原（因为版本库里的没有删除）：

```pascal
$ git checkout -- test.txt   //用版本库里的版本替换工作区的版本。
```

如果想从版本库中删除该文件，就使用 git rm file 指令：

```pascal
$ git rm delete.txt
rm 'delete.txt'

$ git commit -m "remove delete.txt"
[master 8e35586] remove delete.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 delete.txt
```

此时版本库就没有该文件了。



## 3.远程仓库

### 1.新建远程仓库

![](D:\picture\Camera Roll\FB0JRH2K4@6[[Q5~QLJF{MA.png)

点击 new repository ，更改 name 为工作区的名字（此时为learngit），其他选择默认设置，最后 点击“Create repository”按钮，就成功地创建了一个新的Git仓库。

### 2.推送本地库

1.在本地 learngit 仓库下运行命令（第一次）：

```pascal
$ git remote add origin git@github.com:rayloveme/learngit.git 
```

2.使用 git push 命令，把当前分支 master 推送到远程：

```pascal
$ git push -u origin master      //因为第一次推送master，因此要加 -u参数
```

3.从现在起，只要本地作了提交，就可以通过命令：

```pascal
$ git push origin master
```

把本地 master 分支的最新修改推送至GitHub。

