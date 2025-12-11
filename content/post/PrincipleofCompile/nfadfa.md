---
date: "2025-12-10T11:40:52+08:00"
draft: false
title: '正规式 正规文法'  
math: true
categories:
  - PrincipleofCompile  
tags:
  - NFA
  - DFA
image: "img/PrincipleofCompile.svg"  
---

## 正规文法

$A \rightarrow aB$ 或 $A \rightarrow a$

推出“1 个终结符+非终结符”或“1 个终结符”。

## 正规式 $\rightarrow$ 正规文法

1. $A \rightarrow x|y \Rightarrow A \rightarrow x, A \rightarrow y$
2. $A \rightarrow xy \Rightarrow A \rightarrow xB, B \rightarrow y$
3. $A \rightarrow x^*y \Rightarrow A \rightarrow xA, A \rightarrow y$

## 正规文法 $\rightarrow$ 正规式

1. $A \rightarrow xB, B \rightarrow y \Rightarrow A = xy$
2. $A \rightarrow xA|y \Rightarrow A = x^*y$
3. $A \rightarrow x, A \rightarrow y \Rightarrow A = x|y$

## NFA 状态图

![](https://code84.com/wp-content/uploads/2022/09/3484dd1992226c79c2568db8a1f134e8.png)

$e.g. (\epsilon|a)b^*$

![](https://uploadfiles.nowcoder.com/images/20190725/2967044_1564048193370_2F77148314F59B0408830673A9ED4CE6)

## NFA 转 DFA
1. 求初态闭包：以 NFA 初态为起点，经 $\epsilon$ 边可达的所有状态构成 DFA 初态；
2. 对每个 DFA 状态，遍历所有输入符号：
   - 先求该状态的“符号转移集”（沿符号边可达的 NFA 状态）；
   - 再求转移集的 $\epsilon$ 闭包，得到新的 DFA 状态；
3. 重复步骤 2，直到无新 DFA 状态生成；
4. 标记含 NFA 终态的 DFA 状态为终态。

$e.g. a(a|b)^*$

NFA:
![](https://uploadfiles.nowcoder.com/images/20220320/170531835_1647752631842/D2B5CA33BD970F64A6301FA75AE2EB22)

|                    |         a          |         b          |
| :----------------: | :----------------: | :----------------: |
|  {0, 1, 2, 3, 7}   | {1, 2, 3, 4, 6, 7} | {1, 2, 3, 5, 6, 7} |
| {1, 2, 3, 4, 6, 7} | {1, 2, 3, 4, 6, 7} | {1, 2, 3, 5, 6, 7} |
| {1, 2, 3, 5, 6, 7} | {1, 2, 3, 4, 6, 7} | {1, 2, 3, 5, 6, 7} |

DFA:

![](https://uploadfiles.nowcoder.com/images/20220320/170531835_1647752978037/D2B5CA33BD970F64A6301FA75AE2EB22)

## 最小化 DFA
1. 划分初始状态集：
   - 终态集（双圈）
   - 非终态集
2. 对每个状态集，检查每个输入符号的转移目标是否在同一状态集内，若存在状态转移目标分属不同子集，则按转移目标的差异划分子集。
3. 重复步骤 2，直到无法再划分；
4. 每个状态集合并为 DFA 的一个状态，转移关系继承自原 DFA。含原 DFA 终态的合并状态，标记为最小 DFA 的终态。

$e.g. a(a|b)^*$

DFA:

![](https://uploadfiles.nowcoder.com/images/20220320/170531835_1647752978037/D2B5CA33BD970F64A6301FA75AE2EB22)

终态集：{A, B, C}，非终态集：$\emptyset$

划分后，终态集：{A}，非终态集：$\emptyset$

最小化 DFA:

![](https://uploadfiles.nowcoder.com/images/20220321/170531835_1647831313067/D2B5CA33BD970F64A6301FA75AE2EB22)