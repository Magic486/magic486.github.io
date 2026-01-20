---
title: 博客购买域名和CDN加速
tags: 博客构建
top_img: transparent
comments: false
---

# 一.Cloudflare 的免费 CDN加速服务
## 1.阿里云购买.top域名
我的博客链接[我吃马铃薯](https://www.magic486.top/)

由于.top结尾的域名最为便宜，首年只需要1块钱，所以我就购买了这个
注：购买域名需要备案，一般20分钟内就可以搞定
[域名控制台](https://dc.console.aliyun.com/?spm=5176.27097949.J_9138996270.1.52cb4b59NCGbuc#/domain-list/all)

## 2.将“域名解析权”交给 Cloudflare
1. **在 Cloudflare 中添加域名：**
    
    - 登录 [Cloudflare](https://dash.cloudflare.com/)，点击右上角的 **“添加站点 (Add site)”**。
        
    - 输入你刚买的域名（比如 `yexufeng.top`），点击继续。
        
    - 选择 **“Free (免费)”** 计划，点击继续。
        
    - Cloudflare 会扫描你当前的 DNS 记录，直接拉到底部点击继续。
        
    - **关键点来了：** Cloudflare 会给你两个 **Nameservers (名称服务器)**，通常长这样：`linda.ns.cloudflare.com` 和 `tom.ns.cloudflare.com`。**复制这两个地址。**
        
2. **去你买域名的平台修改 DNS：**
    
    - 登录你买域名的平台（比如阿里云控制台）。
        
    - 找到你的域名列表，点击“管理”或“解析”。
        
    - 找到 **“DNS 服务器”** 或 **“修改 DNS/Nameservers”** 的选项。
        
    - 把里面默认的地址删掉，填入刚才在 Cloudflare 复制的两个地址。
        
    - 保存。
        
3. **等待生效：** 回到 Cloudflare 点击“检查名称服务器”。这个过程大概需要 5-15 分钟（最长 24 小时）。当你收到 Cloudflare 发来的邮件说“Status: Active”，就成功了。
## 3. 在 Cloudflare 中添加 CNAME 记录（并点亮小黄云）

现在 Cloudflare 已经接管了你的域名，我们需要告诉它：访问这个域名时，去哪个服务器拿数据。

1. 进入 Cloudflare 后台，点击你的域名，左侧菜单选择 **“DNS” -> “记录 (Records)”**。
    
2. 点击 **“添加记录 (Add record)”**。我们要添加**两条**记录，确保加了 `www` 的和不加的都能访问。
    
    - **第一条记录（针对 yexufeng.top）：**
        
        - 类型 (Type)：选 **CNAME**
            
        - 名称 (Name)：填 **`@`** （代表主域名）
            
        - 目标 (Target)：填你的 GitHub Pages 地址，例如 **`你的用户名.github.io`**
            
        - 代理状态 (Proxy status)：**确保“小云朵”图标是橘黄色的（Proxy 开启状态）**。
            
        - 点击保存。
            
    - **第二条记录（针对 www.yexufeng.top）：**
        
        - 类型 (Type)：选 **CNAME**
            
        - 名称 (Name)：填 **`www`**
            
        - 目标 (Target)：同样填 **`你的用户名.github.io`**
            
        - 代理状态：**同样点亮小云朵**。
            
        - 点击保存。
## 4. 调整 Cloudflare 的 SSL 安全设置

在进行 GitHub 设置前，这一步如果不做，你的网站**绝对会打不开**（会出现“重定向次数过多”错误）。

1. 在 Cloudflare 左侧菜单选择 **“SSL/TLS” -> “概述 (Overview)”**。
    
2. 将 SSL 加密模式从默认的“灵活 (Flexible)”修改为 **“完全 (Full)”** 或者 **“完全严格 (Full (Strict))”**。
    

_原理解释：GitHub Pages 本身强制开启了 HTTPS 安全连接。如果你选“灵活”，Cloudflare 会用 HTTP（不安全）去访问 GitHub，GitHub 会强制把它跳回 HTTPS，形成无限死循环。

## 5.在 GitHub 中认领你的新域名

最后一步，告诉 GitHub 服务器：“嘿，以后如果有通过 `yexufeng.top` 来的请求，你把它对应到我的博客仓库里。”

1. 打开你的博客 GitHub 仓库，点击顶部的 **Settings (设置)**。
    
2. 在左侧菜单点击 **Pages**。
    
3. 在 **Custom domain (自定义域名)** 框里，填入你的域名（建议填带 www 的，比如 `www.yexufeng.top`）。
    
4. 点击 **Save**。
    
5. GitHub 会开始验证 DNS（“DNS check in progress...”）。因为你走了 Cloudflare 代理，这里可能需要等几分钟。
    
6. 下方有一个 **“Enforce HTTPS”** 的选项，**务必勾选它**。

# 二.CI/CD 自动化部署

### 步骤：本地写 Markdown -> `git push` -> GitHub Actions 自动构建 -> 推送到 `gh-pages` 分支 -> 网站更新。

#### 1. 准备工作：检查你的 GitHub 仓库设置

由于 GitHub 的安全策略更新，你需要先开启 Action 的写权限：

1. 进入你的博客仓库 -> **Settings**。
2. 在左侧栏找到 **Actions** -> **General**。
3. 滚动到底部 **Workflow permissions**。
4. 勾选 **"Read and write permissions"**。
5. 点击 **Save**。
#### 2. 创建自动化脚本（Workflow）

你需要告诉 GitHub 怎么帮你在云端干活。

1. 在你的本地博客根目录下，新建目录和文件：`.github/workflows/deploy.yml`。
2. 将以下代码复制进去（这是一个标准的 Hexo 自动化脚本）：

YAML

```
name: Deploy Hexo Blog

on:
  push:
    branches:
      - main  # 监控的分支：这里填你存放源码的分支名，可能是 main 或 master

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # 1. 把代码拉取到虚拟机
      - name: Checkout source
        uses: actions/checkout@v4

      # 2. 安装 Node.js 环境
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20' # 推荐使用较新的 LTS 版本

      # 3. 安装 Hexo 和依赖
      - name: Install Dependencies
        run: |
          npm install -g hexo-cli
          npm install

      # 4. 生成静态文件 (相当于你在本地敲 hexo g)
      - name: Build Hexo
        run: hexo generate

      # 5. 把生成的文件推送到 gh-pages 分支
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3 # 这是一个在大名鼎鼎的第三方 Action
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }} # GitHub 自动生成的令牌，无需手动设置
          publish_dir: ./public # Hexo 生成文件的目录
          publish_branch: gh-pages # 推送到哪个分支，通常是 gh-pages
          commit_message: ${{ github.event.head_commit.message }}
```

#### 3. 提交代码，触发第一次构建

1. 在本地执行 git 命令：
    ```Bash
    git add .
    git commit -m "Add CI/CD workflow"
    git push origin main
    ```
2. 打开 GitHub 仓库页面，点击顶部的 **Actions** 标签。
3. 你应该能看到一个正在转圈的任务（Deploy Hexo Blog）。如果是绿色对勾 ✅，说明构建成功！

#### 4. 调整 GitHub Pages 设置

1. 进入仓库 **Settings** -> **Pages**。
2. 在 **Build and deployment** 下的 **Source** 选择 **Deploy from a branch**。
3. **Branch** 选择 **`gh-pages`** (注意：不是 main，因为 Action 把生成的 HTML 放在了 gh-pages 里)。
4. 保存。

### 第三部分：最后一步，Cloudflare 设置（至关重要）

这步做不对，网站会无限重定向或者打不开。

1. **域名解析：** 在 Cloudflare DNS 设置里，添加 CNAME 记录，指向 `你的用户名.github.io`，并**开启小黄云（Proxy）**。
    
2. **SSL 设置（这是最大的坑）：**
    
    - 进入 Cloudflare 后台 -> **SSL/TLS** -> **Overview**。
        
    - **必须**将加密模式设置为 **Full** 或 **Full (Strict)**。
        
    - _解释：如果选 Flexible，Cloudflare 访问 GitHub Pages 走 HTTP，而 GitHub Pages 强制 HTTPS，两者会打架导致网页无限循环进不去。_
        

### 总结你的新工作流

配置好这一切后，你以后的写博客流程就是：

1. 本地 VScode 写好 `xx.md`。
2. 终端输入：
    ```bash
    git add .
    git commit -m "新文章：吃马铃薯"
    git push
    ```
3. **结束。** 去喝杯水，1分钟后，GitHub Actions 会自动帮你编译，Cloudflare 会自动帮你加速，你的网站在全球（包括国内）就都能访问了。

### 问题列举

### 1.如何创建自动化脚本

VS Code 有个隐藏的小技巧，可以一次性把文件夹和文件都建好。

1. **打开项目：** 用 VS Code 打开你的博客根目录（就是能看到 `source`, `scaffolds`, `themes` 这些文件夹的地方）。
2. **新建文件：**
    - 在左侧的文件资源管理器空白处，点击右键 -> **“新建文件” (New File)**。
        
    - 或者直接点击该区域顶部那个“带加号的文件图标”。
        
3. **输入魔法路径：**
    - 在输入框里，不要只写文件名，而是直接输入完整的路径：
        
    - `.github/workflows/deploy.yml`
        
    - **按下回车**。
4. **见证奇迹：** VS Code 会自动检测到斜杠 `/`，然后帮你自动新建好 `.github` 文件夹，再在里面建好 `workflows` 文件夹，最后创建好 `deploy.yml` 文件。
5. **粘贴代码：** 现在把之前给你的那一长串 YAML 代码粘贴进去，保存即可。

### 2.fatal: not a git repository (or any of the parent directories): .git

当前的文件夹 `My-Blog` 还没有被 Git “初始化”或者“管理”，它只是一个普通的 Windows 文件夹。因为这个文件可能是直接点击 GitHub 网页上的 "Download ZIP" 下载的代码，或者不小心删除了隐藏的 `.git` 文件夹。

#### 1. 初始化仓库（开户）

让 Git 开始管理这个文件夹。

```
git init
```

_执行后，你会看到提示 `Initialized empty Git repository in ...`_

#### 2. 关联远程仓库

我们需要告诉本地 Git，它的“远程总部”在哪里。 你需要去 GitHub 复制你的仓库 HTTPS 地址（通常是 `https://github.com/你的用户名/你的仓库名.git`）。

```
# 例如：git remote add origin https://github.com/magic486/magic486.github.io.git
git remote add origin <你的GitHub仓库地址>
```

#### 3. 同步远程记录

**这一步很关键！** 因为你的本地认为是“全新的”，但远程仓库里已经有东西了。我们要把远程的历史记录“拉”下来，覆盖到本地的 Git 记录中，但**保留你本地的文件修改**。

```
# 先把远程的分支信息抓取下来
git fetch origin

# 这里的 main 是你的分支名，老仓库可能是 master，你可以去 GitHub 页面看一眼
# 这条命令的意思是：重置 Git 记录，让它认为你就是基于远程 main 分支工作的
git reset --mixed origin/main
```

> **注意：** 如果提示 `origin/main` 不存在，试着把 `main` 换成 `master`。

#### 4. 再次提交

现在，你的文件夹已经变成了一个正常的 Git 仓库，且包含了你刚才写的 `.yml` 文件。再次执行你之前的命令：

```
git add .
git commit -m "Add CI/CD workflow"
git push -u origin main
```

### 3.warning刷屏：warning: in the working copy of 'node_modules/core-js/modules/es6.object.is-sealed.js', LF will be replaced by CRLF the next time Git touches it

❌ 你正在尝试把 `node_modules` 文件夹上传到 GitHub！

### 为什么这是一个错误？

1. **体积巨大：** `node_modules` 里包含了成千上万个依赖包的文件（就像你看到的，刷了几千行警告），这会让你的仓库变得巨大无比，上传速度极慢。
2. **没必要：** 只要有 `package.json` 文件，任何人（包括服务器）都可以通过运行 `npm install` 自动下载这些依赖。**不需要**把它们存到 Git 里。
3. **跨平台问题：** 这些依赖包里有很多针对特定系统（Windows/Mac/Linux）的文件。你把 Windows 下的依赖传上去，服务器（通常是 Linux）可能根本跑不起来。

我们要做的就是告诉 Git：**“忽略 `node_modules` 这个文件夹，不要管它，也不要上传它。”**
#### 第一步：创建或修改 `.gitignore` 文件

1. 看看你的博客根目录下，有没有一个叫 `.gitignore` 的文件？
    - **如果有：** 用 VS Code 打开它。
        
    - **如果没有：** 在根目录下新建一个文件，名字就叫 `.gitignore`（注意前面有个点）。
        
2. 在 `.gitignore` 文件里，**单独一行**写入以下内容：
    Plaintext
    ```
    node_modules/
    ```
    _(通常 Hexo 默认的 .gitignore 应该包含 public/ 和 db.json 等，你把 node_modules/ 加进去就行)_
    **保存文件。**

#### 第二步：清理 Git 缓存

因为你之前可能已经执行过 `git add .`，Git 已经把 `node_modules` 里的文件“记在小本本上”了。光改 `.gitignore` 不足以让它“遗忘”。

你需要执行以下命令来**撤销对这些文件的追踪**：

1. 在终端里执行（这会把所有文件从暂存区移除，但**不会删除**你本地的实际文件）：
    ```
    git rm -r --cached .
    ```
    
2. 重新添加文件（这次 Git 会看到 `.gitignore`，就会自动忽略 `node_modules` 了）：
    ```
    git add .
    ```
    _此时你应该不会再看到那几千行的 `node_modules` 警告了。_

### 4.error: src refspec main does not match any
你遇到了 **“名字对不上”** 的问题：

1. **你的现状：** 你的终端第一行显示 `The current branch master`。这说明你本地的“存折”名字叫 **`master`**（这是 Git 以前的默认老名字）。
    
2. **你的操作：** 你输入了 `git push origin main`。你在告诉 Git：“把那个叫 **`main`** 的存折存到银行去”。
    
3. **结果：** Git 懵了，它翻遍了你的口袋，发现你只有 `master`，没有 `main`，于是报错 `src refspec main does not match any`（意思是：我找不到你说的那个叫 main 的东西）。
    

而且，还有一个**极其重要**的隐患：我之前给你的自动化脚本 (`deploy.yml`) 里写的是 `on: push: branches: - main`。如果你强行推送到 `master`，**你的自动化脚本根本不会启动！**

为了配合 GitHub 的新标准，也为了配合你已经写好的自动化脚本，我们最简单的办法是**把本地的 `master` 改名为 `main`**。

请依次执行下面两条命令：

#### 第一步：给本地分支改名

PowerShell

```
git branch -m master main
```

_这条命令没有输出是正常的，它默默地把名字改好了。_

#### 第二步：再次推送

PowerShell

```
git push -u origin main
```

### 5.404页面

原因通常有两个：**GitHub Pages 的设置被重置了**，或者 **CNAME 文件被机器人误删了**。

### 第一步：检查 GitHub Pages 的“分支”设置（最可能的去处）

因为你是第一次用 Action 自动推送，GitHub 可能还没反应过来要切换分支。

1. 打开你博客的 GitHub 仓库页面。
    
2. 点击顶部的 **Settings** (设置)。
    
3. 在左侧菜单点击 **Pages**。
    
4. 看 **Build and deployment** 这一栏：
    
    - **Source:** 确保选的是 **Deploy from a branch**。
        
    - **Branch:** 这里是关键！必须选择 **`gh-pages`** 分支（**不是** `main` 或 `master`）。
        
    - _解释：你的 `main` 分支放的是源码（Markdown），而 Action 自动生成的 HTML 网页是放在 `gh-pages` 分支里的。GitHub 得去 `gh-pages` 拿网页才能显示。_
        
5. 点击 **Save**。
    

### 第二步：检查“自定义域名”是否被清空（这是个坑）

自动化部署（Action）有个“坏毛病”：它每次重新生成网页，都会把 `gh-pages` 分支清空重写。如果你本地的源码里没有保存域名配置，Action 就会把线上的域名配置给**抹掉**。

1. 还在刚才的 **Pages** 设置页面，往下看 **Custom domain** 一栏。
    
2. **检查这里是不是变空了？**
    
    - **如果是空的：** 请重新填入 `www.magic486.top`，然后点击 **Save**。等待几分钟，网站就能访问了。
        
    - **如果没空：** 尝试把里面的内容清空，保存，然后再填进去，保存（相当于重启一下）。
        

---

### 🔥 终极补丁：防止下次更新又挂掉（必做！）

如果你刚才发现 **Custom domain** 确实被清空了，说明你**本地的源码里缺少了一个关键文件**。如果你不补上这个文件，下次你写新文章一推送，网站又会 404。

请在你的 VS Code 里做这件事：

1. 找到你的博客根目录下的 **`source`** 文件夹。
    
2. 在 `source` 文件夹里新建一个文件，文件名叫 **`CNAME`**（注意：**全大写，没有任何后缀名**，不是 `CNAME.txt`）。
    
3. 用记事本打开它，在里面只写一行字：
    
    Plaintext
    
    ```
    www.magic486.top
    ```
    
4. 保存。
    
5. 执行 Git 三连，把这个“护身符”推送到云端：
    
    PowerShell
    
    ```
    git add .
    git commit -m "Add CNAME file"
    git push
    ```

### 6.网页白屏

两个原因

一：
#### 第一步：删除“内鬼” `.git` 文件夹

1. 在你的电脑上打开博客文件夹，进入 `themes` -> `butterfly` 文件夹。
    
2. **开启“显示隐藏文件”：**
    
    - 如果你是 Windows 11：点击顶部菜单栏的 `查看` -> `显示` -> 勾选 `隐藏的项目`。
        
    - 如果你是 Windows 10：点击顶部菜单栏的 `查看` -> 勾选 `隐藏的项目`。
        
3. 你会看到一个半透明的 **`.git`** 文件夹（注意：只删除 `themes/butterfly` 里面的这一个，**千万不要**删除博客根目录下的那个）。
    
4. **选中这个 `.git` 文件夹，直接删除（Delete）。**
    

#### 第二步：告诉 Git 重新识别这些文件

回到你的 VS Code 终端（PowerShell），依次执行下面 4 行命令：

1. **清除错误的记录（解除之前的子仓库绑定）：**
    
    PowerShell
    
    ```
    git rm -r --cached themes/butterfly
    ```
    
    _（如果这条报错说 pathspec 不匹配，没关系，直接做下一步。）_
    
2. **强制添加所有文件（这一步很关键）：**
    
    PowerShell
    
    ```
    git add themes/butterfly/*
    ```
    
    _（注意：这行命令最后有个星号 `*`。如果提示 warnings 关于 LF/CRLF，直接忽略。）_
    
3. **再次把所有变动加进去：**
    
    PowerShell
    
    ```
    git add .
    ```
    
4. **提交并推送：**
    
    PowerShell
    
    ```
    git commit -m "Fix: Upload theme files"
    git push
    ```

二：
#### 第一步：修改 Hexo 配置文件（90% 是因为这个）

Hexo 生成网页时，会根据配置文件里的 `url` 来生成资源链接。如果这里还是旧的 GitHub 地址，或者写的是 `http` 而不是 `https`，浏览器就会拦截资源，导致白屏。

1. 在 VS Code 中打开你的博客根目录下的 **`_config.yml`** 文件。
    
2. 找到 **`# URL`** 这一部分。
    
3. 修改以下两行（**务必完全一致**）：
    
    YAML
    
    ```
    # Replace the following with your url
    url: https://www.magic486.top  # 注意：必须是 https，且要是你的新域名
    root: /
    ```
    
    _(以前你可能设置过子目录，现在有了独立域名，`root` 必须是 `/`)_
    
4. 保存文件。
    

#### 第二步：推送更新

修改完配置后，我们需要重新让 GitHub Actions 构建一次。

1. 在终端执行：
    
    PowerShell
    
    ```
    git add .
    git commit -m "Fix: Update site URL in config"
    git push
    ```
    
2. 等待 1-2 分钟，让 Action 跑完。
    

#### 第三步：清理 Cloudflare 缓存（这一步至关重要！）

Cloudflare 会缓存你的网页。如果不清理，就算你修好了，它可能还是给你展示之前那个坏掉的“白屏版”。

1. 登录 [Cloudflare 后台](https://dash.cloudflare.com/)。
    
2. 点击你的域名 `magic486.top`。
    
3. 在左侧菜单点击 **“缓存 (Caching)”** -> **“配置 (Configuration)”**。
    
4. 点击蓝色的 **“清除所有内容 (Purge Everything)”** 按钮。
    
5. 等待 30 秒。