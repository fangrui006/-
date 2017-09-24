# G+ prep doc

## 0. algorithm complexity
 - Big-O

## 1. sort
[各种算法比较](http://my.csdn.net/uploads/201207/19/1342700879_2982.jpg)

### merge sort
 - 最好、最坏和平均时间复杂度：O(nlogn)
 - 空间复杂度：O(n)
 - 稳定排序

```C++
#include<bits/stdc++.h>
using namespace std;

void merge(vector<int>& nums, vector<int>& tmp, int l, int m, int r) {
	int i = l, j = m + 1, k = l;
	while (i <= m && j <= r) {
		if (nums[i] <= nums[j]) {
			tmp[k++] = nums[i++];
		} else {
			tmp[k++] = nums[j++];
		}
	}
	while (i <= m) tmp[k++] = nums[i++];
	while (j <= r) tmp[k++] = nums[j++];
	for (int i = l; i <= r; i++) nums[i] = tmp[i];
}

void mergeSort(vector<int>& nums, vector<int>& tmp, int l, int r) {
	if (l >= r) return;
	int m = l + (r - l) / 2;
	mergeSort(nums, tmp, l, m);
	mergeSort(nums, tmp, m + 1, r);
	merge(nums, tmp, l, m, r);
}

int main() {
	int a[] = {1, 7, 3, 2, 0};
	int len = sizeof(a) / sizeof(int);
	vector<int> nums(a, a + len);
	vector<int> tmp(len);
	mergeSort(nums, tmp, 0, len - 1);
	for(int i = 0; i < nums.size(); i++)
		cout << nums[i] << " ";
	cout << endl;
	return 0;
}
```

### quick sort
 - 最好、平均时间复杂度：O(nlogn)
 - 最坏时间复杂度：O(n^2)
 - 空间复杂度：O(logn)????????
 - 不稳定排序

```C++
#include<bits/stdc++.h>

using namespace std;

int partition(vector<int>& nums, int low, int high) {
	if (low >= high) return low;
	int pivot = nums[low], i = low, j = high;
	while (i < j) {
		while (i < j && nums[j] >= pivot) j--;
		if (i < j) nums[i++] = nums[j];
		else break;
		
		while (i < j && nums[i] <= pivot) i++;
		if (i < j) nums[j--] = nums[i];
		else break;
	}
	nums[i] = pivot;
	return i;
}

void qSort(vector<int>& nums, int low, int high) {
	if (low >= high) return;
	int pivotKey = partition(nums, low, high);
	qSort(nums, low, pivotKey - 1);
	qSort(nums, pivotKey + 1, high);
}

int main() {
	int a[] = {7, 5, 3, 1, 2};
	int len = sizeof(a) / sizeof(int);
	vector<int> nums(a, a + len);
	qSort(nums, 0, len - 1);
	for (int num: nums)
		cout << num << " ";
	cout << endl;
	return 0;
}
```

## 2. Hash Table
 - You absolutely have to know how they work
 - implement one using only arrays in your favorite language in about the space of one interview

## 3. Trees and Graphs
 - tree construction
 - tree traversal: preOrder, inOrder, postOrder, levelOrder;
 - be familiar with: binary trees, n-ary trees and trie-trees 
 - be familiar with at least one balanced binary tree: R/B tree, splay tree, AVL tree. Know how it is implemented
 
 - three basic ways to represent a graph in memory: objects and pointers, matrix, and adjacency list; pros and cons 
 - graph traversal: BFS, DFS; computaional complexity, tradeoffs.
 - dijkstra and A*(for graph)

## 4. Other Data Structures
 - NP-Complete problem: travelling salesman, knapsack problem.

## 5. Operating System, System programming and Concurrency
 - Know about processes, threads, and concurrency issues. Know about locks, mutexes, semaphores and monitors, and how they work. Know about deadlock and livelock and how to avoid them.
 - Know what resources a processes needs, a thread needs, how context switching works, and how it's initiated by the operating system and underlying hardware.
 - Know a little about scheduling. The world is rapidly moving towards multi-core, so know the fundamentals of "modern" concurrency constructs.

## 6. recursion and induction

## 7. Discrete Math
 - counting problems, probability problems, and other Discrete Math 101 situations
 -  the essentials of combinatorics and probability

## 7. system design
 - take a big problem, decompose it into its basic subproblems, and talk about the pros and cons of different approaches to solving those subproblems as they relate to the original goal.
 - GFS, bigtable, mapreduce
