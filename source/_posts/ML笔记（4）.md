---
title: ML笔记（4）
date: 2019-10-21 18:15:34
categories: [笔记, 理学]
tags: [机器学习]
---

### week 7, 8

<!--more-->

#### lecture

- 10.8
  
  不小心翘掉了一节课。所幸，此课时基本上没有说什么特别重要而我又不太知道，并且没有在今天的summary中覆盖到的部分。
  
  现在开始搞RKHS了。关于这个东西的定义和一些理论，都写在notes 0里面了，包括Moore-Aronszajn定理，Cauchy Sequence，Completion of Hilbert space 等等。基本上，上节课讲的东西的主要内容，就是在有限维空间的情况下证明Moore-Aronszajn定理。为了叙事的连贯性，我还是在这个文件里把必要的地方都说明一下吧。
  
  - RK是如此的一个kernel，即满足
    
    $$
    \bullet \forall x \in \mathcal{X}, \quad k(\cdot, x) \in \mathcal{H} \\ \bullet \forall x \in \mathcal{X}, \forall f \in \mathcal{H}, \quad\langle f, k(\cdot, x)\rangle_{\mathcal{H}}=f(x)
  $$
    
    者。它有一个重要的等价性质，是说evaluation functional在对应的这个RKHS里面连续。这个性质同时也正是RKHS的定义。
  
  - 可以首先在有限维的情况下考虑RKHS的性质。比如说取一个建立了内积的$V\subset\mathbb{R}^m$。那么这时候一个核实际上可以被写成是（作用在基的函数）上的双线性函数$K\in\mathbb{R}^{m\times m}$。上节课给出如下结论，以下诸命题等价：
    
    - K是V上的RK。
    - 存在V上一组正交归一基u使得$K=\sum u_iu_i^t$。
    - $\text{columnspan}(K)=V$并且$K_{ij}=\langle k_i,k_j\rangle$。
    
    这看上去是可以接受的。我无意追寻具体的证明。接下来，可以用这结论证明简单版的MA定理。
  
  内容全在讲义里。

#### reading material

- lecture notes: RKHS and kernel regression
  
  - RKHS: 定义；性质。RKHS是一个很重要的概念，虽然它的实际用处在这个阶段还没有显现出来。
  
  - 我们将会证明，
    
    > 对于一个函数$\kappa: X \times X \rightarrow\mathbb{R}$，以下命题等价：
    > 
    > - 它是半正定的
    > - 它是X上一个RKHS的RK
    > - 它是一个kernel
    
    我们先考察有限维空间上的RK，再推广到无穷维的情况。这时对evaluation的定义比较简单，令$E_{i}(v)=e_{i}^{t} v, \forall v \in \mathbb{R}^{X}$即可。考虑到RK的定义，我们这个时候不妨把k直接当做是一个半正定的矩阵。这个时候，可以把Riesz表示定理写成
    
    > 线性泛函$\phi \in V^{*}$可以被表示成$\langle\cdot, v\rangle$，其中$v=\sum_{i=1}^{n} \phi\left(e_{i}\right) e_{i}$。
    
    然而，在无穷维的情况下，这结论只对连续的线性泛函成立。以后我们将看到，对无穷维HS，加上这种限制的对偶空间叫topological dual，而由所有线性泛函组成的叫algebraic dual。
    
    在特定的operator norm下，V和V\*构成isometric isomorphism：
    
    $\|\psi(v)\|_{V, 1}=\sup _{\|w\|_{V}=1}|\psi(v)(w)|=\sup _{\|w\|_{V}=1}|\langle w, v\rangle| \leq \sup _{\|w\|_{V}=1}\|w\|_{V}\|v\|_{V}=\|v\|_{V}$
  
  - 下面讨论（有限维情况下）RK的构造。
    
    我们可以证明，
    
    > 内积空间V。以下等价：
    > 
    > - K是V上的RK
    > - $K=\sum_{j=1}^{n} u_{j} u_{j}^{t}$，其中U是一组幺正基
    > - $\text{columnspan}(K)=V$并且$K_{ij}=\langle \kappa_i,\kappa_j\rangle$
    
    有了HS，我们就有了RK。反过来呢？
    
    有定理：
    
    > 对正定m维方阵K，存在唯一的带内积的$V \in \mathbb{R}^{m}$使得K为其RK。
    
    张开K的columns，这空间里元素都可写成是$v=K \alpha$的格式。现在定义内积$\langle v, w\rangle \equiv \alpha^{t} K \beta$。需要证明，这个内积不会因为v可以被表为不同的$K\alpha$和$K\alpha'$而变化——事实的确如此。这时候，很容易知道$\left\langle k_{i}, k_{j}\right\rangle= K_{i j}$也是满足的，因此K确实是RK。唯一性是因为无论如何V都是K的columnspan，并且其中的内积也一样。
  
  - Intrinsic和Extrinsic坐标。
    
    V的元素v=(v1, ...vm)（$v \in V \subset \mathbb{R}^{X}$）可以表示成函数v作用在给定的数据集$X=\{x_1, \cdots, x_m\}$上的结果。不过，如果V只是欧氏空间的一个真子集，函数v的值域有可能超出V。因此，使用这一套extrinsic坐标参数化V并不好。
    
    但是，如果使用K，那么$v=K \alpha$，所有的$\alpha \in \mathbb{R}^{m}$都会被映到V里面。
  
  - 无穷维的情况
    
    先讨论一些背景。量子力学里面，关键的假设是$\psi(x)=\sum_{k=1} \alpha_{k} \psi_{k}(x)$收敛。用数学的话说，$f_{n}=\sum_{k=1}^{n} \alpha_{k} \psi_{k}$ 是一个cauchy sequence。回忆曾经在lecture notes中第一章看到的，如果所有柯西列都收敛到本空间内的点，那么这空间就算是完备的；完备赋范，谓之banach；完备内积，谓之hilbert。如果一个HS有可数的幺正基，那么就算是separable的。所有separable的HS都isometrically isomorphic to $\mathbb{R}^{n},$ for some $n \geq 0,$ or to $\ell^{2}$。
    
    其中，$\ell^2$指的是$\sum_{n=1}^{\infty} f_{n}^{2}<\infty$的序列的集合，其内积定义为$\left\langle\left\{f_{n}\right\},\left\{g_{n}\right\}\right\rangle=\sum_{n=1}^{\infty} f_{n} g_{n}$。
    
    这样一来，就把同为HS的 $L^2[a,b]$(是波函数生活的空间，对应薛定谔的波动力学)和$\ell^2$（对应矩阵力学）对应起来。
    
    注意到，$L^2$并非RKHS，而$\ell^{2}$却是，这并不违反什么，只是说明RKHS的RK性质在HS上加了一层结构，这个额外的结构可以用feature space X来表示。
    
    Moore-Aronszajn定理（无穷维）的证明与有限维的情况类似，从feature space里面取X，$\mathcal{H}_{0}=\operatorname{span}\{k(\cdot, x)\}_{x \in X}$，那么把V中的元素类似地写成$f=\sum_{i=1}^{n} \alpha_{i} K\left(\cdot, x_{i}\right)$，内积也类似地定义为
    
    $\langle f, g\rangle=\sum_{i=1}^{n} \sum_{j=1}^{m} \alpha_{i} K\left(x_{i}, x_{j}\right) \beta_{j}$，
    
    那么就得到pre-Hilbert space。然后，作completion，得到我们想要的HS。
  
  - 关于kernel construction，基本上已经在notes 0里面写过了。Mercer Kernel还没有写，故在此补充一下。有定理
    
    > $T: \mathcal{H} \rightarrow \mathcal{H}$是separable HS上的紧的算子。那么，存在两组不一定完备的幺正基和一组正实数，满足
    > 
    > $T=\sum_{n=1}^{N} \lambda_{n} \phi_{n}\left\langle\psi_{n}, \cdot,\right\rangle$
    
    这个式子的意义就是无穷维HS的奇异值分解，这组正实数也就叫奇异值。
    
    进一步的，如果T还是自伴的，那么$\phi$和$\psi$就是同一组，而且还完备，对应的本征值有$n\to\infty$时$\lambda_n\to0$。
    
    Mercer的定理：
    
    > X是n维欧氏空间的紧的子集。$K: X \times X \rightarrow \mathbb{R}$是一对称，连续，半正定函数，那么有核
    > 
    > $K(x, y)=\sum_{n} \lambda_{n} \phi_{n}(x) \phi_{n}(y)$
    > 
    > 对应$T(f)(x)=\int_{X} K(x, y) f(y) d y$，其中$\lambda$和$\phi$对应的是T的eigenset。
    
    关于应用和例子，写在lecture部分了，因为这块lecture稍微多讲了一点。
  
  - 线性回归：普通线性回归
    
    假如我们有随机变量$\left(X_{1}, \ldots, X_{n}\right) \in \mathbb{R}^{n}$，我们想通过它们预测 response Y。现在，我们规定（由MLE可以推出来）损失函数是$E\left[\left(Y-f\left(X_{1}, \ldots, X_{n}\right)\right)^{2}\right]$，其中predictor f是$f\left(x_{1}, \ldots, x_{n}\right)=E\left[Y | X_{1}=x_{1}, \ldots, X_{n}=x_{n}\right]$。
    
    在线性回归中，我们认为Y服从以$\beta X$为均值，$\sigma^2$为方差（各向同性）的高斯分布。写出来就是
    
    $\boldsymbol{Y}=\left(\begin{array}{c}{y_{1}} \\ {y_{2}} \\ {\vdots} \\ {y_{m}}\end{array}\right), \boldsymbol{X}=\left(\begin{array}{ccccc}{1} & {x_{11}} & {x_{12}} & {\cdots} & {x_{1 n}} \\ {1} & {x_{21}} & {x_{22}} & {\cdots} & {x_{2 n}} \\ {\vdots} & {\vdots} & {\vdots} & {\vdots} & {\vdots} \\ {1} & {x_{m 1}} & {x_{m 2}} & {\cdots} & {x_{m n}}\end{array}\right) \quad$ and $\boldsymbol{\beta}=\left(\begin{array}{c}{\beta_{0}} \\ {\beta_{1}} \\ {\vdots} \\ {\beta_{n}}\end{array}\right)$
    
    $\boldsymbol{Y}=\boldsymbol{X} \boldsymbol{\beta}+\boldsymbol{e}$
    
    $\begin{aligned} \boldsymbol{Y} | \boldsymbol{X} & \sim \mathcal{N}\left(\boldsymbol{X} \boldsymbol{\beta}, \sigma^{2} \boldsymbol{I}_{m \times m}\right) \\ \boldsymbol{e} & \sim \mathcal{N}\left(0, \sigma^{2} \boldsymbol{I}_{m \times m}\right) \end{aligned}$
    
    用MLE，可以求出来最优的$\hat{\boldsymbol{\beta}}=\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1} \boldsymbol{X}^{T} \boldsymbol{Y}$。我们对此比较熟悉。如果$\boldsymbol{X}^{T} \boldsymbol{X}$不可逆，那么用MP伪逆代替。
    
    $E[\hat{\boldsymbol{\beta}}]=\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1} \boldsymbol{X}^{T} E[\boldsymbol{Y}]=\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1} \boldsymbol{X}^{T} \boldsymbol{X} \boldsymbol{\beta}=\boldsymbol{\beta}$
    
    所以是无偏估计。这过程里面，X看做是给定的，测出来的；Y是随机变量。
    
    类似的，
    
    $\operatorname{var}(\hat{\boldsymbol{\beta}})=\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1} \boldsymbol{X}^{T} \operatorname{var}(\boldsymbol{Y}) \boldsymbol{X}\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1}=\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1} \sigma^{2}$
    
    在几何上，线性回归的结果的意义如下：
    
    $\hat{\boldsymbol{Y}}=\boldsymbol{X}\left(\boldsymbol{X}^{T} \boldsymbol{X}\right)^{-1} \boldsymbol{X}^{T} \boldsymbol{Y}=\boldsymbol{H} \boldsymbol{Y}$
    
    其中H满足对称和幂等的性质，是投影算符。H把Y投影到X的列张成的空间上。
    
    误差和prediction满足
    
    $\hat{\boldsymbol{Y}} \sim \mathcal{N}\left(\boldsymbol{X} \boldsymbol{\beta}, \sigma^{2} \boldsymbol{H}\right)$
    $\hat{\boldsymbol{e}} \sim \mathcal{N}\left(0, \sigma^{2}(\boldsymbol{I}-\boldsymbol{H})\right)$
    
    并且$\operatorname{cov}[\hat{\boldsymbol{e}}, \hat{\boldsymbol{Y}}]=(\boldsymbol{I}-\boldsymbol{H}) \sigma^{2} \boldsymbol{H}=0$。
  
  - 贝叶斯回归/岭回归
    
    见lecture，不写这个的话周二的lecture没得写了
  
  - Kernel regression
    
    $\hat{\boldsymbol{\beta}}=\left(\boldsymbol{X}^{t} \boldsymbol{X}+\alpha \boldsymbol{I}_{n \times n}\right)^{-1} \boldsymbol{X}^{t} \boldsymbol{Y}$可以写成
    
    $\hat{\boldsymbol{\beta}}=\boldsymbol{X}^{t}\left(\boldsymbol{X} \boldsymbol{X}^{t}+\alpha \boldsymbol{I}_{m \times m}\right)^{-1} \boldsymbol{Y}$，于是
    
    $\hat{\boldsymbol{Y}}=\boldsymbol{X} \hat{\boldsymbol{\beta}}=\boldsymbol{X} \boldsymbol{X}^{t}\left(\boldsymbol{X} \boldsymbol{X}^{t}+\alpha \boldsymbol{I}_{m \times m}\right)^{-1} \boldsymbol{Y}$
    
    对于一个点来说，就是
    
    $\hat{y}=x \boldsymbol{X}^{t}\left(\boldsymbol{X} \boldsymbol{X}^{t}+\alpha \boldsymbol{I}_{m \times m}\right)^{-1} \boldsymbol{Y}$
    
    所谓kernel trick 就是用K代替复杂的gram矩阵$XX^t$，这时有$\hat{y}=\kappa(x, \cdot)\left(\boldsymbol{K}+\alpha \boldsymbol{I}_{m \times m}\right)^{-1} \boldsymbol{Y}$。
    
    一个比较常见的选择是$\kappa\left(x_{1}, x_{2}\right)=\exp \left(-\left(x_{1}-x_{2}\right)^{2} / 2 \sigma^{2}\right)$。

- lecture notes: Multilayer graphs and grassmannian manifolds
  
  - spectral embedding只能处理一张图的嵌入。如果有多张图怎么办？这讲义只讨论这一个问题，并给出行之有效的recipe。
  
  - Spectral Embedding 可以叙述为：最小化$\operatorname{tr}\left(V^{t} L V\right)$，其中$V^{t} V=I_{k \times k}$。V的行给出欧式空间中的嵌入。
    
    在此情形之下，一个想当然的推广是最小化
    
    $\operatorname{tr}\left(\sum_{i=1}^{\ell} V^{t} L_{i} V\right)=\operatorname{tr}\left(V^{t}\left(\sum_{i=1}^{\ell} L_{i}\right) V\right)$
    
    但是，如果$L_i$的scale区别很大，这个过程就会沦为对某一张图的嵌入。这是不行的。
  
  - Stiefel和Grassmannian流形。定义Stiefel Manifold $V(k,n)$是$\mathbb{R}^{n}$上所有k-frame的集合，frame就是一组有序的幺正基。令W是一个给定的$n \times k$矩阵，其列为幺正基，那么
    
    $\forall W^{\prime} \in V(k, n), \exists U \in O(n),$ such that $W^{\prime}=U W$。注意到，U的选取不是唯一的：任何在$\perp W$的部分旋转的U都不会改变什么东西。这部分空间共$n-k$维。所以
    
    $V(k, n) \cong O(n) / O(n-k)$
    
    其中，$O(n)$的维数是$C_n^2$。因为一个旋转对应一个平面。
    
    定义格拉斯曼流形$\operatorname{Gr}(k, n)$为$\mathbb{R}^n$上所有k维子空间的集合。在子空间内部的旋转当然也不会搞出一个新的子空间来，所以
    
    $G r(k, n)=V(k, n) / O(n)=O(n) /(O(n-k) \times O(k))$
    
    我觉得可以把这玩意当成是旋转的一种推广。
  
  - Principle Angles
    
    取$A, B \in G r(k, n)$。递归定义主向量
    
    $\left(p_{i}, q_{i}\right)=\arg \quad \quad \max \quad \quad p^{t} q$
    $p \in A, q \in B$
    $p \perp p_{j}, q \perp q_{j}, j=1, \ldots, i-1$
    $\|p\|_{2}=1,\|q\|_{2}=1$
    
    然后，principle angle 为$\theta_{i}=\cos ^{-1}\left(p_{i}^{t} q_{i}\right), i=1, \ldots, k$。
    
    取$A \in \operatorname{Gr}(k, n)$ and $B \in \operatorname{Gr}(l, n)$。那么，可以用矩阵把他们表示为$A \in \mathbb{R}^{n \times k}$ and $B \in \mathbb{R}^{n \times l}$。写下
    
    $A^{t} B=U \Sigma V^{t}$
    
    那么这个SVD分解出来的奇异值$\sigma_{1} \geq \sigma_{2} \geq \ldots \sigma_{r} \geq 0$（$r=\min (k, l)$）就对应主角$\theta_{i}=\cos ^{-1} \sigma_{i}, \quad i=1, \ldots, r$。这件事情从原则上是可以理解的，所以我就不证明它了。
    
    定义 Chordal(Projection) 距离：
    
    $d_{c}(A, B)=\sqrt{\sum_{i=1}^{r} \sin ^{2} \theta_{i}}$——
  
  - ——那么，就可以进行multilayer的spectral clustering。取lagrangian
    
    $\mathcal{L}=\operatorname{tr}\left(V^{t}\left(\sum_{i=1}^{\ell} L_{i}\right) V\right)+\alpha \sum_{i=1}^{\ell} d_{c}^{2}\left(V, V_{i}\right)$
    
    可以证明，
    
    $\begin{aligned} \mathcal{L} &=\operatorname{tr}\left(V^{t}\left(\sum_{i=1}^{\ell} L_{i}\right) V\right)+\alpha \sum_{i=1}^{\ell}\left(k-\operatorname{tr}\left(V^{t} V_{i} V_{i}^{t} V\right)\right) \\ &=\operatorname{tr}\left(V^{t}\left[\sum_{i=1}^{\ell}\left(L_{i}-\alpha V_{i} V_{i}^{t}\right)\right] V\right)+\alpha k \ell \end{aligned}$
    
    这里面$\alpha$是超参数，一般也是用cross-validation之类的办法确定的。实际上，这个recipe看上去还是挺随意，它的合法性基本上是它的效用决定的。
  
  - Courant-Fischer 定理说，M是$n \times n$的对称阵。它有由小到大排列的本征值$\lambda_{1}, \ldots, \lambda_{n}$。那么，
    
    $\lambda_{k}=\min _{V \in \operatorname{Gr}(k, n)}\left(\max _{v \in V \backslash\{0\}} \frac{v^{t} M v}{v^{t} v}\right)=\max _{V \in \operatorname{Gr}(n-k+1, n)}\left(\min _{v \in V \backslash\{0\}} \frac{v^{t} M v}{v^{t} v}\right)$。
    
    从这里，可以知道，对于对称阵M，
    
    $\min _{V \in \mathbb{R}^{n \times k}, V^{t} V=I_{k \times k}} \operatorname{tr}\left(V^{t} M V\right)=\sum_{i=1}^{k} \lambda_{i}$。原因是：
    
    取$v_{i}=\arg \max _{v \in S,\|v\|_{2}=1, v \perp v_{i+1}, \ldots, v_{k}} v^{t} M v$，
    
    由于求迹对旋转不变，$\operatorname{tr}\left(V^{t} M V\right)=\sum_{i=1}^{k} v_{i}^{t} M v_{i}$。
    
    由Courant-Fischer, $v_{i}^{t} M v_{i} \geq \lambda_{i}$。
  
  - Pareto优化：
    
    对于所有图，用某种方式算某个特定嵌入的loss。如果嵌入1在所有图的表现都不差于嵌入2，且至少在一个图表现更好，那么就说1比2好。现有生成的一大堆嵌入，若不存在比嵌入3更好的，就说3在pareto front上。由此构建pareto front。