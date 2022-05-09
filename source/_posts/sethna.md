---
title: Sethna 统计物理学课本 读书笔记
date: 2019-07-23 00:34:06
categories: [笔记, 理学]
tags: [统计物理]
---

2019-7-28 先更到前八章

<!--more-->

### 第一章：为什么学统计力学

1.

许多概念和方法对于实用的科学影响深远：系综，熵，相和相变，蒙卡方法，涌现，以及临界性。

系综：系统中不能掌握的自由度太大，就别研究系统，而研究分布和统计量，这是一种有益的思维方式。

熵：统计力学对世界的大贡献。毋庸赘言。

相：大大加深我们对于对称性的理解。同时，还有和微扰论的联系——见第八章。

计算方法：以蒙卡为首，具有统计力学的精神。虽然这基本上是个新学科⑧。。。这方面还是要看krauth的书。

涌现：影响科学哲学的思维方式。系统在特定的极限附近产生大尺度上的新规律这一点同样是对既定物理直觉的一种冲击。

相变和临界性：我个人对于种种Ising和类Ising模型挺感兴趣。也许以后会在闲的时候多了解一些相关内容，总结起来。

2.

这一章的习题挺有意思的，介绍了一点随机矩阵的概念。感觉我的计算能力又退步了。

### 第二章：随机行走与涌现

1.

随机行走里面包含了两种涌现：

- 单次随机行走在长时间下有标度不变性
- 随机行走的终点有一个满足扩散定律的分布

我也不知道这是什么意思。

2.

容易从归纳法证明，一维随机行走的终点位置，其分布的方均根$\sigma\propto\sqrt{N}$。高维的时候，由于两次走的有$\left\langle\ell_{m} \cdot \ell_{n}\right\rangle= L^{2}\langle\cos (\theta)\rangle= 0$，该结论还成立。

对于这种情况，如果我们说这个随机行走的“体积”用N度量，线度用R表示，则R^D^=V中的D可知为2。这是一个2维分形。

对于长链聚合物的情形，由于它们是费米子组成的，$\sigma\propto N^\nu$的指数相应的要增加，在二维的情形下，可严格算出是3/4。

在以上的情况中，$\nu$是一个临界指数。随机行走这个东西具有所谓的“普适性”——一个玄学概念，我觉得被封为toy **model**的东西都应该具备才行——而$\nu$是本书中见到的首个“普适性”临界指数（不加前三个字，或许会更好听）。

3.

一堆分子的随机行走的分布自然地由扩散方程描述。它有形式$\frac{\partial \rho}{\partial t}=D \nabla^{2} \rho=D \frac{\partial^{2} \rho}{\partial x^{2}}$。

对于终点分布的密度，现在从$\rho\left(x^{\prime}, t\right)$求$\rho(x, t+\Delta t)$。单步的分布记做$\chi(l)$，

$\begin{aligned} \rho(x, t+\Delta t) &=\int_{-\infty}^{\infty} \rho\left(x^{\prime}, t\right) \chi\left(x-x^{\prime}\right) d x^{\prime} \\ &=\int_{-\infty}^{\infty} \rho(x-z, t) \chi(z) d z \end{aligned}$

如果$\rho$的空间尺度远大于步长，则展开之

$\begin{aligned} \rho(x, t+\Delta t) & \approx \int\left[\rho(x, t)-z \frac{\partial \rho}{\partial x}+\frac{z^{2}}{2} \frac{\partial^{2} \rho}{\partial x^{2}}\right] \chi(z) d z \\ &=\rho(x, t) \int \chi(z) d z-\frac{\partial \rho}{\partial x} \int z \chi(z) d z+1 / 2 \frac{\partial^{2} \rho}{\partial x^{2}} \int z^{2} \chi(z) d z \\ &=\rho(x, t)+1 / 2 \frac{\partial^{2} \rho}{\partial x^{2}} a^{2} \end{aligned}$

a是一个常数。这就是说$\frac{\partial \rho}{\partial t}=\frac{a^{2}}{2 \Delta t} \frac{\partial^{2} \rho}{\partial x^{2}}$，即扩散方程。$D=a^2/2\Delta t$。

4.

从守恒的要求，可以写$\frac{\partial \rho}{\partial t}=-\nabla\cdot J$。由扩散方程，流J是从高密度往低密度走。

如果有一个外力以$x(t+\Delta t)=x(t)+F \gamma \Delta t+\ell(t)$出现，可用前面的办法算出

$\begin{aligned} J &=\gamma F \rho-D \frac{\partial \rho}{\partial x} \\ \frac{\partial \rho}{\partial t} &=-\gamma F \frac{\partial \rho}{\partial x}+D \frac{\partial^{2} \rho}{\partial x^{2}} \end{aligned}$

把J的式子放在前是有理由的。作者在这里教诲说：当搞一个守恒的量时，先写它的流的表达式，确保它真的是守恒的——如果这里F不均匀，扩散方程就得多一项，先写J可以帮助避免在此疏漏。

要感觉这些方程，可以看看稳定的解。给定不可透过的边界，则无F时这假想气体的$\rho$得各处均匀，有F（重力）时则沉到地上。

5.

解这扩散方程的工具包括傅里叶变化和格林函数。前者由于FFT的存在而受计算上的偏好，后者则在解析解方面更重要，可以用来做一些估计。

在相当松的条件下，具有平移不变性的系统中的线性方程都有平面波解。对一个傅里叶分量$\tilde{\rho}_k$来说，

$\begin{aligned} \frac{\partial \rho}{\partial t}=\frac{d \widetilde{\rho}_{k}}{d t} e^{-i k x} &=D \frac{\partial^{2} \rho}{\partial x^{2}}=-D k^{2} \widetilde{\rho}_{k} e^{-i k x} \\ \frac{d \widetilde{\rho}_{k}}{d t} &=-D k^{2} \widetilde{\rho}_{k} \\ \text{是故}\widetilde{\rho}_{k}(t) &=\widetilde{\rho}_{k}(0) e^{-D k^{2} t} \end{aligned}$

这样$\rho(x, t)=\frac{1}{2 \pi} \int_{-\infty}^{\infty} \widetilde{\rho}_{k}(0) e^{-i k x} e^{-D k^{2} t} d k$，而由初始条件可以知道$\widetilde{\rho}_{k}(0)=\int_{-\infty}^{\infty} \rho(x, 0) e^{i k x} d x$，这就解出来了。这说明时间一长，波长短的分量基本上都迅速decay掉了。

其实我也想不起来格林函数能用的具体条件了。取$G(x, 0)=\delta(x)$，用傅里叶方法解出来$\widetilde{G}_{k}(0)=1$，$G(x, t)=\frac{1}{\sqrt{4 \pi D t}} e^{-x^{2} / 4 D t}$，于是

$\rho(x, t)=\int \rho(y, 0) G(x-y, t) d y=\int \rho(y, 0) \frac{e^{-(y-x)^{2} / 4 D t}}{\sqrt{4 \pi D t}} d y$

这个形式（卷积）意味着随着时间的流逝，$\rho(y,0)$的不平之处会逐渐被抹平。

### 第三章：温度和热力学平衡

1.

微正则系综中不加说明（下一章才有说明）地指出，等能面上各相点取到的概率相等。约束NVE后，系统的平衡态即确定。

2.

考虑一个盒子，有2N个粒子，而其中恰有N+m个在盒子的右边。这概率就是$P_{m}=2^{-2 N}\left(\begin{array}{c}{2 N} \\ {N+m}\end{array}\right)$。取对数，用斯特林公式，

$\begin{aligned}\log P_m&\approx2N\log N-(N+m)(\log N+m/N-m^2/N^2)\\ &-(N-m)(\log N-m/N-m^2/N^2)\\&\approx -m^2/N\end{aligned}$

归一化，得$P_{m} \approx \sqrt{1 / \pi N} \exp \left(-m^{2} / N\right)$，粒子数涨落是高斯分布的，其方差$\propto\sqrt{N}$。

3.

考虑单原子分子理想气体。已知高维球体积$\mu\left(\mathbb{S}_{R}^{\ell-1}\right)=\pi^{\ell / 2} R^{\ell} / \frac{\ell}{2} !$，可算出相空间体积的微分$(3 N / 2 E) \pi^{\frac{3 N}{2}}(2 m E)^{\frac{3 N}{2}} / \frac{3 N !}{2}$。针对某一个粒子，$\rho\left(p_{1}\right)=\frac{1}{\sqrt{2 \pi m(2 E / 3 N)}} \exp \left(\frac{-p_{1}^{2}}{2 m} \frac{3 N}{2 E}\right)$。这些都是人们在课上所知道的。

4.

温度是一个重要的东西。有了温度，就有了更方便的正则系综。由于温度是定义出来反映平衡系统的普遍特性（能量）的，它在系综理论的框架下非常高效。为了把能量单位转成温度单位，人们启用了常数$k_B$。

取两个系统相互接触，其能量为E~1~，E~2~。其总能量为E。对于某一个系统，有$\rho\left(E_{1}\right)=\Omega_{1}\left(E_{1}\right) \Omega_{2}\left(E-E_{1}\right) / \Omega(E)$，$\Omega(E)=\int d E_{1} \Omega_{1}\left(E_{1}\right) \Omega_{2}\left(E-E_{1}\right)$。因此，为了取到平衡，有$\frac{1}{\Omega_{1}} \frac{d \Omega_{1}}{d E_{1}}=\frac{1}{\Omega_{2}} \frac{d \Omega_{2}}{d E_{2}}$。引入熵S，则两边是$dS/dE$，定义这个东西是$1/T$。它是从外界买来定量的能量所需要花费（向外输出）的熵值。

考虑平衡附近的涨落。若$E_{1}^{*}$是平衡处系统1的能量，可以得到$\rho\left(E_{1}\right)=\frac{1}{\sqrt{2 \pi} \sigma_{E}} e^{-\left(E_{1}-E_{1}^{*}\right)^{2} / 2 \sigma_{E}^{2}}$。其方差是$k_{B} / \sigma_{E}^{2}=\frac{\partial^{2} S_{1}}{\partial E_{1}^{2}}+\frac{\partial^{2} S_{2}}{\partial E_{2}^{2}}$。考虑到S是广延量，这方差$\propto N^{-1}$。实际上，更详细的说法是，由于$\frac{\partial^2S}{\partial E^2}/k_B=-\frac{1}{k_BT}\frac{1}{Nc_vT}$，而前者是一个能量单位，后者则是广延量，因此其几何平均值$\propto N^{-1}$。这个式子同时还提供了一个很好的测热容的办法。

5.

熵对NVE中的另两者求导可以得到压强和化学势。事实上，在这书里它们就是这样定义的。

有一个有趣的推导，即从微正则系综来推导压强的力学定义$P=-\Delta E/\Delta V$与现在的统计力学定义$P=T\left.\frac{\partial S}{\partial V}\right|_{E, N}$是等价的。要做到这一点，得在新的框架下把$P=-\Delta E/\Delta V$写出来。考虑一个液体绝热变化，从$V$到$V+\Delta V$再回到$V$。这过程中液体的哈密顿量（本书作者特别喜欢用\mathbb来代表相空间的坐标）$\mathcal{H}(\mathbb{P}, \mathbb{Q}, V(t))$有$\frac{d \mathcal{H}(\mathbb{P}(t), \mathbb{Q}(t), V(t))}{d t}=\frac{\partial\mathcal{H}}{\partial t}=\frac{\partial \mathcal{H}}{\partial V}(\mathbb{P}, \mathbb{Q}) \frac{d V}{d t}$

取系综平均。我们假设，在此绝热过程中，系统变得太慢，以至于它总是在平衡态。这样就可以得到$\frac{d\langle\mathcal{H}\rangle}{d t}=\left\langle\frac{\partial \mathcal{H}}{\partial V}\right\rangle_{E(t), V(t)} \frac{d V}{d t}$。由旧的定义即$d\left<\mathcal{H}\right>=-PdV$，$-P=\left\langle\frac{\partial \mathcal{H}}{\partial V}\right\rangle=\frac{1}{\Omega(E)} \int d \mathbb{P} d \mathbb{Q} \delta(E-\mathcal{H}(\mathbb{P}, \mathbb{Q}, V)) \frac{\partial \mathcal{H}}{\partial V}$。另一方面，由于

$\begin{aligned}\Omega(E)&=\frac{\partial}{\partial E}|_{V, N}\int_{E<\mathcal{H}<E+\delta E}d\mathbb{P}d\mathbb{Q}\\&=\frac{\partial}{\partial E}|_{V, N}\int d\mathbb{P}d\mathbb{Q}\Theta(E-\mathcal{H})\end{aligned}$

而新的定义又有$\left.\frac{\partial S}{\partial V}\right|_{E, N}=\frac{\partial}{\partial V} k_{B} \log (\Omega)=\frac{k_{B}}{\Omega}\left.\frac{\partial \Omega}{\partial V}\right|_{E, N}$，可知

$\begin{aligned}\left.\frac{\partial \Omega}{\partial V}\right|_{E, N} &=\left.\frac{\partial}{\partial V}\right|_{E, N}\left.\frac{\partial}{\partial E}\right|_{V, N} \int d \mathbb{P} d \mathbb{Q}(E-\mathcal{H}(\mathbb{P}, \mathbb{Q}, V)) \\ &=\left.\frac{\partial}{\partial E}\right|_{V, N} \int d \mathbb{P} d \mathbb{Q} \frac{\partial}{\partial V} \Theta(E-\mathcal{H}(\mathbb{P}, \mathbb{Q}, V)) \\ &=-\left.\frac{\partial}{\partial E}\right|_{V, N} \int d \mathbb{P} d \mathbb{Q} \delta(E-\mathcal{H}(\mathbb{P}, \mathbb{Q}, V)) \frac{\partial \mathcal{H}}{\partial V} \end{aligned}$

注意这个积分，它正是老定义下的$P\Omega(E)$。代回去，即有

$\left.\frac{\partial S}{\partial V}\right|_{E, N}=P / T+k_{B}\left.\frac{\partial P}{\partial E}\right|_{V, N}$

右边第二项是$N^{-1}$小量。这样就证完了。

6.

我们可以从微正则系综得到理想气体的状态方程。不过，这一节重要的内容是介绍了量子力学对相空间体积计算的修正：

- 经典力学并没规定相空间体积元的大小。这反映在$\Omega$具有未定的（常）系数上，导致熵没有一个自然的取零的地方。在量子力学里面，相空间的体积元将被证明是$h^{3N}$。
- 更大的问题是，经典力学由于没有考虑到粒子的全同性，其计算相空间体积时多了一个因子$N!$，这导致熵的可加性受到损害。在量子力学出来之前，吉布斯敏锐地观察到了这一点并作了修正，但是他不知道这背后具体的缘故。

修正完了之后，有著名的熵公式

$S(E, V, N)=\frac{5}{2} N k_{B}+N k_{B} \log \left[\frac{V}{N h^{3}}\left(\frac{4 \pi m E}{3 N}\right)^{3 / 2}\right]$

我们在这里引入德布罗意热波长$\lambda=h / \sqrt{4 \pi m E / 3 N}$，它的名字是因为它具有长度的量纲并且和粒子的吴志波波长相近。在量子化统计力学的过程中，随着系统的某个特征长度可以与它相比，它将起到重要的作用。

6.

这章的习题也很有启发性。

### 第四章：相空间动力学与各态历经

1.

如果没有这一章的话，上一章中对于微正则系综的许多假设未免有些轻率。通过刘维尔定理和各态历经，我们可以说明相空间的体积不变，因此各处的状态密度也不随着系统演化而变化。各态历经的概念则（有些反直觉的，至少我初学时这样感到）说明系综平均确实等于时间平均（事实上，很多系统并不满足这个性质）。

2.

在**哈密顿**系统中，有关系$\begin{aligned} \dot{q}_{\alpha} &=\partial \mathcal{H} / \partial p_{\alpha} \\ \dot{p}_{\alpha} &=-\partial \mathcal{H} / \partial q_{\alpha} \end{aligned}$。考虑相空间体积的全微分$d\rho/dt=\partial \rho / \partial t+\mathbf{v} \cdot \nabla \rho$，而出于相流局部守恒的假设，任取一小块相空间，有相流带来的密度变化$-\nabla \cdot \mathbf{J}, \text { where } \mathbf{J}=\rho_{3 D} \mathbf{v}$。是故

$\begin{aligned} \frac{\partial \rho}{\partial t} &=-\sum_{\alpha=1}^{3 N}\left(\frac{\partial\left(\rho \dot{q}_{\alpha}\right)}{\partial q_{\alpha}}+\frac{\partial\left(\rho \dot{p}_{\alpha}\right)}{\partial p_{\alpha}}\right) \\ &=-\sum_{\alpha=1}^{3 N}\left(\frac{\partial \rho}{\partial q_{\alpha}} \dot{q}_{\alpha}+\rho \frac{\partial \dot{q}_{\alpha}}{\partial q_{\alpha}}+\frac{\partial \rho}{\partial p_{\alpha}} \dot{p}_{\alpha}+\rho \frac{\partial \dot{p}_{\alpha}}{\partial p_{\alpha}}\right) \end{aligned}$

由上面的关系，上式右边实际上就是$-\mathbf{v} \cdot \nabla \rho$。所以说相空间密度的全微分是0，一般流体密度满足这个性质，即叫做不可压缩流体。

3.

关于各态历经的讨论向来是又玄又tricky。事实上，在绝大多数情况下，即使系统并不符合各态历经假设（例如玻璃态），只要系统中有大量的相互作用的自由度，我们也可以放心大胆地相信统计力学的正确性。在这个时候，我（懒惰的读者）选择暂时不关心统计力学的基础问题。

### 第五章：熵

熵的定义多种多样，熵的力量源远流长。

1.

最初的时候，人们从**不可逆性**这一点来定义熵。这是有历史原因的。在第一类永动机由于能量守恒而失败之后，人们准备从热——能量的新形式中找到无限能量的法门，因为世上的热是很多的。可惜的是，经过一些尝试，人们感觉到热这种形式中似乎有某种原理性的限制，使得热不能为我们所用。能量变成了热，就废了。

我们熟悉尼古拉-卡诺的热机。他利用这个模型说明了这种原理性的限制，并证明了可逆过程中$Q_1/T_1=Q_2/T_2$。在系统同热库接触的过程中，$\Delta S_{thermo}=Q/T$表征了某种东西的流动。那就是熵。

在实际情况下，任何的热机都会创造正熵。这代表着一种不可逆性。这是很奇怪的，因为分子运动的普遍规律具有时间反演对称性，而熵乘着时间的箭头无视了它。解决方案是认为宇宙在向着一个平衡态演化——它就是热寂。

我个人在这个地方仍然感到很混乱。实际上，如果认为是非平衡态向平衡态演化，描述这个过程的应该是H定理。H定理是BBGKY hierarchy的基础，而后者主导着非平衡态统计物理领域，但H定理同时是一个很乱的问题。刚刚推出的时候玻尔兹曼用Stosszahlansatz（分子碰撞运动无关联假设）推掉了洛施密特的质疑（即上文“这是很奇怪的”处的相同质疑，但这回H定理是非平衡态的定理），但是这个假设并不是很能令严谨的数学人满意。他们之前在这个地方出发讨论了热力学极限的定义，并且他们现在还在做H定理的证明。总之，时间箭头的问题上，我了解的越多越困惑。有一篇知乎专栏文章似乎指出了一些可以思考的方向：https://zhuanlan.zhihu.com/p/51920899

2.

熵也被理解为对于**混乱程度**的度量。我们之前已经在此基础上推算了$1 / T=\partial S / \partial E$，这与上一种定义是等价的。我们已经比较了解这个定义了，所以可以看一个例子。在玻璃态物质中，系统（一般是液体）由于温度下降的过快而把许多原子“冻”在非最低能级的位置。因此这个系统降到绝对零度的时候还有一个残余的熵。人们一般用$S_{\text { liquid }}\left(T_{\ell}\right)-\int_{0}^{T_{\ell}} \frac{1}{T} \frac{d Q}{d T} d T$计算这个熵。该熵实际上的大小可以如下估计：对于玻璃态下单个原子的位势，一般有常数个极小值，而这些极小值之间的能量之差远小于$k_BT$和它们之间的势垒。在这个情况下，可以认为每个原子（粒子）有常数个可选的位置，对应$k_B$级别的熵。因此，系统残余的熵依然是很大的。实际上，即使把玻璃降到了绝对零度，随着它里面的原子慢慢地找到全局的最小值，它还是会慢慢地变温暖一些，虽然真的非常慢。

3.

熵也被理解为对于**对系统的无知**的度量。依然使用玻璃态的例子，如果我们拿X光测了各个粒子的实际位置，这系统的熵就降到0，我们就可以利用这些信息从这个玻璃态系统里继续榨取功。

考虑一个概率分布$\left\{p_i\right\}$。可以证明$S_{\text { discrete }}=-k_{B}\left\langle\log p_{i}\right\rangle=- k_{B} \sum_{i} p_{i} \log p_{i}$。这式子在量子统计中需要将取均值用的$\sum_ip_i()$变成$Tr[ \boldsymbol{\rho}()]$。香农熵也是这个定义法，只是把底数的e换成2。当然，双方（物理和CS领域）似乎都是叫做log的。

香农熵的“自然性”来自于以下的结论：对于给定的离散随机变量的泛函$H_{m}\left(p_{1}, p_{2}, \ldots, p_{m}\right)$，如果满足以下三条性质：

- $H_{2}\left(\frac{1}{2}, \frac{1}{2}\right)=1$（归一化）
- $H_{2}(p, 1-p)$是关于p的连续函数（连续性）
- $H_{m}\left(p_{1}, p_{2}, \ldots, p_{m}\right)=H_{m-1}\left(p_{1}+p_{2}, p_{3}, \dots, p_{m}\right)+\left(p_{1}+p_{2}\right) H_{2}\left(\frac{p_{1}}{p_{1}+p_{2}}, \frac{p_{2}}{p_{1}+p_{2}}\right)$（组合性）

那么唯一的解就是香农熵。组合性对应于熵的广延量的性质。注意到，一个分布的香农熵可以看做是该分布到均匀分布的Kullback-Leibler散度的相反数，最大化香农熵实际上就是基于我们对于系统的知识找出最“均匀”的先验分布，而对于均匀的信念正是贯彻于统计物理的精神当中的。在文章 https://zhuanlan.zhihu.com/p/73710585 中，对于选取KL散度完成度量离均匀分布的距离这一任务的合法性通过“自然”的要求而证实了。

### 第六章：自由能

引入自由能的动机主要是研究“部分”系统。人们通过抹掉不重要的自由度（例如外界热库的影响，内部的影响不大的自由度，某些时候甚至是小尺度上的自由度，即做粗粒化）来简化分析，或使之成为可能。

1.

考虑系统和它周围的热库。系统的相空间的密度$\rho(s) \propto \Omega_{2}\left(E-E_{s}\right)=\exp \left(S_{2}\left(E-E_{s}\right) / k_{B}\right)$而由于系统大，平衡附近能量的涨落比较小，可以认为$1 / T_{2}=\frac{\partial S_{2}}{\partial E_{2}}$在平衡附近的系统成立，是故

$\begin{aligned} \rho\left(s_{B}\right) / \rho\left(s_{A}\right) &=\Omega_{2}\left(E-E_{B}\right) / \Omega_{2}\left(E-E_{A}\right) \\ &=e^{\left(S_{2}\left(E-E_{B}\right)-S_{2}\left(E-E_{A}\right)\right) / k_{B}}=e^{\left(E_{A}-E_{B}\right)\left(\partial S_{2} / \partial E\right) / k_{B}} \\ &=e^{\left(E_{A}-E_{B}\right) / k_{B} T_{2}} \end{aligned}$

$\begin{aligned} \rho(s) &=\exp \left(-E_{s} / k_{B} T\right) / \int d \mathbb{P}_{1} d \mathbb{Q}_{1} \exp \left(-\mathcal{H}_{1}\left(\mathbb{P}_{1}, \mathbb{Q}_{1}\right) / k_{B} T\right) \\ &=\exp \left(-E_{s} / k_{B} T\right) / \sum_{n} \exp \left(-E_{n} / k_{B} T\right) \\ &=\exp \left(-E_{s} / k_{B} T\right) / Z \end{aligned}$

我们终于看到了配分函数。注意到，配分函数$Z(\beta)$是状态数$\Omega(E)$的拉普拉斯变换（矩母函数）。无怪乎配分函数可以方便地导出许多可观测量。一般的课上都会教导我们关系$\langle E\rangle=-\partial \log Z / \partial \beta$，以及$N c_{v}=\frac{1}{k_{B} T^{2}} \frac{\partial^{2} \log Z}{\partial \beta^{2}}$。注意这个二阶导正是能量的方差的平方。

2.

熵的公式是$S=\langle E\rangle / T+k_{B} \log Z$，这件事给了我们一个动机去定义$A(T, V, N)=-k_{B} T \log Z=\langle E\rangle- T S$。这个量叫做（亥姆霍兹）自由能，作者表示其中自由的意思是可以自由用来做功（卡诺热机）。

注意到，现在的配分函数是基于$A$的$Z=\exp \left(-A(T, V, N) / k_{B} T\right)$，它正对应了原相空间中由于自由度的限制而产生的降维对本来基于$\mathcal{H}$的配分函数的影响。

3.

相比从微正则到正则的概念上的进步（容我揣测一下，或许这就是canonical的来源？在各个自由能里面，$E-TS$确实最正统），在巨正则系综中没有太多新东西。

4.

考虑亥姆霍兹自由能的密度。我们写亥姆霍兹自由能$A(N, V, T)=N k_{B} T\left[\log \left(\rho \lambda^{3}\right)-1\right]$，而假设粒子数密度满足$n_j=\rho(x_j)\Delta V$。自由能密度$\mathcal{F}^{\text { ideal }}\left(\rho\left(\mathbf{x}_{j}\right), T\right)=\frac{A\left(n_{j}, \Delta V, T\right)}{\Delta V}=\rho\left(\mathbf{x}_{j}\right) k_{B} T\left[\log \left(\rho\left(\mathbf{x}_{j}\right) \lambda^{3}\right)-1\right]$，这个东西不仅可以用来分析平衡态下$\rho$的函数，还可以分析非平衡时系统感到的平衡之力。比如说，化学势有$\mu=\frac{\partial A}{\partial N}=\frac{\partial A / V}{\partial N / V}=\frac{\partial \mathcal{F}}{\partial \rho}$，算出来一维下有$\mu(x)=k_{B} T \log \left(\rho \lambda^{3}\right)$。它的梯度就是一个平衡之“力”，我们如果直接认为近平衡状态下满足线性响应，那么扩散的速度就正比于这个梯度。因此可以写出扩散的流$J=-\gamma k_{B} T \frac{\partial \rho}{\partial x}$，与第二章中的D对比，所得即爱因斯坦关系$D=\gamma k_BT$。

### 第七章：量子统计的几个例子

1.

在量子力学里面，纯态下计算一个可观测量的观测值时人们写$\langle\mathbf{A}\rangle_{\text { pure }}=\int \Psi_{n}^{*}(\mathbb{Q}) \mathbf{A} \Psi_{n}(\mathbb{Q}) d^{3 N} \mathbb{Q}$，而引入统计力学时，人们取一个混合态作为一个系综，写混合态下该量的观测值$\langle\mathbf{A}\rangle=\sum_{n} p_{n} \int \Psi_{n}^{*}(\mathbb{Q}) \mathbf{A} \Psi_{n}(\mathbb{Q}) d^{3 N} \mathbb{Q}$，从这个写法容易引入密度矩阵$\boldsymbol{\rho}=\left(\sum_{n} p_{n}\left|\Psi_{n}\right\rangle\left\langle\Psi_{n}\right|\right)$，使得$\left<\mathbf{A}\right>=\operatorname{Tr}(\mathbf{A} \boldsymbol{\rho})$。密度矩阵对应经典统计力学里头那个指数族。

对于正则系综，有$\rho_{\text { Canon }}=\frac{\exp (-\beta \mathcal{H})}{Z}=\frac{\exp (-\beta \mathcal{H})}{\operatorname{Tr} \exp (-\beta \mathcal{H})}$。选用能量本征态做基底，可以对角化密度矩阵，对角元是$\frac{\exp \left(-\beta E_{n}\right)}{Z}$。这非常方便。

2.

密度矩阵的含时演化对应量子版本的刘维尔定理。在

$\frac{\partial \rho}{\partial t}=\sum_{n} p_{n}\left(\frac{\partial\left|\Psi_{n}\right\rangle}{\partial t}\left\langle\Psi_{n}|+| \Psi_{n}\right\rangle \frac{\partial\left\langle\Psi_{n}\right|}{\partial t}\right)$

中代入薛定谔方程，即得式子右边$=\frac{1}{i \hbar}[\mathcal{H}, \boldsymbol{\rho}]$。

在经典刘维尔定理中，任何平衡态下都有$\partial \rho / \partial t=0$，保证相空间体积的不变性，从而（在一部分方面）为微正则系综的“自然”性提供保障。但在量子体系中，似乎不是这样：任意多体系统的（能量）本征态都满足量子刘维尔定理，而从原理上说，这些态是不满足各态历经的。作者对此的解释是：真正的多体系统本征态在实际情况下几乎不出现。但我并不能满足于此，我需要等到重新预习一遍量子之后再想一想这件事。现在还想不到什么与此相关的有用的东西。

3.

量子谐振子。其配分函数有

$Z_{\mathrm{qho}}=\sum_{n=0}^{\infty} e^{-\beta E_{n}}=\sum_{n=0}^{\infty} e^{-\beta \hbar \omega(n+1 / 2)}=\frac{1}{e^{\beta \hbar \omega / 2}-e^{-\beta \hbar \omega / 2}}=\frac{1}{2 \sinh (\beta \hbar \omega / 2)}$

容易计算，该系统的比热是

$k_{B}\left(\frac{\hbar \omega}{k_{B} T}\right)^{2} \frac{e^{-\hbar \omega / k_{B} T}}{\left(1-e^{-\hbar \omega / k_{B} T}\right)^{2}}$

当高温时，计算这个极限可知比热趋于能量均分定理计算出来的结果：一个$k_B$。当低温时，则近于0。

4.

三种统计：玻色、费米、玻尔兹曼。我们可以首先做近似：只考虑理想气体，即粒子之间没相互作用。事实证明，这个近似的威力/适用范围比乍一看要强大的多。对于玻色系统，光子气体、声子气体和BEC都适用这近似。对于费米系统，这个近似的威力为何这么大是由朗道-费米液体理论来解释的，但我还不了解该理论。

我们为了方便处理使用巨正则系综。

- 玻色系统第k能级的巨正则配分函数是
  
  $\Xi_{k}^{\mathrm{boson}}=\sum_{n_{k}=0}^{\infty} e^{-\beta\left(\epsilon_{k}-\mu\right) n_{k}}=\sum_{n_{k}=0}^{\infty}\left(e^{-\beta\left(\epsilon_{k}-\mu\right)}\right)^{n_{k}}=\frac{1}{1-e^{-\beta\left(\epsilon_{k}-\mu\right)}}$
  
  （注意到，取$\epsilon_k=\hbar\omega, \mu=0$，则它和一个谐振子的正则配分函数只差一个零点能$\hbar\omega/2$。这大概是我们把简正模看做是玻色子的原因之一吧？）
  
  相应的粒子数分布是
  
  $\left\langle n_{k}\right\rangle=-\frac{\partial \Phi_{k}^{\mathrm{boson}}}{\partial \mu}=-k_{B} T \frac{-\beta e^{-\beta\left(\epsilon_{k}-\mu\right)}}{1-e^{-\beta\left(\epsilon_{k}-\mu\right)}}=\frac{1}{e^{\beta\left(\epsilon_{k}-\mu\right)}-1}$

- 费米系统第k能级的巨正则配分函数是
  
  $\Xi_{k}^{\text { fermion }}=\sum_{n_{k}=0}^{1} e^{-\beta\left(\epsilon_{k}-\mu\right) n_{k}}=1+e^{-\beta\left(\epsilon_{k}-\mu\right)}$
  
  相应的粒子数分布是
  
  $\left\langle n_{k}\right\rangle=\frac{\sum_{n_{k}=0}^{1} n_{k} \exp \left(-\beta\left(\epsilon_{k}-\mu\right) n_{k}\right)}{\sum_{n_{k}=0}^{1} n_{k} \exp \left(-\beta\left(\epsilon_{k}-\mu\right)\right.}=\frac{e^{-\beta\left(\epsilon_{k}-\mu\right)}}{1+e^{-\beta\left(\epsilon_{k}-\mu\right)}}=\frac{1}{e^{\beta\left(\epsilon_{k}-\mu\right)}+1}$

- 经典（带有吉布斯的修正因子$1/N!$）的麦克斯韦-玻尔兹曼系统的单粒子配分函数是
  
  $Z_{1}=\sum_{k} e^{-\beta \epsilon_{k}}$
  
  其巨正则配分函数是通过巧妙地利用指数函数的泰勒展开而得到的
  
  $\begin{aligned} \boldsymbol{\Xi}^{\mathrm{NI}, \mathrm{MB}} &=\sum_{M} \frac{1}{M !}\left(Z_{M}^{\mathrm{NI}, \mathrm{MB}}\right)^{M} e^{M \beta \mu}=\sum_{M} \frac{1}{M !}\left(\sum_{k} e^{-\beta \epsilon_{k}}\right)^{M} e^{M \beta \mu} \\ &=\sum_{M} \frac{1}{M}\left(\sum_{k} e^{-\beta\left(\epsilon_{k}-\mu\right)}\right)^{M}=\sum_{M} e^{\sum_{k} e^{-\beta\left(\epsilon_{k}-\mu\right)}} \\ &=\prod_{k} e^{-\beta\left(\epsilon_{k}-\mu\right)} \end{aligned}$
  
  相应的粒子数分布是
  
  $\langle n\rangle_{\mathrm{MB}}=-\frac{\partial \Phi}{\partial \mu}=e^{-\beta(\epsilon-\mu)}$

5.

记录几个例子。

黑体辐射的情况下，光子的波函数可以写成$\psi_{\mathbf{k}}=(1 / L)^{3 / 2} \exp (i \mathbf{k} \cdot \mathbf{r})$，波矢空间的密度是$V/8\pi^3$。考虑到光子气体化学势为0，且服从玻色分布，可以直接写出能量空间中一个球壳（可以认为能级近于连续）的态数（为了方便得到普朗克的公式，直接用频率空间而不是能量空间）。$g(\omega) d \omega=\left(4 \pi k^{2}\right)\left(\frac{d|\mathbf{k}|}{d \omega} d \omega\right)\left(\frac{2 V}{(2 \pi)^{3}}\right)$，其中考虑了光子自旋的两个简并度。令$u(\omega)$是单位体积的能量，那么就有

$\begin{aligned} V u(\omega) d \omega &=\frac{\hbar \omega g(\omega)}{e^{\hbar \omega / k_{B} T}-1} d \omega \\ &=\frac{V \hbar}{\pi^{2} c^{3}} \frac{\omega^{3} d \omega}{e^{\hbar \omega / k_{B} T}-1} \end{aligned}$

这就是普朗克的黑体辐射公式。如果令T趋于高温，则能量均分定理成立，以上公式变为瑞利-金斯公式。

在BEC的情况下，粒子是非相对论性的，因此有$\epsilon=p^{2} / 2 m=-\hbar^{2} \nabla^{2} / 2 m$。在能量空间中，即有

$\begin{aligned} g(\epsilon) d \epsilon &=\left(4 \pi p^{2}\right)\left(\frac{d|\mathbf{p}|}{d \epsilon} d \epsilon\right)\left(\frac{V}{(2 \pi \hbar)^{3}}\right) \\ &=(4 \pi(2 m \epsilon))\left(\sqrt{\frac{m}{2 \epsilon}} d \epsilon\right)\left(\frac{V}{(2 \pi \hbar)^{3}}\right) \\ &=\frac{V m^{3 / 2}}{\sqrt{2} \pi^{2} \hbar^{3}} \sqrt{\epsilon} d \epsilon \end{aligned}$

$N(\mu)=\int_{0}^{\infty} \frac{g(\epsilon)}{e^{(\epsilon-\mu) / k_{B} T}-1} d \epsilon$

当化学势取0，意味着系统里“塞满”了玻色子。如果继续往里塞，化学势接近了基态的能级了，连续性假设就被打破，不能再用积分计算可观测量了。这时候就形成了BEC。达成化学势等于0的临界状态时，

$\begin{aligned} N_{\max }^{\mathrm{cont}} &=\int \frac{g(\epsilon)}{e^{\epsilon / k_{B} T}-1} d \epsilon \\ &=\frac{V m^{3 / 2}}{\sqrt{2} \pi^{2} \hbar^{3}} \int_{0}^{\infty} d \epsilon \frac{\sqrt{\epsilon}}{e^{\epsilon / k_{B} T}-1} \\ &=V\left(\frac{\sqrt{2 \pi m k_{B} T}}{h}\right)^{3} \frac{2}{\sqrt{\pi}} \int_{0}^{\infty} \frac{\sqrt{z}}{e^{z}-1} d z \\ &=\left(\frac{V}{\lambda^{3}}\right) \zeta(3 / 2) \end{aligned}$

在实际操作中，这对应一个临界温度的存在，因为实际上只有温度是可以（最好）操作的。

同时，也可以看到，BEC的极限对应了一个德布罗意热波长和系统的特征长度之比的无量纲量。

最后是费米气体（电子气）的例子。考虑电子自旋的两个简并度，$g(\epsilon)=\frac{\sqrt{2} V m^{3 / 2}}{\pi^{2} \hbar^{3}} \sqrt{\epsilon}$。$N(\mu)=\int_{0}^{\infty} g(\epsilon) f(\epsilon) d \epsilon=\int_{0}^{\infty} \frac{g(\epsilon)}{e^{(\epsilon-\mu) / k_{B} T}+1} d \epsilon$。在绝对零度附近，就只有低于化学势（记费米能量$\epsilon_F=\mu(0)$）的能级被填充了，所以直接数这些能级：

$N=\int_{0}^{\epsilon_{F}} g(\epsilon) d \epsilon=\frac{\sqrt{2} m^{3 / 2}}{\pi^{2} \hbar^{3}} V \int_{0}^{\epsilon_{F}} \sqrt{\epsilon} d \epsilon=\frac{\left(2 \epsilon_{F} m\right)^{3 / 2}}{3 \pi^{2} \hbar^{3}} V$

物质波的波矢在这个能量范围内被限制在费米面以内。

7.

来自一个习题的延伸：https://www.zhihu.com/question/332101879

### 第八章：计与算（数值方法与数值计算）

1.

在大多数情况下，人们不能直接在纸上就把事办了，而是需要数值方法。一般用的有两种：微扰和仿真。微扰法需要被微扰的那个量在附近解析，而不解析的情况我们一般称之为相变。微扰法的例子包括费米气比热的高温（对热波长展开）和低温（索末菲展开，对温度）展开，以及对于玻色气比热的高温展开。（在BEC的临界点，发生相变，比热的一阶导不连续。）

2.

统计物理里面，格点模型是一个大武器库。这些格点模型一般就是做一个网格（算是某种对系统的粗粒化），在格点上规定几个自由度，写出一个哈密顿量，然后算来算去，看看系统的观测量的演化。第一个这种网格模型是伊辛模型。这些网格对于物理之外的领域也格外有吸引力，因为可以做出一些似是而非的解释（例如，谢林的segregation model）。

伊辛模型具有一个$\mathcal{H}=-\sum_{\langle i j\rangle} J s_{i} s_{j}-H \sum_{i} s_{i}$形式的哈密顿量。人们在课上学过怎样解一维的伊辛模型（实际上，我还记得被昂萨格的二维伊辛模型严格解支配的恐惧，直到现在我也完全搞不明白他是怎样想的，并且已经几乎完全忘记了他是怎样做的），但是这里需要额外指出一种让伊辛模型的普适性看上去更好的视角：认为它代表着两个态的混合。例如，朝上的自旋代表相A，朝下的代表相B，同相和不同相之间的连接具有特定的能量。可以证明这个情形下的哈密顿量即是以上伊辛模型哈密顿量减去总粒子数的常数倍。在学习普适性的时候，临界指数是最常见的入手处，而我作为初学者，对于临界指数的了解几乎全来自于不同种类的格点模型。

![](https://i.loli.net/2019/07/27/5d3c358600ce343913.jpg)

一般解这种模型用 heat bath 蒙特卡洛法，我理解 heat bath 是正则系综的意思，每一步按照配分函数对应的概率来**重新确定**各个格点的朝向。

当然，heat bath 并不是最快的方法。凡是满足下一节所说的要求的算法都可以用。Metropolis 法就是机器学习里面的模拟退火，每一步按照配分函数对应的概率来**翻动**各个格点的朝向。还有更快的 Wolff 法，即先找一个格点，记住它的朝向D，然后翻转之；检查它的邻居，如果朝D，则以概率$p=1-e^{-2\beta J}$翻转之；对邻居之邻居继续检查。

3.

马尔可夫过程是一类在自然界特别普遍（或是说，由于我们的能力限制，被迫认为近似于它的过程特别普遍）的随机过程。假如有一系列状态$\left\{\alpha\right\}$，某一步时各状态的概率为$\vec{\rho}_{\alpha}(n)$，则可以知道$\vec{\rho}(n+1)=P \cdot \vec{\rho}(n)$。一般情况下，矩阵$P$不是对称的，这给我们的分析造成了一定的困难。

在这里我又懒了。我不加证明地写出以下定理：有限状态数的系统在随机过程下必定收敛于平衡态，如果

- 该过程是马尔可夫过程
- 该过程是各态历经（ergodic，不包括周期性遍历的情况）的
- 该过程满足细致平衡$P_{\alpha \beta} \rho_{\beta}^{*}=P_{\beta \alpha} \rho_{\alpha}^{*}$。

这是数值模拟一个系统时必须考虑的一些条件。

关于细致平衡。稍加考虑，可以知道细致平衡也可以写作$\mathcal{P}_{\alpha \beta} \mathcal{P}_{\beta \gamma} \mathcal{P}_{\gamma \alpha}=\mathcal{P}_{\gamma \beta} \mathcal{P}_{\beta \alpha} \mathcal{P}_{\alpha \gamma}$，前提是这些转移概率非零。

4.

可以从习题中了解一些例子，增进理解。

考虑一个系统，里面有500个红细菌，500个绿细菌，每个细菌每小时分裂一次，同时每小时会有一个色盲捕食者吃掉1000个细菌。考虑到一旦某种细菌消失，它就会永远消失，我们可以认为平衡态的$\rho$（一个1001维的向量）为$(0.5, 0， \cdots, 0, 0.5)$。问题来了：预计多长时间会达到平衡？

有一个有趣的估计值。假设每次只长出一个细菌，吃掉一个细菌。假如有N和细菌，r个绿细菌，可以计算得出转移矩阵的元

$\begin{aligned} \mathcal{P}_{r+1 \leftarrow r} &=\frac{r}{N} \frac{N-r}{N+1} \equiv p_{r} \\ \mathcal{P}_{r \leftarrow r} &=\frac{r}{N} \frac{r+1}{N+1}+\frac{N-r}{N} \frac{N+1-r}{N+1} \\ \mathcal{P}_{r-1 \leftarrow r} &=\frac{N-r}{N} \frac{r}{N+1}=\mathcal{P}_{r+1 \leftarrow r} \end{aligned}$

这个阵除了头尾两行（各只有一个对角元）之外每行元素的和都是一样的，即为$\lambda=1-2 / N(N+1)$。因此，可以写出一个本征向量是$\rho_{\lambda}=(a, 1,1, \cdots, 1,1, a)$。可以合理猜测（由于本征向量非常“平缓”）这对应的本征值$\lambda$是小于一的最大本征值，即下降的最慢的一个模式，它带来的系统的变化就是

$\rho(t) \propto(\lambda)^{1000 t} \rho_{\lambda} \approx e^{-\frac{2000 t}{N(N+1)} \rho_{\lambda}}$

对称性破缺后占劣势细菌的数目的半衰期约为347小时。

### 第九章：序参量，对称破缺及其拓扑
