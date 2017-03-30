# 写法1
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
		return a.x > b.x;   //小顶堆
		return a.x < b.x;   //大顶堆		
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

# 写法2

```C++
#include<bits/stdc++.h>
using namespace std;

struct Node {
	int x, y;
	Node(int _x, int _y): x(_x), y(_y) {
	}
	//小顶堆	
	bool operator> (const Node& that) const {
		return x > that.x;
	}
	/*
	//大顶堆	
	bool operator< (const Node& that) const {
		return x < that.x;
	}
	*/
};

int main() {
	priority_queue<Node, vector<Node>, greater<Node>> pq;   //小顶堆 
	//priority_queue<Node, vector<Node>, less<Node>> pq;   //大顶堆 
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
