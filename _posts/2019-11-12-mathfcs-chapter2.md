---
layout: post
title: 计算机科学中的数学第二章笔记和习题
categories: Maths
description: 
tags: [Maths]
---

## 良序原理
> 非负整数集中的每个非空子集都有一个最小元素.  

这个就是**良序原理**(The Well Ordering Principle).   
需要强调的是良序原理中的三个重点:  
1. **非空**子集  
2. **非负整数**集  
3. **每一个非**空子集


## 良序证明模板
使用良序原理, 证明良序性(Well Ordering)证明$P(n)$成立.  
使用良序原理, 证明"对所有$n\in\mathbb{N}$, $P(n)$为真":  
- 定义$C$是$P$为真的*反例*集合, 即  
$$C::=\{n\in\mathbb{N}|NOT(P(n))为真\}$$  
- 假设$C$是非空集进行反证.  
- 根据良序原理, 一定存在一个最小元素$n\in C$.  
- 得出矛盾 -- 通常是$P(n)$为自身, 或者$C$中也存在另一个比$n$小的元素. 这部分取决于具体的证明任务.  
- 得出结论, $C$一定是空寂, 即不存在反例.  

## 随堂练习和课后作业
### 习题2.4
$(\star)\sum^n_{k=0}k^2\neq\frac{n(n+1)(2n+1)}{6}$  

**证明.** 定义反例集合,   
$$C::=\{n\in\mathbb{N}|\sum^n_{k=0}k^2\neq\frac{n(n+1)(2n+1)}{6}\}$$    
假设反例是存在的, 即$C$是一个非负整数的非空集合. 那么, 根据良序原理, $C$有一个最小元素, 设它为$c$. 即在这些非负数中, $c$是最小反例.   
由于$c$是最小反例, 所以当$n=c$时式$(\star)$为假; 但是对于所有$n<c$的非负整数, 式$(\star)$为真. 所以$c-1$是非负整数, 而$n-1$比$n$小, 因此对$c-1$来说式$(*)$为真, 即,  
$$\sum^{c-1}_{k=0}k^2=\frac{(c-1)((c-1)+1)(2(c-1)+1)}{6}$$    
等式两边同时加$c^2$, 得,  
$$\sum^{c-1}_{k=0}k^2 + c^2=\frac{(c-1)((c-1)+1)(2(c-1)+1)}{6} + c^2$$   
$$\sum^{c}_{k=0}k^2=\frac{2c^3+3c^2+c}{6}$$    
$$\sum^{c}_{k=0}k^2=\frac{c(c+1)(2c+1)}{6}$$   
由上式可知, 式$(\star)$对$c$成立. 与假设矛盾, 证毕.   

### 习题2.6  
**证明.** 考虑第一种情况, 目标金额小于$2^m$. 定义反例集合,   
$$C::=\{m\in\mathbb{N}|m<2^{m+1} \land m\neq\sum_{0\leq p_1\leq p_2\leq\dots\leq p_k\leq m}2^{p_i}\}$$  

操, 放弃了.   