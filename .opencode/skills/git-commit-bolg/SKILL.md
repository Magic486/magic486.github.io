# Skill: 叶落的笔记与博客同步创建 + Git 推送

## 触发条件

当用户（叶落/magic486）说"帮我写一篇笔记/博客"、"帮我写一篇文章"、"记录一下xxx"、"把这个写成博客"等类似意图时，执行以下流程。

---

## 环境信息

| 项目 | 路径 |
|------|------|
| **Obsidian 笔记目录** | `C:\Users\yelfs\Desktop\笔记` |
| **Hexo 博客目录** | `C:\Users\yelfs\Desktop\My-Blog` |
| **博客文章目录** | `C:\Users\yelfs\Desktop\My-Blog\source\_posts` |
| **GitHub 博客仓库** | `git@github.com:Magic486/magic486.github.io.git` |
| **GitHub 用户名** | `Magic486`（注意大写 M） |
| **博客域名** | `https://www.magic486.top` |
| **笔记按文件夹分类** | 数据库/、人工智能学习/、计算机网络/、操作系统原理/、java/、力扣刷题/、课题组/、博客/、偷懒小技巧/…… |
| **博客现有标签** | C++、算法、博客构建、Linux、python学习、排版、爬虫、文件插入本地测试、有趣的小玩意 |

---

## 操作流程

### 第 1 步：确定主题和分类

- 如果主题匹配已有的笔记文件夹（如数据库相关内容），笔记放入对应文件夹
- 如果没有匹配的文件夹，询问用户想放在哪个文件夹，或新建一个
- 博客标签取分类文件夹名（如 `数据库`），多标签用逗号分隔

### 第 2 步：同时撰写两份文件

**笔记文件**（`C:\Users\yelfs\Desktop\笔记\{分类}/{文件名}.md`）：
- 纯 Markdown 格式，不需要 Frontmatter
- 适合 Obsidian 阅读格式
- 如果有代码块，写清楚语言标注

**博客文件**（`C:\Users\yelfs\Desktop\My-Blog\source\_posts\{标签}/{文件名}.md`）：
- 必须包含 YAML Frontmatter 头部
- 内容与笔记保持一致，格式可以略有调整以适应博客

#### Frontmatter 模板

```yaml
---
title: 文章标题（清晰明了，适合SEO）
tags: 标签1, 标签2
date: YYYY-MM-DD
top_img: transparent
comments: true
---
```

#### 标签规则

- **标签即文件夹名**：博客文件放入 `source/_posts/{标签}/` 子文件夹
- 第一个标签决定文件夹名（如 `tags: C++, 笔记` → 文件夹 `C++/`）
- 没有标签时放入 `未分类/`
- 标签名中不能有 `\ / : * ? " < > |` 这些字符

#### 文件名规则

- 使用有意义的短命名，如 `Git基本操作速查.md`、`逻辑回归推导笔记.md`
- 不要用 `未命名.md`、`test.md` 等无意义名称

### 第 3 步：Git 推送博客

在 `C:\Users\yelfs\Desktop\My-Blog` 目录执行：

```bash
git add -A
git commit -m "publish: {文章标题} via AI"
git push origin main
```

推送后 GitHub Actions 会自动运行 `hexo generate` 并部署到 `gh-pages` 分支，1-3 分钟后博客更新。

### 第 4 步：反馈给用户

告诉用户：
- 笔记已保存到 `笔记/{分类}/{文件名}.md`
- 博客已推送到 GitHub，约 1-3 分钟后在 `https://www.magic486.top` 可见

---

## 博客文章格式要求

### 标题层级

- `#` 文章大标题（只有这一个一级标题）
- `##` 主要章节
- `###` 子章节

### 代码块

````markdown
```python
print("Hello World")
```
````

**一定要标注语言**（python、cpp、java、sql、bash、yaml、json 等）。

### 图片处理

- 笔记中可以用本地路径：`![描述](assets/img.png)` 或 `![[img.png]]`
- 博客中如果有本地图片引用，后续用户可以用插件自动上传到图床
- 如果图片已在图床，直接用 CDN 链接（Gitee raw 或 jsDelivr）

### 格式偏好

- 用户偏好中文标点
- 列表用 `-` 无序列表
- 强调用 `**粗体**` 或 `==高亮==`
- 引用用 `>`
- 表格尽量精简
- 不要使用 emoji（除非用户要求）
- 不要添加"总结"章节（除非用户要求）

---

## 完整示例

### 用户说："帮我写一篇 Git 常用命令的笔记，顺便发到博客"

**笔记文件** `C:\Users\yelfs\Desktop\笔记\偷懒小技巧\Git常用命令速查.md`：

```markdown
# Git 常用命令速查

## 基础操作

### 初始化与克隆
```bash
git init                  # 初始化仓库
git clone <url>           # 克隆远程仓库
```

### 暂存与提交
```bash
git add .                 # 暂存所有更改
git commit -m "message"   # 提交更改
```

## 分支操作

```bash
git branch                # 查看分支
git checkout -b <name>    # 创建并切换分支
git merge <branch>        # 合并分支
```

## 远程操作

```bash
git push origin main      # 推送
git pull origin main      # 拉取
```
```

**博客文件** `C:\Users\yelfs\Desktop\My-Blog\source\_posts\偷懒小技巧\Git常用命令速查.md`：

```markdown
---
title: Git 常用命令速查
tags: 偷懒小技巧
date: 2026-04-30
top_img: transparent
comments: true
---

# Git 常用命令速查

工欲善其事，必先利其器。整理一份最常用的 Git 命令速查表。

## 基础操作

### 初始化与克隆
```bash
git init                  # 初始化仓库
git clone <url>           # 克隆远程仓库
```

...（内容与笔记相同）...
```

**执行推送**：

```bash
git -C "C:\Users\yelfs\Desktop\My-Blog" add -A
git -C "C:\Users\yelfs\Desktop\My-Blog" commit -m "publish: Git常用命令速查 via AI"
git -C "C:\Users\yelfs\Desktop\My-Blog" push origin main
```

---

## 注意事项

1. **两份文件都要写**：笔记目录一份（无 Frontmatter），博客目录一份（有 Frontmatter）
2. **博客一定要加 Frontmatter**：没有 Frontmatter 的文章 Hexo 无法正确渲染
3. **Git 推送是博客目录的**：只推送 `My-Blog` 仓库，笔记目录不是 Git 仓库
4. **日期用当天**：`date` 字段填执行当天的日期
5. **commit message 带文章标题**：方便在 Git log 里快速定位
6. **先建目录再写文件**：如果博客的子文件夹（如 `偷懒小技巧/`）不存在，用 `mkdir -p` 创建
7. **SSH 是首选的推送方式**：`git push` 使用 SSH（`git@github.com:Magic486/...`），如果 SSH 不可用则用 HTTPS

---

## 快速参考

```
笔记目录: C:\Users\yelfs\Desktop\笔记\{分类}\{文件名}.md
博客目录: C:\Users\yelfs\Desktop\My-Blog\source\_posts\{标签}\{文件名}.md
博客仓库: git@github.com:Magic486/magic486.github.io.git
博客网址: https://www.magic486.top
Git 路径: C:\Users\yelfs\Desktop\My-Blog
```
