# B. Tree Restoration
## 题目
给定一棵N个结点的树，编号从1到N。现在树中所有的边都丢失了，但是我们知道：
(1) 哪些结点是叶子结点
(2) 任意两个叶子结点之间的距离
(3) 每个结点的深度（root结点的深度是1）
(4) 对应每一层结点，该层节点的先后顺序
你能否根据以上信息恢复树中所有结点的边？注意到如果结点u出现在结点v的左边，那么u的父节点不会在v结点的右边。

输入：
第一行n,m,k。n表示树中结点总数，m表示树的高度，k表示叶子结点的数量
第二行包含M个整数A1~Am，Ai表示第i层结点的个数。
接下来有M行。其中第i行包含Ai个整数，分别是第i层从左到右的结点编号。
接下来一行有K个数L1~Lm，表示叶子结点的编号。
然后是一个K x K的矩阵D，其中Dij表示叶子结点Li和Lj的距离。
（1 ≤ N ≤ 100）
输出：
输出结点1~N的父节点编号。root结点的父节点编号为0

## 思路
从最后一层开始按层从底向上恢复父节点。全部恢复完一层所有结点的父节点，再恢复他们的上一层结点。
对于某一层，按照从右向左的顺序依次恢复父节点，如果还有父节点未确定，执行循环：
（1）（从最右边开始）找连续的结点序列，相邻结点的距离为2。这些结点共有父节点。他们的父节点就是上一层从右边开始的第一个非叶子结点
（2）更新这个结点序列的父节点，把这些结点标记为非叶子结点；
（3）更新所有其他叶子结点与父节点之间的距离为：其他叶子结点与该父节点孩子结点的距离
（4）把父节点标记为叶子结点，更新本层未确定父节点的结点个数
```C++
#include<iostream>
#include<cstdio>
#include<cstring>   //memset

using namespace std;

int n, m, k;
int a[110][110];   //记录每一层的所有结点编号 
int G[110][110];   //记录所有结点之间的距离 
int l[110];   //记录所有叶子结点的编号 
int s[110];   //s[i] = 1表示结点i是叶子结点，0表示不是叶子结点 
int fa[110];   //fa[i]表示结点i的父节点编号
int main() {
	memset(s, 0, sizeof s);
	memset(l, 0, sizeof l);
	memset(fa, 0, sizeof fa);
	for (int i = 0; i < 110; i++) memset(a[i], 0, sizeof a[i]);
	for (int i = 0; i < 110; i++) memset(G[i], 0, sizeof G[i]);
	scanf("%d %d %d", &n, &m, &k);
	for (int i = 1; i <= m; i++) {
		scanf("%d", &a[i][0]);
	}
	for (int i = 1; i <= m; i++) {
		for (int j = 1; j <= a[i][0]; j++) {
			scanf("%d", &a[i][j]);
		}
	}
	for (int i = 1; i <= k; i++) {
		scanf("%d", &l[i]);
		s[l[i]] = 1;
	}
	for (int i = 1; i <= k; i++) {
		for (int j = 1; j <= k; j++) {
			scanf("%d", &G[l[i]][l[j]]);
		}
	}
	for (int depth = m; depth > 1; depth--) {
		while (a[depth][0] > 0) {
			//找出距离为2的叶子序列 
			int now = a[depth][0];
			while (now >= 2 && G[a[depth][now]][a[depth][now - 1]] == 2) now--;
			//找上一层第一个非叶子结点 
			int father = a[depth - 1][0];
			while (s[a[depth - 1][father]] == 1) father--;
			//记录叶子序列的父节点为father结点，同时把叶子序列结点标记为非叶子结点
			for (int i = now; i <= a[depth][0]; i++) {
				fa[a[depth][i]] = a[depth - 1][father];
				s[a[depth][i]] = 0;
			}
			//更新所有其他叶子结点与father的距离
			for (int i = 1; i <= n; i++) {
				if (s[i])
					G[i][a[depth - 1][father]] = G[a[depth - 1][father]][i] = G[i][a[depth][now]] - 1;
			}
			//把father结点标记为叶子结点
			s[a[depth - 1][father]] = 1;
			//更新depth层未确定父节点的结点个数 
			a[depth][0] = now - 1;
		}
	}
	for (int i = 1; i < n; i++)
		printf("%d ", fa[i]);
	printf("%d\n", fa[n]);
	return 0;
}
```
