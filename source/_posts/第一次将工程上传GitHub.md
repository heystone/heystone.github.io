---
title: 第一次将工程上传GitHub
date: 2018-09-05 17:59:38
urlname: gitpush
tags: github
categories: 问题汇总 git
---
第一次提交代码的一般步骤：
1、新建仓库,不要手残点击生成readme.md,后面提交会有很多意外的坑
2、进入项目目录执行 `git init`
3、`git add .`
4、`git commit -m ""`
5、`git remote add origin https://github.com/heystone/wx.git` 改成自己的仓库地址
6、`git pull`
报错：
`fatal: refusing to merge unrelated histories`
执行：
`git merge origin/master --allow-unrelated-histories`
7、`git push`