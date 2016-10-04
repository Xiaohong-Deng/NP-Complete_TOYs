## Branch and Bound for 0-1 Knapsack
这个方法的优势(相对于exhaustive approaches)在于对search space的pruning, 如果pruning效果差这个方法就没有优势.

大局观上可以视为整个搜索过程有一个全局lower bound和每个node上各自有的upper bound, 如果upper bound小于lower bound就可以prune掉后续nodes;所以B&B的核心是尽可能地收紧每个node上upper bound-lower bound interval. 最原始的情况下, 每个node上的partial solution也就是背包内物品的value是node的lower bound, 和全局lower bound比较决定是否更新, 而包内item和还未考虑的item的value sum是分支上的upper bound; 在此基础上可以增加计算量来收紧interval达到提升pruning效果的目的(trade off); 比如可以初始以greedy解作为全局lower bound, 每个node上重新计算greedy, 一旦等于该node upper bound就不再往下搜索, 这也是一种pruning.

bounding function是计算upper bound的主要方法, 这个[umu page](https://www8.cs.umu.se/~jopsi/dinf504/chap13.shtml)给出一种对linear relaxation的改善,可惜大概是印错了可以轻易举出反例, 不能产生有效的upper bound;其目的是尽可能抛弃高密度高weight的item,在最后取partial item的时候不取, 往后找一个能完全放入sack的item, 然后再回到取partial的道路上来,一种可能的取法是111100011+partial, 第5个item要取partial而不取直到第8个才找到一个能完全放进sack的item, 第10个取partial后结束;如何校正这个方法使它有效呢?

反例是W=100, v1=4, w1=2, v2=100, w2=99, v3=1, w3=1

##Greedy as a Heuristic
Greedy经过tuning可以在NP问题上展示出惊人的效率, 而DP由于其exhaustive的本质似乎不能, 尽管DP还有一些我不知道的timing上的蜜汁优化

###Greedy for TSP
你可以用简单的triangle ineuqality证明, euclidean平面下的tsp的最优解是不可能有crossing edge的, 那最优解在circle里找就好了, 这就prune掉了一大堆candidates

这个方法是从一个很厉害的, 上海南洋理工-新加坡南洋理工计算学博士那里偷师来的一个方法, 伪码如下

    1. Order the nodes randomly and rename them as 1,2,...,n according to their ordering position
    2. Initialize tour T by involving only node 1 (i.e., self-loop on node 1)
    3. For each node i (i=2,3,...,n)
       probe-and-insert node i to T by making smallest increment of T.length
    4. Return T.length


for step 3

    Probe-and-Insert-by-Smallest-Increment(node i, tour T)
    probe every edge of T and find out (u,v) s.t. min{distance(u,i) + distance(i,v) - distance(u,v)}
    delete edge (u,v) in T
    add edges (u,i) and (i,v) to T

这和任何fancy的heuristic一样, 是不保证全局最优解的, 需要randomness来跳出local optimal; 问题是我都不知道给定一个order, 这方法一定能给出local optimal? 或者用完greedy后再local search, 那么greedy也就退化成和在B&B for knapsack一样, 成为一个boost
###Greedy for Graph Coloring

四色定理只适用于planar graph<sup class="reference">[1]</sup>, 即crossing edges之间必有intersection, 给出一个有crossing edge而crossing edge之间没有交点的graph, 也许你能morph它成一个no crossing edge graph, 也许不能.请问给出一个这样的图, 想知道它最终能否morph成一个没有crossing edge的图,这是一个NPC问题吗?

##References

1. Mathematics for Computer Science