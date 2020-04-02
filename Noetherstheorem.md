---
title: 诺特定律
typora-copy-images-to: Noetherstheorem
date: 2020-03-30 21:33:47
math: true
tags:
- 学习
categories: 物理
---

诺特定律, 也即对称性蕴含守恒流, 更加准确的说法是

每一个局部作用的可微的对称性, 都蕴含某种守恒流.

但是我更想讨论的是场论下的特例:

系统有连续对称性, 满足运动方程, 则存在某个守恒流
### 什么是守恒流

守恒流是满足:
$$
\partial_\mu j^\mu = 0
$$
的一个场. 
### 对称变换的描述: 场在对称变换下的描述

而对称性的考量数学上就有两种看法

- 主动视角, 对应参照系不变, 物理点运动;
- 被动视角, 物理点不变, 参照系运动.

接下来我们会更加自然的考虑后者, 因为后者更加容易给出明确的数学表示. 

也就是时空点 `$P$`, 坐标`$x^\mu$`, 场`$f\left(x^\mu\right)$`, 变为坐标`$x'^\mu$`, 场`$f'\left(x'^\mu\right)$`, 

考虑在一个无穷小变换 `$x' \to x + \delta x$` 下, 场的变换可以写作:
$$
\begin{aligned}
	\delta f	& = f'(x'^\mu) - f(x^\mu)	\\
				& = f'(x^\mu + \delta x^\mu) - f(x^\mu)\\
				& = f'(x^\mu) - f(x) + \delta x^\mu\partial_\mu f'(x^\mu) + \mathcal{O}(\delta x^2)	\\
				& = f'(x^\mu) - f(x) + \delta x^\mu\partial_\mu f(x^\mu) + \mathcal{O}(\delta x^2)
\end{aligned}
$$

而 `$\partial_\mu f' $` 被换成 `$\partial_\mu f$` 带来的差异不会大于 `$\delta x$` 的一阶. 并定义 `$f'(x^\mu) - f(x)$` 为 `$\delta_0 f$` 从而可以写出:
$$
\delta f = \delta_0 f + \delta x^\mu\partial_\mu f
$$
或者说
$$
\delta = \delta_0 + \delta x^\mu\partial_\mu
$$
其中`$\delta_0 f$` 表述场本身的变化, 而 `$\delta x^\mu\partial_\mu f$` 表示由于点坐标的变化带来的变化. 

> 对于时空平移变换 `$ x'^\mu = x^\mu + a^\mu,\,\delta\phi = 0$`
> 
> 此时有
> $$
> \delta_0 \phi = - a^\mu\partial_\mu\phi
> $$
> 其实这就对应于场在平移下的变换: 
> $$
> \phi'(x^\mu) = \phi(x^\mu - a^\mu)
> $$

在此之外, 场可能有内禀变换, 此时`$\delta x = 0$`, 变化的只有场.
## 诺特流的推导

考虑一个场, 其作用量可以写作
$$
\newcommand{\dif}{\mathop{}\!\mathrm{d}}
S(\phi(x)) = \int \dif^4x \mathcal{L}(\phi, \partial_\mu\phi,x)
$$

系统的演化路径遵循 `$\delta S = 0$`, 则:
$$
\begin{aligned}
0 = \delta S &= \int\left[\delta(\dif^4x)\mathcal{L}+\dif^4x\delta\mathcal{L}\right]\\
& = \int\dif^4x(\partial_\mu\delta x^\mu\mathcal{L}+\delta\mathcal{L})\\

\end{aligned}
$$
而根据前文以及链式法则
$$
\begin{aligned}
\delta \mathcal{L} &= \delta x^\mu\partial_\mu\mathcal{L} + \delta_0\mathcal{L}\\
&= \delta x^\mu \partial_\mu\mathcal{L}+ \frac{\partial\mathcal{L}}{\partial\phi}\delta_0\phi + \frac{\partial \mathcal{L}}{\partial (\partial_\mu \phi)}\delta_0(\partial_\mu\phi) \\
&= \delta x^\mu \partial_\mu\mathcal{L} + \left[ \frac{\partial\mathcal{L}}{\partial \phi} - \partial_\mu \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)} \right] \delta_0\phi + \partial_\mu\left( \frac{\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_0\phi \right)
\end{aligned}
$$
最后一步是使用了分部积分, 并且使用了 `$\delta_0(\partial_\mu\phi) = \partial_\mu(\delta_0\mu\phi)$`. 同时注意到有: 
$$
\frac{\partial\mathcal{L}}{\partial \phi} - \partial_\mu \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)} = 0
$$
这是场的欧拉-拉格朗日运动方程. 则
$$
\begin{aligned}
0 = \delta S & = \int \dif^4x \partial_\mu\left(\delta x^\mu \mathcal{L}  + \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_0\phi\right)
\end{aligned}
$$
考虑到变分的任意性, 则有 `$\partial_\mu\left(\delta x^\mu \mathcal{L}  + \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_0\phi\right) = 0$`, 对扩号内的部分使用 `$\delta_0 = \delta - \delta x^\mu\partial_\mu$`, 则:

$$
\begin{aligned}
0 = \delta S&= \int \dif^4x\partial_\mu \left[\mathcal{L}\delta x^\mu + \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)} (\delta - \delta x^\nu\partial_\nu)\phi\right] \\
& = \int \dif^4x\partial_\mu\left[ \left(\mathcal{L}\delta^\nu_\mu - \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_\nu \phi\right)\delta x^\nu +\frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)} \delta\phi \right]
\end{aligned}
$$
这样, 就得到了一个守恒流, 也就是本文的核心: 
$$
j^\mu = \left(\mathcal{L}\delta^\nu_\mu - \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_\nu \phi\right)\delta x^\nu +\frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)} \delta\phi
$$

### 诺特荷
对于一个在有限空间内分布的流(无穷远处, 场应该趋近于零, 这是其物理意义要求的), 考虑场的等时变分
$$
\begin{aligned}
0 	& = \int \dif^4 x\partial_\mu j^\mu		\\
	& = \int_{t_1}^{t_2} \dif x^0 \int \dif x^3 \left(\partial_0 j^0 + \nabla \cdot \vec{j}\right)		\\
	& =  \int_{t_1}^{t_2} \dif x^0 \partial_0 \int \dif^3 j^0		\\
 	& = Q(t_2) - Q(t_1) 
\end{aligned}
$$

则 `$ Q = \int \dif^3 j^0 $` 就是诺特荷, 是在这个对称性给出的守恒量.

对于时空平移变换的特殊情况:

> 平移变换具有各向同性(说人话就是朝着时空四个轴有四个**生成元**)
> 那么, 诺特流就会升级成为能量动量张量
> 原来的守恒流长这样
> $$
> j^\mu = \left(\mathcal{L}\delta^\nu_\mu - \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_\nu \phi\right)\delta x^\nu +\frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)} \delta\phi
> $$
> 去掉场的本身的变换: `$ \delta\phi = 0$`, 考虑到`$ \delta x^\nu = a^\nu$` 的任意性
> $$
> \partial_\mu\left(\mathcal{L}\delta^\nu_\mu - \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_\nu \phi\right) = 0
> $$
> 这就是场论的能量动量张量: 
> $$
> \begin{aligned}
> T^{\mu}_{\nu} =  \left(- \mathcal{L}\delta^\nu_\mu + \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta_\nu \phi\right) \\
> T^{\mu\nu} =  \left(- \mathcal{L}\eta^{\mu\nu} + \frac{\partial\mathcal{L}}{\partial (\partial_\mu \phi)}\delta^\nu \phi\right)
> \end{aligned}
> $$

对于一个场的内禀变换而言, 比如复 Klein-Gordon 场的 `$U(1)$` 对称性

> $$
> \begin{aligned}
> 	\phi	&\to \mathrm{e}^{i\alpha} \phi	\\
> 	\phi^*	&\to \mathrm{e}^{-i\alpha} \phi^*
> \end{aligned}
> $$
> 借助生成元 `$\delta\phi = i\phi,\, \delta\phi^* = i\phi^*$`, 可以写出
> $$
> j^\mu = i\left[ (\partial^\mu \phi^*)\phi - \phi^*(\partial^\mu \phi)\right]
> $$
> 它对应的守恒荷就是:
> $$
> Q = \int \dif^3 j^0 = i\int \dif^3 x\left(\dot{\phi}^*\phi - \phi^*\dot{\phi}\right)
> $$
> 可以通过正则量子化计算发现这个守恒就对应电荷守恒.