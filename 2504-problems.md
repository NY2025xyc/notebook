[TOC]

### [[NOI2011] 阿狸的打字机](https://www.luogu.com.cn/problem/P2414)

离线下来做。  
$x$ 是 $y$ 的子串，转换为前缀的后缀，$y$ 的前缀即为 $\text{trie}$ 上祖先链，$x$ 是某个点的后缀则 $x$ 在 $\text{fail}$ 树上为该点的祖先。  
因此扫描线在 $\text{fail}$ 树上标记 $y$ 在 $\text{trie}$ 前缀上的所有点，而后查询 $x$ 的 $\text{fail}$ 子树内有几个标记即可。

### [Legen...](https://codeforces.com/problemset/problem/696/D)

> 虽然说 $\text{dp of dp}$ 本质上就是 $\text{DFA-dp}$，但这么思考自动机上 $\text{dp}$ 或者 $\text{dp of dp}$，可能就像用费用流思考带悔贪心，有点绕。  
> 所以这道题还是放到这儿吧。

关键点上记录权值 $a_i$ 的和，在 $\text{ACAM}$ 上 $\text{dp}$ 走到一个关键点时加上对应权值，并对走到这个点的所有转移取 $\text{max}$。  
由于 $+$ 对 $\max$ 有分配律，因此可以通过矩阵乘法优化 $\text{dp}$。

### [[JSOI2009] 有趣的游戏](https://www.luogu.com.cn/problem/P6125)

建出 $\text{trie}$ 图，转换成在自动机上的随机游走模型。  
发现考虑正推是不好做的，但是 $\text{trie}$ 图上后继状态很容易确定，因而考虑概率倒推。  
这样方程就很好设了，根据 $\text{trie}$ 图上期望转移的权值设出方程高斯消元即可。

### [[SDOI2017] 硬币游戏](https://www.luogu.com.cn/problem/P3706)

发现是前一题的加强。找规律太玄学了，考虑主元法。  
概率正推的转移为

$$
P(u)=\frac{1}{2}\sum_{v\mid<v,u>\in \text{Edge}}P(v)
$$

发现每个 $u$ 转移内除了一个 $\text{trie}$ 树上的父亲，其余点在 $\text{trie}$ 上的深度均大于 $u$。  
移项，表示出 $u$ 的父亲

$$
P(f_u)=2P(u)-\sum_{v\mid<v,u>\in\text{Edge}\wedge v\not=f_u}P(v)
$$

将每个关键点设成未知数，当两个关键点走到它们的最近公共祖先时，可以列出一个方程，这样可以列出 $n-1$ 个方程。  
又有 $\sum_{u\in\text{Key}} P(u)=1$，凑成 $n$ 个方程。

### [Boring Problem](https://codeforces.com/gym/103119/problem/B)

和前两题不一样的是，这道题需要算到达关键点的步数期望，因此关键点的答案就是 $E(u)=0$，相当于需要在关键点处列出相应的方程。  
需要正推，此时节点 $u$ 边指向的节点，只有深度小于 $u$ 的与 $u$ 的儿子，那么当 $u$ 只有一个儿子时，期望的转移是确定的，否则由于 $\text{fail}$ 边的存在，不能直接确定所有儿子的信息，需要设元。  
发现在 $u$ 处设元表示不出所有儿子，因此当 $u$ 有 $k$ 个儿子时，选取其中 $k-1$ 个设元，可以仿照上题推出剩余的儿子 $v$ 被其余节点表达的式子。  
因为

$$
E(u)=(\sum_{v\mid <u,v>\in\text{Edge}}p(u,v)\cdot E(v))+1
$$

所以有

$$
E(v)=\frac{E(u)-\sum_{w\mid<u,w>\in\text{Edge}\wedge w\not=v}E(w)-1}{p(u,v)}
$$

构造出系数矩阵按照 $\text{bfs}$ 序转移，$-\frac{1}{p(u,v)}$ 的常数移项到增广矩阵内即可。
