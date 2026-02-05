---
title: obsidian插入图片教程
tags: 排版
date: 2026-02-05
top_img: transparent
comments: false
---

这里采用的是**Obsidian + PicGo + Gitee** 图床的方法,好处是想要插入图片是直接粘贴进obsdian即可,而更新博客时也是直接复制文章即可.

# 详细步骤

### 第一步：获取 Gitee 的私人令牌 (Token)

1. 登录 [Gitee 官网](https://gitee.com/)。

2. 点击右上角头像 -> **设置**。

3. 在左侧菜单栏找到 **安全设置** -> **私人令牌**。

4. 点击 **生成新令牌**。

- **私人令牌描述**：填入“Obsidian_PicGo”等便于识别的名字。

- **权限选择**：只需勾选 **`projects`**（项目权限）即可。

5. 点击提交，输入密码后会显示一段字符串。**请务必立即保存该 Token**，关闭网页后将无法再次查看。

---

### 第二步：配置 PicGo 软件

1. **安装 PicGo**：确保你已安装 PicGo 客户端。

2. **安装 Gitee 插件**：

- PicGo 默认不内置 Gitee 图床，点击左侧的 **插件设置**。
- 在搜索栏输入 `gitee`，安装常用插件（如 `gitee-uploader`）。
- _注意：安装插件可能需要你电脑预装了 [Node.js](https://nodejs.org/)。_

3. **设置 Gitee 图床**：

- 点击左侧 **图床设置** -> **Gitee**（安装插件后才会出现）。
- **repo**：填写 `用户名/仓库名`。
- **branch**：通常填写 `master`。
- **token**：粘贴刚才在 Gitee 生成的私人令牌。
- **path**：填写图片存储的文件夹路径，例如 `img/`（不填则存放在根目录）。
- **customPath** 和 **customUrl**：一般保持默认即可。

4. **设为默认图床**：点击确定并设为默认。

---

### 第三步：配置 Obsidian 插件

为了实现“粘贴即上传”，你需要在 Obsidian 里配置 `Image Auto Upload Plugin`：

1. **安装插件**：在 Obsidian **第三方插件** 市场搜索 `Image Auto Upload Plugin` 并安装启用。
2. **连接 PicGo**：
   - 进入该插件设置界面。
   - **Uploader**：选择 `PicGo(app)`。
   - **PicGo Server**：默认为 `http://127.0.0.1:36677`。

3. **确认 PicGo 设置**：
   - 打开 PicGo 软件设置 -> **设置 Server**。
   - 确保 **开启 Server** 按钮是打开的，且 **监听端口** 是 `36677`。

---

### 第四步：测试

1. 在 Obsidian 中按下快捷键截图，或者复制一张网络图片。
2. 在笔记页面按下 `Ctrl + V` 粘贴。
3. 如果配置成功，你会看到图片链接从本地路径（或 `uploading...` 提示）瞬间变为 `https://gitee.com/xxx/xxx/raw/master/xxx.png`。

### 可能会遇到的问题

#### 插件设置里面找不到gitee

通常是由于 PicGo 无法正确调用 npm 路径，或者是国内访问 npm 官方源的网络延迟导致的
![image.png](https://gitee.com/magic486/note/raw/master/20260205225410565.png)
==解决方法==

通过命令行手动安装插件

- 按下 `Win + R`，输入 `cmd` 回车。
- 输入以下命令并回车（这会自动下载并安装 gitee 插件到 PicGo 的插件目录）
  ```bash
  cd %appdata%\picgo
  npm install picgo-plugin-gitee-uploader
  ```
- **重启 PicGo**。此时点击左侧的“图床设置”，你应该能看到 **Gitee** 选项了。
