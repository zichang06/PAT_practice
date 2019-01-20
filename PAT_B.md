# PAT (Basic Level) Practice （中文）

## 略的题目

06, 21, 31, 02，48，15

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
## 1028 ☆ 人口普查 
此题有两点值得注意：
1. scanf读取的格式。
2. younger函数，比较日期大小

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>  // INT_MIN
#include <vector>
#include <string>
using namespace std;


struct person {
	char name[6];
	int year;
	int month;
	int day;
}youngest, oldest, low, high, tmp;  // 设置全局变量

void init() {  // main函数开始调用初始化函数，以初始化全局变量
	youngest.year = low.year = 1814;  // high 年龄最大的
	oldest.year = high.year = 2014;  // low 年龄最小的
	youngest.month = high.month = oldest.month = low.month = 9;
	youngest.day = high.day = oldest.day = low.day = 6;
}

bool younger(person& a, person& b) {
	/* if a is younger than b, return true */
	if (a.year != b.year)
		return a.year >= b.year;
	else if (a.month != b.month)
		return a.month >= b.month;
	else if (a.day != b.day)
		return a.day >= b.day;
}
bool isValid(person& a) {
	return younger(high, a) && younger(a, low);
}
int main() {
	init();
	int k, count = 0;
	cin >> k;
	while (k--) {
		scanf("%s %d/%d/%d", tmp.name, &tmp.year, &tmp.month, &tmp.day);
		if (isValid(tmp)) {
			count++;
			if (younger(oldest, tmp))
				oldest = tmp;
			if (younger(tmp, youngest))
				youngest = tmp;
		}
	}
	if (count == 0)
		cout << "0";
	else
		printf("%d %s %s\n", count, oldest.name, youngest.name);
	system("pause");
	return 0;
}
```

## 1027 ☆ 打印沙漏 

两个注意点：

1. 根据公式算得x，可能是偶数，此时x必须减1。
2. 每行后面的空格无需打印，否则会提示格式错误。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;


int main() {
	int n, x, sum;
	char ch;
	cin >> n >> ch;
	x = (int)sqrt((double)(2 * (1 + n))) - 1;
	if (x % 2 == 0)
		x -= 1;
	sum = (1 + x) * (1 + x) / 2 - 1;

	int line = x;
	while (line != 1)
	{
		for (int i = 0; i < (x - line) / 2; i++) {
			cout << " ";
		}
		for (int i = 0; i < line; i++) {
			cout << ch;
		}
		/*for (int i = 0; i < (x - line) / 2; i++) {  后面的空格不需要啦！！
			cout << " ";
		}*/
		cout << endl;
		line -= 2;
	}
	while (line <= x)
	{
		for (int i = 0; i < (x - line) / 2; i++) {
			cout << " ";
		}
		for (int i = 0; i < line; i++) {
			cout << ch;
		}
		/*for (int i = 0; i < (x - line) / 2; i++) {
			cout << " ";
		}*/
		cout << endl;
		line += 2;
	}
	cout << n - sum;
	system("pause");
	return 0;
}
```

# 1037 在霍格沃茨找零钱

重点在于都转成同一量，相减后再规格化。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;


int main() {
	int g1, s1, k1, g2, s2, k2, price, money;
	scanf("%d.%d.%d %d.%d.%d", &g1, &s1, &k1, &g2, &s2, &k2);
	price = g1 * 17 * 29 + s1 * 29 + k1;
	money = g2 * 17 * 29 + s2 * 29 + k2;
	int change = money - price;
	if (change < 0) {
		cout << "-";
		change = -change;
	}
	printf("%d.%d.%d", change / 17 / 29, change % (17 * 29) / 29, change % 29);
	system("pause");
	return 0;
}
```

## 1014/A1061 福尔摩斯的约会

注意比较字符时的范围，如A-G，A-N，而不是A-Z，不然会答案错误。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <math.h>
#include <vector>
#include <string>
using namespace std;

int main() {
	string week[7] = { "MON","TUE","WED","THU","FRI","SAT","SUN" };
	string a1, a2, b1, b2;
	cin >> a1 >> a2 >> b1 >> b2;
	int i;
	for (i = 0; i < a1.size() && i < a2.size(); i++) {
		if (a1[i] == a2[i] && a1[i] <= 'G' && a1[i] >= 'A') {
			cout << week[a1[i] - 'A'] << " ";
			break;
		}
	}

	int h = 0;;
	for (i++; i < a1.size() && i < a2.size(); i++) {
		if (a1[i] == a2[i]) {
			if (a1[i] <= '9' && a1[i] >= '0')
				printf("%02d:", a1[i] - '0');
			else if (a1[i] <= 'N' && a1[i] >= 'A')
				printf("%02d:", a1[i] - 'A' + 10);
			else
				continue;
			break;
		}
	}
	

	for (i = 0; i < b1.size() && i < b2.size(); i++) {
		if (b1[i] == b2[i] && ((b1[i] <= 'Z' && b1[i] >= 'A')
			|| (b1[i] <= 'z' && b1[i] >= 'a'))) {
			printf("%02d", i);
			break;
		}
	}

	system("pause");
	return 0;
}
```



## 1024/A1073 ☆ ??? 科学计数法 (实战P69)

1. cstring和string的使用
2. 注意分情况讨论，具体见注释
3. 最后一个用例总是运行时出错。原先我设置的结果数组是10000，改了22000后就AC了。网上有人如下说，但我没懂。???
> 这里数字存储长度不超过9999个字节，9999个字节可以存储多大的数？
1个字节可以存储2^8-1 = 127
9999个字节可以存储2^(8*9999)-1 数字用十进制表示长度为21070位，所以数组的大小不够存储。修改后测试通过。

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
using namespace std;

int main() {
	string a;
	cin >> a;
	char res[22000] = { '\0' };
	int pos_dot = (int)a.find('.');
	int pos_E = (int)a.find('E');
	char sign = a[0];
	char part1 = a[pos_dot - 1];
	// string part1 = a.substr(1, pos_dot - 1);  // 一般就是'1'，错了
	string part2 = a.substr(pos_dot + 1, pos_E - pos_dot - 1);
	char expSign = a[pos_E + 1];
	string exp = a.substr(pos_E + 2, a.size() - pos_E);
	int expNum = atoi(exp.c_str());  // ☆ 需先将string转成char*，再转成int
	int start = 0;

	if (sign == '-') {
		res[0] = '-';
		start++;
	}
	if (expSign == '-') {  // 指数为负数时
		res[start++] = '0';
		res[start++] = '.';
		for (int i = 0; i < expNum - 1; i++) {
			res[start++] = '0';
		}
		res[start++] = part1;
		strcat(res, part2.c_str());
	}
	else {  // 指数为正数时, 可能有小数点，若指数太大，则可能没有小数点
		res[start++] = part1;
		if (expNum < part2.size()) {
			string part2_1(part2.substr(0, expNum));
			string part2_2 = part2.substr(expNum, part2.size() - expNum);
			strcat(res, part2_1.c_str());
			start += part2_1.size();
			res[start++] = '.';
			strcat(res, part2_2.c_str());
		}
		else {
			strcat(res, part2.c_str());
			start += part2.length();
			for (int i = 0; i < expNum - part2.length(); i++) {
				res[start++] = '0';
			}
		}
	}

	printf("%s", res);
	system("pause");
	return 0;
}
```

