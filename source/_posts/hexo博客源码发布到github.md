---
title: hexo博客源文件发布到github
date: 2018-08-09 08:50:12
tags:
    - hexo
    - github
categories: hexo
---
这几天刚刚搭建的hexo博客，玩的还不太熟，由于白天上班没太有时间写博客，下班回家电脑又没有hexo环境和
博客源文件，写起来很不方便。于是又折腾一下，把hexo的源文件也一起发布到github上，这样就可以随时随地
的去更新了。
具体的方法网上也有一大堆，说的都比较笼统，本身自己对github的很多操作也不是很熟，所以也花了一个多小时
才搞定，方法自己记录了下来：
## github上建立两个分支，一个默认就有的master，一个hexo（我的名字，随意起）
  1、master分支，用来存放编译后的博客文件
  2、hexo分支，用来存放博客的源文件
## 新建hexo分支
  1、在本地新建一个分支：
  `git branch hexo`
  2、切换到你的新分支: 
  `git checkout hexo`
  3、将新分支发布在github上： 
  `git push origin hexo`
  然后登陆github如图就显示有了hexo分支
  ![](/images/sourcetogithub/branch.png)
  把新建的hexo分支设为默认分支
  ![](/images/sourcetogithub/default.png)
  为什么要把hexo分支设为默认分支呢？
  由于你git push的源文件是提交到hexo分支里的，而你编译的博客文件，是按照_config.yml里的配置里提交的，配置里配的是
  master分支，这样做的话，git push 就提交到了默认的hexo分支里了，而博客就按照配置发布到了master分支里了。
## 上传hexo源文件到github
  执行：
  `git add .`
  `git commit`
  `git push`
  hexo分支里就是源文件部分了
  ![](/images/sourcetogithub/hexosource.png)

## 换电脑重新写博客
  首先clone你的工程，由于上一步，设置的默认分支是hexo，所以clone的也就是默认的hexo分支里的源文件了
  clone以后如果电脑有hexo和node环境，就可以直接
  cnpm install
  然后就可以开始写博客了
  如果电脑没有环境，可以参考上一篇文章配一下基础hexo和node 环境了，地址：
  [从头开始使用hexo和github搭建博客](http://heyheystone.com/2018/08/01/%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E4%BD%A0%E5%A6%82%E4%BD%95%E7%94%A8hexo%E5%92%8Cgithub%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)
  
