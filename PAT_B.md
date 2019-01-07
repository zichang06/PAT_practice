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
## 1009 说反话
VS2017的<Ctrl+Z>结合<Enter>么得用，但OJ上确实过了。据说gcc或者DEV上都能正常使用。罢了，凑活凑活之后转C++打码。
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
using namespace std;

int main() {	
	int num = 0;  // 单词的个数
	char words[81][50];
	// 使用scanf因为它以空格为终止，而gets则以换行为终止
	while (scanf("%s", words[num]) != EOF) {  
		num++;
	}
	for (int i = num - 1; i >= 0; i--) {
		printf("%s", words[i]);
		if (i > 0)
			printf(" ");
	}
	system("pause");
	return 0;
}
```