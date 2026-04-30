---
title: Obsidian博客部署插件使用指南
tags: 有趣的小玩意
date: 2026-04-30
top_img: transparent
comments: true
---

> 这是一个 Obsidian 第三方插件，让你在 Obsidian 里右键一下笔记，就能自动部署到 Hexo 博客。

## 一、它能做什么

以前我写完一篇笔记想发到博客，要经过这样一套流程：

1. 把笔记复制到 `My-Blog\source\_posts\`
2. 手动写 Frontmatter（标题、标签、日期……）
3. 打开终端 `git add` → `git commit` → `git push`
4. 等 GitHub Actions 自动部署

如果有图片就更麻烦，还要手动传图床、改链接。

现在这个插件把上面所有步骤自动化了：

- **右键一键入队**：在 Obsidian 文件浏览器里右键笔记 → "部署到博客"
- **自动生成 Frontmatter**：标题、标签、日期全部自动填好
- **按标签自动分类**：部署到博客时自动按标签创建子文件夹（如 `_posts/C++/`、`_posts/数据库/`）
- **自动上传图片**：笔记里的本地图片自动通过 PicGo 上传到图床，替换为 CDN 链接（**原笔记不会修改**）
- **批量延迟部署**：多篇笔记加入队列，10 分钟后统一推送（也可以立即推送）
- **自动 Git 推送**：`git add` → `git commit` → `git push` 全自动

---

## 二、安装

### 2.1 下载插件文件

去 GitHub Releases 页面下载两个文件：

- `main.js`
- `manifest.json`

> 地址：https://github.com/Magic486/obsidian-blog-deploy/releases

### 2.2 放入 Obsidian 插件目录

把这两个文件放到你的 Obsidian vault 的 `.obsidian/plugins/obsidian-blog-deploy/` 文件夹里：

```
你的vault/
└── .obsidian/
    └── plugins/
        └── obsidian-blog-deploy/
            ├── main.js
            └── manifest.json
```

如果 `obsidian-blog-deploy` 文件夹不存在，手动新建一个。

### 2.3 启用插件

1. 打开 Obsidian
2. 进入 **设置** → **第三方插件**
3. 找到 **Blog Deploy** → 点击启用

---

## 三、配置 PicGo（图片上传需要）

如果你笔记里有本地图片，需要先安装并配置 PicGo。

### 3.1 下载安装 PicGo

去 [PicGo 官网](https://molunerfinn.com/PicGo/) 下载 Windows 版，安装后启动。

### 3.2 配置 Gitee 图床（推荐）

> 为什么用 Gitee 而不是 GitHub：国内网络环境下 Gitee 上传更稳定。

1. 打开 PicGo → 左侧点击 **插件设置**
2. 搜索 `gitee`，安装 `gitee-uploader` 插件
3. 安装后左侧会出现 **图床设置 → Gitee**

然后去 Gitee 创建一个仓库来存图片（比如 `你的用户名/blog-images`），再生成一个私人令牌：

- Gitee → 设置 → 私人令牌 → 生成新令牌
- 权限勾选 `projects`
- 复制生成的 Token

回到 PicGo 填写 Gitee 图床配置：

| 设置项 | 填写内容 |
|--------|----------|
| repo | `你的用户名/仓库名` |
| branch | `master` |
| token | 刚才复制的 Gitee Token |
| path | `img/`（图片存在仓库的 img 文件夹下） |

填完后点 **设为默认图床**。

### 3.3 开启 PicGo Server

插件是通过调用 PicGo 的本地 HTTP 服务器来上传图片的，所以必须开启：

- PicGo → 设置 → **Server 设置**
- 确保 **开启 Server** 是打开状态
- 端口保持默认 `36677`

---

## 四、配置插件

打开 Obsidian → **设置** → **Blog Deploy**：

| 设置项 | 默认值 | 说明 |
|--------|--------|------|
| 博客本地路径 | `C:\Users\yelfs\Desktop\My-Blog` | 你的 Hexo 博客 Git 仓库路径 |
| 文章子目录 | `source\_posts` | posts 文件夹的相对路径 |
| 部署延迟（分钟） | `10` | 加入队列后等多久自动推送（0 = 立即） |
| 自动 Git Push | 开启 | 推送后自动 commit + push |
| PicGo 服务器地址 | `http://127.0.0.1:36677` | 一般不需要改 |

---

## 五、使用方法

### 5.1 基本操作：部署一篇笔记

**第 1 步**：在 Obsidian 左侧文件浏览器里，**右键** 要部署的 `.md` 文件，选择 **📤 部署到博客**。

**第 2 步**：弹出部署确认窗口：

```
┌─────────────────────────────────┐
│  📤 部署到博客                  │
│                                 │
│  文件：test.md                  │
│  标题：[test__________________] │
│  标签：[C++___________________] │
│                                 │
│  已有标签：                     │
│  [C++] [算法] [Linux] [排版]    │
│  [python学习] [爬虫] [博客构建] │
│                                 │
│  ☑ 🖼️ 上传本地图片到图床 (PicGo)│
│  目标位置：source/_posts/C++/te │
│                                 │
│              [取消]  [✅ 加入队列]│
└─────────────────────────────────┘
```

- **标题**：自动取笔记中的第一个 `# 标题`，可手动修改
- **标签**：自动根据笔记所在文件夹名检测（如 `C++/xxx.md` → 标签 `C++`），可手动修改
- **已有标签**：点击下方的标签芯片，快速添加或移除标签
- **上传图片**：默认勾选，笔记中的本地图片会自动上传到 PicGo 图床

**第 3 步**：确认无误后，点击 **✅ 加入队列**。

**第 4 步**：Obsidian 底部状态栏显示：

```
⏳ 1 篇待部署 | 剩余 9:58  [立即推送] [取消全部]
```

此时你可以继续写别的笔记，继续右键加入队列。每新加一篇，倒计时会重置为 10 分钟。

**第 5 步**：倒计时归零后自动执行：
1. 给笔记添加 Frontmatter（标题、标签、日期等）
2. 复制到 `My-Blog/source/_posts/标签子文件夹/`
3. 上传笔记中的本地图片到图床，替换为 CDN 链接
4. `git add` → `git commit` → `git push`
5. GitHub Actions 自动 `hexo generate` + 部署

弹窗提示：`✅ 成功部署 1 篇笔记，1 张图片已上传，已推送至 GitHub`

### 5.2 立即推送

不想等倒计时？点状态栏的 **立即推送**，或按 `Ctrl+P` 搜索 `立即部署队列中的所有笔记`。

### 5.3 取消部署

点状态栏的 **取消全部**，或按 `Ctrl+P` 搜索 `清空部署队列`。

### 5.4 查看队列

按 `Ctrl+P` 搜索 `查看部署队列`，会弹出当前等待部署的笔记列表。

### 5.5 已有标签快速选择

部署弹窗会扫描你博客 `_posts/` 下所有文章已有的标签，展示为可点击的标签芯片。点击即可添加或移除，比自己打字更快。

### 5.6 命令面板总览

| 命令 | 快捷键 | 作用 |
|------|--------|------|
| 部署当前笔记到博客 | Ctrl+P | 部署当前打开的笔记 |
| 立即部署队列中的所有笔记 | Ctrl+P | 跳过倒计时 |
| 清空部署队列 | Ctrl+P | 取消所有 |
| 查看部署队列 | Ctrl+P | 列表查看 |

---

## 六、标签文件夹整理

插件部署文章时会自动按标签在 `_posts/` 下创建子文件夹：

```
source/_posts/
├── C++/
│   ├── first-blog.md
│   └── C++1.md
├── 博客构建/
│   └── 博客购买域名和CDN加速.md
├── 算法/
│   └── eigth-blog.md
├── 数据库/
│   └── 基本操作.md
└── 未分类/          ← 没有标签的文章放这里
```

**不用担心 URL 变**：Hexo 的 `permalink` 是根据文章头部的 `date` 和 `title` 生成 URL 的，和文件在哪个子文件夹里没关系。

---

## 七、关于图片的详细说明

### 7.1 原笔记不会被修改

这是最重要的设计原则：**插件的图片上传和链接替换，只发生在博客副本中**。你的 Obsidian vault 里的原笔记完全不受影响，本地图片路径保持原样。

### 7.2 支持哪些图片引用格式

| 格式 | 示例 | 是否支持 |
|------|------|----------|
| Markdown 本地相对路径 | `![截图](assets/img.png)` | ✅ |
| Markdown 本地绝对路径 | `![截图](C:\Users\...\img.png)` | ✅ |
| Obsidian Wikilink | `![[img.png]]` | ✅ |
| 已是 CDN 链接 | `![截图](https://gitee.com/.../img.png)` | 跳过，不需重复上传 |

### 7.3 不想上传图片怎么办

部署弹窗里取消勾选 **🖼️ 上传本地图片到图床 (PicGo)**，插件就只复制文件不处理图片。

---

## 八、防盗链配置（使用 Gitee 图床必做）

Gitee 对图片有防盗链限制。如果博客里 Gitee 图片不显示，在 `_config.butterfly.yml` 中添加：

```yaml
inject:
  head:
    - <meta name="referrer" content="no-referrer">
```

添加后 `git push`，等 GitHub Actions 部署完成即可。

---

## 九、常见问题

**Q：右键菜单里没有"部署到博客"？**

A：确认插件已在设置中启用；确认右键的是 `.md` 文件而不是文件夹。

**Q：点击"加入队列"没反应？**

A：可能是博客路径设置不对，检查设置中的「博客本地路径」是否存在。

**Q：图片上传失败？**

A：检查 PicGo 是否在运行、Server 是否开启、默认图床是否为 Gitee。

**Q：git push 失败？**

A：确认博客路径下有 `.git` 文件夹，且 Git 已配置远程仓库和推送权限。

---

## 十、源码与反馈

- 插件源码：https://github.com/Magic486/obsidian-blog-deploy
- 有问题欢迎提 Issue
