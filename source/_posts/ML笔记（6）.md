---
title: ML笔记（6）
date: 2019-11-14 19:23:53
categories: [笔记, 理学]
tags: [机器学习]
---

### week 11,12

>  小平学得很辛苦， 不懂的证明反复再三地看， 还抄在笔记上背下来。这么一来好像懂了。  

——《游里工夫独造微，小平邦彦传》

这句话其实适合上一章的内容：对于我贫乏的脑子来说很抽象的东西，抄几遍好像就懂了。虽然说这一章比较简单，但是抄书还是有帮助的。况且，从前学的时候，概率相关的很多东西当成数学课来学，对于我现在的脑子来说，motivation也不知道，对知识结构的划分和重要性的判断都很差，近乎于没有。讲义是物理学家的角度写下来的，我个人觉得写得不错，抄一遍可以厘清这些东西。

另外，kunihiko貌似是氼畚儿国一个很常见的男性用名。Kunihiko Kaneko 和 Kunihiko Kodaira，光我听说过的学者就有两个。

<!--more-->

### Monte Carlo Sampling

#### 动机

从概率分布里面抽取iid（独立同分布）的sample，主要是有以下几个应用场合：

- 积分的数值结果
- 算Bayesian posterior
- 数值优化
- 计算物理的一些别的场景。

特别的，考虑到电脑里头伪随机数的生成特点，很重要的是怎样把任意的分布的sampling转化为均匀分布的sampling。

#### 概率复习

从相空间体积元不变的想法，可以写出

$p_{Y}\left(y_{1}, \ldots, y_{n}\right)d\mathbf{y}=p_{X}\left(x_{1}(\mathbf{y}), \ldots, x_{n}(\mathbf{y})\right)d\mathbf{x}$

因此

$p_{Y}\left(y_{1}, \ldots, y_{n}\right)=p_{X}\left(x_{1}(\mathbf{y}), \ldots, x_{n}(\mathbf{y})\right)\left|J\left(y_{1}, \ldots, y_{n} ; x_{1}, \ldots, x_{n}\right)\right|^{-1}$

其中

$J\left(y_{1}, \ldots, y_{n} ; x_{1}, \ldots, x_{n}\right)=\left|\begin{array}{cccc}{\frac{\partial y_{1}}{\partial x_{1}}} & {\frac{\partial y_{1}}{\partial x_{2}}} & {\cdots} & {\frac{\partial y_{1}}{\partial x_{n}}} \\ {\frac{\partial y_{2}}{\partial x_{1}}} & {\frac{\partial y_{2}}{\partial x_{2}}} & {\cdots} & {\frac{\partial y_{2}}{\partial x_{n}}} \\ {\vdots} & {\vdots} & {\vdots} & {\vdots} \\ {\frac{\partial y_{n}}{\partial x_{1}}} & {\frac{\partial y_{n}}{\partial x_{2}}} & {\cdots} & {\frac{\partial y_{n}}{\partial x_{n}}}\end{array}\right|$

- 举例：X和Y是独立分布的。取$Z=X+Y$，想知道Z的分布。取W=Y，$|J(X, Y ; Z, W)|=1$，再考虑独立性，$p_{Z, W}(z, w)=p_{X}(z-w) p_{Y}(w)$。因此，独立分布随机变量之和，其分布是卷积：
  
  $p_{Z}(z)=\int p_{Z, W}(z, w) d w=\int p_{X}(z-w) p_{Y}(w) d w=\left(p_{X} * p_{Y}\right)(z)$

- 又举一例：$F_{X}(x)$是cumulative distribution，其密度为p。定义
  
  $Y=F_{X}(X)=\int_{-\infty}^{X} p(t) d t$
  
  那么
  
  $p(y)=p(x(y)) J(x ; y)=p(x(y))\left(\frac{d y}{d x}\right)^{-1}=p(x(y)) p(x(y))^{-1}=1$
  
  所以说分布曲线下的面积是均匀的，这提供了p值的合法性。
  
  原则上讲，可以直接在[0,1]去sample Y，然后$x=F_{X}^{-1}(y)$得到x。可惜，大多数实际情况下，后一步不那么容易，所以一般不这么做。

- 比如最正常的gaussian，$p(x)=\frac{1}{\sqrt{2 \pi}} e^{-\frac{1}{2} x^{2}}$。
  
  $\Phi(x)=\int_{-\infty}^{x} \frac{1}{\sqrt{2 \pi}} e^{-\frac{1}{2} x^{2}} d x=\frac{1}{2}+\frac{1}{2} \operatorname{erf}\left(\frac{x}{\sqrt{2}}\right)$
  
  这玩意的反函数就很不好办。实际上，可以采用一个技巧，正如计算高斯积分的时候那样，引入第二维
  
  $p(x, y)=\frac{1}{2 \pi} e^{-\frac{1}{2}\left(x^{2}+y^{2}\right)}$
  
  参数化成
  
  $p(R, \theta)=\frac{e^{-R}}{2 \pi}$
  
  $\theta$直接就是均匀分布。这样，sample了两个参数之后，直接就把x,y也算出来。

#### Rejection Sampling

核心思想是把较难sample的$p_{X}(x)$通过容易些的$q(x)$sample出来。有的时候，甚至很难归一化成p，需要以未归一化的形式g来处理：$p=g(x) / c$。这个时候，取M，令在全空间$g(x) \leq M q(x)$。现在，先从q里面sample一个值x，再取$r=\frac{g(x)}{M q(x)}$为接受这个x的概率。取够足够被接受的x后，停止sample。

原理上是清楚的：这是利用了q分布下面积的均匀性。从理论上说，可以这样证明：

$p(x | \text { accept })=\frac{p(x, \text { accept })}{P(\text { accept })}=\frac{p(\operatorname{accept} | x) q(x)}{P(\text { accept })}=\frac{\frac{g(x)}{M q(x)} q(x)}{\int_{-\infty}^{\infty} \frac{g(x)}{M q(x)} q(x) d x}=\frac{g(x)}{c}=p_{X}(x)$

为了能够把$P(accept)$搞大一些，我们最好把Mq弄的几乎是紧贴着g。

- 这个还可以用来sample贝叶斯后验分布：

$P(\theta | \mathbf{x})=\frac{P(\mathbf{x} | \theta) P(\theta)}{P(\mathbf{x})}=\frac{likelihood\cdot prior}{partition}$

取$L=\max _{\theta} P(\mathbf{x} | \theta)$，那么

$M=L / P(\mathbf{x})$。可以转化成从先验分布中sample。

- 这个还可用来sample nonhomogeneous的泊松分布：

首先，回忆（预习）泊松分布。$\Omega \in \mathbb{R}^{d}$上有测度$\mu$，$\mu(A)$给出A的面积。点过程满足

$P(N(A)=n)=\frac{\mu(A)^{n} e^{-\mu(A)}}{n !}$

且对于不重合的$A_{1}, \ldots, A_{k} \subset \Omega$，$N\left(A_{1}\right), \ldots, N\left(A_{k}\right)$是独立的。有表示A的面积的不均匀（nonhomogeneous）性质的rate function $\lambda$使得

$\mu(A)=\int_{A} \lambda(x) d x$

可以证明，固定$N(A)$的情况下，这n个点是从

$p(x)=\left\{\begin{array}{ll}{\frac{\lambda(x)}{\mu(A)},} & {x \in A} \\ {0,} & {x \notin A}\end{array}\right.$

sample的iid。

取$\Omega=\mathbb{R}_{+}$，即一维泊松过程，有

$P\left(T_{n+1}-T_{n}>t\right)=P\left(N\left(\left[T_{n}, T_{n}+t\right]\right)=0\right)=\exp \left(-\int_{T_{n}}^{T_{n}+t} \lambda(\tau) d \tau\right)$

$p_{T_{n+1}-T_{n}}(t)=\left\{\begin{array}{ll}{\lambda\left(t+T_{n}\right) \exp \left(-\int_{T_{n}}^{T_{n}+t} \lambda(\tau) d \tau\right),} & {t \geq 0} \\ {0} & {t<0}\end{array}\right.$

我们可以sample一个均匀的二维泊松分布（取$\lambda(x)$和x轴围的面积），然后以此来sample这个一维的不均匀泊松分布。

泊松分布有种“thinning”操作，是说取$\lambda^{\prime}(t) \geq \lambda(t)$的泊松过程N'，然后以概率$1-\lambda(x) / \lambda^{\prime}(x)$拒掉N'的sample结果，得到N的sample结果。实际操作中我们可以取$\lambda'$是$max(\lambda)$。

#### Importance Sampling

举个例子。我们若想sample一些点来确定积分

$f(z)=\int_{z}^{\infty} \frac{1}{\sqrt{2 \pi}} e^{-x^{2} / 2} d x$

的取值，我们可以写出

$f(z)=\int_{-\infty}^{\infty} I(x \geq z) p(x) d x$

相当于从p里面sample，然后用I来判断是否接受sample结果。这样，得到

$\hat{f}(z) \equiv \frac{1}{N} \sum_{i=1}^{N} I\left(x_{i} \geq z\right) \approx f(z)$

显然这是一个无偏估计，但是它的坏处在于，当z离0太远的时候，方差会过大

$\frac{\operatorname{sd}[\hat{f}(z)]}{f(z)}=\sqrt{\frac{1}{N}} \sqrt{\frac{1-f(z)}{f(z)}}$

并不经济。对此，我们可以改写

$f(z)=\int_{z}^{\infty} I(x \geq z) \frac{p(x)}{q(x)} q(x) d x$

然后从q里面sample。比如，对于我们的例子，可以有

$q(x)=\lambda e^{-\lambda(x-z)}$

这样，估计量$\hat{f}$的方差

$\operatorname{var}\left[\hat{f}(z)_{I m p}\right]=\frac{1}{N}\left(E_{p(x)}\left[I(x>z) \frac{p(x)}{q(x)}\right]-f(z)^{2}\right)$

不至于太大。

#### 马氏链

马尔可夫性就是健忘性：$P\left(x_{n} | x_{n-1}, \ldots, x_{0}\right)=P\left(x_{n} | x_{n-1}\right)$

这使得我们可以写出

$P\left(x_{n}, \ldots, x_{0}\right)=\pi\left(x_{0}\right)\left(\prod_{k=1}^{n} T_{x_{k-1} x_{k}}\right)$

并基于此式来分析马尔可夫过程。

- 一些概念：一个stationary state是说$\pi_{s} T=\pi_{s}$，但不要求全局收敛到它。一个steady state是说$\lim _{n \rightarrow \infty} T^{n}=\mathbb{I} \pi_{s}$，全局收敛。细致平衡是说$\pi_{x} T_{x y}=\pi_{y} T_{y x}$。细致平衡显然stationary，但是不一定steady。irreduciblility是说，任意两个态之间都存在可能的路径，不至于隔断。（对某个态x来说的）周期$d_x$是指一切回到以它为初始态的路径的长度的最大公约数。
- 定理：有限的马氏链都有stationary state。这是因为$T\mathbb{I}=\mathbb{I}$，所以它有特征值为1的左本征向量，而我们可以构造得使它所有分量全为正。
- 定理：steady state若存在，那么它是唯一stationary state。这是由于任何态必然演化到该steady state。而任何stationary state只会演化到它自身。
- 定理：有限的，irreducible的马氏链存在唯一stationary state。
- 定理：存在$\left(T^{n}\right)_{x y}>0$，$\left(T^{m}\right)_{y x}>0$，那么$d_{x}=d_{y}$。证明：$d_{x} | n+m$是显然的，故对于所有起终点在y的loop，其长度k都有$d_{x} | n+k+m$或直接$d_{x} | k$。$d_y$是k的最大公约数，所以$d_{x} | d_{y}$。由对称性，$d_x=d_y$。

#### MCMC

我们希望创建一个马氏链，它存在一个steady state，正是我们希望从中sample的$\pi_s$。

- positive recurrence：一个态如果有有限的mean recurrence time，就说它是postitive recurrent的。

- 各态历经：irreducible，positive recurrent，非周期的马氏链有steady state（不要求有限长）。

- positive recurrence的检查非常难。然而，有定理：irreducible的马氏链，positive recurrence和有stationary state等价。这个stationary state是唯一的。这样的话，我们就可以从细致平衡入手，把过程弄到stationary state，然后自然而然的就是steady state。这就引出了以下的算法：

- Metropolis-Hastings 算法。
  
  - 建立初始的转移矩阵$Q_{x y}$。
  - 利用细致平衡，$T_{x y}=Q_{x y} \min \left(1, \frac{\left(\pi_{s}\right)_{y} Q_{y x}}{\left(\pi_{s}\right)_{x} Q_{x y}}\right)$。
  
  注意到，$\pi_s$只露了个比例，所以我们不需要把它归一化。这是大好事，例如我们若需要sample一个bayesian posterior，这能省掉很多事。
  
  - poor mixing指的是例如$\pi_s$是双峰分布的时候，马氏链堵在那个谷底过不去的现象。解决办法包括对Q做文章调节步长，或者burn-in，指的是扔掉前面几步，直接从第n步开始sample。

- Gibbs sampling 算法。
  
  假设我们遇到一种情形，即我们的多维统计量的分布$P\left(X_{1}, \ldots, X_{d}\right)$的sample较为困难，但对于每一个分量，其条件分布$P\left(X_{i} | X_{1}, \dots, X_{\hat{i}}, \dots, X_{d}\right)$是易于sample的，这时便可以使用gibbs sampling。这是一个十分简单的过程，我们首先从初始的$x^{0}=\left(x_{1}^{(0)}, \ldots, x_{d}^{(0)}\right)$开始，然后利用条件概率sample出$x_1^{(1)}$，并且迭代所有的上指标，然后迭代所有的下指标。可以证明，这是metropolis-hastings的一个特例，具体来说，是后者在$\min$函数始终取1的时候得到的结果。
  
  这种办法在什么地方有用处呢？一个简单的例子是Ising model。考虑
  
  $P(s)=\frac{e^{\beta\left(H \sum_{i} s_{i}+J \sum_{(i, j) \in \mathcal{N}} s_{i} s_{j}\right)}}{Z}, s_{i} \in\{-1,1\}$
  
  一次sample完所有的自旋是不现实的。作为替代，我们从一个随机的构型开始，作
  
  $\begin{aligned} P\left(s_{i} | s_{j \neq i}\right)=& \frac{\exp \left[\beta\left(H s_{i}+J s_{i} \sum_{j:(i, j) \in \mathcal{N}} s_{j}\right)\right]}{\exp \left[\beta\left(H+J \sum_{j ;(i, j) \in \mathcal{N}} s_{j}\right)\right]+\exp \left[\beta\left(-H-J \sum_{j ;(i, j) \in \mathcal{N}} s_{j}\right)\right]} \\=& \frac{\exp \left[\beta\left(s_{i}+1\right)\left(H+J \sum_{j ;(i, j) \in \mathcal{N}} s_{j}\right)\right]}{1+\exp \left[2 \beta\left(H+J \sum_{j ;(i, j) \in \mathcal{N}} s_{j}\right)\right]} \end{aligned}$
  
  即可。不过，这毕竟是一个非常naive的算法。我们知道，二维以上的Ising模型是有criticality的，这个时候由于长程序显露出来，gibbs sampling收敛到实际分布的速度将会极其的慢。对于这种情形，人们发明了Swendsen-Wang算法来改进。

- 收敛性
  
  检查算法的收敛性是必要的。有两个相关而不同的概念：
  
  - 马氏链是否收敛到steady state。(实际上，就是看需要取多少burn-in，取足够的burn-in才能在steady state上sample)
  - 各态历经是否达成。亦即，$\int f(x) \pi_{s}(x) d x \approx \frac{1}{N-n_{0}} \sum_{i=n_{0}+1}^{N} f\left(x_{i}\right)$。
  
  对于马氏链收敛的一个比较简单的判断办法是取边缘分布的积分$Z_{n}=I\left(Y_{n} \leq y\right)$，y是某个先验的值。（非常大胆地）假设Z在0和1 之间变来变去的过程是马氏过程（实际上并不是，会有correlation，叫做high order markov process），看一看这个过程是不是收敛到steady state $\pi_s$，也就是$\left|P\left(Z_{n_{0}}=i | Z_{0}=j\right)-\left(\pi_{s}\right)_{i}\right|<\epsilon$。
  
  $\frac{|\alpha|^{n_{0}}}{p+q} \max (p, q)<\epsilon \Rightarrow \quad n_{0}=\left[\frac{\log \left(\frac{\epsilon(p+q)}{\max (p, q)}\right)}{\log |\alpha|}\right]$
  
  从pq的值和我们预设的精度可以确定burn-in的步数。
  
  接下来我们看大概要多少步才能让各态历经成立。各态历经意味着$S_{N}=\frac{1}{N-n_{0}} \sum_{n=n_{0}+1}^{N} Z_{n}$收敛到$p /(p+q)$。考虑到步数较大时，
  
  $\operatorname{Var}\left[S_{N}\right]=\frac{1}{N-n_{0}} \frac{p q(2-p-q)}{(p+q)^{3}}$
  
  由CLT，
  
  $P\left(\left|S_{N}-\frac{p}{p+q}\right|<r\right) \geq s$
  
  意味着
  
  $\frac{r}{\sqrt{\operatorname{Var}\left[S_{N}\right]}} \geq \Phi^{-1}\left(\frac{1+s}{2}\right) \Rightarrow N-n_{0} \geq\left[\Phi^{-1}\left(\frac{1+s}{2}\right)\right]^{2} \frac{p q(2-p-q)}{r^{2}(p+q)^{3}}$
  
  这个方法很让人不舒服，因为实际上p和q的取得是通过sample很多步之后用MLE得到的，至少对burn-in步数的选取完全是先验的，这样就造成了一种循环论证的效果。不过，其他一些更加高明的算法，似乎讲起来也要复杂许多。
