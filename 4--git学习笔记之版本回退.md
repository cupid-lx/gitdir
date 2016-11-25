#git学习笔记四
***
######*本文摘自廖雪峰的[git实用教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*######
***
###版本回退###
现在已经学会了修改文件并且进行多次提交，现在再练习一次，现在修改readme.txt文件如下：
<pre>
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL
</pre>
然后再次提交
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git add readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git commit -m "append GPL"
[master b42b911] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
</pre>
像这样的过程，我们不断的对文件进行修改，然后不断的提交。天长日久，我们不可能记住一个月之前都改过什么内容，所以这时候就需要有个机制来记住之前进行过的操作。在Git中，我们可以用`git log`命令查看：
>`git log`命令显示从最近到最远的提交日志
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git log
commit 37617716045f98ee83f71357cde29d9c5686c0ef
Author: lixiao <280781201@qq.com>
Date:   Tue Nov 22 14:43:31 2016 +0800

    append GPL

commit 4fb883b6304505a0c18ab4c18c4a18c38a85fdd1
Author: lixiao <280781201@qq.com>
Date:   Tue Nov 22 14:41:52 2016 +0800

    add distributed

commit 7ca853b88d20796dcdc35a514c241849299e4436
Author: lixiao <280781201@qq.com>
Date:   Tue Nov 22 14:30:27 2016 +0800

    wrote a readme file
</pre>
> 如果感觉以上输出的信息太多可以使用`--pretty=oneline`参数
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git log --pretty=oneline
37617716045f98ee83f71357cde29d9c5686c0ef append GPL
4fb883b6304505a0c18ab4c18c4a18c38a85fdd1 add distributed
7ca853b88d20796dcdc35a514c241849299e4436 wrote a readme file
</pre> 
> ***提示：git的版本号和SVN不同，git的版本号是一个SHA1计算出来的非常大的数字，用十六进制表示***

在提交完新的代码以后，忽然发现有行修改的代码会导致数据库宕机，那么就要进行版本回退，但是回退需要知道版本号，幸好我们已经学习了`git log`命令，得到了提交文件的版本号。
在Git中，还可以用`HEAD`代表当前的版本，可以用`HEAD^`代表上一个版本，`HEAD^^`代表上上个版本，但是如果往上10个版本，100个版本呢？总不能写100个`^`号吧。在git中，那我们可以写成`HEAD ~100`。现在我们通过`git reset`命令就回退到上一个版本：
> `git reset`使用该命令可以切换到指定的代码版本
<pre>
回退到上个版本：
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git reset --hard HEAD^

或者使用指定版本号的方式回退
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git reset --hard 4fb883b6  ##此处版本号可以不用写全，只写前几位就可以了 
</pre>
现在我们看一下是否已经回退到上一个版本了呢？
<pre>>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git log
commit 4fb883b6304505a0c18ab4c18c4a18c38a85fdd1
Author: lixiao <280781201@qq.com>
Date:   Tue Nov 22 14:41:52 2016 +0800

    add distributed

commit 7ca853b88d20796dcdc35a514c241849299e4436
Author: lixiao <280781201@qq.com>
Date:   Tue Nov 22 14:30:27 2016 +0800

    wrote a readme file

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git log --pretty=oneline
4fb883b6304505a0c18ab4c18c4a18c38a85fdd1 add distributed
7ca853b88d20796dcdc35a514c241849299e4436 wrote a readme file

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
</pre>
如果回退到上一个版本后，忘了之前是怎么修改的了，想再恢复到新版本怎么办呢？这时候只能看运气了，你是否保留着新版本的commit_id呢？其实没保留也没关系，在git中，有后悔药卖。git中的`git reflog`记录了你每一次执行过的命令:
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git reflog
4fb883b HEAD@{0}: reset: moving to 4fb883b6
3761771 HEAD@{1}: commit: append GPL
b42b911 HEAD@{2}: commit: add distributed
b42b911 HEAD@{3}: commit (initial): wrote a readme file 
</pre>
这时可以看到第二行的GPL版本的ID，然后再通过`git reset`命令恢复新版本
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ git reset --hard 3761771
HEAD is now at 3761771 append GPL

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
</pre>
是不是又恢复到新版本了呢？？