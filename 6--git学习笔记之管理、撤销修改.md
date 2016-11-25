#git学习笔记六
***
######*本文摘自廖雪峰的[git实用教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*######
***
###管理修改###
对于管理修改，我们不多解释，下面我们通过测试操作来进行说明
####测试操作####
第1步，先修改readme.txt文件，增加一行内容
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
</pre>
第2步，将修改后的readme.txt文件添加至工作区
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git add readme.txt readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt
</pre>
第3步，在修改readme.txt文件
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
</pre>
第4步，commit提交，并且查看状态
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git commit -m "git tracks changes"
[master d208cc4] git tracks changes
 1 file changed, 1 insertion(+)

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
#####是不是发现第二次修改的没有被提交呢？
> 原因就是，当我们进行了第一次修改，并且`git add`命令之后，文件在工作区的第一次修改被放入了暂存区，准备提交。但是在工作区的第二次修改并没有放入暂存区，所以，这时直接执行`git commit`只会把暂存区的修改提交，第二次修改的不会被提交。

来让我们用`git diff`命令看下工作区和版本库里最新版本的区别:
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git diff HEAD -- readme.txt
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
</pre>
#####那么，如何才能让第二次修改的也被提交呢？
>我们可以执行完`git commit`之后，再执行一次`git add`和`git commit`。也可以在第二次修改完成之后先再执行一次`git add`,然后执行`git commit`，把两次修改过的一并提交。

###撤销修改###
当我们努力的程序猿在凌晨3点还在很代码打交道的时候，脑袋一迷糊，难免会出错
####测试操作####
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN;
</pre>
在准备提交的时候，忽然发现了这个严重的错误，这时候很容易纠正，只需要修改一下就可以了。我们先用`git status`命令查看下状态：
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
这时我们可以使用`git checkout -- "filename"`命令来丢弃工作区的修改
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git checkout -- readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
nothing to commit, working tree clean

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
</pre>
是不是发现已经把修改过的文件撤销会上一个版本的状态了呢！
> 命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

> 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

> 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

> 总之，就是让这个文件回到最近一次git commit或git add时的状态。

假如很不幸，我们亲爱的程序猿同志不但修改了文件，而且还TM的`git add`了，但是庆幸的是只是添加到了暂存区，并没有commit提交，那么我们就可以用`git reset HEAD "filename"`命令把暂存区的修改给撤销掉，重新放回工作区，然后再执行`git checkout -- "filename"`撤销修改。
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
My stupid boss still prefers SVN.

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git add readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git reset HEAD -- readme.txt
Unstaged changes after reset:
M       readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git reset HEAD -- readme.txt
Unstaged changes after reset:
M       readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git checkout -- readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
nothing to commit, working tree clean

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
</pre>
是不是惊喜的发现又恢复到上一个版本了呢？