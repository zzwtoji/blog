---
date: "2025-12-10T15:01:14+08:00"
draft: false
title: 'LL(1) 文法'  
math: true
categories:
  - PrincipleofCompile
tags:
  - LL(1)
image: "img/PrincipleofCompile.svg"  
---

## 左递归

产生式左侧字母和右侧首字母相同，$S \rightarrow Sa$。

### 消除左递归

$$ P \rightarrow P \alpha | \beta \Rightarrow\left\{
\begin{array}{rcl}
P \rightarrow \beta P'\\
P' \rightarrow \alpha P' | \epsilon
\end{array} \right. $$

## 左公因子

产生式右侧有多个产生式以相同符号开头，$P \rightarrow \alpha \beta_1 | \alpha \beta_2$，$P \rightarrow \alpha（ \beta_1 | \beta_2）$。

### 消除左公因子

$$ P \rightarrow \alpha \beta_1 | \alpha \beta_2 \Rightarrow\left\{
\begin{array}{rcl}
P \rightarrow \alpha P'\\
P' \rightarrow \beta_1 | \beta_2
\end{array} \right. $$

## FIRST 集

1. A $\rightarrow$ aB，FIRST(A)={a}
2. A $\rightarrow$ aB| $\epsilon$，FIRST(A)={a，$\epsilon$}
3. A $\rightarrow$ BCD，FIRST(A)=FIRST(B)，B $\nrightarrow$ $\epsilon$
4. A $\rightarrow$ BCD 且 B $\rightarrow$ $\epsilon$，FIRST(A)={FIRST(C)}
5. A $\rightarrow$ a，FIRST(A)={a}

## FOLLOW 集

1. $ 为开始符号，FOLLOW(S)={$，...}
2. A $\rightarrow$ $\alpha B \beta$，FOLLOW(B)=FIRST($\beta$) - {$\epsilon$}
3. A $\rightarrow$ $\alpha B \beta$ 且 $\beta \rightarrow \epsilon$，FOLLOW(B)=FOLLOW(A)

## SELECT 集 $A \rightarrow \alpha$

1. 若 $\alpha \rightarrow \epsilon$，则 SELECT(A $\rightarrow \alpha$)=FOLLOW(A)
2. 若 $\alpha \nrightarrow \epsilon$，则 SELECT(A $\rightarrow \alpha$)=FIRST($\alpha$)

## 判断 LL(1) 文法

1. 不含左递归。
2. 不含左公因子。$A \rightarrow aB| \epsilon$，则 $A \rightarrow aB$ 和 $A \rightarrow \epsilon$ 的 FIRST 集无交集。
3. FIRST 集中含有 $\epsilon$ 时，FIRST(A) $\cap$ FOLLOW(A)=$ \emptyset $。

e.g.

E $\rightarrow$ E + T | T  
T $\rightarrow$ T * F | F  
F $\rightarrow$ ( E ) | i

消除左递归：

E $\rightarrow$ T E'  
E' $\rightarrow$ + T E' | $\epsilon$  
T $\rightarrow$ F T'  
T' $\rightarrow$ * F T' | $\epsilon$  
F $\rightarrow$ ( E ) | i

计算 FIRST 集：

FIRST(E)={FIRST(T)}={ ( , i }  
FIRST(E')={ + , $\epsilon$ }  
FIRST(T)={FIRST(F)}={ ( , i }  
FIRST(T')={ * , $\epsilon$ }  
FIRST(F)={ ( , i }

计算 FOLLOW 集：

FOLLOW(E)={$ , ) }  
FOLLOW(E')={FOLLOW(E)}={$ , ) }  
FOLLOW(T)={FIRST(E')} - {$\epsilon$} $\cup$ FOLLOW(E)={ + , $ , ) }  
FOLLOW(T')={FOLLOW(T)}={ + , $ , ) }  
FOLLOW(F)={FIRST(T')} - {$\epsilon$} $\cup$ FOLLOW(T)={ * , + , $ , ) }

计算 SELECT 集：

SELECT(E $\rightarrow$ T E')=FIRST(T)={ ( , i }  
SELECT(E' $\rightarrow$ + T E')={ + }  
SELECT(E' $\rightarrow$ $\epsilon$)=FOLLOW(E')={$ , ) }  
SELECT(T $\rightarrow$ F T')=FIRST(F)={ ( , i }  
SELECT(T' $\rightarrow$ * F T')={ * }  
SELECT(T' $\rightarrow$ $\epsilon$)=FOLLOW(T')={ + , $ , ) }  
SELECT(F $\rightarrow$ ( E ))={ ( }  
SELECT(F $\rightarrow$ i)={ i }

分析表：

|       |   (   |   )   |   i   |    +    |    *    |   $   |
| :---: | :---: | :---: | :---: | :-----: | :-----: | :---: |
|   E   | E→TE' |       | E→TE' |         |         |       |  |
|  E'   |       | E'→ε  |       | E'→+TE' |         | E'→ε  |
|   T   | T→FT' |       | T→FT' |         |         |       |
|  T'   |       | T'→ε  |       |  T'→ε   | T'→*FT' | T'→ε  |
|   F   | F→(E) |       |  F→i  |         |         |       |  |

对输入串 i*i+i 的分析过程：

|   栈    | 输入串 |     输出      |
| :-----: | :----: | :-----------: |
|   $E    | i*i+i$ |               |
|  $E'T   | i*i+i$ |     E→TE'     |
| $E'T'F  | i*i+i$ |     T→FT'     |
| $E'T'i  | i*i+i$ |      F→i      |
|  $E'T'  | *i+i$  |               |
| $E'T'F* | *i+i$  |    T'→*FT'    |
| $E'T'F  |  i+i$  |               |
| $E'T'i  |  i+i$  |      F→i      |
|  $E'T'  |  +i$   |               |
|   $E'   |  +i$   | T'→$\epsilon$ |
|  $E'T+  |  +i$   |    E'→+TE'    |
|  $E'T   |   i$   |               |
| $E'T'F  |   i$   |     T→FT'     |
| $E'T'i  |   i$   |      F→i      |
|  $E'T'  |   $    |               |
|   $E'   |   $    | T'→$\epsilon$ |
|    $    |   $    | E'→$\epsilon$ |