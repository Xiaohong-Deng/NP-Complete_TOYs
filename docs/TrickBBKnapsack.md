besides linear relaxation what are the things we can apply to BB to optimize the algorithm.

### Tricks to Branch and Bound to 0-1 Knapsack
+ Order the items in descending order of density, not necessarily the best choice for all datasets.
+ Use a greedy subroutine to set an alternative lower bound, extra computation with the benefit of speeding up convergence.
+ There are various heuristics for searching strategy such as dfs or bfs. I used dfs. Not necessarily the best choice for all datasets.
### Pitfalls and Heads up
+ Keep track of `level`, this variable tells us exactly which items have been considered and which are not. Combined with `taken` list, it guides us in `relaxation` and `greedy` subroutines, making sure the correct items to be stacked up to form the upper bound.
+ Encapsulate key values in a node of the binary tree. Use a stack to store the nodes to simulate recursion.