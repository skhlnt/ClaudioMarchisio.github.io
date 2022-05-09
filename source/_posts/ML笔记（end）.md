---
title: ML笔记（end）
date: 2019-12-13 23:53:43
categories: [笔记, 理学]
tags: [机器学习]
---

## week 13,14

第七章讲的是数值算法。此外，由于这篇太短了，我决定瞎鸡巴议论一番。可能会很naive。。。

<!--more-->

#### 交叉熵

- KL散度
  
  $D\left(P_{1} \| P_{2}\right)=\sum_{x \in \mathcal{X}} P_{1}(x) \log _{2} \frac{P_{1}(x)}{P_{2}(x)}$
  
  是一个度量两个分部之间相似程度的量。说实话，这个地方divergence看上去和“散度”并没有什么关系（因为此时并不存在与它对应的“旋度”的概念），更像是字面意义上的“分歧度”。它始终大于零，除非两个分布完全一样。
  
  KL散度体现了我们对于“熵最大的分布是均匀分布”的认识。考虑$P_2=1/n$，
  
  $D\left(P_{1} \| P_{2}\right)=-H(X)+\log _{2} n \geq 0 \Rightarrow \log _{2} n \geq H(X)$，
  
  其中$H(X)=-\sum_{x \in \mathcal{X}} P(x) \log _{2} P(x)$是香农熵。如果我们进一步考虑能量守恒$\sum xP=\langle E\rangle$这个约束，最小化$D(P_1||P_2)$，就得到指数族，这很可以作为正则系综的一个解释。至于香农熵用来代表热力学熵的合法性，是来自于几个基本的直觉的，jaynes深入地讨论过。
  
  坏处上，KL散度这个量是不对称的，而且也没归一化。作为一种改进，我们有

- JS（jensen-shannon）散度：
  
  $J\left(P_{1}, P_{2}\right)=\frac{1}{2} D\left(P_{1} \| P\right)+\frac{1}{2} D\left(P_{2} \| P\right)$
  
  $P(x)=\frac{1}{2} P_{1}(x)+\frac{1}{2} P_{2}(x)$
  
  这个量不仅对称，还被限制在0-1之间。引入一个变量$z=\{1, 2\}$，$P(z=i):=\pi_i$。在我们的例子中，$P_i(x)=P(x|z=i)$，$\pi\equiv0.5$。定义分布x和z的互信息
  
  $I(x, z)=\sum_{x,z}p(x,z)\log_2\frac{p(x, z)}{p(x)p(z)}$
  
  把它写成$H(z)-H(z|x)$，在我们的例子里就是$1-H(z|x)\leq 1$。
  
  同时，还可以证明这个互信息，也就是$P_{1,2}$的平均的分布和均匀二点分布的互信息，正是JS散度。因此，可以证明JS散度是有上限的。

- 交叉熵
  
  $H_{c}(P, Q):=-\sum_{x \in \mathcal{X}} P(x) \log _{2} Q(x)=\sum_{x \in \mathcal{X}} P(x) \log _{2} \frac{1}{Q(x)}=H(P)+D(P \| Q)$
  
  假如我们想拿一族分布Q去找$D(P||Q)$的最小值，处理$H_{c}(P, Q)$一般要方便些。例如，试图通过sample来计算积分
  
  $f(z)=\int_{z}^{\infty} \frac{1}{\sqrt{2 \pi}} e^{-x^{2} / 2} d x=\int_{-\infty}^{\infty} I(x \geq z) p(x) d x$
  
  我们想拿指数族$q(x ; \lambda)=\lambda e^{-\lambda(x-z)}$去做这个importance sampling的分布q。作极值问题
  
  $\arg \min _{q}\left(-\int_{-\infty}^{\infty} \pi(x) \log _{2} q(x ; \lambda) d x\right)=\arg \max _{q} \int_{-\infty}^{\infty} I(x \geq z) p(x) \log q(x ; \lambda) d x$
  
  解出来$\lambda$的取值是
  
  $\frac{\int I(x\geq z)p(x)dx}{\int I(x\geq z)(x-z)p(x)dx}$
  
  看起来很不妙，又回到原点。但是实际上，并不需要太担心：有迭代的算法。先随便找一个$\lambda_0$，比如说是1吧，稍微sample一下：
  
  $\int_{-\infty}^{\infty} I(x \geq z) p(x) d x \approx \frac{1}{N} \sum_{i=1}^{N} I\left(x_{i} \geq z\right) \frac{p\left(x_{i}\right)}{q\left(x_{i} ; \lambda_{0}\right)}=\frac{1}{N} \sum_{i=1}^{N} \frac{p\left(x_{i}\right)}{q\left(x_{i} ; \lambda_{0}\right)}$
  
  $\int_{-\infty}^{\infty}(x-z) I(x \geq z) p(x) d x \approx \frac{1}{N} \sum_{i=1}^{N}\left(x_{i}-z\right) I\left(x_{i} \geq z\right) \frac{p\left(x_{i}\right)}{q\left(x_{i} ; \lambda_{0}\right)}=\frac{1}{N} \sum_{i=1}^{N}\left(x_{i}-z\right) \frac{p\left(x_{i}\right)}{q\left(x_{i} ; \lambda_{0}\right)}$
  
  就可得到
  
  $\lambda_{t+1}=\frac{\sum_{i=1}^{N} \frac{p\left(x_{i}\right)}{q\left(x_{i} ; \lambda_{t}\right)}}{\sum_{i=1}^{N} \frac{p\left(x_{i}\right)}{q\left(x_{i} ; \lambda_{t}\right)}\left(x_{i}-z\right)}$
  
  迭代到$\lambda$收敛即可。

- 交叉熵方法的一些应用
  
  首先是小概率事件的模拟（可以认为是上一节讲的方法的推广）：假设有个向量随机变量X服从分布$p$，一个标量函数S，想知道$P(S(X) \geq \gamma)=E_{p}[I(S(X) \geq \gamma)]$，然而$\{X | S(X) \geq \gamma\}$发生的概率极其之小。这个时候，仍然是做importance sampling：
  
  $q^{\*}(x) \propto I(S(X) \geq \gamma) p(x)$
  
  对分布族q猜一个$\theta_0$，求交叉熵
  
  $H_{c}\left(q^{\*}, q_{\theta}\right) \propto-E_{p}\left[I(S(X) \geq \gamma) \log q_{\theta}(X)\right]=-E_{q_{\theta_{0}}}\left[I(S(X) \geq \gamma) W(X, \theta) \log q_{\theta}(X)\right]$
  
  其中$W=p(X) / q_{\theta_{0}}(X)$。
  
  同样是迭代。做法是这样的：
  
  - 猜一个$\theta_{0}$；
  
  - 从$q_{\theta_{t-1}}$sample一些数据$X_{1}, \ldots, X_{N}$；
  
  - 从$\left\{S\left(X_{1}\right), \ldots, S\left(X_{N}\right)\right\}$中选取$1-\rho$百分位的值作为$\gamma_{t}$(我们希望从q中sample出来的X大多数都有$S(x)>\gamma$)；$\gamma_{t}>\gamma$就令$\gamma=\gamma_{t}$;
  
  - $\theta_{t}=\arg \max _{\theta} \frac{1}{N} \sum_{i=1}^{N} I\left(S\left(X_{i}\right) \geq \gamma_{t}\right) W\left(X_{i} ; \theta_{t-1}\right) \log q_{\theta}\left(X_{i}\right)$
  
  - $\gamma_{t} \geq \gamma$就中止循环；
  
  - 做采样$\hat{E}_{q \theta_{t}}\left[I(S(X) \geq \gamma) W\left(X, \theta_{t}\right)\right]=\frac{1}{N} \sum_{i=1}^{N} I\left(S\left(X_{i}\right) \geq \gamma\right) W\left(X_{i}, \theta_{t}\right)$
  
  参数$\rho$一般是选0.01到0.1之间，经验上选取$N \propto n / \rho$。
  
  然后，还有一个关于组合优化问题的例子：令$\Omega$是一个不连续的位形空间（其实连续的也可以），$S: \Omega \rightarrow \mathbb{R}$是一个标量函数，我们现在想找$X^{\*}=\max _{X \in \Omega} S(X)$
  
  这个问题的背景是网络的切割，或者（在位形空间连续的情况下）是流形的剖分（假设切流形费刀，怎么切损伤最小）。S就可以写成例如
  
  $S=-\frac{vol(A_1\cap A_2)}{\min(vol(A_1), vol(A_2))}$
  
  对于流形，这个截面积叫Cheeger's constant。我们怎样求呢？我们可以找一个分布$P_{\theta^{\*}}(X)$使得它堆在最优的$X^\*$附近。我们的目标就是使得$P_{\theta_t}$的$(1-\rho)$百分位$\gamma_t$越大越好。所以我们每次求交叉熵，都是力图优化下一个迭代的P，使得它尽量选上一个P里面最大的那部分S(X):
  
  $H_{c}\left(I\left(S(X) \geq \gamma_{t}\right) P_{\theta_{t-1}}(X), P_{\theta}(X)\right)=-\frac{1}{N} \sum_{i=1}^{N} I\left(S\left(X_{i}\right) \geq \gamma_{t}\right) \log _{2} P_{\theta}\left(X_{i}\right)$
  
  算法是这样的：
  
  - 猜一个$\theta_{0}$；
  - 从$P_{\theta_{t-1}}$sample一些数据$X_{1}, \ldots, X_{N}$；
  - 从$\left\{S\left(X_{1}\right), \ldots, S\left(X_{N}\right)\right\}$中选取$1-\rho$百分位的值作为$\gamma_{t}$；
  - 交叉熵优化$\eta_{t}=\arg \max _{\theta} \sum_{i=1}^{N} I\left(S(X) \geq \gamma_{t}\right) \log P_{\theta}\left(X_{i}\right)$；
  - $\theta_{t}=\alpha \eta_{t}+(1-\alpha) \theta_{t-1}$（这么干是为了防止陷入局部极值，即函数-S有局部极小值）
  - $\theta_{t}=\theta_{t-1}=\cdots=\theta_{t-d}$即达到稳定的时候停止迭代。
  
  很显然，这玩意让人想到梯度下降。

#### 模拟退火

我们知道Metropolis算法会卡在局部极小（或者说$\pi_s$的极大）值附近，造成poor mixing的问题。如果我们把温度升得很高，以至于$\pi_{s}^{\beta}(x) \approx \pi_{s}^{\beta}(y)$，那么就不会有这样的问题。我们可以在在高温开始走，然后慢慢降温（退火），重复N次，就得到好的sampling。实践中，一般是降一次温走N步，降温的做法是$\beta_{t}=\beta_{0}\left(\frac{\beta_{\max }}{\beta_{0}}\right)^{\frac{t}{n}}$。注意，只要是降温，必定改变平衡分布，解决这问题一般靠的是并行退火（parallel tempering）。

并行退火说的是在同一时间，在不同温度下作多个马氏链，并把它们最终合起来。目的是让这个非物理的高维的马氏链的steady state在变温时保持不变。怎样做呢？在不同的温度下，平衡分布被抹平，即记$\pi_{k}(y) \propto \pi(y)^{\beta_{k}}$，其中$0<\beta_{0}<\beta_{1}<\ldots<\beta_{N}=1$，记录退火过程。对于每一个$\pi_k$，我们都建立一个空间S上的MCMC使其steady state为$\pi_k$。然后我们建立$\Omega:=S^{N}$上的分布

$\pi_{P T}(x)=\prod_{k=1}^{N} \pi_{k}\left(y_{k}\right)$

x演化时，让k个元素分别演化。同时，我们还允许$y_{k}^{(t)}$ 和 $y_{k+1}^{(t)}$互换。为了满足细致平衡，需要

$r=\min \left\{1, \frac{\pi_{P T}\left(\left(y_{1}^{(t)}, \ldots, y_{k+1}^{(t)}, y_{k}^{(t)}, \ldots, y_{N}^{(t)}\right)\right.}{\pi_{P T}\left(y_{1}^{(t)}, \ldots, y_{k}^{(t)}, y_{k+1}^{(t)}, \ldots, y_{N}^{(t)}\right)}\right\}$

即

$r=\min \left\{1, \frac{\pi_{k}\left(y_{k+1}^{(t)}\right) \pi_{k+1}\left(y_{k}^{(t)}\right)}{\pi_{k}\left(y_{k}^{(t)}\right) \pi_{k+1}\left(y_{k+1}^{(t)}\right)}\right\}$

关于第15,16周的事情，事实上其中一周基本上是放假放掉了，还有一周提了一下gaussian process和相关的regression（其实是interpolation）的方法，不过我懒得记了。

期末我做了一个关于spin glass algorithm of community detection的展示。我在选题目的时候，没有料到这个办法是如此之菜，尽管它的物理出发点非常好（community detection 的每一种算法看上去都很平凡，或者说都有很自然的动机，我在想，这种问题没有一个简单的物理模型实在是太可惜了——而Bornholdt和Reichart在2004年发在prl上的工作（和06年pre上更详尽的后续）或许可以看作是这个方向的一个非常不错的尝试——至少在物理上是这样），但是它太慢了。无论如何，他们的思路就是用community的大概的概念（内部连接多，外部少）写一个哈密顿，然后通过几个假设，把这个哈密顿和modularity连接起来。然后通过对spin glass基态的分析来探究被partition的这个网络的一些性质和结构。

bornholdt现在应该是在不莱梅大学，他们实验室特别喜欢用ising-ish models去解释一切东西，看着都挺有意思的，比如说金融市场的ising model分析，传染病的ising model分析，等等。bornholdt，出人意料而又十分合理地，是sneppen的学生。这就使得bornholdt不可避免地被看做是bak学派（国内这个学派的代表人物是汤超，虽然汤老师已经转行了，但是精气神还在）的一员了。他们这群人做的东西我都十分喜欢。

关于bornholdt的事情有两个议论。第一个是关于统计力学现在的精神的问题。我认识的人中，有人觉得现在stat mech学界（特别是欧洲人）毫无节制地拿几个模型去套一切现象。比如说自旋玻璃，代表了一种interacted system里面的rugged landscape，于是万物都可以SG，DNA也可以SG，网络聚类也可以SG，这种做法就像是拿着锤子看啥都像钉子。还有人是对一个特定的研究发表看法，觉得单凭一两个exponent就判定某一实验现象属于KPZ、KPZQ universality class这种做法值得商榷（当然，这是在某课作业中憋出来的一项批评，况且他本行是天体物理）。我对于统计力学基本上只有一些很浅显的看法，而且很多知识也不懂，但是对于这种说法我也可以做一点评论。首先，universality class这个东西本来就是拿critical exponent定义的，不管这个概念本身虚无缥缈或well defined与否，我的感觉是统计力学本身就有一种universality class驱动的倾向。这个倾向究其渊源在哪里呢？我认为其渊源在中心极限定理。中心极限定理给了我们一个信念，就是说哪怕底层的规律不一样，但是只要满足某种类型的特征，那么大量粒子构成的系统就会表现出相同的行为。包括boltzmann分布——比如说我们理解正态分布是一堆无边界的随机行走叠加出来的，那么我们在左边卡一个边界，就会得到boltzmann分布。不管是boltzmann还是正态分布都能够把很多不同的model统一到一套处理方法之下，这就是我所理解的universality的概念。至于为什么存在这么多universality，这是数学家关心的问题，我听说有数学家靠研究KPZ方程拿fields的，说明数学家的确关心这个问题。当然后面人们用scaling invariance定义这个universality，把universality的特征表示成临界指数，这是更深的一层理解了。总而言之，我是在为这种拿着锤子找钉子的行为辩护的，本来这就是这门学科的传统，对于一次能砸中尽可能多的真钉子的追求也是这门学科进步的原动力。

第二个议论是关于一个很有意思的事实，也就是bornholdt这个人被引次数最多的一篇文章居然是这篇pre上关于community detection algorithm的文章。他被引数目有一万多，算是很有影响力的物理学家了（他教职是理论物理学教授），而他被引次数最多的文章是他很少的关于CS的文章之一——当然，这篇文章写得很漂亮，但这也侧面表明CS领域确实发了很多文章。我讲完这个展示之后和人出去恰饭，就提到CS领域发文章的灌水的事情，比如说先写abstract后写文章这种事，有点不可思议。我就说实际上这个是学科之间的一种metabolic tradeoff，搞CS的有点像专心发酵的细胞，nutrient flux非常大，但是nutrient进来之后产ATP的效率就低。搞物理的（材料：我不是物理？）像是专心呼吸的细胞，nutrient flux被限制，但是一套流程走下来产ATP的效率就比较高。那么我们就要问，说为什么会分别选择不同的代谢策略呢？生态学告诉我们说，如果系统营养充足，那么fermentation-respiration tradeoff可以用公地悲剧来描述，最后大家都会走向fermentation，体现在社会学上就叫做“人人有功练，人人转CS”。实际上，越是理论化的方向，低垂的果实越少，营养越少，那么靠呼吸作用养活自己的人就越多。当然，前提是方向得好，像我这种刚开始做劝退方向的，天天感觉低垂的果实一大把，但是摘了也没用啊。

其实CS领域是马克思主义社会学的一个非常好的体现。最主要是怎样体现的呢？在CS里面生产力对于生产关系的影响简直是立竿见影的。我对于CS非常不了解，但是像www一出现，生产力立刻大发展，像git这种生产关系的直接物化立刻就出现，并且迅速反哺其他行业，这就是一个例子。GPU大发展，CS几个领域立刻跑马圈地，文章增多，于是搞活了arxiv，产生了medium，包括我们已经习以为常的开源的概念，这些都是CS的生产力大发展带来的新型生产关系。如果将来建设成功了社会主义，那我们都要感谢CS。