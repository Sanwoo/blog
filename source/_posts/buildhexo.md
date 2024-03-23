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

https://hexo.io/zh-cn/docs/github-pages

根据文档中项目页面这一部分继续，将_config.yml文件中的url: 更改为 <你的 GitHub 用户名>.github.io/<repo的名字>

在储存库中前往 Settings > Pages > Source，并将 Source 改为 GitHub Actions，选择create your own

创建pages.yml（记得加后缀名.yml否则不生效）

``` yml
.github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
      - main # default branch  //这里改为你的分支名，我的是master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 16.x   // 这里16换成你的node版本号
        uses: actions/setup-node@v2
        with:
          node-version: '16'     // 这里16换成你的node版本号
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

等部署完成后，即可在gh-pages网址中看到内容了。下次再push新内容前需要先pull，因为pages.yml是在远程仓库创建的。



