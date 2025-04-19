---
title: WSL+VSCode使用指南
author: GalaxyRover59
date: 2025-04-19 14:30:00 +0800
category: [计算机科学, 其他]
tags: [WSL, VSCode]
img_path: /images/computer_others/
layout: post
---
WSL全称Windows Subsystem for Linux（Windows上的Linux子系统），其通过Windows NT内核层面支持了Linux的内核接口，使得能直接在Windows系统中运行Linux环境。

# 1.安装WSL

推荐查看官方文档：[How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)([中文版](https://learn.microsoft.com/zh-cn/windows/wsl/install))

注意有WSL1和WSL2的两种，现在一般推荐安装WSL2，其区别为[Comparing WSL Versions](https://learn.microsoft.com/en-us/windows/wsl/compare-versions).

### 忘记WSL的root密码

首先以管理员身份运行PowerShell，输入`wsl.exe --user root`切换到root用户。最后执行`passwd root`命令来修改root密码。

# 2.联动VSCode

## (1). 在VSCode的扩展界面搜索WSL插件并进行安装。

## (2). 配置虚拟环境

### • 使用Anaconda

### • 不使用Anaconda

#### ①. 准备工作

在WSL终端，执行以下命令以更新系统并安装创建Python虚拟环境所需的组件：
```bash
sudo apt update && sudo apt upgrade
sudo apt install python3-venv
```
