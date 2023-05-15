---
title: CO4. 计算机体系结构
author: GalaxyRover59
date: 2023-05-15 10:10:00 +0800
category: [计算机科学, 计算机组成原理]
tags: [hardware, nand2tetris]
img_path: /images/computer_organization/
layout: post
---


这一章会将之前章节中构建的硬件，集成到一个通用的计算机系统中，能够运行特定的机器语言编写的程序。这个特定的计算机，称为Hack[^1]Hack虽然简单，但功能也足够丰富，可以用来说明任何通用计算机的关键操作原理和硬件元素。因此，构建它将可以亲身了解现代计算机是如何工作的，以及它们是如何构建的。

[^1]:Noam Nisan and Shimon Schocken. *The elements of computing systems: building a modern computer from first principles.* MIT press, 2021.

## 1. 冯·诺伊曼架构

![von Neumann computer architecture](vonNeumannArchitecture.png "通用的冯·诺伊曼计算机架构")

上图所示的冯·诺伊曼架构基于中央处理单元(Central Processing Unit, CPU)，从输入设备接收数据，与存储设备交互，并将数据发送到输出设备。这种架构的核心在于存储程序的概念：计算机的内存不仅存储计算机所操作的数据，还存储告诉计算机该做什么的指令。

## 2. 内存

计算机的内存可以从物理和逻辑两个角度进行讨论。在物理上，内存是由许多可寻址的、固定大小的寄存器组成的线性序列，每个寄存器都有一个唯一的地址和一个值。从逻辑上讲，这个地址空间有两个目的：存储数据和存储指令。“指令”和“数据”的表现方式完全相同——作为特定长度的二进制数。

专用于存储数据的内存区域称为数据内存，专用于存储指令的内存区域称为指令内存。在冯·诺伊曼架构的一些变体中，数据内存和指令内存根据需要，可以在相同的物理地址空间内动态分配和管理。在其他变体中，数据内存和指令内存被保存在两个物理上独立的内存区域中，每个内存区域都有自己不同的地址空间。