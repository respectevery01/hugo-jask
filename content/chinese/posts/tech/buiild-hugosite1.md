---
title: "搭建hugo静态博客(搭建篇)"
date: 2024-10-13T13:13:01+08:00
lastmod: 2024-10-13T13:13:01+08:00
author: ["Jask"]
tags:
- 建站
---

# 1. 准备工作

- [hugo](https://github.com/gohugoio/hugo)
- [Git](https://git-scm.com/)



# 2. 搭建站点

**新建hugo站点**：`hugo new site your-blog-site`

**目录格式应该是这样的**：

```markdown
---archetypes

---assets

---content

---date

---i18n

---layout

---static

---themes

---hugo.toml
```



# 3. 更改主题

- 将主题文件拖入`themes`

- 在hugo.toml中添加如下代码：

  ```toml
  baseURL = "https://your-site.com"
  title = "your-site-name"
  languageCode = "en-us"
  theme = "name"#主题文件夹名字需要和这个一致
  ```

# 4. 新建页面

- 在content中新建一个`archive.md`

```yaml
title: "archive
layout: "archives"
url: "/archives/"
summary: archives
```

# 5. 新建文章

**在git中输入**

```git
hugo new /content/posts/the-first-post.md
```

# 6. 部署站点到本地

```git
hugo server -D
```

![image-20241013134216700](/img/git.png)

