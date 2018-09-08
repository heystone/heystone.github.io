---
title: Git基本知识和常用命令（一）
date: 2018-09-07 18:06:56
urlname: gitcommand
tags: github
categories: git
---
## 常用的一些基本的Git命令

对Git的了解只是存在于表面，平时里用的最多最多的命令也就是一下几个了
```javascript
    git init    //初始化仓库
    git add xxx  //把文件添加到仓库
    git commit -m "xxx"  //文件提交到仓库
    git push //推送到远程库
    git pull // 从远程库拉取
    git clone //从远程库克隆工程
    git log  //显示操作日志
    git log --pretty=oneline  //格式化可加参数
```
也是下班的时间里，看了廖雪峰老师的关于Git的介绍，学习了一下，顺便蹭个加班，总结一下，要看原版的地址在这里
[廖雪峰Git介绍](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

### 版本回退
在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^  依次类推
要回退到上一个版本的话执行命令
`git reset --hard HEAD^`
依次类推，但是要回归的版本太多，总不能一直用HEAD^^^吧，这也不太现实，所以需要找到某一个版本的一个唯一的id,
可以通过
```javascript
$ git log
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```
可以看到每次操作后面都有一串常常的SHA1码了吧，那就是记录版本的唯一id，我们要回退到哪一个版本只需要操作
`git reset --hard e475a` 
只需要输入前几位就行了，git会自动匹配，如果操作记录比较多，就多输入几位，但是版本回退有一个问题就是，如果我回退以后
又后悔了怎么办，因为你回退以后，再运行 git log就看不到回退版本以后的那些版本了，怎么办呢？git当然是有办法的了。
### 版本前进
命令 reflog 可查看每一次的命令
```javascript
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```
然后重复操作
`git reset --hard xxxx`
就好了
