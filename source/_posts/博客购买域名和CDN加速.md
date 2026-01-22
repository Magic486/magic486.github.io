---
title: 博客购买域名和CDN加速
tags: 博客构建
date: 2026-01-20

top_img: transparent
comments: false
---

## 🚀 第一部分：核心部署流程 (Standard Procedure)

### 一、 基础设施搭建 (域名与 CDN)

1. **购买域名**
   - 平台：阿里云等。
   - 推荐：`.top` 域名（首年便宜）。
   - 状态：需完成实名认证/备案。

2. **Cloudflare 托管 DNS (接管解析权)**
   - **添加站点**：在 Cloudflare 添加你的域名，选择 Free 计划。
   - **修改 Nameservers**：复制 Cloudflare 提供的两个 NS 地址 -> 去阿里云控制台 -> 修改 DNS 服务器 -> 替换并保存。
   - **等待生效**：Cloudflare 邮件通知 "Status: Active" 即成功。

3. **Cloudflare 解析与安全设置 (关键)**
   - **添加 CNAME 记录**：
     - `@` -> `你的用户名.github.io` (开启小黄云 Proxy)
     - `www` -> `你的用户名.github.io` (开启小黄云 Proxy)
   - **SSL 设置 (必做)**：
     - 位置：SSL/TLS -> Overview。
     - 设置：必须选 **Full** 或 **Full (Strict)**。
     - _原理：GitHub 强制 HTTPS，若选 Flexible 会导致无限重定向循环。_

### 二、 自动化流水线配置 (CI/CD)

1. **开启 GitHub 权限**
   - 位置：仓库 Settings -> Actions -> General -> Workflow permissions。
   - 操作：勾选 **Read and write permissions** -> Save。

2. **创建 Workflow 脚本**
   - 路径：`.github/workflows/deploy.yml`
   - 内容：(复制下方标准代码)

   <details>

   <summary>点击展开查看 deploy.yml 代码</summary>

   YAML

   ```
   name: Deploy Hexo Blog

   on:
     push:
       branches:
         - main  # 监控的分支

   jobs:
     build:
       runs-on: ubuntu-latest
       permissions:
         contents: write
       steps:
         - name: Checkout source
           uses: actions/checkout@v4
         - name: Setup Node.js
           uses: actions/setup-node@v4
           with:
             node-version: '20'
         - name: Install Dependencies
           run: |
             npm install -g hexo-cli
             npm install
         - name: Build Hexo
           run: hexo generate
         - name: Deploy
           uses: peaceiris/actions-gh-pages@v3
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             publish_dir: ./public
             publish_branch: gh-pages
   ```

   </details>

3. **初次推送**

   Bash

   ```
   git add .
   git commit -m "Init CI/CD"
   git push -u origin main
   ```

### 三、 绑定 GitHub Pages

1. **设置 Pages 源**
   - 位置：仓库 Settings -> Pages -> Build and deployment。
   - **Source**: Deploy from a branch。
   - **Branch**: 选择 **`gh-pages`** (注意：不是 main)。

2. **绑定自定义域名**
   - **Custom domain**: 填入 `www.你的域名.top`。
   - **Enforce HTTPS**: 勾选。

---

## ✍️ 第二部分：日常写作工作流 (Daily Workflow)

以后写文章只需要做这三步，**无需** `hexo g` 或 `hexo d`：

1. **写作**：在 VS Code 本地写好 Markdown 文章。
2. **推送**：

   Bash

   ```
   git add .
   git commit -m "新文章：吃马铃薯"
   git push
   ```

3. **等待**：喝杯水，GitHub Actions 会自动构建并发布。

---

## 🔧 第三部分：疑难杂症速查手册 (Troubleshooting)

如果遇到报错，请按错误现象对号入座：

### 🛑 现象 1：Git 报错类

| **错误信息 / 现象**                          | **解决方案**                                                                                                                                                                                  |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **fatal: not a git repository**              | **初始化仓库：**<br><br> <br><br>1. `git init`<br><br> <br><br>2. `git remote add origin <仓库地址>`<br><br> <br><br>3. `git fetch origin`<br><br> <br><br>4. `git reset --mixed origin/main` |
| **src refspec main does not match**          | **分支名不统一：**<br><br> <br><br>本地是 master，GitHub 是 main。<br><br> <br><br>执行：`git branch -m master main` 然后再 push。                                                            |
| **warning: ... LF will be replaced by CRLF** | **正常提示，忽略即可。** 这是 Windows 和 Linux 换行符的差异，Git 会自动处理。                                                                                                                 |
| **warning: node_modules/... 刷屏**           | **误传了依赖包：**<br><br> <br><br>1. 创建 `.gitignore` 文件，写入 `node_modules/`<br><br> <br><br>2. 清理缓存：`git rm -r --cached .`<br><br> <br><br>3. 重新添加：`git add .`               |

### 🛑 现象 2：网页访问类

| **错误现象**          | **核心原因**                | **解决方案**                                                                                                                                                                                                                       |
| --------------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **404 Not Found**     | **CNAME 文件丢失**          | **补全护身符：**<br><br> <br><br>1. 在 `source/` 目录下新建无后缀文件 `CNAME`。<br><br> <br><br>2. 里面只写域名（如 `www.magic486.top`）。<br><br> <br><br>3. 重新 push。                                                          |
| **网页白屏 (无内容)** | **原因 A：主题文件为空**    | **修复子模块问题：**<br><br> <br><br>1. 删除 `themes/butterfly/.git` 文件夹。<br><br> <br><br>2. `git rm -r --cached themes/butterfly`<br><br> <br><br>3. `git add themes/butterfly/*`<br><br> <br><br>4. `git add .` 后重新提交。 |
| **网页白屏 (无样式)** | **原因 B：Config URL 错误** | **修改配置文件：**<br><br> <br><br>打开根目录 `_config.yml`：<br><br> <br><br>`url: https://www.magic486.top` (必须是 https)<br><br> <br><br>`root: /`                                                                             |
| **重定向次数过多**    | **SSL 模式错误**            | **修改 Cloudflare 设置：**<br><br> <br><br>SSL/TLS 设置必须改为 **Full** 或 **Full (Strict)**。                                                                                                                                    |

---

### 💡 实用小技巧

- **快速创建目录结构**：在 VS Code 新建文件时，直接输入路径 `.github/workflows/deploy.yml`，它会自动创建中间的文件夹。
- **强制刷新缓存**：网页更新后如果没有变化，按 `Ctrl + F5` 强制刷新，或去 Cloudflare 后台点击 "Purge Everything"（清除所有缓存）。
