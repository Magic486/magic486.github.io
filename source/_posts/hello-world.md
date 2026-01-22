---
title: 博客更新日志
tags: 博客构建
date: 2025-03-20
top_img: transparent
comments: false
---

# 2.24

**博客部署**

用[hexo](#)和[butterfly](https://butterfly.js.org/)搭建的个人博客，用于记录知识与学习经验，还在持续更新中

# 2.26

**鼓搞了音乐功能**

# 3.3

**鼓搞了代码块高亮功能**

# 3.4

**调整主题样式，添加友链**

# 3.5

**增添评论**

# 2026.01.20

**博客购买域名和 CDN 加速**

---

# 网站构建和部署

这一项 CSDN 等各个平台上都有详细的教程，在此便不再讲述，因此在此写出应用 butterfly 主题后的美化和常见问题

## 1.主题页面的美化

在这里插入主题图片

```yml
backgound:

//如果为 backgound_img: 则直接将其改为 backgound:
```

将这些全部设为 transparent，意为透明

```yml
footer_img: transparent
default_top_img: transparent
index_img: transparent
```

这时会有黑色半透遮罩,在这里调整即可

```yml
mask:
  header: false
  footer: true
```

在文章的 md 文档里设置 front-matter 中的 top_img

```
top_img: transparent
```

否则文章顶部会显示 cover 的图片
