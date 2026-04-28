---
title: python如何管理虚拟环境
tags: python学习
date: 2026-04-28
top_img: transparent
comments: false
---

三种虚拟环境创建方式，以下分别介绍它们的异同点和常见命令行操作

# 1.venv

## 特点
python3.3后官方自带的工具，无需额外安装，比较轻量

**简单、直接**。它会在你的项目目录下创建一个文件夹，里面包含了 Python 解释器的副本和私有的 `site-packages`
## 命令行指令

|**操作**|**命令行指令 (Windows)**|**命令行指令 (macOS/Linux)**|
|---|---|---|
|**创建环境**|`python -m venv .venv`|`python3 -m venv .venv`|
|**激活环境**|`.venv\Scripts\activate`|`source .venv/bin/activate`|
|**退出环境**|`deactivate`|`deactivate`|
|**删除环境**|直接手动删除 `.venv` 文件夹|直接手动删除 `.venv` 文件夹|
**提示**：建议将环境命名为 `.venv`，这样很多编辑器（如 VS Code）会自动识别并询问是否切换解释器。
# 2.Conda

## 特点

**环境和包管理器**

可以安装非 Python 的依赖（如 C++ 编译好的库、CUDA 驱动等）。它能让你自由切换 Python 解释器的版本（比如在这个环境用 3.8，那个用 3.12）

## 命令行指令
- **创建环境**（指定 Python 版本）：

    ```
    conda create --name myenv python=3.10
    ```
- **创建环境并自动确认**（适合写脚本）：
    ```
    conda create -n myenv python=3.9 -y
    ```
- **激活环境**：
    ```
    conda activate myenv
    ```
- **退出当前环境**：
    ```
    conda deactivate
    ```
- **查看已创建的所有环境**：
    ```
    conda env list
    ```
- **完全删除某个环境**：
    ```
    conda remove -n myenv --all
    ```
# 3.Poetry / Pipenv

## 特点
更高层级的封装，侧重于**依赖管理**和**确定性构建**

使用 `pyproject.toml` 或 `Pipfile` 来锁定库的具体版本（Lock 文件），确保**“在我的电脑上能跑，在你的电脑上也能跑”**

## 命令行指令
- **初始化新项目**（交互式创建配置文件）：
    ```
    poetry init
    ```
- **安装依赖并自动创建虚拟环境**：
    ```
    poetry install
    ```
- **添加并安装一个库**：
    ```
    poetry add requests
    ```
- **在虚拟环境中执行脚本**（无需显式激活）：
    ```
    poetry run python main.py
    ```
- **显式激活并进入虚拟环境的 Shell**：
    ```
    poetry shell
    ```
- **查看环境信息**：
    ```
    poetry env info
    ```

| **维度**          | **venv**          | **Conda**               | **Poetry / Pipenv** |
| --------------- | ----------------- | ----------------------- | ------------------- |
| **Python 版本**   | 使用当前系统的 Python 版本 | 可以安装任意版本的 Python        | 基于系统已有的 Python      |
| **非 Python 依赖** | 不支持（需手动安装系统库）     | **支持**（如 MKL, OpenBLAS） | 不支持                 |
| **环境存放位置**      | 通常在项目文件夹内         | 统一存放在 Conda 的安装目录下      | 统一管理或配置在项目内         |
| **依赖解析能力**      | 弱（可能出现版本冲突）       | 强                       | **极强**（自动解决复杂的依赖树）  |
| **主要用途**        | 简单的 Web 开发、小型脚本   | **深度学习、数据科学**、跨平台开发     | 专业的软件工程项目、包发布       |


# 4.创建虚拟环境的原因：

1. **隔离性 (Isolation)：** 避免 A 项目需要 `numpy 1.20`，B 项目需要 `numpy 1.26` 导致的“打架”问题。

2. **可移植性 (Reproducibility)：** 通过配置文件（如 `requirements.txt` 或 `conda.yaml`），其他开发者可以一键还原你的开发环境。

3. **系统保护：** 防止在安装第三方库时意外损坏操作系统自带的 Python 环境（尤其是 Linux 和 macOS 用户）。

4. **环境清理：** 如果一个项目做完了，直接删除对应的虚拟环境文件夹即可，不会在系统中留下“垃圾”。

# 5.Anaconda的作用
###  Anaconda Prompt (命令行控制台) —— 核心大脑

这是你最常用的工具。它本质上是一个**预先配置好路径的终端**。

- **激活环境变量的“传送门”：** 普通的 CMD 或 PowerShell 往往找不到 `conda` 命令。Anaconda Prompt 在启动时会自动运行一个脚本，把 Conda 的路径临时加入到系统的 `PATH` 中。
    
- **环境切换的中心：** 你在这里输入 `conda activate my_ai_env`，它会瞬间把你的 `python` 指令指向你创建的那个独立房间，而不会污染系统自带的 Python。
    
- **解决安装难题：** 你之前遇到的 `grpcio` 或 `openai-whisper` 安装报错，在 Anaconda Prompt 里处理会更稳定，因为它自带了编译器环境的依赖。
    

---

###  Anaconda Navigator (图形化管理界面) —— 可视化地图

如果你不喜欢敲命令，Navigator 就是你的“仪表盘”。

- **一键式管理：** 你可以直观地看到自己创建了哪些环境，每个环境里安装了哪些库（支持搜索和勾选升级）。
    
- **集成开发工具的“跳板”：** 它可以让你直接从界面启动 **Jupyter Notebook**、**Spyder** 甚至是 **VS Code**。它会自动确保这些工具启动时，已经加载好了对应的环境。
    
- **学习资源：** 里面集成了很多官方文档和教程，适合新手快速上手数据科学。