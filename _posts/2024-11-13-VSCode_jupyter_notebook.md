---
title: VSCode上打开Jupyter Notebook可能出现的工作目录不在notebook所在路径的问题
author: GalaxyRover59
date: 2024-11-13 21:44:00 +0800
category: [计算机科学, 其他]
tags: [VSCode, jupyter notebook]
layout: post
---
这个问题出现在：我用VSCode远程连接服务器后，打开Jupyter Notebook，在一个单元格中有几行代码，import了同文件夹下的几个python模块，但返回报错“ModuleNotFoundError”.

排查后发现这是由于notebook运行在VSCode打开的路径，而不是notebook本身所在的目录。亦即，假设VSCode的工作目录为`/home/username`，而notebook的路径为`/home/username/UserProject/my_notebook.ipynb`，在notebook中调用`os.getcwd()`，返回的是`/home/username`而不是期望的`/home/username/UserProject`.

## 1.问题原因

这似乎是由于IPython内核的特性导致的，具体可以参考下面几个帖子：

[Jupyter Notebook/Lab set current directory to ipynb file's](https://stackoverflow.com/questions/72877765/jupyter-notebook-lab-set-current-directory-to-ipynb-files)

[How to obtain Jupyter Notebook's path?](https://stackoverflow.com/questions/52119454/how-to-obtain-jupyter-notebooks-path)

[How to figure out the path of the current ipynb file from within IPython?](https://github.com/ipython/ipython/issues/10123)

## 2.解决办法

一种方式是VSCode远程连接服务器时，切换其工作目录到notebook所在的目录。但这样对大型项目，尤其是多个notebook在不同的路径下时，可能会比较麻烦。

另一个方式就是在notebook的开头加上以下部分代码：
```Python
import os
import IPython
file_path =IPython.extract_module_locals()[1]['__vsc_ipynb_file__']
os.chdir(file_path.rsplit('/', 1)[0])
```
将该notebook的工作目录切换至当前目录。另外需要记得，如果之后将notebook转为.py文件，需要将上述代码注释掉或者删除。
