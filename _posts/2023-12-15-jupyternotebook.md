---
title: 远程访问服务器上的Jupyter Notebook与TensorBoard
author: GalaxyRover59
date: 2023-12-15 15:10:00 +0800
category: [计算机科学]
tags: [jupyter notebook, tensorboard]
math: true
layout: post
---
## 1. jupyter notebook使用技巧

### (1). 设置根目录
生成配置文件：
```bash
jupyter notebook --generate-config
```
若配置文件存在，则会询问是否覆盖。

进入配置文件“jupyter_notebook_config.py”后，查找关键词：“c.NotebookApp.notebook_dir”，其后即为根目录。

### (2). 拓展功能


## 2. ssh远程使用jupyter notebook

- ### ssh
SSH是一种网络协议，用于计算机之间的加密登录。

常用参数：

-b 在拥有多个接口或地址别名的机器上，指定收发接口

-f 在后台运行，该选项隐含了 -n 选项

-n 把 stdin 重定向到 /dev/null (实际上防止从 stdin 读取数据)，在后台运行时一定会用到这个选项

-N 不执行远程命令

-p 指定远程主机的端口

-q 静默模式，消除所有的警告和诊断信息

-L 将本地机(客户机)的某个端口转发到远端指定机器的指定端口

-R 将远程主机(服务器)的某个端口转发到本地端指定机器的指定端口

### (1). 在远程服务器上，启动jupyter notebooks服务
```bash
jupyter notebook --no-browser --port=8889
```

### (2). 在本地终端中启动SSH
```bash
ssh -N -f -L localhost:8888:localhost:8889 username@serverIP
```

注意将serverIP替换成服务器IP，将username替换为对应的账号

### (3). 最后打开浏览器，访问：<http://localhost:8888/>

## 2. 利用jupyter notebook自带的远程访问功能
### (1). 在服务器端生成默认配置文件
```bash
jupyter notebook --generate-config
```

### (2). 打开文件 ~/.jupyter/jupyter_notebook_config.py，在末尾输入以下内容（这些内容在文件中可以找到，但都被注释掉了）：
```python
c.NotebookApp.ip = '*'                    # 允许任意ip访问
c.NotebookApp.port = 8888                 # 使用的端口，可随意设置
c.NotebookApp.open_browser = False        # 运行时不打开本地浏览器
c.NotebookApp.allow_remote_access = True  # 允许远程访问
c.NotebookApp.allow_root = True

```

### (3). 使用如下命令，设置密码：
```bash
jupyter notebook password
```

### (4). 在服务器端输入`jupyter notebook`，正常运行后，在客户端浏览器访问http://serverIP:port/（注意将 serveIP 和 port 替换成相应的值）。输入上一步设置的密码后即可使用。

- ### 为防止远程中断，可使用挂起操作，即执行`nohup jupyter notebook`

