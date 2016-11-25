#git学习笔记三
***
######*本文摘自廖雪峰的[git实用教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*######
***
###添加文件到版本库###
#####准备工作####
先用`Notepad++`编写一个readme.txt文件，放在`testdir`目录下，内容如下：
<pre>
Git is version control system.
Git is free sofrware.
</pre>
#####第1步,把文件添加到仓库#####
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git add readme.txt
</pre>
#####第2步，把文件提交到仓库
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git commit -m "this is first test"
[master (root-commit) da2f59a] this is test
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
参数：-m 输入本次提交的说明，可以是任何内容，当然最好是有意义的，方便从历史记录查找
</pre>
> `git add`与`git commit`的区别：

> 你可以多次`git add`不同的文件到仓库

>`git add`了多次不同的文件到仓库以后，可以执行`git commit`，一次性提交多个文件到仓库

例：
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git add filename1.txt

李肖@lixiao MINGW32 /e/testdir (master)
$ git add filename2.txt

李肖@lixiao MINGW32 /e/testdir (master)
$ git add filename3.txt

李肖@lixiao MINGW32 /e/testdir (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   filename1.txt
        new file:   filename2.txt
        new file:   filename3.txt

李肖@lixiao MINGW32 /e/testdir (master)
$ git commit -m "add 3 files"
[master 1da1d21] add 3 files
 3 files changed, 6 insertions(+)
 create mode 100644 filename1.txt
 create mode 100644 filename2.txt
 create mode 100644 filename3.txt
</pre>
###修改提交过的版本内容，并再次提交###
修改后的版本内容如下：
<pre>
Git is distribted version control system.
Git is free software.
</pre>
#####通过`git status`查看修改了那个文件#####
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
#####使用`git diff`对比文件修改的内容#####
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index f3adad1..e831e3c 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is version control system.
+Git is distribted version control system.
 Git is free software.
\ No newline at end of file
</pre>
#####再次添加`readme.txt`文件#####
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git add readme.txt

李肖@lixiao MINGW32 /e/testdir (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt
</pre>
#####提交文件到仓库#####
<pre>
李肖@lixiao MINGW32 /e/testdir (master)
$ git commit -m "this is second file"
[master 730f759] this is second file
 1 file changed, 1 insertion(+), 1 deletion(-)

李肖@lixiao MINGW32 /e/testdir (master)
$ git status
On branch master
nothing to commit, working directory clean
</pre>
