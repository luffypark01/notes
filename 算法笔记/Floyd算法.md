# floyd算法(多源最短路径) python实现



Floyd(弗洛伊德)算法用来找每个点两两之间的最短距离（多源[最短路径](https://so.csdn.net/so/search?q=最短路径&spm=1001.2101.3001.7020)），是典型的动态规划

原理很简单：
一个点 i 到另一个点 j 的最短路径无非有两种：

1. 直接到达( i --> j )
2. 通过走其他点(k1, k2 … kn)，一个或多个点到达（ i --> k1–>k2–> … --> j )

**所以找最短路就直接 比较 1 和 2 哪条路径更短**

[数据结构](https://so.csdn.net/so/search?q=数据结构&spm=1001.2101.3001.7020)：邻接矩阵(n*n) graph, graph[ i ][ j ]表示 i --> j 的最短路径。
初始化：一开始全设为无穷大，如果 i == j , graph[ i ][ j ] == 0，然后遍历所有图中的所有边 i --> j ，令 graph[ i ][ j ] = 边权

要找到 i 到 j 的最短路径，我们需要 i 去经过另外的结点再走回 j（i --> k --> j），看和原路径（i --> j）谁更短，然后更新graph[ i ][ j ]。要得出这个结果就得每个结点都走一遍。

这里作个演示。比如我们可以先只经过编号为 1 的结点，比较 i --> j 和 i --> 1–> j，哪条路小，然后更新 i --> j的最短路径
graph[ i ][ j ] = min ( graph[ i ][ j ]， graph[ i ][ 1 ] + graph[ 1 ][ j ] )

```stylus
for i in range(n):
	for j in range(n):
		graph[i][j] = min(graph[i][j], graph[i][1] + graph[1][j])
```

同理经过其他点，比如编号为2的点

```stylus
for i in range(n):
	for j in range(n):
		graph[i][j] = min(graph[i][j], graph[i][2] + graph[2][j])
```

最后我们每个点都要经过：

```stylus
for k in range(n):
	for i in range(n):
		for j in range(n):
			graph[i][j] = min(graph[i][j], graph[i][k] + graph[k][j])
```

是不是很简单。。。核心代码就只有4行代码。。。

因为每次 i --> j 的路径都更新为最优的而且都被保存下来，所以后面的结点要通过这个结点去往别的结点，就能保证是最优的。

例子：

图片来自：https://www.cnblogs.com/wangyuliang//9216365.html

这篇博客里面讲的更详细

![img](img/20190626201419358.png)

如果要打印路径，我们就需要用一个二维数组parents记录每个结点的父结点。在找最短路的时候，更新父结点。最后递归寻找父结点，回溯打印。

```clean
import sys

sys.setrecursionlimit(100000000)


# 弗洛伊德算法
def floyd():
    n = len(graph)
    for k in range(n):
        for i in range(n):
            for j in range(n):
                if graph[i][k] + graph[k][j] < graph[i][j]:
                    graph[i][j] = graph[i][k] + graph[k][j]
                    parents[i][j] = parents[k][j]  # 更新父结点


# 打印路径
def print_path(i, j):
    if i != j:
        print_path(i, parents[i][j])
    print(j, end='-->')


# Data [u, v, cost]
datas = [
    [0, 1, 2],
    [0, 2, 6],
    [0, 3, 4],
    [1, 2, 3],
    [2, 0, 7],
    [2, 3, 1],
    [3, 0, 5],
    [3, 2, 12],
]

n = 4

# 无穷大
inf = 9999999999

# 构图
graph = [[(lambda x: 0 if x[0] == x[1] else inf)([i, j]) for j in range(n)] for i in range(n)]
parents = [[i] * n for i in range(4)]  # 关键地方，i-->j 的父结点初始化都为i
for u, v, c in datas:
    graph[u][v] = c	# 因为是有向图，边权只赋给graph[u][v]
    #graph[v][u] = c # 如果是无向图，要加上这条。

floyd()

print('Costs:')
for row in graph:
    for e in row:
        print('∞' if e == inf else e, end='\t')
    print()

print('\nPath:')
for i in range(n):
    for j in range(n):
        print('Path({}-->{}): '.format(i, j), end='')
        print_path(i, j)
        print(' cost:', graph[i][j])

# 最终的graph
# graph[i][j]表示i-->j的最短路径
# 0	2	5	4
# 9	0	3	4
# 6	8	0	1
# 5	7	10	0
#
# Path:
# Path(0-->0): 0--> cost: 0
# Path(0-->1): 0-->1--> cost: 2
# Path(0-->2): 0-->1-->2--> cost: 5
# Path(0-->3): 0-->3--> cost: 4
# Path(1-->0): 1-->2-->3-->0--> cost: 9
# Path(1-->1): 1--> cost: 0
# Path(1-->2): 1-->2--> cost: 3
# Path(1-->3): 1-->2-->3--> cost: 4
# Path(2-->0): 2-->3-->0--> cost: 6
# Path(2-->1): 2-->3-->0-->1--> cost: 8
# Path(2-->2): 2--> cost: 0
# Path(2-->3): 2-->3--> cost: 1
# Path(3-->0): 3-->0--> cost: 5
# Path(3-->1): 3-->0-->1--> cost: 7
# Path(3-->2): 3-->0-->1-->2--> cost: 10
# Path(3-->3): 3--> cost: 0
```

