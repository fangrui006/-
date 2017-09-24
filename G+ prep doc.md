# G+ prep doc

## sort
[各种算法比较](http://my.csdn.net/uploads/201207/19/1342700879_2982.jpg)

### merge sort
最好、最坏和平均时间复杂度：O(nlogn)
空间复杂度：O(n)
稳定排序

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
