---
title: "搭建hugo静态博客(部署篇)"
date: 2024-10-13T13:45:44+08:00
lastmod: 2024-10-13T13:45:44+08:00
author: ["Jask"]
tags:
- 建站
---



# 1. 准备工作

- [Github](https://github.com/)
- [Vercel](https://vercel.com/)(可选)

# 2. 创建库和环境

- **在GitHub中`Create a new repository`**
  - 名字：`yourusername.github.com`

- **创建TOKEN**

  - 账户-设置-Develop Settings-Personal access tokens
  - Expiration设置永不
  - 勾选`repo`和`workflow`

  ![image-20241013135433025](/img/token.png)

- **链接Token到`repository`**

  - 打开库-设置
  - 新建密钥。名称TOKEN

  ![image-20241013135847601](/img/token2.png)

# 3. 本地创建workflow

- **在your-blog-site目录里面依次创建`.github/workflows/hugo_de.yaml`**

```yaml
name: deploy

# 代码提交到main分支时触发github action
on:
  push:
    branches:
      - dev

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
        - name: Checkout
          uses: actions/checkout@v4
          with:
              fetch-depth: 0

        - name: Setup Hugo
          uses: peaceiris/actions-hugo@v3
          with:
              hugo-version: "latest"
              extended: true

        - name: Build Web
          run: hugo -D

        - name: Deploy Web
          uses: peaceiris/actions-gh-pages@v4
          with:
              PERSONAL_TOKEN: ${{ secrets.TOKEN }} #TOKEN是刚才在库里面创建的名字，按照你的填。
              EXTERNAL_REPOSITORY: your-username/your-repository-name
              PUBLISH_BRANCH: main
              PUBLISH_DIR: ./public
              commit_message: auto deploy

```

**这个自动程序是将你本地未部署的文件发布到dev分支，然后Action自动部署到main分支**

# 4. 部署到GitHub

- **在主目录下输入：**

```git
git init
git add .
git commit -m "update"
git branch -M dev
git remote add origin https://github.com/your-username/your-repository-name.git
git push -u origin dev
```

- 设置GitHub Page
  - repository-setting-general-Default branch：**`main`**
  - repository-setting-page-Deploy from a branch：**`main`**

