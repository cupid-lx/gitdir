#git学习笔记五
***
######*本文摘自廖雪峰的[git实用教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)*######
***
###工作区和暂存区###
####1. 工作区####
此处的工作区就是创建的`learngit`目录
####2. 版本库####
在工作区`learngit`经过`git init`初始化以后，会生成一个隐藏的`.git`目录，这个是Git的版本库
Git的版本库里存了很多东西，其中最重要的就是称为`stage（或者index）`的暂存区，还有Git为我们自动创建的第一个分支`master`,以及指向`master`的一个指针叫`HEAD`
<pre>
lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ pwd
/e/learngit

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ ll -a
total 9
drwxr-xr-x 1 lixiao 197121  0 11月 22 17:29 ./
drwxr-xr-x 1 lixiao 197121  0 11月 22 14:08 ../
drwxr-xr-x 1 lixiao 197121  0 11月 22 17:29 .git/
-rw-r--r-- 1 lixiao 197121 95 11月 22 17:29 readme.txt

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit (master)
$ cd .git/

lixiao@DESKTOP-506CIS0 MINGW64 /e/learngit/.git (GIT_DIR!)
$ ll -a
total 18
drwxr-xr-x 1 lixiao 197121   0 11月 22 17:29 ./
drwxr-xr-x 1 lixiao 197121   0 11月 22 17:29 ../
-rw-r--r-- 1 lixiao 197121  11 11月 22 16:34 COMMIT_EDITMSG
-rw-r--r-- 1 lixiao 197121 130 11月 22 14:24 config
-rw-r--r-- 1 lixiao 197121  73 11月 22 14:24 description
-rw-r--r-- 1 lixiao 197121  23 11月 22 14:24 HEAD
drwxr-xr-x 1 lixiao 197121   0 11月 22 14:24 hooks/
-rw-r--r-- 1 lixiao 197121 145 11月 22 17:29 index
drwxr-xr-x 1 lixiao 197121   0 11月 22 14:24 info/
drwxr-xr-x 1 lixiao 197121   0 11月 22 14:30 logs/
drwxr-xr-x 1 lixiao 197121   0 11月 22 16:34 objects/
-rw-r--r-- 1 lixiao 197121  41 11月 22 17:29 ORIG_HEAD
drwxr-xr-x 1 lixiao 197121   0 11月 22 14:24 refs/
</pre>
前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
