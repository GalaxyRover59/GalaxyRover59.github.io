---
title: Appendix A. 门电路原理
author: GalaxyRover59
date: 2023-05-10  16:30:00 +0800
category: [计算机科学, 计算机组成原理]
tags: [hardware]
math: true
img_path: /assets/img/computer_organization/
layout: post
---


## 1. pn结

把P型半导体和N型半导体结合在一起后，在接触面附近的电子和空穴由于扩散运动会向对方区域移动，两者又会因为复合而消失。失去电子和空穴的原子变成了带电离子，并且由于共价键束缚不可移动，于是在接触面附近形成了一个内部电场。

![pn结内电场的形成]({{ page.img_path }}pnJunction.png "pn结内电场")

这个内电场形成后，反过来又阻止扩散运动，最后会达到一个动态的平衡。中间这个内电场区域也叫空间电荷区。

![pn结与外加偏压]({{ page.img_path }}pn_E.png "pn结与外加偏压")

给PN结外加正向电压时，我们称为正向偏置，简称正偏。正偏状态下，外加的电场会削弱内电场的壁垒作用，空间电荷区变窄，电子和空穴更容易穿越空间电荷区。在外电场的持续作用下，便可以形成持续的电流。外电场越大，电流越大。

给PN结外加反向电压时，我们称为反向偏置，简称反偏。反偏状态下，外加电场会增强内电场的壁垒作用，空间电荷区变宽，电子和空穴更加难以进入空间电荷区，不能形成持续的电流。

## 2. MOSFET

MOSFET，即金属-氧化物半导体场效应晶体管，全称Metal-Oxide-Semiconductor
Field-Effect
Transistor，它名字的由来是因其Gate部分由上往下依次由金属(Metal)、二氧化硅(氧化层，Oxide)、硅晶片(半导体，Semiconductor)组成，且原理是利用电场(Field-Effect)。该器件的作用在于制造可控制的电流，并使其从源极S(Source)流向漏极D(Drain).

![MOSFET结构示意图与电路符号]({{ page.img_path }}MOSFET.png "MOSFET结构示意图与电路符号")

记栅极电压为$V_G$，阈值电压为$V_{th}$；当$V_G <V_{th}$时，源极与漏极间没有电子的移动，晶体管不导通；当$V_G$逐渐增大并超过$V_{th}$时，此时由于提供了足够大的自上而下的电场$E$，电子会往栅极方向聚集，从而形成载流子通道。

![NMOS]({{ page.img_path }}NMOS.png "NMOS示意图"){: w="400" }

MOSFET根据通道里的载流子类型可分为N型和P型，分别为NMOS和PMOS。NMOS在栅极接高电压(High
Voltage 或 VDD)时形成通道；PMOS在栅极接低电压(Low Voltage 或
GND)时形成通道。

如果把高电压看作1，低电压看作0，那么我们就可以利用NMOS和PMOS进行各种逻辑运算了。

## 3. 逻辑门

- ### 非门

非门的物理实现如下图所示，其中$V_{in}$是输入端，$V_{out}$是输出端。

当输入端为高电压时，即$V_{in}=1$，PMOS不形成通道而NMOS形成通道，因此GND一端与输出端相连，从而输出低电压，即$V_{out}=0$；反之VDD一端与输出端相连，$V_{out}=1$.

![非门的实现]({{ page.img_path }}NotGate.jpg "非门")

- ### 与非门

与非门的物理实现如下图所示。这种涉及多个输入的逻辑门，可以分析晶体管之间的串并联关系；可以发现它们在VDD端并联，在GND端串联，因此显然$V_A, V_B$中只要有0则输出为1；两者同时为1输出才为0。

![与非门的实现]({{ page.img_path }}NandGate.jpg "与非门")

- ### 或非门

![或非门的实现]({{ page.img_path }}NorGate.jpg "或非门")

同理，分析上图中的串并联关系可得到或非门的逻辑表达式。
