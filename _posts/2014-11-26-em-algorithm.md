---
title: "EM算法学习笔记"
description: ''
layout: post
output: word_document
tags:
- EM
- incomplete data
- MLE
category: 模式识别
---
## EM算法简介
EM算法主要是用于不完全数据的统计分析。很多情况下，无法获得样本的精确观测。比如，对观测变量$X$，我们不知道其确切的观测值，只知道其属于某个区间$A$。则此时，对数观测似然函数为：
$$ \log L(\theta;A)=P_X(x\in A; \theta).$$


不完全数据最常见的情形就是对于来自混合分布的样本，不知道样本来自哪一个确切的子类。在这种情况下，很难通过直接极大化观测似然函数的方法来获取MLE。 EM算法采取了一种迭代策略来得到对数观测似然函数的极大值，即迭代处理完全对数似然函数：
$$ \log L(\theta;x)=p_X(x;\theta).$$


迭代过程如下：

E-step: 计算完全似然函数在观测样本下的条件期望：
$$Q(\theta,\theta^{(q)})=\mathrm{E}_{\theta^{(q)}}[\log L(\theta;X)|A].$$

M-step: 极大化E步计算的Q函数(拟似然函数)。

E-step 和 M-step交替迭代，直到$L(\theta^{(q+1)};A)-L(\theta^{(q)};A)$小于一个给定的小量。


## EM算法证明