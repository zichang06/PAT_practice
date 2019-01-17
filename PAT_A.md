# PAT (Advanced Level) Practice

## 略的题

06，36, 05,35

# 有疑问的题

以???标出，自上而下搜索即可得疑问队列。

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

## 1065 ☆ A+B and C    SP23

看下课本

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>  // INT_MIN
#include <vector>
using namespace std;


int main() {
	long long a, b, c, num;
	cin >> num;
	for(int i = 1; i <= num; i++) {
		cin >> a >> b >> c;
		bool flag;
		long long res = a + b;
		if (a > 0 && b > 0 && res < 0)
			flag = true;
		else if (a < 0 && b < 0 && res >= 0)
			flag = false;
		else if (res > c)
			flag = true;
		else
			flag = false;
		if (flag == true)
			printf("Case #%d: true\n", i);
		else
			printf("Case #%d: false\n", i);
	}
	system("pause");
	return 0;
}
```

## 1002 A+B for Polynomials

注意代码中注释处的细节

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>  // INT_MIN
#include <vector>
using namespace std;

struct Poly {
	int exp;  // exponential 指数
	double cof;  //  coefficient 系数
};

int main() {
	vector<Poly> a, b, c;
	Poly tmp;
	int n1, n2;
	cin >> n1;
	while (n1--) {
		cin >> tmp.exp >> tmp.cof;
		a.push_back(tmp);
	}
	cin >> n2;
	while (n2--) {
		cin >> tmp.exp >> tmp.cof;
		b.push_back(tmp);
	}
	int i, j;
	for (i = 0, j = 0; i < a.size() && j < b.size();) {
		if (a[i].exp > b[j].exp) {
			c.push_back(a[i]);
			i++;;
		}
		else if (a[i].exp < b[j].exp) {
			c.push_back(b[j]);
			j++;
		}
		else {
			tmp.exp = a[i].exp;
			tmp.cof = a[i].cof + b[j].cof;
			if(tmp.cof != 0)  // 相加后系数不为0才添加
				c.push_back(tmp);
			i++;
			j++;
		}
	}
	while (i < a.size()) {
		c.push_back(a[i++]);
	}
	while (j < b.size()) {
		c.push_back(b[j++]);
	}
	if (c.size() == 0) { // 注意单独处理0的情况
		cout << "0" << endl;
	}
	else {
		cout << c.size() << " ";
		for (int k = 0; k < c.size(); k++) {
			printf("%d %.1f", c[k].exp, c[k].cof);  // 注意题目中限制的格式！保留一位小数
			if (k != c.size() - 1)
				cout << " ";
		}
	}
	system("pause");
	return 0;
}
```

## 1009 Product of Polynomials

该题应注意的是结果多项式数组应该开2001。因为两个1000次项相乘结果可达2000。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>  // INT_MIN
#include <vector>
using namespace std;

const int N = 1001;

int main() {
	double a[N] = { 0 }, b[N] = { 0 }, c[2001] = { 0 };
	int k;
	int tmpExp;
	double tmpCof;
	cin >> k;
	while (k--) {
		cin >> tmpExp >> tmpCof;
		a[tmpExp] = tmpCof;
	}
	cin >> k;
	while (k--) {
		cin >> tmpExp >> tmpCof;
		b[tmpExp] = tmpCof;
	}

	int i, j;
	for (i = 0; i < N; i++) {
		if (a[i] != 0) {
			for (int j = 0; j < N; j++) {
				if (b[j] != 0) {
					int newExp = i + j;
					c[newExp] += a[i] * b[j];
				}
			}
		}
	}
	int count = 0;
	for (i = 0; i < 2001; i++) {
		if (c[i] != 0)
			count++;
	}
	cout << count;
	for (i = 2000; i >= 0; i--) {
		if (c[i] != 0) {
			printf(" %d %.1f", i, c[i]);
		}
	}
	system("pause");
	return 0;
}
```

## 1011 World Cup Betting

Emmmm其实没看懂题目，但是结合题意输入输出规则，还是可以猜出怎么做的2333

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>  // INT_MIN
#include <vector>
#include <string>
using namespace std;


int main() {
	double max[3] = {-1};
	int m[3] = { -1 };
	char c[3] = { 'W', 'T', 'L' };
	double x;
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			cin >> x;
			if (max[i] < x) {
				m[i] = j;
				max[i] = x;
			}
		}
	}
	double res = (max[0] * max[1] * max[2] * 0.65 - (double)1) * (double)2;
	for (int i = 0; i < 3; i++) {
		cout << c[m[i]] << " ";
	}
	printf("%.2f", res);
	system("pause");
	return 0;
}
```

## 1019 General Palindromic Number 

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;

int z[40];
// 返回的数组要反过来输出
int ten2Q(int q, int y, int* z) { 
	int num = 0;
	do {
		z[num++] = y % q;
		y /= q;
	} while (y != 0);
	return num;
}

bool isPalindromic(int* z, int num) {
	for (int i = 0, j = num - 1; i < j; i++, j--) {
		if (z[i] != z[j])
			return false;
	}
	return true;
}

int main() {
	int n, b;
	cin >> n >> b;
	int num = ten2Q(b, n, z);
	bool res = isPalindromic(z, num);
	if (res)
		cout << "Yes" << endl;
	else
		cout << "No" << endl;
	for (int i = num - 1; i > -1; i--) {
		cout << z[i];
		if (i != 0)
			cout << " ";
	}
	system("pause");
	return 0;
}
```



## 1031 Hello World for U

这题思路有点难度，作图，把握n1,n2值的规律，之后就水到渠成了。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;


int main() {
	string str;
	cin >> str;
	int n = str.size();
	int n1, n2, n3;
	n1 = n3 = (int)(n + 2) / 3;
	n2 = n + 2 - n1 - n3;
	for (int i = 0; i < n1 - 1; i++) {
		cout << str[i];
		for (int j = 0; j < n2 - 2; j++) {
			cout << " ";
		}
		cout << str[n - i - 1] << endl;
	}
	for (int i = 0; i < n2; i++) {
		cout << str[i + n1 - 1];
	}

	system("pause");
	return 0;
}
```

## 1027 ☆ Colors in Mars P56 

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;

char radix[13] = { '0','1','2','3', '4', '5', '6', '7', '8',
'9', 'A', 'B', 'C' };

int main() {
	int r, g, b;
	cin >> r >> g >> b;
	cout << "#";
	cout << radix[r / 13] << radix[r % 13];
	cout << radix[g / 13] << radix[g % 13];
	cout << radix[b / 13] << radix[b % 13];
	system("pause");
	return 0;
}
```

## 1058 A+B in Hogwarts  P56

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;

int main() {
	int a[3], b[3], c[3], carry;
	scanf("%d.%d.%d %d.%d.%d", &a[0], &a[1], &a[2], &b[0], &b[1], &b[2]);
	c[2] = (a[2] + b[2]) % 29;
	carry = (a[2] + b[2]) / 29;
	c[1] = (a[1] + b[1] + carry) % 17;
	carry = (a[1] + b[1] + carry) / 17;
	c[0] = a[0] + b[0] + carry;
	printf("%d.%d.%d", c[0], c[1], c[2]);
	system("pause");
	return 0;
}
```

## 1001 A+B Format

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
using namespace std;

int main() {
	int a, b, sum;
	cin >> a >> b;
	sum = a + b;
	if (sum < 0) {
		sum = -sum;
		cout << "-";
	}
	int num[7] = {-1};
	int digits = 0;
	if (sum == 0)  // 注意要考虑结果为0的情况
		num[digits++] = 0;
	while (sum != 0) {
		num[digits++] = sum % 10;
		sum /= 10;
	}
	for (int i = digits - 1; i >= 0; i--) {
		cout << num[i];
		if (i > 0 && i % 3 == 0)  // 每三位一个逗号，最后一位除外!
			cout << ",";
	}
	system("pause");
	return 0;
}
```

## 1077 ☆ Kuchiguse 公共后缀  实战P80

1. getline(cin, str)
2. 求公共后缀 → 公共前缀。土方法，杠杠滴。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
#include <limits.h>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
	int n;
	string tmp;
	vector<string> strs;

	cin >> n;  // cin 没有吸收换行符，需要getchar一下
	getchar();
	while (n--) {
		getline(cin,tmp);  // !!! c++的读取完整的包括空格的一行。
		strs.push_back(tmp);
	}
	int minLength = INT_MAX;
	for (int i = 0; i < strs.size(); i++) {
		minLength = minLength <= strs[i].length() ? minLength : strs[i].length();
		reverse(strs[i].begin(), strs[i].end());
	}

	int i, res;
	for (i = 0; i < minLength;) {
		bool flag = false;
		char ch = strs[0][i];
		for (int j = 1; j < strs.size(); j++) {
			if (strs[j][i] != ch)
				flag = true;
		}
		if (flag)
			break;
		i++;  // 放在这里，只有第i位合上了，才自加
	}
	int length = i;

	if (length <= 0) {
		cout << "nai";
	}
	else {
		string resStr = strs[0].substr(0, length);
		reverse(resStr.begin(), resStr.end());
		cout << resStr;
	}
	system("pause");
	return 0;
}
```

## ☆ 1082 Read Number in Chinese 实战P84 ???

