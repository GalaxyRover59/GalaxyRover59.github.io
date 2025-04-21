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
首先输入命令`wsl --shutdown`关闭系统，接着导出安装的Linux发行版本到自己选择的目录，此处为
```bash
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

注意，迁移后再次启动系统会默认为root用户，若想修改为username用户，可编辑wsl.conf文件：
```bash
sudo vim /etc/wsl.conf
```

添加如下内容：
```
[user]
default = username
```

保存后，退出并关闭WSL，在下次启动时，就会默认以username用户启动了。

### • 忘记WSL的root密码

首先以管理员身份运行PowerShell，输入`wsl.exe --user root`切换到root用户。最后执行`passwd root`命令来修改root密码。


# 2. 安装CUDA, cuDNN

## 2.1 安装WSL CUDA

进入[官网](https://developer.nvidia.com/cuda-toolkit-archive)，选择显卡驱动能运行的CUDA Toolkit

![CUDA Toolkit installation](CUDA_Toolkit_downloads.png "CUDA Toolkit安装选项")

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

进入[官网](https://developer.nvidia.com/cudnn-downloads)

![cuDNN installation](cuDNN_downloads.png "cuDNN安装选项")

选择选项后将对应的命令输入终端进行安装。如有需要，也可以选择[历史版本](https://developer.nvidia.com/rdp/cudnn-archive)。

# 3.联动VSCode

## 3.1 配置虚拟环境

### • 使用Anaconda

#### ①. 安装miniconda
```bash
wget -c https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod 777 Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

打开~/.bashrc设置环境变量
```bash
export PATH=~/miniconda3/bin:$PATH
```

为了安装各种库时不会速度过慢，需要更新下镜像源。创建~/.condarc文件，添加
```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

#### ②. 创建虚拟环境并安装所需的库

这步与直接在Linux或Windows系统里无异，故不做赘述。

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

## 3.2 在VSCode的扩展界面搜索WSL插件并进行安装

在VSCode的扩展界面里，主要搜索并安装以下两个插件

![VSCode WSL extensions](VSCode_WSL_extensions.png "VSCode里安装WSL相关插件")

以上步骤完成后，可以在WSL终端跳转到项目目录，输入`code .`就可以打开VSCode。也可以先打开VSCode，以快捷方式Ctrl+Shift+P调出命令面板，输入WSL，选择选项来打开项目文件。

具体可以参考[在 Visual Studio Code 中打开 WSL 项目](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-vscode)。
