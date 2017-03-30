下面是小顶堆
```C++
#include<bits/stdc++.h>
using namespace std;

struct Node {
	int x, y;
	Node(int _x, int _y): x(_x), y(_y) {
	}
};

struct cmp{
	bool operator()(Node &a, Node &b) {
		return a.x > b.x;
	}
};

int main() {
	priority_queue<Node, vector<Node>, cmp> pq;
	pq.push(Node(2, 3));
	pq.push(Node(1, 2));
	pq.push(Node(3, 1));	
	while (!pq.empty()) {
		cout << pq.top().x << ", " << pq.top().y << endl;
		pq.pop();
	}
	return 0;
} 
```

如果要定义大顶堆，就让cmp函数返回<
