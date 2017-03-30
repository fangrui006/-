# heap优化的Dijkstra
Dijkstra的过程：
1. 初始化集合S，最短距离数组dist。
2. p <- src
3. 检查与节点p直接相连的节点q，判断是否可以更新dist[q]：如果可以更新，更新之
4. 在所有!S节点中找出dist最小的一个节点，加入集合S；
5. 将新加入S中的节点记为p，重复3~4过程

未优化的dijkstra算法时间复杂度是O(V^2)，因为上述算法描述的第4步中需要遍历整个dist数组找出最小值。我们可以用堆存储所有更新过的(dist, 节点id)。将算法的时间复杂度优化到O(ElogV)。

堆优化的算法流程：
1. 将[0, src]加入PQ，初始化dist数组
2. 从PQ中取出top结点
3. 检查所有与该节点相连的节点q，判断是否可以更新dist[q]；如果可以更新，更新之并且将更新后的[dist[q],q]加入PQ
4. 重复2~3过程，直到PQ为空。

注意：这个算法会遇到的状态：PQ中可能同时出现多个相同id的节点Pair(dist, id)，原因可以用下图解释:
```
  A ---1-->B
   \       |
     \4    |1
       ↘  ↓
          C
假设src是A，第一个我们更新了B和C的dist，并且都加入了PQ，此时PQ中有两个节点[1,B]和[4,C]。
下一次取出PQ的top，也就是B。更新与B相邻的节点C的dist，将C加入PQ，此时PQ中有两个节点[2,C]和[4,C]。
```
```C++
#include<iostream>
#include<utility>
#include<vector>
#include<queue>
#include<stdio.h>
using namespace std;

#define MAX 100010
#define INF 99999999
struct Edge {
	int id;
	int cost;
	Edge(int _x, int _y):id(_x), cost(_y) {
	}
};
vector<Edge> map[MAX];

typedef pair<int, int> P;   //first:最短距离  second: id 
priority_queue<P> pq;
vector<int> dist(MAX, INF);

void dijkstra(int src) {
	dist[src] = 0;
	pq.push(P(0, src));
	while (!pq.empty()) {
		P p = pq.top();
		pq.pop();
		int from = p.second;
		for (int i = 0; i < map[from].size(); i++) {
			Edge e = map[from][i];
			int to = e.id;
			if (dist[to] > dist[from] + e.cost) {
				dist[to] = dist[from] + e.cost;
				pq.push(P(dist[to], to));
			}
		}
	}
}

int main() {
	int from, to, d;
	freopen("in.txt", "r", stdin);
	while (cin >> from >> to >> d) {
		map[from].push_back(Edge(to, d));
	}
	dijkstra(1);
	for (int i = 1; i <= 4; i++) {
		cout << dist[i] << endl;
	}
	fclose(stdin);
	return 0;
}
```
test case:
```
1 2 1
1 3 3
2 3 1
2 4 5
3 4 1
```
