---
title: ML笔记（2）
date: 2019-09-24 13:41:43
categories: [笔记, 理学]
tags: [机器学习]
---

### week 3,4

<!--more-->

#### lecture

- 9.10
  
  - 今天首先提了一些零碎的点，可以略去。
  - 讲了算子的连续性和有界性，以及矩阵的范数。上一周已经看过讲义了，不再重复。
  - 一点应用。实际上就是奇异值分解的应用动机。假设$X$是random vector，将其标准化，取mean=0，std=1。令$\Sigma=Cov(X)=(Cov(x_i, x_j))$，**为了理解数据，我们经常要降维，并且我们希望低维下数据尽可能稀疏**。取模长为1的向量w作为降维“平面”的第一个轴的方向，要求$var(w^tX)=w^t\Sigma w$取最大值，而由于奇异值分解可以给出$\Sigma=\sum\lambda_iv_iv_i^t$，$\lambda_i=\sigma_i^2$，我们知道$w^t\Sigma w=\sum\lambda_i(v_iw)^2$，其中$\sum(v_i^tw)^2=const.$，这样，低维“平面”首先确立的一个轴肯定就是最大的奇异值对应的v的方向，第二个轴就是次大的奇异值对应的方向（因为要求垂直于上一个轴），诸如此类。
  - 补充（9.17）从作业题目中意识到，在均值为零的情形下，由勾股定理，低维下数据最稀疏（或说方差最大）意味着向低维投影的过程中损失的最少（或说各数据点离低维平面的距离平方和最小）。

- 9.12
  
  - PCA的主要精神是保 similarity （最大化variance）。作为对照，今天引出的另外一个降维手段 multidim scaling 的主要精神是保距离（的序）。这些东西的目的都是**把数据可视化的牺牲降到最低**。我们需要数据可视化，因为我们现在对高维数据的性质的理解还不完全，特别是对物理学家而言，他们的思维需要一个实在的处理对象。
  
  - Rayleigh Quotient。定义为$R(v)=\frac{x^tAx}{x^tBx}$，一个主要的问题是maximize它，例如$A=\Sigma, B=I$时，最大化RQ的过程也就是做PCA的过程。对于scaling系数$\alpha\neq0$，RQ具有 scaling 不变性，因此我们令分母为1，定义一个Lagrangian $\mathcal{L}=x^tAx-\lambda(x^tBx-1)$。求导，得到$Ax=\lambda Bx$，这个叫 general eigenvalue equation。解法是先得出$B^{-1}Ax=\lambda x$，然后写成$(B^{-1/2}AB^{-1/2})(B^{1/2}x)=\lambda(B^{1/2}x)$，然后转化成人们熟练的sym mat的本征值问题（因为sym mat的本征向量才确保正交）。解出来之后，考虑到$R(v_i)=\lambda_i$，我们找到$\lambda_1$对应的$v_1$即可。
  
  - Kernalized PCA。假使我们有数据集$X=(\vec{x_1}, \cdots\vec{x_m})^t$，我们的数据有n个feature，m个sample。现在，常常出现的一个情况是$m\ll n$。比如说，人有2e4个基因，但是一次实验一般只能测到1e2个人。对一个上万甚至是上百万维的矩阵做SVD是有困难的，我们可以使用kernel trick绕开这一困难。
    
    假使我们的PC为$v_i=\frac{X^tw_i}{\sqrt{\lambda_i}}$，那么数据点x的第i个分量将会是$\sum_j\frac{x^t\vec{x_j}(w_i)_j}{\sqrt{\lambda_i}}$。我们选取kernel $K:\mathbb{R}^n\times\mathbb{R}^n\to\mathbb{R}$。我们将内积$x^ty$换成hilbert space中的一个内积$K(x,y)=\langle\phi(x),\phi(y)\rangle$。我们将本征值和本征向量$\sqrt{\lambda_i},w_i$换成矩阵$(K(x_i,x_j))$的相关参数。这样，**我们就不需要处理一个$m\times n$矩阵的SVD，只需要处理一个$m\times m$矩阵的本征值问题。**
    
    9/17补充：这个操作在动机上合法，是因为 Gram Matrix $XX^t$本来就具有一个内积的形式，而我们做的所有替换，不过是把定义在 feature space 上的内积换成了嵌入 hilbert space 之后的内积（kernel）而已。但是具体为什么合法，细节上的缘由，我并不十分清楚。
  
  - Multidimensional Scaling。

- 9.17 今天的内容基本上都放在讲义部分了。
  
  - 等距同构嵌入。除了讲义上内容以外，补充了一条引理，表明一组m个p维欧氏空间中的数据点（距离按2-norm定义）可以被等距同构地嵌入到m-1维欧氏空间。显然，这个进步太小了，人们需要别的办法。
  - cMDS（approximation）。
  - metric MDS：开了个头。

- 9.19 
  
  - Metric MDS的一个具体的例子。一般做mMDS的时候，令变换$T(\delta_{ij})=a+b\delta_{ij}(i\neq j)$。这个的作用是考虑到由于
    
    $\min_a\min_X||B(a)-XX^t||^2=\min_a(\sum_{i=1}^n\bar{\lambda_i}^2(a)+\sum_{i=n+1}^m)\lambda_i^2$
    
    如果我们把B弄成正定的，那么所有的$\bar{\lambda_i}^2$都是0，所以我们把a调到$a\gg b$，这样可以保证B正定。
  
  - Spectral Clustering。
    
    - Spectral Embedding。主要内容记在讲义部分了。最后落到最小化作用量$E=2\tau^tL\tau$上，$\tau$就是从feature space到欧氏空间的映射，它的分量由L的谱决定（这大概就是spectral的来历），所以说如果L的谱在某处有一个跃变（实际上很常见），我们就可以把显著更小的那一部分本征值摘出来，其数目（减一，考虑到trivial的本征值0应该除去）作为被嵌入的欧氏空间的维度是很自然的。
      
      L也不是满秩的，事实上，对于无向图，其kernel的维数正等于图里面互不相连的部分（connected component）的数目（某一这种部分的点取1，其他点取0，即为一特征向量）。
    
    - k-nearest neighbor。一般来讲，想要k个cluster的话，就将图嵌入k维欧氏空间。先选定kernel，建立feature space对应的“图”，然后降序排列每个节点的权重$\kappa_{ij}$，做cutoff，给每个点只留k个邻居。
    
    - k-means clustering。有一个recipe，但是没有展开讲。留待下周细说。
  
  - 提了一个 grassmanian manifold的概念，我不是很懂，希望下周能讲清楚些。大致是说，有同样的节点连了许多不同图，产生许多graph laplacian。我们又不能同时把他们对角化，但是每个L的本征向量构成的subspace可以被认为是一个grassmanian manifold上的点，而同时对许多图聚类可以通过在grassmanian manifold上寻找一个特定的点来完成。
  
  - 不同类型的graph laplacian。一般的L的问题在于，它是unnormalized。$L_{\mathrm{rw}}=D^{-1} L=I-D^{-1} K$是random walk的L，因为$D^{-1}K$可以认为是一个随机行走的transition probability的矩阵。但这个$L_{rw}$不对称。$L_{\mathrm{sym}}=D^{1 / 2} L_{\mathrm{rw}} D^{-1 / 2}=D^{-1 / 2} L D^{-1 / 2}$就是symmetric normalized graph laplacian。

#### reading material

- 讲义的第二章是关于降维方法的。
  
  - PCA：关于PCA的部分基本上是在课上听的，所以记到了 lecture 部分。
  
  - MDS。这方面在taxonomy上感觉极其混乱。
    
    - Classical MDS（1）：等距同构嵌入（isometric embedding）。首先，定义距离。距离就是不需要满足三角不等式的一个度规。选取一个距离$\delta:X\times X\hookrightarrow\mathbb{R}$。寻找一个embedding$X\hookrightarrow(\mathbb{R}^n,||\cdot||_2)$，使得$\delta(x_i, x_j)=||f(x_i)-f(x_j)||_2$。当然这个时候这个distance已经是个metric了，这也是为什么这种embedding被叫做iso*metric*。
      
      定义mean-centering operator $J=I_{m\times m}-\mathbb{I\cdot I}^t/m$，其中$\mathbb{I}=(1, \cdots, 1)^t$。现在我们定义${H}=(\delta^2(x_i, x_j))$，$B=-\frac{1}{2}JHJ$。我们可以算出来$B_{ii}+B_{jj}-2B_{ij}=H_{ij}$，且我们有定理：$(x,\delta)$可以被isometrically embedded into $(\mathbb{R}^n,||\cdot||_2)$，当且仅当B半正定，且$rank(B)\leq n$。
      
      这个过程看上去像巫术。它的动机是这样的：**1）H记录了原feature空间中各点距离的结构，2）用J把这个结构平移到原点周围，方便我们把对距离的约束转成对内积的约束，而后者是可以解的。**两个要求意味着存在一个内积的结构，并且它能放进$\mathbb{R}^n$。
      
      具体来说，比如已经知道存在一个等距同构的嵌入，那么我们直接把H用欧氏空间中的距离写成
      
      $H_{i j}=\delta\left(s_{i}, s_{j}\right)^{2}=\left\|x_{i}-x_{j}\right\|_{2}^{2}=\left\|x_{i}\right\|_{2}^{2}+\left\|x_{j}\right\|_{2}^{2}-2 x_{i} \cdot x_{j}$
      
      或者说
      
      $H=v \mathbb{I}^{t}+\mathbb{I} v^{t}-2 X X^{t}$
      
      因此
      
      $$
      B=J X X^{t} J=(J X)(J X)^{t}
      $$
      
      满足条件。其中JX就是在欧氏空间中mean-centered了的、被嵌入了的S的结构。
    
    - Classical MDS（2）：近似版本。Isometric 这一点太严格了，基本上不能把数据点嵌入低维，但是它的思维方式非常有帮助。
      
      现在，我们把被欧氏空间压到很低的n维，希望找到一个$B'$，和B尽可能近，并且能够表示一个内积的结构。这个“尽可能近”的标准，出于计算难度的考量，选取frobenius norm：
      
      $$
      X=\arg \min _{X}\left\|B-X X^{t}\right\|_{F}^{2}
      $$
      
      因为$\left\|B-X X^{t}\right\|_{F}^{2}=\left\|\Lambda-V^{t} X X^{t} V\right\|_{F}^{2}$，令$S=V^{t} X X^{t} V$，我们有
      
      $$
      \left\|B-X X^{t}\right\|_{F}^{2}=\sum_{i=1}^{m}\left(\lambda_{i}-S_{i i}\right)^{2}+\sum_{i \neq j} S_{i j}^{2}
      $$
      
      最小化这东西，我们得让$S_{ij}=0,$并且尽量多的把正的、大的$\lambda_i$消掉。因此，
      
      $$
      S=\operatorname{diag}\left(\lambda_{1}-\overline{\lambda}_{1}, \ldots, \lambda_{n}-\overline{\lambda}_{n}, 0, \ldots, 0\right)
      $$
      
      其中$\bar{\lambda}=min(\lambda, 0)$。Scaling在这里就是指把feature空间中的距离scale到欧氏空间的距离的过程。
    
    - metric MDS。我们寻找单调的$T(\delta)$，使得$T\left(\delta\left(s_{i}, s_{j}\right)\right) \approx d\left(x_{i}, x_{j}\right)$，其中$d\left(x_{i}, x_{j}\right)=\left\|x_{i}-x_{j}\right\|_{2}$。然后我们使用一个stress function来优化这个T。例如，可以选取stress function 为
      
      $\mathcal{L}(d, T(\delta))=\sum_{i j}\left(d\left(x_{i}, x_{j}\right)-T\left(\delta\left(s_{i}, s_{j}\right)\right)\right)^{2}$
      
      然后在欧氏空间找到最合适的一群$\{x_i\}$。这个优化过程和cMDS很像，$T(\delta)$相当于B，$d(x_i, x_j)$相当于$XX^T$。
  
  - spectral embedding and graph laplacian。这个方法的主要思想是**聚类，即把相似的数据点放到一起。**实际操作上，先找一个核，代表feature空间在hilbert space的嵌入（回忆kernel的定义，$\kappa$越大表示这两个点联系越紧密），再找一个映射$f : \mathcal{H} \rightarrow \mathbb{R}^{k}$表示在k维空间做聚类，我们最小化
    
    $$
    \begin{aligned} E(\tau) &=\sum_{i, j=1}^{m}\left\langle\phi\left(s_{i}\right), \phi\left(s_{j}\right)\right\rangle\left\|f\left(\phi\left(s_{i}\right)\right)-f\left(\phi\left(s_{j}\right)\right)\right\|_{2}^{2} \\ &=\sum_{i, j=1}^{m} \kappa\left(s_{i}, s_{j}\right)\left\|\tau\left(s_{i}\right)-\tau\left(s_{j}\right)\right\|_{2}^{2} \end{aligned}
    $$
    
    我们做一些情有可原的约束：
    
    - standardization: 对$\tau$的各个分量，都有$\sum_{i=1}^{m} \tau_{\alpha}\left(s_{i}\right)=0$，$\sum_{i=1}^{m} \tau_{\alpha}\left(s_{i}\right) \tau_{\alpha}\left(s_{i}\right)=1$；
    - uncorrelated：$\sum_{i=1}^{m} \tau_{\alpha}\left(s_{i}\right) \tau_{\beta}\left(s_{i}\right)=0,$ for $\alpha \neq \beta$。
  
  $$
  \begin{aligned} E(\tau) &=\sum_{\alpha=1}^{k} \sum_{i, j=1}^{m} \kappa\left(s_{i}, s_{j}\right)\left(\tau_{\alpha}\left(s_{i}\right)-\tau_{\alpha}\left(s_{j}\right)\right)^{2} \\ &=2 \sum_{\alpha=1}^{k} \sum_{i, j=1}^{m} \kappa\left(s_{i}, s_{j}\right)\left[\tau_{\alpha}^{2}\left(s_{i}\right)-\tau_{\alpha}\left(s_{i}\right) \tau_{\alpha}\left(s_{j}\right)\right] \\ &=2 \sum_{\alpha=1}^{k}\left[\sum_{i=1}^{m} \tau_{\alpha}\left(s_{i}\right)\left(\sum_{\ell=1}^{m} \kappa\left(s_{i}, s_{\ell}\right)\right) \tau_{\alpha}\left(s_{i}\right)-\sum_{i, j=1}^{m} \tau_{\alpha}\left(s_{i}\right) \kappa\left(s_{i}, s_{j}\right) \tau_{\alpha}\left(s_{j}\right)\right] \\ &=2 \sum_{\alpha=1}^{k} \sum_{i, j=1}^{m} \tau_{\alpha}\left(s_{i}\right)\left[\left(\sum_{\ell=1}^{m} \kappa\left(s_{i}, s_{\ell}\right) \delta_{i j}\right)-\kappa\left(s_{i}, s_{j}\right)\right] \tau_{\alpha}\left(s_{j}\right) \\ &=2 \sum_{\alpha=1}^{k} \tau_{\alpha}^{t} L \tau_{\alpha} \end{aligned}
  $$
  
   L就是kernel阵对应的graph laplacian。最后这玩意的最小化，加上对于$\tau$的约束，完全可以由 rayleigh quotient部分的讨论而得到：令v表示L前k个最小的本征值（扣除trivial的0和其本征向量$(1, \cdots, 1)^t$）对应的本征向量，$\tau_i(s_j)=(v_i)_j$。

- 和经典力学的联系：**我们把一堆小球用弹簧连起来，ij球之间的弹簧，其倔强系数是$k_{ij}$，那么就有$\frac{d^{2} x}{d t^{2}}=-L x$，于是振动的模由L的谱决定。设想有一些小球之间关系紧密，其之间的弹簧甚倔强，这些小球就会作为一个整体振动，如同它们被聚类了一样。**