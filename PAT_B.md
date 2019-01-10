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
## 1011 A+B和C
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <vector>
using namespace std;

int main() {	
	long long a, b, c;
	int t;
	vector<bool> res;
	cin >> t;
	while (t-- > 0) {
		cin >> a >> b >> c;
		res.push_back(a + b > c);
	}
	for (int i = 0; i < res.size(); i++) {
		// boolalpha 是用来输出false和true的
		cout << "Case #" << i + 1<< ": " << boolalpha << res[i] << endl;
	}
	return 0;
}
```
## 1016 部分A+B
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <vector>
using namespace std;

int main() {	
	long long a, b;
	int da, db;
	long long pa = 0, pb = 0;
	cin >> a >> da >> b >> db;
	while (a != 0) {
		if (a % 10 == da) {
			pa = pa * 10 + da;
		}
		a /= 10;
	}
	while (b != 0) {
		if (b % 10 == db) {
			pb = pb * 10 + db;
		}
		b /= 10;
	}
	cout << pa + pb << endl;
	system("pause");
	return 0;
}
```
## ☆1026 程序运行时间
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
using namespace std;

int main() {	
	int a, b;
	int h, m, s;
	cin >> a >> b;
	//int time = round(((double)b - (double)a) / (double)100);
	// 有时直接调用Math.h的round会发生错误，不妨自己定义
	int time = b - a;
	if (time % 100 >= 50)
		time = time / 100 + 1;
	else
		time /= 100;
	h = time / 3600;
	m = time % 3600 / 60;
	s = time % 60;
	printf("%02d:%02d:%02d", h, m, s);
	system("pause");
	return 0;
}
```
## 1046 划拳
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
using namespace std;

int main() {	
	int as, ah, bs, bh;  // 甲喊 甲划 乙喊 乙划
	int abottle = 0, bbottle = 0;
	int t;
	bool awin, bwin;
	cin >> t;
	while (t--) {
		cin >> as >> ah >> bs >> bh;
		awin = bwin = false;
		if (ah == as + bs)
			awin = true;
		if (bh == as + bs)
			bwin = true;
		if (awin == true && bwin == false)
			bbottle++;
		if (awin == false && bwin == true)
			abottle++;
	}
	cout << abottle << " " << bbottle;
	system("pause");
	return 0;
}
```


## ☆1008 数组元素循环右移问题
下面考虑一些特殊情况:
当m=0, 很显然原序列不进行任何移动
当m=6, 把6移动6个位置, 结果仍然是原序列
当m=7, 现在m>n, 根据题意这种情况是可能的, 把6往右移动7个位置, 我们发现6移到了1的位置.
**综上所述, 移动的次数m = m % n，如果m == 0,则不做修改**

解法二：直接输出序列从N-M号元素到N-1号元素，再输出0号元素到N-M-1号元素。代码略。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
using namespace std;

void reverse(int* list, int low, int high) { 
	int i = low, j = high;
	for (; i <= (high - low) / 2 + low; i++, j--) {
		int tmp = *(list + i);
		*(list + i) = *(list + j);
		*(list + j) = tmp;
	}
}

int main() {	
	int n, m;
	const int maxn = 100;
	cin >> n >> m;
	int list[maxn];
	for (int i = 0; i < n; i++) {
		cin >> list[i];
	}
	m %= n;  // 这段易忽视
	if (m > 0) {
		reverse(list, 0, n - m - 1);
		reverse(list, n - m, n - 1);
		reverse(list, 0, n - 1);
	}
	for (int i = 0; i < n - 1; i++) {
		cout << list[i] << " ";
	}
	cout << list[n - 1];
	system("pause");
	return 0;
}
```
## 1018 锤子剪刀布
略
