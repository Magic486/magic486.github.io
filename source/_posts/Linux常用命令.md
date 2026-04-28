---
title: Linux常用命令
tags: Linux
date: 2026-04-28
top_img: transparent
comments: false
---

1. ## **文件/目录操作**
    

2. **`ls`**
    
    1. 列出目录内容
        
    2. 常用选项：`ls -l`（详细信息）、`ls -a`（显示隐藏文件）
        
3. **`cd`**
    
    1. 切换目录
        
    2. 示例：`cd /home`、`cd ..`（返回上级目录）
        
4. **`pwd`**
    
    1. 显示当前工作目录路径
        
5. **`cp`**
    
    1. 复制文件/目录
        
    2. 示例：`cp file1.txt file2.txt`、`cp -r dir1 dir2`（递归复制目录）
        

  

6. **`mv`**
    
    1. 移动或重命名文件/目录
        
    2. 示例：`mv old.txt new.txt`（重命名）、`mv file /tmp`（移动文件）
        

  

7. **`rm`**
    
    1. 删除文件/目录
        
    2. 示例：`rm file.txt`、`rm -r dir`（递归删除目录）、`rm -rf asr_workspace`强制递归删除
        

  

8. **`mkdir`**
    
    1. 创建目录
        
    2. 示例：`mkdir new_dir`
        

  

9. **`touch`**
    
    1. 创建空文件或更新文件时间戳
        
    2. 示例：`touch file.txt`
        

  

10. **`cat`**
    
    1. 查看文件内容
        
    2. 示例：`cat file.txt`
        

  

11. **`more`**** / **`**less**`
    
    1. 分页查看文件内容（支持翻页）
        

  

12. **`head`**** / **`**tail**`
    
    1. 查看文件头部/尾部内容
        
    2. 示例：`tail -f log.txt`（实时追踪日志）
        

  

---

2. ## **系统信息与管理**
    

3. **`top`**** / **`**htop**`
    
    1. 实时显示系统进程和资源占用（`htop` 为增强版）
        

  

4. **`ps`**
    
    1. 查看进程状态
        
    2. 示例：`ps aux`（显示所有进程）
        

  

5. **`kill`**
    
    1. 终止进程
        
    2. 示例：`kill -9 PID`（强制终止进程）
        

  

6. **`df`**
    
    1. 查看磁盘空间使用情况
        
    2. 示例：`df -h`（以易读格式显示）
        

  

7. **`du`**
    
    1. 查看目录/文件占用空间
        
    2. 示例：`du -sh /home`（统计总大小）
        

  

8. **`free`**
    
    1. 查看内存使用情况
        
    2. 示例：`free -h`
        

  

9. **`uname`**
    
    1. 显示系统信息
        
    2. 示例：`uname -a`（显示内核版本等）
        

  

10. **`shutdown`**** / **`**reboot**`
    
    1. 关机或重启
        
    2. 示例：`shutdown now`（立即关机）
        

  

---

3. ## **网络相关**
    

4. **`ping`**
    
    1. 测试网络连通性
        
    2. 示例：`ping google.com`
        

  

5. **`curl`**/ `wget`
    
    1. 下载文件或测试网络请求
        
    2. 示例：`curl -O http://example.com/file.zip`
        

  

6. **`ifconfig`**** / **`ip`
    
    1. 查看或配置网络接口（`ip` 更现代）
        
    2. 示例：`ip addr show`
        

  

7. **`netstat`**
    
    1. 显示网络连接、路由表等信息
        
    2. 示例：`netstat -tulnp`（查看监听端口）
        

  

8. **`ssh`**
    
    1. 远程登录其他主机
        
    2. 示例：`ssh user@192.168.1.100`
        

  

9. **`scp`**
    
    1. 安全复制文件到远程主机
        
    2. 示例：`scp file.txt user@host:/path`
        

  

10. **`traceroute`**** / **`**mtr**`
    
    1. 追踪网络路由路径
        

  

---

4. ## **文本处理**
    

5. **`grep`**
    
    1. 文本搜索工具
        
    2. 示例：`grep "error" log.txt`（查找包含 "error" 的行）
        

  

6. **`sed`**
    
    1. 流编辑器（用于文本替换/处理）
        
    2. 示例：`sed 's/old/new/g' file.txt`（全局替换）
        

  

7. **`awk`**
    
    1. 强大的文本分析工具
        
    2. 示例：`awk '{print $1}' file.txt`（输出第一列）
        

  

8. **`sort`**
    
    1. 对文本行排序
        
    2. 示例：`sort file.txt`
        

  

9. **`cut`**
    
    1. 按列提取文本
        
    2. 示例：`cut -d',' -f1 data.csv`（以逗号分隔，取第一列）
        

  

10. **`diff`**
    
    1. 比较文件差异
        
    2. 示例：`diff file1.txt file2.txt`
        

  

---

5. ## **压缩与解压**
    

6. **`tar`**
    
    1. 归档文件
        
    2. 示例：
        
        - 压缩：`tar -czvf archive.tar.gz dir/`
            
        - 解压：`tar -xzvf archive.tar.gz`
            

  

7. **`gzip`**** / **`**gunzip**`
    
    1. 压缩/解压 `.gz` 文件
        

  

8. **`zip`**** / **`**unzip**`
    
    1. 处理 `.zip` 文件
        

  

---

6. ## **权限管理**
    

7. **`chmod`**
    
    1. 修改文件权限
        
    2. 示例：`chmod 755 file.sh`、`chmod +x script.sh`
        

  

8. **`chown`**
    
    1. 修改文件所有者
        
    2. 示例：`chown user:group file.txt`
        

  

---

7. ## **查找命令**
    

8. **`find`**
    
    1. 查找文件
        
    2. 示例：`find /home -name "*.txt"`
        

  

9. **`locate`**
    
    1. 快速查找文件（依赖数据库）
        
    2. 示例：`locate filename`
        

  

10. **`which`**
    
    1. 显示命令的完整路径
        
    2. 示例：`which ls`
        

  

---

  

8. ## **软件包管理**
    

- `apt update`（更新源列表）
    
- `apt install package`（安装软件包）
    
- `apt remove package`（卸载）
    

---

  

9. ## **其他实用命令**
    

10. **`history`**
    
    1. 查看命令历史记录
        

  

11. **`alias`**
    
    1. 创建命令别名
        
    2. 示例：`alias ll='ls -alF'`
        

  

12. **`man`**
    
    1. 查看命令手册
        
    2. 示例：`man ls`
        

  

13. **`echo`**
    
    1. 输出文本或变量
        
    2. 示例：`echo $PATH`
        

  

14. **`date`**
    
    1. 显示或设置系统时间
        

  

---

10. ## **学习建议**
    

- 使用 `man <command>` 查看命令详细手册（如 `man grep`）。
    
- 尝试 `tldr <command>` 获取简化版命令示例（需安装 `tldr` 工具）。