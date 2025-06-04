---
title: TortoiseGit无法拉代码，但是GitBash可以
date: 2025-06-04 10:04:43
tags:
  - Git
categories: 问题记录
---

### **问题分析：TortoiseGit 无法拉取代码，但 Git Bash 可以**

- **Git Bash 可以拉取** → 说明你的 **SSH 密钥配置正确**，Git Bash 能正确识别 `~/.ssh/id_rsa`（或 `id_ed25519`）。
- **TortoiseGit 报错** → 说明 TortoiseGit **没有正确加载 SSH 密钥**，导致无法认证。

------

## **解决方案**

### **1. 确保 TortoiseGit 使用正确的 SSH 客户端**

`TortoiseGit` 默认使用 `TortoiseGitPlink`（PuTTY 的 SSH 工具），而 `Git Bash` 使用 `OpenSSH`。你需要确保 `TortoiseGit` 使用正确的 SSH 客户端：

1. **打开 TortoiseGit 设置**

   - 右键 → **TortoiseGit → Settings**
   - 进入 **Network** 选项卡

2. **设置 SSH 客户端路径**

   - 确保 `SSH Client` 指向 `Git` 自带的 `ssh.exe`（而不是 `TortoiseGitPlink.exe`）

     ```markdown
     C:\Program Files\Git\usr\bin\ssh.exe
     ```

     （具体路径取决于你的 Git 安装位置）

3. **保存并测试**

   - 点击 **OK**，然后尝试 `git pull`，应该可以正常拉取。
