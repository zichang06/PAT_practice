# PAT (Basic Level) Practice （中文）
## 1022 D进制的A+B 
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;

int P2ten(int p, int x) {
	int y = 0;
	int product = 1;
	while (x != 0) {
		y += (x % 10) * product;
		x /= 10;
		product *= p;
	}
	return y;
}

int z[40];  // 返回的数组要反过来输出
int ten2Q(int q, int y, int* z) {
	int num = 0;
	do {
		z[num++] = y % q;
		y /= q;
	} while (y != 0);
	return num; // 返回位数
}

int main() {
	int a, b, q;
	cin >> a >> b >> q;
	int y = a + b;
	int num = ten2Q(q, y, z);

	for (int i = num - 1; i >= 0; i--) {
		cout << z[i];
	}
		
	//system("pause");
	return 0;
}
```