---
date: "2025-12-10T17:06:34+08:00"
draft: false
title: 'LR(0) SLR(1)'  
math: true
categories:
  -   PrincipleofCompile
tags:
  - LR(0)
  - SLR(1)
image: "img/PrincipleofCompile.svg"  
---

- 原文法：S $\rightarrow$bBB
  - 移进： S $\rightarrow \cdot$ bBB
  - 归约： S $\rightarrow$ bBB$\cdot$
  - 待约： S $\rightarrow$ b$\cdot$BB

## LR(0) 文法

1. 拓广文法。
2. LR(0) 有限自动机 DFA。
3. LR(0) 分析表。
4. 分析流程。

e.g.  
S $\rightarrow$ CC  
C $\rightarrow$ cC | d

拓广文法：

（0）S' $\rightarrow$ S  
（1）S $\rightarrow$ CC  
（2）C $\rightarrow$ cC  
（3）C $\rightarrow$ d  

项目集族：

- $I_0$
  - S' $\rightarrow$ $\cdot$ S
  - S $\rightarrow$ $\cdot$ CC
  - C $\rightarrow$ $\cdot$ cC
  - C $\rightarrow$ $\cdot$ d
- $I_0 \stackrel{S}{\longrightarrow} I_1$
  - S' $\rightarrow$ S $\cdot$
- $I_0 \stackrel{C}{\longrightarrow} I_2$
  - S $\rightarrow$ C $\cdot$ C
  - C $\rightarrow$ $\cdot$ cC
  - C $\rightarrow$ $\cdot$ d
- $I_0 \stackrel{c}{\longrightarrow} I_3$
  - C $\rightarrow$ c $\cdot$ C
  - C $\rightarrow$ $\cdot$ cC
  - C $\rightarrow$ $\cdot$ d
- $I_0 \stackrel{d}{\longrightarrow} I_4$
  - C $\rightarrow$ d $\cdot$
- $I_2 \stackrel{C}{\longrightarrow} I_5$
  - S $\rightarrow$ CC $\cdot$
- $I_2 \stackrel{c}{\longrightarrow} I_3$
- $I_2 \stackrel{d}{\longrightarrow} I_4$
- $I_3 \stackrel{C}{\longrightarrow} I_6$
  - C $\rightarrow$ cC $\cdot$
- $I_3 \stackrel{c}{\longrightarrow} I_3$
- $I_3 \stackrel{d}{\longrightarrow} I_4$ （图片缺了这根）

![](https://pic3.zhimg.com/v2-319d6122785e122d6680424161acb120_1440w.jpg)

LR(0) 分析表：

|       | ACTION | ACTION | ACTION | GOTO  | GOTO  |
| :---: | :----: | :----: | :----: | :---: | :---: |
|       |   c    |   d    |   $    |   S   |   C   |
|   0   |   S3   |   S4   |        |   1   |   2   |
|   1   |        |        |  acc   |       |       |
|   2   |   S3   |   S4   |        |       |   5   |
|   3   |   S3   |   S4   |        |       |   6   |
|   4   |   R3   |   R3   |   R3   |       |       |
|   5   |   R1   |   R1   |   R1   |       |       |
|   6   |   R2   |   R2   |   R2   |       |       |

FOLLOW 集：

S $\rightarrow$ CC  
C $\rightarrow$ cC | d

FOLLOW(S) = { $ }  
FOLLOW(C) = { c, d, $ }

SLR(1) 分析表：

|       | ACTION | ACTION | ACTION | GOTO  | GOTO  |
| :---: | :----: | :----: | :----: | :---: | :---: |
|       |   c    |   d    |   $    |   S   |   C   |
|   0   |   S3   |   S4   |        |   1   |   2   |
|   1   |        |        |  acc   |       |       |
|   2   |   S3   |   S4   |        |       |   5   |
|   3   |   S3   |   S4   |        |       |   6   |
|   4   |   R3   |   R3   |   R3   |       |       |
|   5   |        |        |   R1   |       |       |
|   6   |   R2   |   R2   |   R2   |       |       |
