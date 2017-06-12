# 常用算法和数据结构

## 树
 - 二叉树先序、中序、后序遍历的递归+非递归实现

## 排序
 - 常用排序算法时间复杂度等
 - 手写快排

## 字符串
 - KMP

## 图
n表示节点数量
```
算法名称           时间复杂度       空间复杂度

dijkstra+heap     O(Vlog(V) + E)    O(V)

bellman-ford      O(VE)             O(V)

spfa              O(kE)(最差O(VE))  O(V)

floyd-warshall    O(V^3)            O(V^2)
```
1. 当权值为非负时，用Dijkstra。
2. 当权值有负值，且没有负圈，则用SPFA，SPFA能检测负圈，但是不能输出负圈。
3. 当权值有负值，而且可能存在负圈，则用BellmanFord，能够检测并输出负圈。
