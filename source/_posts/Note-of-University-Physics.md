---
title: 大学物理
date: 2023-03-10 22:42:27
tags: 学科笔记
---
# 质点运动学

## 质点及其运动描述

### 质点及其有关概念

定义：

* 质点（mass point）是指值只有质量而没有体积和形状的点，是一种理想的物理模型；<!--more-->
* 位置矢量（position vector），简称位矢，是用来描述质点相对于某参考点的「几何位置」的向量，一般用 $\boldsymbol{r}$ 表示，在三维空间下，可以用三个维度的分量表示，即 $\boldsymbol{r} = x\boldsymbol{i} + y\boldsymbol{j} + z\boldsymbol{k}$，其值 $|\boldsymbol{r}| = \sqrt{x^2 + y^2 + z^2}$；
* 运动方程是用来描述位矢 $\boldsymbol{r}$ 关于时间 $t$ 的函数，即 $\boldsymbol{r} = \boldsymbol{r}(t) = x(t)\boldsymbol{i} + y(t)\boldsymbol{j} + z(t)\boldsymbol{k}$；
* 位移（displacement）用于描述质点的位置变换，是矢量，与过程无关，只与初末位矢有关；
* 路程指质点运动轨迹的长度，是标量，与初末位置无关，只与过程有关，用 $s$ 表示。

### 位移、速度和加速度

已知一个运动方程 $\boldsymbol{r}(t)$，易知平均速度 $\overline v = \frac{\boldsymbol{r}(t_2) - \boldsymbol{r}(t_1)}{t_2 - t_1}$，当 $\Delta t = t_2 - t_1$ 趋近于零，所得的速度就是瞬时速度，即
$$
\boldsymbol{v}(t) = \lim\limits_{\Delta t \rightarrow 0}\frac{\boldsymbol{r}(t_2) - \boldsymbol{r}(t_1)}{t_2 - t_1}
$$
对运动方程求导即可得到速度方程
$$
\boldsymbol{v}(t) = \lim\limits_{\Delta t \rightarrow 0}\frac{\boldsymbol{r}(t_2) - \boldsymbol{r}(t_1)}{t_2 - t_1} = \frac{\mathrm{d}\boldsymbol{r}(t)}{\mathrm{d}t}
$$

相似地，可以求得加速度方程 
$$
\boldsymbol{a}(t) = \lim\limits_{\Delta t \rightarrow 0}\frac{\boldsymbol{v}(t_2) - \boldsymbol{v}(t_1)}{t_2 - t_1} = \frac{\mathrm{d}\boldsymbol{v}(t)}{\mathrm{d}t} = \frac{\mathrm{d}^2 \boldsymbol{r}(t)}{\mathrm{d} t^2}
$$
可以看出，位移和速度，速度和加速度都是函数和导函数的关系；反之，加速度和速度，速度和位移是函数和原函数的关系，注意求积分会产生的常量 $C$ 的意义。

## 圆周运动

### 匀速圆周运动

严格来说，匀速圆周运动应该叫匀速率圆周运动，因为速度的方向一直在变，而速率保持不变。

依据上一节速度的定义，速度依旧可以描述圆周运动，此时称为线速度；此外还可以用角度和半径描述圆周运动 $r\theta$，由于半径是确定的，所以可以用角速度 $\omega$ 来描述位矢的变化，即 $\omega = \lim\limits_{\Delta t \rightarrow 0}\frac{\theta_2 - \theta_1}{t_2 - t_1}$，又因为
$$
\boldsymbol{r}(t_2) - \boldsymbol{r}(t_1) = r(\theta_2 - \theta_1)
$$
所以有
$$
\boldsymbol{v}(t) = r\boldsymbol{\omega}(t)
$$

再研究匀速圆周运动的加速度 $a$，思路是：先列出速度与位移的关系，对两边求导就得到了加速度与速度的关系。设质点做圆周运动的过程中，从 $t_1$ 时刻的 $A$ 点移动到 $t_2$ 时刻的 $B$ 点，速度从 $\boldsymbol{v}_1$ 变成了 $\boldsymbol{v}_2$，$\Delta \boldsymbol{v} = \boldsymbol{v}_2 - \boldsymbol{v}_1$ 速率 $v = |\boldsymbol{v}_1| = |\boldsymbol{v}_2|$，由相似三角形可得：
$$
\begin{align}
\frac{|\Delta \boldsymbol{v}|}{v} & = \frac{|AB|}{r} \newline
|\Delta \boldsymbol{v}| & = \frac{v}{r} |AB| \newline
\frac{\mathrm{d}|\Delta \boldsymbol{v}|}{\mathrm{d}t} & = \frac{v}{r} \frac{\mathrm{d}|AB|}{\mathrm{d}t} \newline
\boldsymbol{a} & = \frac{v^2}{r} \newline
& = \omega^2r
\end{align}
$$

### 变速圆周运动

同样从 $\boldsymbol{v}_1$ 变成 $\boldsymbol{v}_2$，将 $\Delta \boldsymbol{v} = \boldsymbol{v}_2 - \boldsymbol{v}_1$ 拆分成「法向」和「切向」上的变化。

具体地，法向速度变化 $\Delta \boldsymbol{v}_n$ 使得 $\boldsymbol{v}_1 + \Delta \boldsymbol{v}_n // \boldsymbol{v}_2$ 且 $|\boldsymbol{v}_1 + \Delta \boldsymbol{v}_n| = |\boldsymbol{v}_1|$；切向速度变化 $\Delta \boldsymbol{v}_t$ 使得 $\boldsymbol{v}_t // \boldsymbol{v}_2$ 且 $\boldsymbol{v}_1 + \Delta \boldsymbol{v}_n + \Delta \boldsymbol{v}_t = \boldsymbol{v}_2$。即 $\Delta \boldsymbol{v} = \Delta \boldsymbol{v}_n + \Delta \boldsymbol{v}_t$

由此可以得出变速圆周运动的加速度

$$
\begin{align}
\boldsymbol{a} & = \lim\limits_{\Delta t \rightarrow 0}\frac{\Delta\boldsymbol{v}}{\Delta t} \newline
& = \lim\limits_{\Delta t \rightarrow 0}\frac{\Delta\boldsymbol{v}_n}{\Delta t} + \lim\limits_{\Delta t \rightarrow 0}\frac{\Delta\boldsymbol{v}_t}{\Delta t}
\end{align}
$$

分别设法向加速度 $\boldsymbol{a}_n = \lim\limits_{\Delta t \rightarrow 0}\frac{\Delta\boldsymbol{v}_n}{\Delta t}$ 和切向加速度 $\boldsymbol{a}_t = \lim\limits_{\Delta t \rightarrow 0}\frac{\Delta\boldsymbol{v}_t}{\Delta t}$，则此时有

$$
\boldsymbol{a} = \boldsymbol{a}_n + \boldsymbol{a}_t
$$

**注意圆周运动切向和法向加速度**!
$$
\begin{align}
\boldsymbol{a}&=\sqrt{a_t^2\boldsymbol{e_t}+a_n^2\boldsymbol{e_n}} \newline
&=\sqrt{(\frac{\mathrm{d}v}{\mathrm{d}t})^2\boldsymbol{e_t}+(\frac{v^2}{r})^2\boldsymbol{e_n}}
\end{align}
$$

## 牛顿运动定律

### 牛顿第一定律（惯性定律）

物体受到的合外力为零时，速度保持不变，公式表示为
$$
\sum_i\boldsymbol{F}_i = 0 \Rightarrow \frac{\mathrm{d}\boldsymbol{v}}{\mathrm{d}t} = 0
$$
其中 $\boldsymbol{F}_i$ 是第 $i$ 个外力，$\boldsymbol{v}$ 是速度，$t$ 是时间。

### 牛顿第二定律（加速度定律）

定义：动量（momentum）是物体质量与速度的乘积，记作 $\boldsymbol{p}$，即 $\boldsymbol{p} = m\boldsymbol{v}$。

# 刚体运动

## 势能

重力势能
$$
E_p=mgh
$$
弹性势能
$$
E_p=\frac{1}{2}kx^2
$$
引力势能
$$
E_p=-\frac{GMm}{r}
$$
电势能
$$
E_p=\frac{1}{4\pi\varepsilon_0}\frac{q_1q_2}{r}
$$

## 动能

两个动能表达式

平动
$$
E_k=\frac{1}{2}mv^2
$$
转动
$$
E_k=\frac{1}{2}J\omega^2
$$

## 角动量相关

速度与角速度的关系，唯一一个 $\boldsymbol{r}$ 在右边的
$$
\boldsymbol{v}=\boldsymbol{\omega}\times\boldsymbol{r}
$$
力矩与力的关系
$$
\boldsymbol{M}=\boldsymbol{r}\times\boldsymbol{F}
$$
力矩与角加速度的关系
$$
\boldsymbol{M}=J\boldsymbol{\alpha}
$$


离散体的转动惯量
$$
J=\sum m_i{r_i}^2
$$
连续体的转动惯量
$$
J=\int r^2\mathrm{d}m
$$
常见形状的转动惯量

- 细棒绕中心旋转：$J=\frac{ml^2}{12}$
- 细棒绕一端旋转：$J=\frac{ml^2}{3}$
- 圆盘绕中心旋转：$J=\frac{ml^2}{2}$
- 球体绕中心旋转：$J=\frac{2ml^2}{5}$

常配合平行轴定理：
$$
J=J_c+md^2
$$
角动量和动量的关系
$$
\boldsymbol{L}=\boldsymbol{r}\times\boldsymbol{p}=\boldsymbol{r}\times m\boldsymbol{v}
$$
角动量和角速度的关系
$$
\boldsymbol{L}=J\boldsymbol{\omega}
$$


# 静电场

库仑定律
$$
\boldsymbol{F}=\frac{1}{4\pi\varepsilon_0}\frac{q_1q_2}{r^2}\boldsymbol{e_r}
$$

结合电场强度的定义 $\boldsymbol{E}=\frac{\boldsymbol{F}}{q_0}$，得出
$$
\boldsymbol{E}=\frac{1}{4\pi\varepsilon_0}\frac{q_1}{r^2}\boldsymbol{e_r}
$$
接下来是两个通过积分得到的特殊情况下的结论，能积就积，积不出来就记着：

- 电荷线密度为 $\lambda$ 的无限长带电直线，距其 $r$ 一点的电场强度为

  $$
  E=\frac{\lambda}{2\pi\varepsilon_0r}
  $$

- 电荷面密度为 $\sigma$ 的无限大带点平面，任意一点的电场强度为
  $$
  E=\frac{\sigma}{2\pi\varepsilon_0}
  $$

高斯定理，这里 $Q$ 表示高斯面内的电荷量，$\Phi_e$ 是电通量
$$
\begin{align}
\Phi_e&=\oint_{S}E\mathrm{d}S \newline
&=\frac{Q}{\varepsilon_0}
\end{align}
$$
注意通过高斯定理的两个等式求电场强度！

前面通过电场强度的定义积分得到了**无线长带电直线**和**无限大带点平面**周围点的电场强度，此外特殊的形状如球壳和球体的电场强度就可以通过**高斯定理**求得。

$A$ 点处的电势
$$
V_A=\int_{AB}E\mathrm{d}l + V_B
$$

电容器
$$
\begin{align}
C&=\frac{Q}{U} \newline
&=\frac{\varepsilon_0S}{d}
\end{align}
$$

# 磁场

电流的定义
$$
\begin{align}
I&=\frac{\mathrm{d}q}{\mathrm{d}t} \newline
I&=nqSv
\end{align}
$$
毕奥-萨伐尔定律
$$
\begin{align}
\mathrm{d} \boldsymbol{B}&=\frac{\mu_0}{4\pi}\frac{I\mathrm{d}\boldsymbol{l}\times\boldsymbol{e_r}}{r^2} \newline
\boldsymbol{B}&=\frac{\mu_0}{4\pi}\frac{q\boldsymbol{v}\times\boldsymbol{e_r}}{r^2}
\end{align}
$$
特殊情况下的磁场强度

- 无线长直电流，距其 $r$ 处一点
  $$
  \boldsymbol{B}=\frac{\mu_0I}{2\pi r}
  $$

- 环形电流，圆心处
  $$
  \boldsymbol{B}=\frac{\mu_0I}{2r}
  $$
  

洛伦兹力
$$
\boldsymbol{F}=q\boldsymbol{v}\times\boldsymbol{B}
$$
安培力
$$
\begin{align}
\mathrm{d}\boldsymbol{F}&=I\mathrm{d}\boldsymbol{l}\times\boldsymbol{B} \newline
\boldsymbol{F}&=I\boldsymbol{l}\times\boldsymbol{B} \newline
\end{align}
$$

磁通量
$$
\Phi_m=\boldsymbol{B}\cdot\boldsymbol{S}
$$
磁场高斯定理：通过闭合曲面的磁通量为零，即
$$
\oint \boldsymbol{B}\cdot\mathrm{d}\boldsymbol{S}=0
$$

# 电磁感应

法拉第电磁感应定律：感应电动势
$$
\varepsilon=-\frac{\mathrm{d}\Phi_m}{\mathrm{d}t}
$$
