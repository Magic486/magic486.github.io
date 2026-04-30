---
title: Obsidian 一键部署博客插件：Blog Deploy 使用指南
tags: 工具
date: 2026-04-30
top_img: transparent
comments: false
---

## 为什么需要这个插件

我平时使用 Obsidian 记笔记，同时维护一个 Hexo 博客。以前每次想把笔记发到博客，需要手动做这些事：

1. 复制 Markdown 内容
2. 手动写 Frontmatter（标题、标签、日期）
3. 把本地图片一张张上传到图床
4. 替换所有图片链接
5. 放到 `_posts/` 对应目录
6. `git add` → `git commit` → `git push`

一套流程下来至少 5-10 分钟。于是写了这个插件，把以上步骤压缩到 **右键 → 点两下 → 等 10 分钟自动完成**。

---

## 功能一览

| 功能 | 说明 |
|------|------|
| 右键部署 | 文件浏览器右键任意 `.md` 直接部署 |
| 自动 Frontmatter | 提取 `# 标题` 为 title，文件夹名为 tag |
| 标签选择器 | 部署弹窗展示博客已有标签，点击即添加 |
| 按标签分类 | 自动在 `_posts/` 下创建标签子目录 |
| 图片自动上传 | 通过 PicGo 上传本地图片到 Gitee 图床 |
| 原笔记不动 | 所有图片路径替换只在博客副本生效 |
| 延迟队列 | 加入队列后倒计时，可批量操作后统一推送 |
| 自动 Git Push | 部署完成后自动 commit + push |

---

## 安装与配置

从 [GitHub Releases](https://github.com/Magic486/obsidian-blog-deploy/releases) 下载 `main.js` 和 `manifest.json`，放入 vault 的 `.obsidian/plugins/obsidian-blog-deploy/`，启用后在设置中配置博客本地路径和 PicGo 服务器地址即可。

---

## 工作流程

### 1. 部署单篇笔记

在 Obsidian 文件浏览器中右键 `.md` 文件 → **📤 部署到博客**：

- 弹窗自动填充标题（取自文档首个 `# 标题`）和标签（取自所在文件夹名）
- 可点击下方标签芯片快速添加或移除已有标签
- 勾选是否上传本地图片到图床
- 点击 **✅ 加入队列**

### 2. 队列管理

状态栏显示倒计时，默认 10 分钟：

```
⏳ 3 篇待部署 | 剩余 8:32 [立即推送] [取消全部]
```

支持批量操作：写多篇笔记 → 逐个加入队列 → 倒计时结束自动部署，或点「立即推送」直接部署。

### 3. 命令面板

| 命令 | 使用方式 |
|------|----------|
| 部署当前笔记到博客 | `Ctrl+P` 搜索 |
| 立即部署队列所有笔记 | `Ctrl+P` 搜索 |
| 清空部署队列 | `Ctrl+P` 搜索 |

---

## 图片上传

部署时自动检测笔记中的本地图片引用：

```
![示例图片](assets/demo.png)
![[another-image.jpg]]
```

通过 PicGo Server（`http://127.0.0.1:36677`）上传到 Gitee 图床，替换为 CDN 链接：

```
![示例图片](https://gitee.com/magic486/image/raw/master/img/202604301430_abc123.png)
```

**原 vault 笔记完全不受影响**——替换只发生在写入博客的副本中。

---

## 生成的 Frontmatter 格式

```yaml
---
title: 你的笔记标题
tags: 数据库
date: 2026-04-30
top_img: transparent
comments: false
---
```

- **title**：取笔记第一个一级标题，否则用文件名
- **tags**：自动从文件夹路径提取，可在弹窗中手动修改
- **date**：部署当天的日期

---

## 按标签分目录

部署时根据第一个标签在 `_posts/` 下创建子文件夹：

```
笔记：C++ 内存管理 (tags: C++, 内存管理)
  → 部署到：source/_posts/C++/C++ 内存管理.md

笔记：MySQL 优化 (tags: 数据库, MySQL)
  → 部署到：source/_posts/数据库/MySQL 优化.md
```

---

## 注意事项

### Gitee 图床防盗链

Gitee 图床默认有防盗链限制。解决方法：在博客 HTML `<head>` 中添加：

```html
<meta name="referrer" content="no-referrer">
```

Hexo Butterfly 主题在 `_config.butterfly.yml` 中配置 `inject.head`。

### PicGo Server

确保 PicGo 桌面端已开启 Server 模式，默认端口 `36677`。

### Git 权限

确认本地 Git 已配置好博客仓库的推送权限（推荐 SSH key）。

---

## 源码

项目开源在 [GitHub](https://github.com/Magic486/obsidian-blog-deploy)，MIT 协议。
