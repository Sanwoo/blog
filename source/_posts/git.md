---
title: git相关
date: 2024-03-15 21:03:14
tags:
---

![](/images/image-20220725175548618.png)



https://git-scm.com/book/zh/v2
https://nulab.com/zh-cn/learn/software-development/git-tutorial/
https://www.liaoxuefeng.com/wiki/896043488029600

<br/>

.gitignore文件中的文件将不会被git add


每次开发之前先pull
如果push时和远端版本不一致，那么add、commit之后再pull，否则可能会将本地代码覆盖



> git push origin master:master

将本地和远端的两个master分支关联，远端没有则顺便创建新分支，本地必须先有这个分支



###### 本地查看远程分支和实际远程分支清单不一致

> git remote update origin --prune
>
> 或者
>
> git fetch origin
>
> [2020-12-18 Git 更新远程分支列表命令_海月汐辰的博客-CSDN博客_git刷新远程分支命令](https://blog.csdn.net/qq_37858386/article/details/111386170)

<br/>

https://juejin.cn/post/6844903710007492621
https://juejin.cn/post/6982454524317302792
https://juejin.cn/post/6844904080276455437?searchId=20240315223948E0AC34C0F7C760321617
规范是什么
常见的分类有下面几种：

build：修改项目的的构建系统（xcodebuild、webpack、glup等）的提交
ci：修改项目的持续集成流程（Kenkins、Travis等）的提交
chore：构建过程或辅助工具的变化
docs：文档提交（documents）
feat：新增功能（feature）
fix：修复 bug
pref：性能、体验相关的提交
refactor：代码重构
revert：回滚某个更早的提交
style：不影响程序逻辑的代码修改、主要是样式方面的优化、修改
test：测试相关的开发

> 其实，执行添加了-u 参数的命令 git push -u origin master就相当于是执行了
git push origin master 和
git branch --set-upstream master origin/master
后者是用于关联远程分支

> 查看全局配置 
 git config --global user.name
 git config --global user.email
 设置全局配置
 git config --global user.name "Your Name"
 git config --global user.email "email@example.com"

> 移除远程仓库 
git remote remove origin


 https://juejin.cn/post/6982454524317302792

> 1、本地创建仓库且有内容，在git创建远程仓库且有readme文件
  2、在本地git remote add origin url关联远程仓库
  3、git add .  git commit -m 'msg'
  4、此时直接git push -u origin master会报错，根本原因在于我们在创建仓库的时候，都会勾选“使用Reamdme文件初始化这个仓库”这个操作初识了一个README文件并配置添加了忽略文件。当点击创建仓库时，它会帮我们做一次初始提交。于是我们的仓库就有了README.m和.gitignore文件，然后我们把本地项目关联到这个仓库，并把项目推送到仓库时，我们在关联本地与远程时，两端都是有内容的，但是这两份内容并没有联系，当我们推送到远程或者从远程拉取内容时，都会有没有被跟踪的内容，于是你看git报的详细错误中总是会让你先拉取再推送，但是拉取总是失败。
  <br/>
  解决方法一：创建仓库时不要生成readme文件
  解决方法二：git pull --rebase origin master git push -u origin master


  test!!!!!!!!!!!!!!!!!!!