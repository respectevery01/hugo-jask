---
title: "搭建vitepress静态博客"
date: 2024-10-26T22:02:37+08:00
lastmod: 2024-10-26T22:02:37+08:00
author: ["Jask"]
tags:
- 建站
---

## 1. 准备工作

- [Vitepress](https://vitepress.dev/)
- [Git](https://git-scm.com/)

## 2. 搭建站点

~~~npm
npx vitepress init
~~~

**a. 将需要回答几个简单的问题：**

```git
┌  Welcome to VitePress!
│
◇  Where should VitePress initialize the config?
│  ./docs
│
◇  Site title:
│  My Awesome Project
│
◇  Site description:
│  A VitePress Site
│
◆  Theme:
│  ● Default Theme (Out of the box, good-looking docs)
│  ○ Default Theme + Customization
│  ○ Custom Theme
└
```

**b. 文件结构**

~~~git
.
├─ docs
│  ├─ .vitepress
│  │  └─ config.js
│  ├─ api-examples.md
│  ├─ markdown-examples.md
│  └─ index.md
└─ package.json
~~~

## 3. 配置文件

配置文件 (`.vitepress/config.mts`) 让你能够自定义 VitePress 站点的各个方面，参考我的文件：

~~~js
import { defineConfig } from 'vitepress'

// https://vitepress.dev/reference/site-config
export default defineConfig({
base:"/",
  title: "原初混沌",
  description: "一本长篇科幻小说",
  themeConfig: {
    // https://vitepress.dev/reference/default-theme-config
    nav: [
      { text: '主页', link: '/' },
      { text: '关于', link: '/about' }
    ],

    sidebar: [
      {
        text: '瓦尔多斯',
        items: [
          { text: '第一章 回忆', link: '/waldoz/chapter1' },
          { text: '第二章 失忆', link: '/waldoz/chapter2' },
		  { text: '第三章 我', link: '/waldoz/chapter3' }
        ]
      }
    ],

    socialLinks: [
      { icon: 'github', link: 'https://github.com/respectevery01' },
	  { icon: 'twitter', link: 'https://x.com/jask_don'}
    ]
  }
})

~~~

## 4. 启动并运行

确保你的`package.json`是正确的

```json
{
  ...
  "scripts": {
    "docs:dev": "vitepress dev docs",
    "docs:build": "vitepress build docs",
    "docs:preview": "vitepress preview docs"
  },
  ...
}
```

`docs:dev` 脚本将启动具有即时热更新的本地开发服务器。使用以下命令运行它：

```git
npm run docs:dev
```

> **注意！**
>
> 如果你需要部署到Github，确保你的根目录下有`package-lock.json`
>
> 没有请使用:
>
> ~~~git
> npm install
> ~~~

## 5. 部署到github

创建`./.github/workflows/deploy.yml`

`deploy.yml`内容参考以下：

```yaml
name: Deploy VitePress Site to GitHub Pages

on:
  # 当 `dev` 分支有推送时触发工作流
  push:
    branches: [dev]
  # 允许从 GitHub Actions 手动触发
  workflow_dispatch:

permissions:
  contents: write  # 允许内容写入权限，用于推送到 main 分支

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # 检出代码
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
      
      - name: Install dependencies
        run: npm install

      - name: List install packages
        run: npm list

      # 设置 Node.js 环境
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: npm

      # 安装依赖
      - name: Install dependencies
        run: npm install

      # 构建 VitePress 项目
      - name: Build with VitePress
        run: npm run docs:build

      # 将生成的文件夹推送到 main 分支
      - name: Deploy to GitHub Pages
        run: |
          # 设置 Git 用户信息
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"

          # 将生成的文件拷贝到一个新目录
          mkdir -p output
          cp -R docs/.vitepress/dist/* output

          # 切换到 main 分支并清空内容
          git fetch origin main
          git switch main || git checkout -b main
          git rm -rf .

          # 将构建产物复制到 main 分支，提交并推送
          cp -r output/* .
          git add .
          git commit -m "Deploy VitePress site"
          git push origin main --force

```

> 注意！
>
> 请在github页面创建一个main分支，因为我的配置是push到dev分支，而非main分支。
>
> 你需要手动创建一个main分支

**利用Git命令发布到github**

~~~git
git init
git add .
git commit -m "update"
git branch -M dev
git remote add origin https://github.com/your-username/your-repository-name.git
git push -u origin dev
~~~

**设置默认分支为`main`**
