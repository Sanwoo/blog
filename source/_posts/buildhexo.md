---
title: buildhexo
date: 2024-03-16 17:53:57
tags:
---
git init

hexo init

hexo g

npm install hexo-theme-keep

_config.yml文件将theme: landscape改为theme: keep

hexo s 检测本地是否运行成功

github创建远程仓库

git remote add origin url

git add .

git commit -m 'xxx'

git push -u origin master 此处会报错因为有readme文件，建议先git pull --rebase origin master



