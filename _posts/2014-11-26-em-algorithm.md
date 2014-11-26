---
title: "EM算法学习笔记"
description: ''
layout: post
output: html_document
tags:
- EM
- incomplete data
- MLE
category: 模式识别
---
## EM算法简介
EM算法主要用于不完全数据的统计分析。很多情况下，无法获得样本的精确观测。比如，对观测变量$X$，我们不知道其确切的观测值，只知道其属于某个区间$A$。则此时，对数观测似然函数为：
$$ \log L(\theta;A)=P_X(x\in A; \theta).$$


不完全数据最常见的情形就是对于来自混合分布的样本，不知道样本来自哪一个确切的子类。在这种情况下，很难通过直接极大化观测似然函数的方法来获取MLE。 EM算法采取了一种迭代策略来得到对数观测似然函数的极大值，即迭代处理完全对数似然函数：
$$ \log L(\theta;x)=p_X(x;\theta).$$


迭代过程如下：

E-step: 计算完全似然函数在观测样本下的条件期望：
$$Q(\theta,\theta^{(q)})=\mathrm{E}_{\theta^{(q)}}\left[\log L(\theta;x)|A\right].$$

M-step: 极大化E步计算的Q函数(拟似然函数)。

E-step 和 M-step交替迭代，直到$L(\theta^{(q+1)};A)-L(\theta^{(q)};A)$小于一个给定的小量。


## EM算法证明
一般情况下，对数似然函数是有界的。在这种情况下，我们只需要证明，在每步迭代之后，观测似然函数值增加。

令
$$g(x|A;\theta)=\frac{L(\theta;x)}{L(\theta;A)}=p_X(x|A;\theta),$$
则有
$$\log L(\theta;A)=\log L(\theta;x)-\log g(x|A;\theta).$$


两边对$p_X(x|x\in A;\theta^{(q)})$条件期望可得：

$$\log L(\theta;A)=\mathrm{E}\left[\log L(\theta;x)|A;\theta^{(q)}\right]-\mathrm{E}\left[\log g(x|A;\theta)|A;\theta^{(q)}\right]
\\\\ \triangleq Q(\theta,\theta^{(q)})-H(\theta,\theta^{(q)}).$$
我们需要证明
$$L(\theta^{(q+1)},\theta^{(q)}) \geq L(\theta^{(q)},\theta^{(q)}).$$
可以看到，对数观测似然函数可以分解了$Q$和$H$两部分。其中$Q$就是我们要在E步计算的条件期望，也是在M步要对$\theta$求极大化的函数。所以，通过M步，便有会
$$Q(\theta^{(q+1)},\theta^{(q)})\geq Q(\theta^{(q)},\theta^{(q)}).$$
于是，下面只需要证明
$$H(\theta^{(q+1)},\theta^{(q)}) \leq H(\theta^{(q)},\theta^{(q)}).$$
事实上，对任意的$\theta$,
$$H(\theta,\theta^{(q)}) - H(\theta^{(q)},\theta^{(q)})
=\mathrm{E}\left[\log g(x|A;\theta)|A;\theta^{(q)}\right]-\mathrm{E}\left[\log g(x|A;\theta^{(q)})|A;\theta^{(q)}\right]\\
=\mathrm{E}\left[\left.\log \frac{g(x|A;\theta)}{g(x|A;\theta^{(q)})}\middle| A;\theta^{(q)}\right.\right]\\
\leq \log \mathrm{E}\left[ \left.\frac{g(x|A;\theta)}{g(x|A;\theta^{(q)})}\middle| A;\theta^{(q)}\right.\right]\\
=\log \int \frac{g(x|A;\theta)}{g(x|A;\theta^{(q)})} g(x|A;\theta^{(q)}) \mathrm{d}x \\
=\log \int p_X(x|A;\theta) \mathrm{d}x=0.$$
上式中的不等号是由Jensen不等式得到的。

从上面的证明过程可以看出，M步造成了$Q$部分的增加，而E步造成了$H$部分的增加。所以，经过一次交替的迭代，观测似然函数总是不减的。

## 总结
EM算法的过程类似于爬山的过程。在E步，它为对数观测似然函数找到了一个下界。然后在M步，极大化这个下界。然后，再通过E步，将这个极大值提升。有时不会想明白，E步找到的是下界，为什么对数观测似然函数的值会上升呢？其实，在EM算法刚开始的时候，给定初始点，进行E步，观测似然函数值并没有提升，它只是找到了一个下界函数$Q$，并且这个函数和对数观测似然函数在初始点的值相等。在极大化之后，我们再计算期望，才会提升观测似然函数值。
