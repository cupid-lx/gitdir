#git学习笔记七
***
######*本文摘自廖雪峰的[git实用教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*######
***
###删除文件###
话不多说，看实战
####测试操作####
第1步，先添加一个test.txt临时文件，然后Git提交
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat test.txt
test.txt
i am test files

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git add  test.txt
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory.

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git commit -m "add test.txt"
[master cfd70fd] add test.txt
 1 file changed, 2 insertions(+)
 create mode 100644 test.txt
</pre>
第2步，临时文件测试完成后就没用了，所以这时会选择删掉，通常情况下我们会用`rm`命令将该文件删除
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ rm test.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
通过状态查看，知道删除了哪个文件，但是，工作区和版本库就不一致了。所以现在需要把版本库的文件也删除掉
第3步，使用`git rm`命令把版本库的test文件删除，并且`git commit`
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git rm test.txt
rm 'test.txt'

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    test.txt


lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git commit -m "remove test.txt"
[master faa5679] remove test.txt
 1 file changed, 2 deletions(-)
 delete mode 100644 test.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git status
On branch master
nothing to commit, working tree clean
</pre>
现在，test文件就在版本库中删除了！

第4步，当我们通过rm命令把工作区的test文件删除，但是还没有执行`git rm`命令的时候，发现文件删错了，那么我们可以通过`git checkout -- test.txt`恢复
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ rm test.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ ls
license  readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git checkout -- test.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ ls
license  readme.txt  test.txt
</pre>
`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

>命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。