#git学习笔记二
***
######*本文摘自廖雪峰的[git实用教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*######
***
##创建版本库##
#####什么是版本库？#####
版本库有可以称作仓库（repository），可以把它当作一个目录。只是这个目录的所有文件都可以被`git`管理起来，包括目录下每个文件的修改、删除等操作都能被`git`跟踪，这样将来可以在某个时刻还原以前的版本

####第1步 创建版本库####
注：在使用`git bash`的时候，操作命令与linux的操作命令一样，例如`cd e:\`即是切换目录
<pre>
李肖@lixiao MINGW32 ~
$ cd e:

李肖@lixiao MINGW32 /e
$ mkdir testdir

李肖@lixiao MINGW32 /e
$ ls
$RECYCLE.BIN/  pagefile.sys  testdir/ 
  
李肖@lixiao MINGW32 /e
$ cd testdir/

李肖@lixiao MINGW32 /e/testdir
$ pwd
/e/testdir
</pre>
####第2步 将testdir目录变为git可以管理的仓库####
<pre>
李肖@lixiao MINGW32 /e/testdir
$ git init
Initialized empty Git repository in E:/testdir/.git/

李肖@lixiao MINGW32 /e/testdir (master)
$ ls -ah
./  ../  .git/
</pre>
经过上述步骤操作后，瞬间就把git仓库搭建好了，执行`ls -ah`命令会发现在当前目录下多了一个`.git`的一个目录，这个目录就是用来跟踪管理版本库的，所以这个目录的文件千万不要修改，否则就把`.git`仓库破坏了。
####第3步 将文件添加到仓库####
> 所有的版本控制系统，只能跟踪文本文件的改动，比如TXT文件、网页、所有的程序代码等，`git`也是如此。

> 版本控制系统可以告诉你每次的改动，比如在某行修改了一个linux的单词，。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但是没有办法跟踪文件的变化，最多也就跟踪这些二进制文件的大小变动，至于具体修改了那些内容是不得而知的

**Windows系统不要用系统自带的记事本编辑任何文件，否则会遇到很多不可思议的问题。原因是Microsoft开发记事本的团队在每个文件的开头增加了0xefbbbf（十六进制）的字符，目的是用来保存UTF-8编码的文件**