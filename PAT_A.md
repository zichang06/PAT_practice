# PAT (Advanced Level) Practice
## ☆ 1046 Shortest Distance 
此题容易卡超时，必须在输入时就预处理好 1 到各点的距离。
```c++
int main() {
	int nodes, pathNum, tmp, n1, n2, res, dis1, dis2;
	vector<int> distance;
	cin >> nodes;
	distance.push_back(0);
	for (int i = 1; i <= nodes; i++) {
		cin >> tmp;
		distance.push_back(tmp + distance[i - 1]);
	} // distance[i]表示1个节点到第i+1个节点的顺时针距离; 最后一个值表示一整圈的距离

	cin >> pathNum;
	while (pathNum--) {
		cin >> n1 >> n2;
		if (n1 > n2) {
			tmp = n1;
			n1 = n2;
			n2 = tmp;
		}
		dis1 = distance[n2 - 1] - distance[n1 - 1];
		dis2 = distance[nodes] - dis1;
		res = dis1 > dis2 ? dis2 : dis1;
		cout << res << endl;
	}
	system("pause");
	return 0;
}
```
## ☆ Shuffling Machine
思想值得记住！
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>  // INT_MIN
#include <vector>
using namespace std;


int main() {
	const int N = 54;
	char map[5] = { 'S', 'H', 'C', 'D', 'J' };
	int start[N + 1], next[N + 1], end[N + 1];
	int k = 1;
	cin >> k;
	for (int i = 1; i <= N; i++) {
		cin >> next[i];
	}
	for (int i = 1; i <= N; i++) {
		start[i] = i;
	}
	while (k--) {
		for (int i = 1; i <= N; i++) {
			end[next[i]] = start[i];
		}
		for (int i = 1; i <= N; i++) {
			start[i] = end[i];
		}
	}
	
	for (int i = 1; i <= N; i++) {
		cout << map[(start[i] - 1) / 13] << (start[i] - 1) % 13 + 1;
		if (i != N)
			cout << " " ;
	}
	//system("pause");
	return 0;
}
```