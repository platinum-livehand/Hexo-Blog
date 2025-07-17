---
title: Linux面试题
date: 2025-07-17 22:46:10
tags:
  - 计算机基础知识
  - 操作系统
categories: 计算机基础知识
---

### **1. 查CPU和IP命令**

- **查CPU信息**：

  ```
  lscpu       # 查看CPU架构信息
  cat /proc/cpuinfo  # 详细CPU信息
  ```

- **查IP地址**：

  ```
  ip a       # 推荐（新版）
  ifconfig   # 旧版（需安装net-tools）
  hostname -I # 仅显示IP
  ```

------

### **2. 常用Linux命令分类**

- **文件操作**：`ls`, `cp`, `mv`, `rm`, `chmod`
- **文本处理**：`cat`, `grep`, `sed`, `awk`, `head/tail`
- **系统监控**：`top`, `ps`, `free`, `df`, `netstat`
- **权限管理**：`chown`, `chmod`, `sudo`
- **网络工具**：`ping`, `curl`, `wget`, `ssh`

------

### **3. `cp`和`chmod 777`含义**

- **`cp`**：复制文件/目录

  ```
  cp file1 file2      # 复制文件
  cp -r dir1 dir2     # 递归复制目录
  ```

- **`chmod 777`**：

  - `777`表示所有用户（所有者、组、其他）都有**读+写+执行**权限（`r+w+x=7`）
  - 慎用！开放全部权限可能存在安全风险。

------

### **4. 查看进程**

```
ps aux       # 查看所有进程
top          # 动态监控进程（按q退出）
pstree       # 树形显示进程
kill -9 PID  # 强制终止进程
```

------

### **5. 部署测试环境步骤**

1. **准备服务器**：安装Linux系统（如CentOS/Ubuntu）

2. **安装依赖**：

   ```
   yum install -y java git nginx  # CentOS
   apt-get install ...           # Ubuntu
   ```

3. **部署应用**：

   ```
   git clone 项目仓库
   mvn package    # 编译Java项目
   nohup java -jar app.jar &  # 后台运行
   ```

4. **验证服务**：

   ```
   curl http://localhost:8080
   netstat -tulnp | grep 8080
   ```

------

### **6. Linux定位Bug场景**

- **日志分析**：

  ```
  grep "ERROR" app.log    # 搜索错误日志
  tail -f app.log         # 实时跟踪日志
  ```

- **接口调试**：

  ```
  curl -X POST http://api.com -d '{"param":1}'
  ```

- **资源监控**：

  ```
  top -p PID      # 查看进程资源占用
  free -h         # 内存使用情况
  ```

------

### **7. 分页查看日志**

```
less app.log      # 支持上下翻页（推荐）
more app.log      # 基础分页
cat app.log | grep "关键词" | less
```

------

### **8. 进程与路径操作**

- **查看当前进程**：

  ```
  ps -ef | grep 进程名
  ```

- **退出进程**：

  ```
  kill -9 PID     # 强制终止
  Ctrl+C          # 终止前台进程
  ```

- **查看当前路径**：

  ```
  pwd             # 打印工作目录
  ```

------

### **9. `grep`命令详解**

- **基本搜索**：

  ```
  grep "text" file.txt
  ```

- **忽略大小写**：

  ```
  grep -i "hello" file.txt
  ```

- **反向匹配（不含该串的行）**：

  ```
  grep -v "error" file.txt
  ```

