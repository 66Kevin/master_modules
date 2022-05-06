## Week1 Combinatorial Optimisation

组合优化问题

### Complexity

时间复杂度是指在 问题的规模扩大以后，程序求解所须要的时间**增加的**有多快

$O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n) < O(n!) < O(n^n)$

- 前面的$O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) $是**多项式时间polynomial-time**

- 后面$O(2^n) < O(n!) < O(n^n)$是**指数时间**

### P / NP / NPC / NPH

- P问题：能在多项式时间内解决的问题就是P问题
- NP问题：能在**多项式时间验证**，但不确定能不能在多项式时间内解决
  - P问题一定是NP问题(能解决就一定能验证)；相反，**NP问题是不是P问题还未解决**，世界难题
  - 比如*哈密顿回路*问题：在一个有多个城市的地图网络中，寻找一条从给定的起点到给定的终点沿途恰好经过所有其他城市一次的路径。哈密顿回路问题是一个NP问题，人们往往要通过穷举这样笨拙的方法寻找问题的解，然而，显然我们可以在多项式时间内判断一个既定的解是否是正确的。
- NP- Complete问题：1.如果一个问题Q是NP问题，并且2.所有的NP问题都可以归约成Q，则Q是NPC问题
- NP-Hard问题：若所有的NP问题都能归约到问题X，**X的复杂度大于等于原NP问题**，那么X就是NP-Hard问题。**但问题X不一定是NP问题**

简单地说，一个问题A可以归约为问题B的定义是，可以用问题B的解法解决问题A。也就是说，问题A可以“变成”问题B。书上对这个定义举了一个非常清晰的例子，比如二元一次方程 $ax^2 + bx + c = 0$可以归约为一个一元一次方程，只要我们把二次项系数a设置为0即可，即$0x^2 + bx + c = 0$那么我们就可以说，可以使用二元一次方程的解法来解决一元一次方程，因此一元一次方程可以归约成为二元一次方程。

若一个问题A可以归约为问题B，则我们说问题B至少和A一样难，并且可能比A还难。上述例子中，问题A就是一元一次方程，问题B就是二元一次方程。显然，二元一次方程要比一元一次方程难解。

**那么如果我们能找到一个n元一次方程的解法，也就意味着我们找到了所有m元一次方程（m < n）的解法。NPC就是运用了这种思想：**如果所有的NP都可以归约成一类问题，而我们找到了这类问题的多项式时间解法，那么我们就可以找到所有NP问题的多项式时间解法， 也就证明了P=NP。这一类问题，就是NPC问题。换句话说，NPC问题可以理解为科学家们为了证明或证伪P=NP时创造的一套理论。

我们把众多的NP问题层层归约，必定会得到一个或多个“终极问题”，这些归约的终点就是所谓的**NPC问题**（NP-complete）,因此NP问题远比P问题要多，而NP之中的NPC问题也仅占极少数

NP难问题（NP-hard）是一类非常难的问题。它的定义比NPC问题少了一个条件，即它不一定是NP问题。也就是说，对于一个NP难问题，我们非但没办法找到多项式时间的解法，甚至没办法在多项式时间内验证它的解。比如上文中提到的“是否有一条路径长度小于等于15，且覆盖所有城市”是一个NP问题，但如果我们把问题改成“是否不存在一条路径长度小于等于15，且覆盖所有城市”，那么它就是一个NP难问题，因为除非我们遍历所有的路径，否则我们不能说“不存在这样的路径”。那么这样的话我们就没办法在多项式时间内验证一个解。因此这个问题是NP难问题。

### SAT 问题

SAT(Satisfiability problem)：可满足性问题

- SAT问题：SAT问题就是问某一个布尔表达式是不是“可满足”的问题。这里的术语“可满足”的意思是存在一组“真值赋值”(truth assignment)使得布尔表达式为真。举例来说，“a & !b”(a与非b)就是一个"可满足"的布尔表达式，因为存在一组真值赋值(a=1,b=0)使其为真
- 计算机领域大部分复杂的NP问题都可以在多项式时间内归约为SAT问题，并基于SAT算法求解
- SAT算法作用：假定公式以转化为合取范式（CNF），SAT算法要求在有限的时间内判定命题逻辑是否可满足。
- 直接办法之一：暴力穷举法（构造真值表法）

### 3-SAT和Max-SAT问题

3-SAT：

举个例子：小红，小王，小江等n人想找个地方去做运动，他们觉得这个地方可能会有m种情况。为了尽量满足所有人的癖好，每个人都根据可能出现的情况提了一个包含3个需求的列表：小红想找个“有窗户”或者“有厕所”或者“没臭味”的地方，小王想找个“没窗户”或“有臭味”或“没厕所”的地方，小江想找个“有厕所”或“没臭味”或“没窗户”的地方。。。问，是否存在一个地方可以满足所有人的（至少一个）要求？

其中，每个人提的需求列表就是一个“子句”，比如小红提的 " ‘有窗户’或’有大床’或’没臭味’ " 就是“子句”。
需求列表中的每一项具体的需求都是一个“文字”，比如“有窗户”就是一个文字，“没臭味”也是一个文字。
需求的选项(可能出现的情况)就是“变量”，比如“窗户”就是一个变量，“厕所”是一个变量，“臭味”也是一个变量。
如果最后找到了一个“有窗户，没厕所，没臭味”的地方，那么“有窗户，没厕所，没臭味”就是变量的真值赋值。
所有人需求就是合取范式，如果“每个人的需求都有3项且至少要满足1项”就是“3合取范式”。

3SAT问题就是问 "是否存在"一个满足所有人需求的地方。



Max-SAT问题是经典的 NP-hard 问题 而SAT问题是NP-complete问题

MaxSAT 问题是 SAT 问题的延伸和扩展，其求解目标是找出一种赋值，使得被满足的子句数达到最多

### Tree Graph

树与图的关系：没有环的连通图叫做树

A tree is a connected graph连通图:

- Has no cycles
- has exactly one path between any pair of vertices
- breaking any edge disconnects the graph（断开任意一条边就会断开整张图）

Complete Graph完全图

- **每对不同的顶点之间都恰连有一条边相连**
- 完全图的edges数量：$\frac{n(n-1)}{2}$

Spanning Trees

- A complete graph on n vertices $K_n$ Has $n^{n-2}$Spanning trees
- DFS / BFS to find spanning trees

Minimum Cost Spanning Tree(MST)

最小生成树问题：在某地分布着`N`个村庄，现在需要在`N`个村庄之间修路，每个村庄之前的距离不同，问怎么修最短的路，将各个村庄连接起来

- Prim's：从任意一个顶点开始，每次选择一个与当前顶点集最近的一个顶点，并将两顶点之间的边加入到树中

- Kruskal's：所有边取出按边权排序，这时图中只有点，没有边，然后从最小的开始再放回图中，**放入后不成环就可以留下（有环就不是树了）**，不然就拿走，就这样一直放，直到放进去了n-1条边，结束

- Prim和Kruskal：

  - Prim和Kruskal都是贪心法，选择当前最佳，也就是“局部最优”
  - *Prim*算法是逐个找，而Kruskal是先排序再找

  - 使用**邻接矩阵**作为存储结构的Prim算法的时间复杂度为$O(V^2)$，如果使用**二叉堆与邻接表**表示的话，Prim算法的时间复杂度可缩减为O(E log V),其中E为连通图的边数，V为顶点数。
    如果使用较复杂的斐波那契堆，则可将运行时间进一步缩短为O(E + V log V)，这在连通图足够密集时(边较多，即E满足Ω（V log V）)可较显著地提高运行速度。
  - Prim's:$O(|V|^2)$ 使用**二叉堆与邻接表**可缩减为$O(|E| log |V|)$
  - Kruskal's:$O(|E| log |V|)$,其中E为连通图的边数，V为顶点数

  - **prim算法适合稠密图**，其时间复杂度与边得数目无关，而kruskal算法的时间复杂度为O(elogV)跟边的数目有关，**适合稀疏图**。
  - 如果**边的权重不重复**，则Prim和Kruskal的结果相同

- <img src="/Users/kevin/Library/Application Support/typora-user-images/image-20220505005347397.png" alt="image-20220505005347397" style="zoom:50%;" />

## Week2 TSP - Branch and Bound, Approximation Methods

### Travelling Salesman Problem

旅行商人问题：**一个旅行商人需要拜访n个城市，要选择一个路径能够拜访到所有的城市，且每个城市只能被拜访一次，最终回到起点，求得的路径路程为所有路径可能之中的最小值。**从图论的角度来看，该问题实质是在一个带权完全无向图中，找一个权值最小的Hamilton回路（Hamilton回路是这样的：给你一个图，问你能否找到一条经过每个顶点一次且恰好一次最后又走回来的路）。

**NPC问题**，这类问题暴力搜索是一种直接求解的方法，我们只需要穷举所有的路线可能，必定能够将解精确到最优。然后当城市数目较多，路网结构复杂时，这样的方法(O(n!))显然是不经济也不现实的。这时候，能在伪多项式时间内得出**近似最优解的算法**就派上用场了。

- Non-metric TSP: Graph representation
- Metric TSP: Space representation
  - Metric TSP satisfies the **triangle inequality**

#### Christofide's Algorithm

https://www.docin.com/p-1590423137.html

Christofides 算法由双生成树算法改进而来，使最坏解在最优解的1.5倍之内，而双生成树算法的最坏解在最优解的2倍之内

- 2-Approximation Algorithm

  将TSP问题转化为满足三角不等式的模型，即欧拉平面内任意一个三角形的两边之和大于第三边，在TSP问题中为**如果一条路径经过了某个中转站，则它不可能具有比直接到达更小的代价**。当费用函数满足三角不等式时，近似算法找出的哈密顿回路产生的总费用和实际近似解费用的比值不会超过2（算法求的解不超过最优解的2倍）。

  - 计算MST
  - DFS获取a cycle C of the tree by doubling the edges
  - Obtain a $C'$ from C by removing duplicate nodes（**利用三角不等式**，如果一个顶点已被访问，则直接访问下一个顶点）
  - 最优解 <= $C'$ <= 2MST （因为DFS会走两遍，最差的情况是走两遍回到终点）

- 1.5-Approximation Algorithm **(Christofide's Algorithm)**

  https://blog.csdn.net/meiyoushui_/article/details/114949295

  - Step 1：构造图的最小生成树T
  - Step 2：O为在最小生成树T上度数为奇数的顶点集，则O中有偶数个顶点
  - Step 3：构造点集O在原完全图上的最小完全匹配M（Minimum cost matching among nodes in O）
    - 完全图：每对不同的顶点之间都恰连有一条边相连
    - 匹配：是指图中，任意两条边都没有公共的顶点，这时每个顶点都至多连出一条边
    - **最小完全匹配：是连接边总长度最小的两两连接**，在node中找到链接边长最短的组合
  - Step 4：将M和T的边集取并，构造重图J，将满足其每个顶点都是偶数度的
  - Step 5：J可以形成一个欧拉回路E
    - 欧拉回路：从起点经过图中所有的边又回到出发点，且每条边只经过一次
  - Step 6：去除欧拉回路E中的重复点，形成回路解（**利用三角不等式**，如果一个顶点已被访问，则直接访问下一个顶点）

  1. **最小生成树长度L(T)<=TSP最优路线L(I)**：因为最优路径去掉一条边就是一个生成树，肯定不可能比最小生成树长度还要小

  2. 

### 0-1 Knapsack

01背包问题是**NP-hard**的，我们无法在多项式时间求到它的最优解。但是我们可以在多项式时间内求到它的一个可行解

#### Branch and Bound Method

https://www.bilibili.com/video/BV1Ct4y1X7T9/?spm_id_from=333.788.recommend_more_video.3

- 按**性价比**（单位重量价值降序排序）排序
- 确定上界u函数：该节点最多可以得到的价值
- 如果u < bestP，则可以剪枝。因为如果该节点最多可以得到的价值小于bestP意味着没有必要往下进行，则可以剪枝

## Week3 Complexity Classes

### Combinatorial Optimisation Problems(COP) & Decision Problems(DP)

- Shortest Path问题
  - COP：问题转换成find the path from s to t with minimum cost
  - DP：问题转换成是否有一条从s到t的最短路径
- Travelling Salesman Problem
  - COP：问题转换成找到最小cost的哈密顿回路(每个node只访问一次)
  - DP：问题转换成是否存在一条哈密顿回路**（NPC问题）**

### Complexity Classes

- 象棋决策问题不属于NP问题，因为没有一种有效的算法可以检查给定的棋盘是否有效。

- 魔方决策问题属于NP问题，因为判断一个给定的魔方是否是一个解是很简单的

## Week4 Randomness and Local Search

### Randomness

- Witness问题：想要检验两个database内容是否一致

  - Need to change n bits to verify
  - A randomized Protocol that uses $4[log_2n]$ bits
    - 思路是用两个sequence x和y里面都是0或1的数字，然后用【指纹】来判断是否相等
    - $Number(x) = \sum_{i=1}^n 2^{n-i}x_i$，$Number(y) = \sum_{i=1}^n 2^{n-i}y_i$，$PRIM(m)$是小于m所有质数的集合
    - A随机从$PRIM(n^2)$中随机选择一个prime，R1计算`s = Number(x) mod p`并把s和p发送给R2，如果R2计算的结果与R1相同，则通过
    - 但是，8928 mod 23 = 4，8951 mod 23 也等于4，所以如果R2的数据为8923+k*23就会出错
    - 出错的概率 = $\frac{p不能将R1R2分开的次数}{Prim(n^2)}$  <= $\frac{2lnn}{n}$（其中**n>=9时才成立**）

- Primality testing

  检验一个数是否是质数（只能被1和本身整除的数，其余的为合数）

  - Fermat's Theory费马定理：if $p$ is prime then $a\in\{1,...,p-1\}:a^{p-1} = 1$mod p。但是费马定理不是每次都成立的，多测几次只要存在某个$a^{p-1}$ != 1 mod p就说明p不是素数
    - $a^{p-1}$ 不等于 1 mod p：p不为prime一定成立；
    - 相反$a^{p-1}$ 等于 1 mod p：p为prime不一定成立 ，也可以说费马定理检测素数可能会出错
  - Miller Rabin Test：在费马定理的基础上改进，**但同样并非一定正确**但错误出现率小到可以接受
    - Miller-Rabin是随机算法，如果对这个过程重复100次，每次都没说它是合数，那这个数是素数的概率只有(1/2)^5100可能不是素数
    - 对于待测数n，当找到的a不符合条件，那么称a为一个"witness for the compositeness of n"，且n一定是一个合数

- Approximation Algorithm

  - 1.5-approximation for metric TSP

  - Polynomial Time Approximation Scheme(PTAS)

    https://www.youtube.com/watch?v=ykjS8ZWh1SQ

- 哈密顿回路

  - 一个有n个顶点的complete graph有$\frac{1}{2}(n-1)!$个哈密顿回路

### Local Search

- Hill Climbing
  - 随机选择一个位置爬山，每次朝着更高的方向移动，直到到达山顶，即每次都在临近的空间中选择最优解作为 当前解，直到局部最优解
  - 缺点：
    - local optima： 局部最大一般比状态空间中全局最大要小，一旦到达了局部最大，算法就会停止
    - Plateau： 平顶是状态空间中评估函数值**基本不变的一个区域**，在某一局部点周围f（x）为常量。一旦搜索到达了一个平顶，搜索就无法确定要搜索的最佳方向，会产生随机走动，这使得搜索效率降低
    - Ridge： 山脊可能具有陡峭的斜面，所以搜索可以比较容易地到达山脊的顶部，但是山脊的顶部到山峰之间可能倾斜得很平缓，搜索的前进步伐会很小。

## Week5 Neighbourhoods Functions

很像遗传算法里的突变，通过创造新的case来达到更好的效果，是一种近似算法（Approximate algorithms）

局部搜索会先从一个初始解开始，通过**邻域动作**。产生初始解的**邻居解**，然后**根据某种策略选择邻居解**。一直重复以上过程，直到达到终止条件。
不同局部搜索算法的区别就在于：**邻域动作的定义**以及**选择邻居解的策略**。这也是决定算法好坏的关键之处。

**邻域动作：**一个函数，通过该函数，针对当前解s，产生s对应的邻居解的一个集合

- Local search in MAX-3-SAT Santa
  - flip a bit

- Local search in Symmetry TSP
  - 1. 改变两个连续node的顺序
    2. replacing 2 non-consecutive edges with 2 new edges
    3. replacing 3 non-consecutive edges with 2 new edges

- Local search in Weighted Graph Bisection Problem(WGBP)
  - Swapping node between L and R
- **Construction Heuristics**: produce feasible config rather than choose randomly
  - Nearest neighbour tour: Proceed from the current city to the nearest unvisited city
    - $O(n^2)$
  - Insertion heuristics: start with a triangle, add the city that increase the total length at minima one by one
    - $O(n^3)$

## Week6 Simulated Annealing

一种Meta-heuristic元启发式算法

**什么事Meta- heuristic算法**：Meta 代表其比一般启发式算法在搜寻能力上更为高阶。而 heuristic 则代表其算法能够在一个合理的计算成本内找到一个接近真实最佳解的解，但启发式算法并不能够保证其解的可行性与最佳性。通常是使用大量的试误以在庞大的解空间中搜寻最佳解。而**元启发算法旨在全域搜索与区域搜索中取得权衡**，若算法着重区域搜索能力则容易落入区域最佳解陷阱，若着重全域搜索则可能无法收敛解。



一句话总结：**高温阶段优化搜寻单元更加活跃，容忍大，即使非优解也愿意接受；随着温度降低，容忍度降低，越来越抵触非优解。以一定的概率来接受一个比当前解要差的解，因此有可能会跳出这个局部的最优解，达到全局的最优解**

- 初始阶段，使用较高温度跳出local optima，尽量去寻找global optima
- 后期，类似爬山算法，如果移动会让结果变得更好则移动，否则不动

伪代码：

```cpp
/*
* J(y)：在状态y时的评价函数值
* Y(i)：表示当前状态
* Y(i+1)：表示新的状态
* r： 用于控制降温的快慢
* T： 系统的温度，系统初始应该要处于一个高温的状态
* T_min ：温度的下限，若温度T达到T_min，则停止搜索
*/
while(T>T_min){
　　ΔE = J(Y(i+1)) - J(Y(i)) ;
 
　　if ( dE >=0 ) //移动后得到更优解，则总是接受移动
       Y(i+1) = Y(i) ; //接受从Y(i)到Y(i+1)的移动
　　else{
      if (exp(-ΔE/T)>random(0,1)) //否则以概率exp(-ΔT/T)接受S′作为新的当前解
      		Y(i+1) = Y(i) ; //接受从Y(i)到Y(i+1)的移动
　　}
　　T = r * T ; //降温退火 ，0<r<1 。r越大，降温越慢；r越小，降温越快
　　i ++ ;
}
```

参数分析：

- 初始温度T0：初温越大，获得高质量解的机率越大，但花费较多的计算时间
- 温度衰减系数r：若r过大，则搜索到全局最优解的可能会较高，但搜索的过程也就较长。若r过小，则搜索的过程会很快，但最终可能会达到一个局部最优值

## Week7 Genetic Algorithm

Meta-heuristic算法



Crossover两种方法：

1. Random split point k. copy first k bits from A and remaining `n-k` bits from B
2. Random split point k and k'. copy first k bits and last `n-k'`bits from A, and remaining from B	

Selection strategy:

- 轮盘赌：$\frac{fitness(A)}{sum fitness}$
- Rank Selection：种群中的个体首先根据适应度的值从小到大排序，然后给所有个体赋予一个序号，再按照轮盘赌计算概率
  - 比如有3个个体，适应度分别为：$f(h_1) = 2, f(h_2) = 1, f(h_3) = 3$，排序后$f(h_2), f(h_1), f(h_3)$,按照排序重新赋予fitness：$f(h_2) = 1, f(h_1) = 2, f(h_3) = 3$，计算概率：$p(h_2) = \frac{1}{6}$,$p(h1) = \frac{2}{6}$...
- Tournament Selection：整个种群中抽取n个体，让他们进行竞争，抽取其中的最优的个体
  - 总共：$C_n^m = \frac{n!}{m!(n-m)!}$

## Week8&9 Job Shop Scheduling

### Neighbourhood Function

在同一台机器上相邻的操作，对工件进行加工顺序的调整，因而产生新的调度，是求解JSP的一种方法。



## Week10 Ant Colony Optimisation, Partial Swarm Optimisation

### Ant Colony Optimization （蚁群算法）

每只蚂蚁在解空间独立地搜索可行解，解越好留下的信息素越多，随着算法推进，较优解路径上的信息素增多，选择它的蚂蚁也随之增多，最终收敛到最优或近似最优的解上。



**参数分析：**

- $\alpha$信息素因子：反映了蚂蚁运动过程中路径上积累的信息素含量在指导蚁群搜索中的相对重要程度
  - 过大：蚂蚁选择之前已经走过的路可能性较大，减弱了算法的随机搜索的能力
  - 过小：选择之前已经走过的路可能性较小，搜索范围过小容易过早的收敛陷入，局部最优
- $\beta$启发函数因子：反映了启发信息(距离)在指导蚁群搜索中的相对重要程度，**相当于先验信息的指导**
  - 过大：收敛速度的加快，容易陷入局部最优
  - 过小：纯粹的随机搜素，很难找到最优解

优点：

- Parallel Methods：每只蚂蚁搜索的过程彼此**独立**，仅通过信息激素进行通信。在问题空间的多点进行独立的解搜索，不仅增加了算法的可靠性，也使得算法具有较强的全局搜索能力
- Feedback procedure accounts for good solutions：正反馈算法，蚂蚁能够找到最短路径，直接依赖于最短路径上的**信息素的堆积**，而信息素的堆积是个正反馈过程。正反馈是蚁群算法的重要特征。
- inherit structure更适合像TSP问题：与正反馈类似，每只蚂蚁都继承了上一只蚂蚁的优势

**缺点：**

- 容易陷入local optima：蚁群算法中初始信息素匮乏，蚁群算法一般需要较长的搜索时间；而且该方法容易出现停滞现象，即搜索进行到一定程度后，所有个体发现的解完全一致，不能对解空间进一步进行搜索，不利于发现更好的解。
- Probability distribution changes after each iteration：蚁群算法中α，β的选取往往是通过经验来取得的，而选取不当时会造成算法的性能大大降低
- convergence时间无法得知



流程：

1. 随机放置蚂蚁到各个城市
2. 选择下一个城市，**直到选择到最后一个城市（结束**）。
   1. 如果没有信息素，则平均概率
   2. 如果有信息素，计算$p_{ij}$进行轮盘选择
3. 更新信息素浓度