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

# 1.安装WSL及Ubuntu系统

## 1.1安装WSL

推荐查看官方文档：[How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)([中文版](https://learn.microsoft.com/zh-cn/windows/wsl/install))

注意有WSL1和WSL2的两种，现在一般推荐安装WSL2，其区别为[Comparing WSL Versions](https://learn.microsoft.com/en-us/windows/wsl/compare-versions).

## 1.2 安装Ubuntu

在PowerShell中查看发行版本（为防止之后的安装出现错误，如权限不足等，可以一开始就以管理员身份打开PowerShell）
```bash
wsl.exe --list --online
```
可选择任一版本进行安装。此处选择Ubuntu-20.04版本。

### • 迁移至非系统盘

WSL默认装在C盘，如果C盘空间不够，可考虑迁移至其它盘。
首先输入命令`wsl --shutdown`关闭系统，接着导出安装的Linux发行版本到自己选择的目录，此处为```bash
wsl --export Ubuntu-20.04 F:\Ubuntu\ubuntu.tar
```

接着注销并删除原系统
```bash
wsl --unregister Ubuntu-20.04
```

最后导入
```bash
wsl --import Ubuntu-20.04 F:\Ubuntu\ F:\Ubuntu\ubuntu.tar --version 2
```

注意，迁移后再次启动系统会默认为root，

### • 忘记WSL的root密码

首先以管理员身份运行PowerShell，输入`wsl.exe --user root`切换到root用户。最后执行`passwd root`命令来修改root密码。


# 2. 安装CUDA, cuDNN

## 2.1 安装WSL CUDA

进入[官网](https://developer.nvidia.com/cuda-toolkit-archive)，选择显卡驱动能运行的CUDA Toolkit

![CUDA Toolkit installation](CUDA_Toolkit.png "CUDA Toolkit安装选项")

将对应的命令输入Ubuntu的命令行终端来进行安装。

安装完成后，还需要设置环境变量，打开~/.bashrc，在合适的地方加上
```bash
export PATH=/usr/bin:$PATH
export PATH=/usr/local/cuda-12.8/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.8/lib64:$LD_LIBRARY_PATH
```
其中cuda的版本12.8要换成自己安装的对应版本。

`source ~/.bashrc`后，可通过`nvcc -V`来验证是否安装成功。

## 2.2 安装cuDNN

# 3.联动VSCode

## 3.1 在VSCode的扩展界面搜索WSL插件并进行安装。

## 3.2 配置虚拟环境

### • 使用Anaconda

### • 不使用Anaconda

#### ①. 准备工作

在WSL终端，执行以下命令以更新系统并安装创建Python虚拟环境所需的组件：
```bash
sudo apt update && sudo apt upgrade
sudo apt install python3-venv
```

#### ②. 创建虚拟环境并激活

我们可以将所有虚拟环境放在同一个目录下，也可以放在各自所用的项目的目录下。这里采用第一种方式，我么执行如下命令：
```bash
mkdir ~/.virtualenvs
cd ~/.virtualenvs
python3 -m venv myenv
```
这样就创建了一个叫myenv的虚拟环境。

在终端通过输入
```bash
source ~/.virtualenvs/myenv/bin/activate
```
就可以激活该环境，之后就可以使用`pip install`来安装python库。
