# PAT (Basic Level) Practice （中文）

## 略的题目

1006、1021、1031、1002、1048、

1015、1038、1033、1043、1047、

1041、1004、1010、1018

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

## 1029 旧键盘

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>
#include <string>
#include <vector>
using namespace std;

bool characters[37] = { false };
/* 0-25 corresponds to A-Z; 26 - 35 corresponds to 1-10; 36 means _ */

/*
 * 首先扫描完整字符串，所经历字符置为true
 * 再扫描输入的字符串，扫的字符置为false
 * 最后看仍然为true的字符，就是坏掉的键盘
*/

int hashFuc(char ch) {
	if (ch <= 'z' && ch >= 'a')
		return ch - 'a';
	if (ch <= 'Z' && ch >= 'A')
		return ch - 'A';
	if (ch <= '9' && ch >= '0')
		return ch - '0' + 26;
	if (ch == '_')
		return 36;
	else
		return -1;
}

char reverseHash(int tmp) {
	if (tmp == 36)
		return '_';
	if (tmp >= 0 && tmp <= 25)
		return tmp + 'A';
	if (tmp >= 26 && tmp <= 35)
		return tmp - 26 + '0';
}

int main() {
	string str1, str2;
	getline(cin, str1);
	getline(cin, str2);
	int i, pos;

	for (i = 0; i < str1.size(); i++) {
		pos = hashFuc(str1[i]);
		characters[pos] = true;
	}

	for (i = 0; i < str2.size(); i++) {
		pos = hashFuc(str2[i]);
		characters[pos] = false;
	}


	char ch;
	// 按照原本字符串中字符出现顺序
	for (i = 0; i < str1.size(); i++) {
		pos = hashFuc(str1[i]);
		if (characters[pos]) {
			ch = reverseHash(pos);
			cout << ch;
			characters[pos] = false;
		}
	}
	
	system("pause");
	return 0;
}
```

## 1039 到底买不买

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <string>
#include <vector>
using namespace std;

int beadNum[62] = { 0 };

int hashFuc(char ch) {
	if (ch <= 'z' && ch >= 'a')
		return ch - 'a';
	if (ch <= 'Z' && ch >= 'A')
		return ch - 'A' + 26;
	if (ch <= '9' && ch >= '0')
		return ch - '0' + 52;
	else
		return -1;
}
int main() {
	string str1, str2;
	getline(cin, str1);
	getline(cin, str2);
	int i, pos;

	for (i = 0; i < str2.size(); i++) {
		pos = hashFuc(str2[i]);
		beadNum[pos]++;
	}

	for (i = 0; i < str1.size(); i++) {
		pos = hashFuc(str1[i]);
		beadNum[pos]--;
	}

	// 正数的珠子说明不够，累积之则为缺的珠子数量
	// 负数不用参与计数
	// 如果珠子够的话，两个字符串长度差值即为多余珠子数
	int lack = 0;
	for (i = 0; i < 62; i++) {
		if (beadNum[i] > 0) {
			lack += beadNum[i];
		}
		
	}

	if (lack > 0) {
		printf("No %d", lack);
	}
	else {
		printf("Yes %d", str1.size() - str2.size());
	}
	
	system("pause");
	return 0;
}
```

## 1042 字符统计

```c++
提交成功
×



子长


题目集

题目列表

提交列表

排名
PAT (Basic Level) Practice （中文）

1900 分

编程题
共 95 小题，共计 1900 分
原PAT网站用户可在 https://patest.cn/bind_old_pat_user 页面绑定至拼题A账号。绑定后，原PAT网站的提交将被合并至拼题A网站用户的对应题目集中。

编程题
1042 字符统计 （20 分）
请编写程序，找出一段给定文字中出现最频繁的那个英文字母。
输入格式：
输入在一行中给出一个长度不超过 1000 的字符串。字符串由 ASCII 码表中任意可见字符及空格组成，至少包含 1 个英文字母，以回车结束（回车不算在内）。
输出格式：
在一行中输出出现频率最高的那个英文字母及其出现次数，其间以空格分隔。如果有并列，则输出按字母序最小的那个字母。统计时不区分大小写，输出小写字母。
输入样例：
This is a simple TEST.  There ARE numbers and other symbols 1&2&3...........
输出样例：
e 7
作者: CHEN, Yue
单位: 浙江大学
时间限制: 400 ms
内存限制: 64 MB
代码长度限制: 16 KB

编译器:共 31 种编译器可用


12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42




    return ch - 'a';
  if (ch <= 'Z' && ch >= 'A')
    return ch - 'A';
  else
    return -1;
}
int main() {
  string str;
  getline(cin, str);

  int pos;
  for (int i = 0; i < str.size(); i++) {
    pos = hashFuc(str[i]);
    if (pos != -1) {
      characters[pos]++;
    }
  }
  
  int maxNum = 0, maxCh = 0;
  for (int i = 0; i < 26; i++) {
    if (characters[i] > maxNum) {
      maxCh = i;
      maxNum = characters[i];
    }
  }

  printf("%c %d", maxCh + 'a', maxNum);
  
  system("pause");
  return 0;
}
```

## 1005 🔺☆ 继续（3n+1）猜想

第二次做尝试想出思路思路!!!

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

bool cmp(int a, int b) {
	return a > b;  // 从大到小排序
}

int main() {
	int n, m, a[101];
	cin >> n;
	bool hashTable[10000] = { 0 };

	for (int i = 0; i < n; i++) {
		cin >> a[i];
		m = a[i];
		while (m != 1) {
			if (m % 2 == 1)
				m = (3 * m + 1) / 2;
			else
				m = m / 2;
			hashTable[m] = true;
		}
	}

	int count = 0;  // count 计数“关键字”个数
	for (int i = 0; i < n; i++) {
		if (hashTable[a[i]] == false)
			count++;
	}

	sort(a, a + n, cmp);
	for (int i = 0; i < n; i++) {
		if (hashTable[a[i]] == false) {
			cout << a[i];
			count--;
			if (count > 0)
				cout << " ";
		}
	}
	system("pause");
	return 0;
}
```

## 1023 ☆ 组个最小数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

int main() {
	int count[10];  // 记录数字0-9的个数
	for (int i = 0; i < 10; i++) {
		cin >> count[i];
	}
	// 从1-9中选择count不为0的最小的数字
	for (int i = 1; i < 10; i++) {
		if (count[i] > 0) {
			cout << i;
			count[i]--;
			break;
		}
	}
	for (int i = 0; i < 10; i++) {
		for (int j = 0; j < count[i]; j++) {
			cout << i;
		}
	}
	system("pause");
	return 0;
}
```

## 1020 月饼

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

struct moonCake {
	double store;  // 库存量
	double sell;  // 总售价
	double price;  // 单价
}cakes[1001];

bool cmp(moonCake a, moonCake b) {
	return a.price > b.price;  // 从大到小排序
}
int main() {
	int n;
	double d, left, profit;
	cin >> n;
	cin >> d;
	for (int i = 0; i < n; i++) {
		cin >> cakes[i].store;
	}
	for (int i = 0; i < n; i++) {
		cin >> cakes[i].sell;
		cakes[i].price = cakes[i].sell / cakes[i].store;
	}

	sort(cakes, cakes + n, cmp);
	left = d;
	profit = 0;

	for (int i = 0; i < n; i++) {
		if (cakes[i].store <= left) {
			left -= cakes[i].store;
			profit += cakes[i].sell;
		}
		else {
			profit += cakes[i].price * left;
			left = 0;
			break;
		}
	}
	
	printf("%.02f", profit);
	system("pause");
	return 0;
}

```

## 1030 🔺???完美数列

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
int nums[100001];

int main() {
	int n, p;
	cin >> n >> p;
	for (int i = 0; i < n; i++) {
		cin >> nums[i];
	}
	sort(nums, nums + n);  // 递增排序
	int ans = 1;
	for (int i = 0; i < n; i++) {
		// 在nums[i+1] ~ nums[n-1]中查找第一个超过a[i] * p的数，返回其位置给j
		int j = upper_bound(nums + i + 1, nums + n, (long long)nums[i] * p) - nums;
		ans = max(ans, j - i);  // 更新最大长度
	}
	cout << ans;
	system("pause");
	return 0;
}

```

## 1010

#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
long long map[256];  // 0-9、a-z与0-35相对应

void init() {
​	for (char c = '0'; c <= '9'; c++)
​		map[c] = c - '0'; 
​	for (char c = 'a'; c <= 'z'; c++)
​		map[c] = c - 'a' + 10;  // 将'a'-'z'映射到10-35
}

// 将a转换为十进制，t为上界
long long convertNum10(char a[], long long radix, long long t) {
​	long long ans = 0;
​	int len = strlen(a);
​	for (int i = 0; i < len; i++) {
​		ans = ans * radix + map[a[i]];  // 进制转换
​		if (ans < 0 || ans > t)
​			return -1;
​	}
​	return ans;
}

// N2的十进制与t比较
bool cmp(char N2[], long long radix, long long t) {
​	int len = strlen(N2);
​	long long num = convertNum10(N2, radix, t);
​	if (num > 0)
​		return true;
​	if(t > )
}

int main() {
​	
​	system("pause");
​	return 0;
}

## 1039 ☆ Course List for Students

学生姓名的hash值得看下~~P233

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
long long map[256];  // 0-9、a-z与0-35相对应

const int N = 40001;  // 总人数
const int M = 26 * 26 * 26 * 10;  // 由姓名散列的数字上界
vector<int> selectCourse[M];  // 每个学生选择的课程编号

int getId(string name) {
	int id = 0;
	for (int i = 0; i < 3; i++) {
		id = id * 26 + (name[i] - 'A');
	}
	id = id * 10 + (name[3] - '0');
	return id;
}

int main() {
	string name;
	int n, k;  // 人数及课程数
	cin >> n >> k;
	for (int i = 0; i < k; i++) {
		int course, x;
		cin >> course >> x;  // 输入课程数和选课人数  
		for (int j = 0; j < x; j++) {  
			cin >> name;
			int id = getId(name);
			selectCourse[id].push_back(course);
		}
	}

	for (int i = 0; i < n; i++) {
		cin >> name;
		int id = getId(name);
		sort(selectCourse[id].begin(), selectCourse[id].end());
		printf("%s %d", name.c_str(), selectCourse[id].size());
		for (int j = 0; j < selectCourse[id].size(); j++) {
			printf(" %d", selectCourse[id][j]);
		}
		printf("\n");
	}
	system("pause");
	return 0;
}

```

## 1047 ☆ Student List for Course

避免直接对字符串排序，可以用编号P235

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;
long long map[256];  // 0-9、a-z与0-35相对应

const int maxn = 40001;  // 总人数
const int maxc = 2501;  // 由姓名散列的数字上界

char name[maxn][5];  // maxn个学生
vector<int> course[maxc];  // course[i]存放第i门课的所有学生编号

bool cmp(int a, int b) {
	return strcmp(name[a], name[b]) < 0;  // 按姓名字典序从小到大排序
}

int main() {
	int n, k, c, courseID;  // 人数及课程数
	cin >> n >> k;  // 学生人数及课程数
	for (int i = 0; i < n; i++) {
		scanf("%s %d", name[i], &c);  // 学生姓名及选课数
		for (int j = 0; j < c; j++) {  
			scanf("%d", &courseID);
			course[courseID].push_back(i);  //将学生i加入第courseID门课中
		}
	}

	for (int i = 1; i <= k; i++) {
		printf("%d %d\n", i, course[i].size());
		sort(course[i].begin(), course[i].end(), cmp);  // 对第i门课的学生排序!!!根据name排其序号！！
		for (int j = 0; j < course[i].size(); j++) {
			printf("%s\n", name[course[i][j]]);  // 输出学生姓名
		}
	}
	system("pause");
	return 0;
}

```

## 1044 ☆ 火星数字

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
#include <map>
using namespace std;
// [0.,12]的火星文
string unitDigit[13] = { "tret", "jan", "feb", "mar", "apr", "may", "jun", "jly", "aug", "sep", "oct", "nov", "dec" };
// 13的[0，12]倍火星文
string tenDigit[13] = { "tret", "tam", "hel", "maa", "huh", "tou", "kes", "hei", "elo", "syy", "lok", "mer", "jou" };

string numToStr[170];  // 数字 → 火星文 !!!见下init，实质就是hash
map<string, int> strToNum;  // 火星文 → 数字  用map进行字符串到数字的映射

void init() {  // !!!
	for (int i = 0; i < 13; i++) {
		numToStr[i] = unitDigit[i];  // 个位为[0, 12]， 十位为0
		strToNum[unitDigit[i]] = i;
		numToStr[i * 13] = tenDigit[i];
		strToNum[tenDigit[i]] = i * 13;  
	}
	for (int i = 1; i < 13; i++) {  // 十位
		for (int j = 1; j < 13; j++) {  // 个位，注意从1开始计数
			string str = tenDigit[i] + " " + unitDigit[j];  // 火星文
			numToStr[i * 13 + j] = str;  // 数字 → 火星文
			strToNum[str] = i * 13 + j;  // 火星文 → 数字
		}
	}
}
int main() {
	init();  // 打表
	int t;
	cin >> t;
	getchar();  // 吸收一下回车
	while (t--) {
		string str;
		getline(cin, str);
		if (str[0] >= '0' && str[0] <= '9') {  // 如果是数字
			int num = 0;
			for (int i = 0; i < str.length(); i++) {
				num = num * 10 + (str[i] - '0');
			}
			cout << numToStr[num] << endl;
		}
		else {  // 如果是火星文
			cout << strToNum[str] << endl;
		}
	}
	system("pause");
	return 0;
}
```

## 1052 ☆ 🔺 Linked List Sorting

静态链表排序

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <queue>
#include <algorithm>
using namespace std;
const int maxn = 100001;

struct node {
	int address, data, next;
	bool flag;  // 结点是否在链表上
}nodes[maxn];

// 筛选有效结点，并按data从小到大排序
bool cmp(node a, node b) {  // !!!
	if (a.flag == false || b.flag == false) {
		return a.flag > b.flag;  // 只要a和b中有一个无效结点，就把无效结点放到后面去 !!!
	}
	else {
		return a.data < b.data;  // 如果都是有效结点，则按要求排序
	}
}
int main() {
	for (int i = 0; i < maxn; i++) {
		nodes[i].flag = false;
	}
	int n, begin, address;
	scanf("%d%d", &n, &begin);
	for (int i = 0; i < n; i++) {
		scanf("%d", &address);
		scanf("%d%d", &nodes[address].data, &nodes[address].next);
		nodes[address].address = address;
	}
	int count = 0, p = begin;
	// 枚举链表，对flag进行标记，同时计数有效结点个数 
	while (p != -1) {
		nodes[p].flag = true;
		count++;
		p = nodes[p].next;
	}
	if (count == 0) {
		printf("0 -1");  // 特判，新链表没有结点时输出0 -1. !!!
	}
	else {
		// 筛选有效结点，并按data从小到大排序
		sort(nodes, nodes + maxn, cmp);
		printf("%d %05d\n", count, nodes[0].address);  // 防止-1被%05d话，提前判断
		for (int i = 0; i < count; i++) {
			if (i != count - 1) {
				printf("%05d %d %05d\n", nodes[i].address, nodes[i].data, nodes[i + 1].address);
			}
			else {
				printf("%05d %d -1\n", nodes[i].address, nodes[i].data);
			}
		}
	}
	system("pause");
	return 0;
}
```

## ☆ 🔺1029  Median

??? 还有一两个样例没过，内存超限，PAT去年新增加的卡内存考点

网上参考：https://blog.csdn.net/Hide_in_Code/article/details/81986596，好困下次做吧

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <queue>
#include <limits.h>
using namespace std;
const int maxn = 200001;

int s1[maxn], s2[maxn];  // 两个递增序列
int main() {
	int n, m;
	cin >> n;
	for (int i = 0; i < n; i++) {
		scanf("%d", &s1[i]);
	}
	cin >> m;
	for (int i = 0; i < m; i++) {
		scanf("%d", &s2[i]);
	}
	s1[n] = s2[m] = INT_MAX;
	int medianPos = (n + m - 1) / 2;  //事先找出 medianPos 为中间位置
	int i = 0, j = 0, count = 0;   // count 计数当前位置
	// 只要count 未达到medianPos, 就继续循环!!!直到找出两个递增序列中是中位数那个数字
	while (count < medianPos) {  
		if (s1[i] < s2[j])  
			i++;
		else
			j++;
		count++;
	}
	// count 达到中位数位置时，上述while循环中没有对s1[i]和s2[j]的大小进行判断
	if (s1[i] < s2[j]) {  // 输出两个序列当前位置较小的元素
		cout << s1[i];
	}
	else
		cout << s2[j];
	system("pause");
	return 0;
}
```

## 1003 🔺 ☆ ??? 我要通过!

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

int main() {
	int t;
	cin >> t;
	getchar();
	while (t--) {
		string str;  
		getline(cin, str);
		int len = str.length();
		// 分别代表P的个数、T的个数、除PAT外字符的个数
		int num_p = 0, num_t = 0, other = 0;
		int loc_p = -1, loc_t = -1;  // 分别代表P的位置和T的位置
		for (int i = 0; i < len; i++) {
			if (str[i] == 'P') {  // 若当前字符为P，则P的个数加1，位数变为1
				num_p++;
				loc_p = i;
			}
			else if (str[i] == 'T') {  // 若当前字符为T，则T的个数加1、位置变为i
				num_t++;
				loc_t = i;
			}
			else if (str[i] != 'A')
				other++;
		}
		// 如果P的个数不为1，或者T的个数不为1
		// 或者存在除PAT外的字符，或者P和T之间没有字符
		if ((num_p != 1) || (num_t != 1) || (other != 0) || (loc_t - loc_p <= 1)) {
			cout << "NO" << endl;
			continue;
		}
		// x,y,z的含义见“思路”，可以通过loc_p和loc_t 得到
		int x = loc_p, y = loc_t - loc_p - 1, z = len - loc_t - 1;
		if (z - x * (y - 1) == x) {  // 条件2成立的条件
			cout << "YES" << endl;
		}
		else {
			cout << "NO" << endl;
		}
	}
	system("pause");
	return 0;
}
```

## 1007 ☆ 素数对猜想

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
#include <cmath>
using namespace std;

bool isPrime(int n) {  // 判断n是否为素数!!!
	if (n <= 1)
		return false;
	int sqr = (int)sqrt(1.0 * n);
	for (int i = 2; i <= sqr; i++) {
		if (n % i == 0) 
			return false;
	}
	return true;
}
int main() {
	int n, count = 0;
	cin >> n;
	for (int i = 3; i + 2 <= n; i += 2) {
		if (isPrime(i) && isPrime(i + 2)) {
			count++;
		}
	}
	cout << count << endl;
	system("pause");
	return 0;
}
```

## 1013 ☆ 数素数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 1000001;
bool p[maxn] = { 0 };
int prime[maxn], num = 0;

// 筛法!!!
void find_prime(int n) {  // 求素数表
	for (int i = 2; i < maxn; i++) {
		if (p[i] == false) {
			prime[num++] = i;
			if (num >= n)  // 只需要n个素数
				break;
			for (int j = i + i; j < maxn; j += i) {
				p[j] = true;  // 该素数的整数倍都置为非素数!!!
			}
		}
	}
}

int main() {
	int m, n, count = 0;
	cin >> m >> n;
	find_prime(n);
	for (int i = m; i <= n; i++) {
		cout << prime[i - 1];
		count++;
		if (count % 10 != 0 && i < n)
			cout << " ";
		else
			cout << endl;
	}
	system("pause");
	return 0;
}
```

## 1017 ☆ 🔺 ??? A 除以B

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <cstring>
#include <string>
#include <cmath>
#include <algorithm>
using namespace std;
typedef long long ll;

struct bign {  // 大N
	int d[1001];
	int len;
	bign() {
		memset(d, 0, sizeof(d));  // string.h
		len = 0;
	}
};

bign change(string str) {  // 将整数转换为bign!!!
	bign a;
	a.len = str.length();
	for (int i = 0; i < a.len; i++) {
		a.d[i] = str[a.len - i - 1] - '0';
	}
	return a;
}

bign divide(bign a, int b, int& r) {  // 高精度除法，r为余数!!!
	bign c;
	c.len = a.len; // 被除数的每一位和商的每一位是一一对应的，因此先令长度相等
	for (int i = a.len - 1; i >= 0; i--) {
		r = r * 10 + a.d[i];  // 和上一位遗留的余数组合
		if (r < b)
			c.d[i] = 0;  // 不够除，该位为0
		else {  // 够除
			c.d[i] = r / b; // 商
			r = r % b;  // 获得新的余数
		}
	}
	while (c.len - 1 >= 1 && c.d[c.len - 1] == 0) {
		c.len--;  // 去除高位的0，同时至少保留一位最低位
	}
	return c;
}

void print(bign a) {  // 输出bign
	for (int i = a.len - 1; i >= 0; i--) {
		cout << a.d[i];
	}
}

int main() {
	string str1, str2;
	int b, r = 0;
	cin >> str1 >> b;
	bign a = change(str1);  // 将a转换为bign型
	print(divide(a, b, r));
	printf(" %d", r);
	system("pause");
	return 0;
}
```

## 1050 螺旋矩阵

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
#include <cmath>
using namespace std;
const int maxn = 10001;
int matrix[10001][1001], a[maxn];
/* WC!!!更改数组size还会影响运行时间的，原来maxn*maxn会超时
*/

bool cmp(int a, int b) {
	return a > b;
}

int main() {
	int N;
	cin >> N;
	for (int i = 0; i < N; i++) {
		cin >> a[i];
	}
	if (N == 1) {
		cout << a[0];
		system("pause");
		return 0;
	}
	sort(a, a + N, cmp);
	int m = (int)ceil(sqrt(1.0*N));
	while (N % m != 0) {
		m++;
	}
	int n = N / m, i = 1, j = 1, now = 0;
	int U = 1, D = m, L = 1, R = n;
	while (now < N)
	{
		while (now < N && j < R) {
			matrix[i][j] = a[now++];
			j++;
		}
		while (now < N && i < D) {
			matrix[i][j] = a[now++];
			i++;
		}
		while (now < N && j > L) {
			matrix[i][j] = a[now++];
			j--;
		}
		while (now < N && i > U) {
			matrix[i][j] = a[now++];
			i--;
		}
		U++, D--, L++, R--;
		i++, j++;
		if (now == N - 1) {
			matrix[i][j] = a[now++];
		}
	}
	for (int i = 1; i <= m; i++) {
		for (int j = 1; j <= n; j++) {
			cout << matrix[i][j];
			if (j < n)
				cout << " ";
			else
				cout << endl;
		}
	}
	system("pause");
	return 0;
}
```

## 1051 复数乘法

注意为零时的处理

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
#include <cmath>
using namespace std;
// !!!
// 如果A的绝对值小于0.01，A = 0。
// 如果B的绝对值小于0.01，B = 0。
int main() {
	// 复数乘法：!!! (a + bi)(c + di) = (ac－bd)+(bc + ad)i
	double r1, p1, r2, p2;
	double a, b, c, d, e, f;
	cin >> r1 >> p1 >> r2 >> p2;
	a = r1 * cos(p1);
	b = r1 * sin(p1);
	c = r2 * cos(p2);
	d = r2 * sin(p2);
	e = a * c - b * d;
	f = b * c + a * d;
	if (abs(e - 0) < 0.01)
		e = 0;
	if (abs(f - 0) < 0.01)
		f = 0;
	printf("%.2f", e);
	//if (b != 0)
	if (f >= 0)
		printf("+");
	printf("%.2fi", f);
	system("pause");
	return 0;
}
```

## 1052 🔺 卖个萌

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <string>
#include <vector>
#include <algorithm>
#include <cmath>
using namespace std;

int main() {
	vector<string> v[4];
	for (int i = 0; i < 3; i++) {
		string s;
		getline(cin, s);
		for (int j = 0; j < s.length(); j++) {  // !!!
			if (s[j] == '[') {
				for (int k = 0; j + k < s.length(); k++) {
					if (s[j + k] == ']') {
						v[i].push_back(s.substr(j + 1, k - 1));
						break;
					}
				}
			}
		}
	}

	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int a1, b1, c, b2, a2;
		scanf("%d %d %d %d %d", &a1, &b1, &c, &b2, &a2);
		if (a1 > v[0].size() || b1 > v[1].size() || c > v[2].size() || a2 > v[0].size() || b2 > v[1].size() || a1 < 1 || a2 < 1 || b1 < 1 || b2 < 1 || c < 1)
			printf("Are you kidding me? @\\/@\n");
		else
			cout << v[0][a1 - 1] << '(' << v[1][b1 - 1] << v[2][c - 1] << v[1][b2 - 1] << ')' << v[0][a2 - 1] << '\n';
	}
	return 0;
}

```

## 1053 住房空置率

```c++
#include <iostream>
#include <iomanip>
using namespace std;
double v[1001][100];

int main() {
	int n, d;
	double e;
	double poss, must;
	cin >> n >> e >> d;
	//住房套数 低电量阈值 观察期阈值 
	for (int i = 0; i < n; i++) {
		cin >> v[i][0];
		for (int j = 1; j <= v[i][0]; j++) {
			cin >> v[i][j];
		}
	}
	int tian = 0;
	int count = 0;
	poss = 0; must = 0;
	for (int i = 0; i < n; i++) {
		count = 0;
		tian = v[i][0] / 2;
		for (int j = 1; j <= v[i][0]; j++) {
			if (v[i][j] < e)
				count++;
		}
		if (tian < count && v[i][0] > d)//观察期大于观察期阈值 低电量天数大于观察期一半 
			must++;//空置 
		else if (count > tian)
			poss++;
	}

	poss = poss / (double)n * 100.0;  //!!!
	must = must / (double)n * 100.0;
	cout << fixed << setprecision(1) << poss << '%' << " " << must << '%' << endl; //!!!保留小数点1位
	return 0;
}
```

## 1053 住房空置率

??? case3 WA???

```c++
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;
double v[1001][100];

int main() {
	int n, d;
	double e;
	double poss, must;
	cin >> n >> e >> d;
	//住房套数 低电量阈值 观察期阈值 
	for (int i = 0; i < n; i++) {
		cin >> v[i][0];
		for (int j = 1; j <= v[i][0]; j++) {
			cin >> v[i][j];
		}
	}
	int tian = 0;
	int count = 0;
	poss = 0; must = 0;
	for (int i = 0; i < n; i++) {
		count = 0;
		//tian = ceil(v[i][0] / 2);
		tian = floor(v[i][0] / 2);
		for (int j = 1; j <= v[i][0]; j++) {
			if (v[i][j] < e)
				count++;
		}
		if (count > tian && v[i][0] > d)//观察期大于观察期阈值 低电量天数大于观察期一半 
			must++;//空置 
		else if (count > tian)
			poss++;
	}

	poss = poss / (double)n * 100.0;  //!!!
	must = must / (double)n * 100.0;
	cout << fixed << setprecision(1) << poss << '%' << " " << must << '%' << endl; //!!!保留小数点1位

	system("pause");
	return 0;
}

```

## 1054 ☆ 🔺 求平均值

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<cstdio>
#include<string.h>
using namespace std;

int main() {
	int n, cnt = 0;
	char a[50], b[50];
	double temp, sum = 0.0;
	cin >> n;
	for (int i = 0; i < n; i++) {
		scanf("%s", a);
		sscanf(a, "%lf", &temp);  //  从一个字符串中读进与指定格式相符的数据,否则无法读入 !!!
		sprintf(b, "%.2lf", temp);
		int flag = 0;
		for (int j = 0; j < strlen(a); j++) {  // 如果真是小数，sscanf前后前几位一定相同
			if (a[j] != b[j]) flag = 1;  // 否则不相同
		}
		if (flag || temp < -1000 || temp > 1000) {  // 判断是否是小数且大小符合要求
			printf("ERROR: %s is not a legal number\n", a);
			continue;
		}
		else {
			sum += temp;
			cnt++;
		}
	}
	if (cnt == 1) {
		printf("The average of 1 number is %.2lf", sum);
	}
	else if (cnt > 1) {
		printf("The average of %d numbers is %.2lf", cnt, sum / cnt);
	}
	else {
		printf("The average of 0 numbers is Undefined");
	}
}
```

## 1055 ??? 集体照

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<cstdio>
#include<string>
#include<cmath>
#include<algorithm>
using namespace std;

struct person {
	string name;
	int height;
};

person people[10001];
int matrix[11][1001];

bool cmp1(person a, person b){
	return a.height < b.height;  // 从小到大排序
}
bool cmp2(person a, person b) {
	return a.height > b.height;  // 从大到小排序
}

int main() {
	int n, k;
	cin >> n >> k;
	int rowNum = floor(n / k);
	int lastRowNum = n - rowNum * (k - 1);	
	for (int i = 0; i < n; i++) {
		cin >> people[i].name >> people[i].height;
	}
	stable_sort(people, people + n, cmp1);  // !!!稳定排序
	int i;
	for (i = 0; i < k - 1; i++) {
		sort(people + i * rowNum, people + (i + 1) * rowNum, cmp2);
	}
	stable_sort(people + (k - 1) * rowNum , people + n, cmp2);
	for (i = 0; i < k - 1; i++) {
		int offset = i * rowNum;
		int pos = floor(rowNum / 2) + 1;
		bool flag = true;
		for (int j = 0; j < rowNum; j++) {
			if (flag) {
				flag = false;
				pos -= j;
			}
			else {
				flag = true;
				pos += j;
			}
			matrix[i][pos] = j + offset;	
		}
	}
	int offset = i * rowNum;
	int pos = floor(lastRowNum / 2) + 1;
	bool flag = true;
	for (int j = 0; j < lastRowNum; j++) {
		if (flag) {
			flag = false;
			pos -= j;
		}
		else {
			flag = true;
			pos += j;
		}
		matrix[i][pos] = j + offset;
	}
	for (int j = 1; j <= lastRowNum; j++) {
		cout << people[matrix[k - 1][j]].name;
		if (j != lastRowNum)
			cout << " ";
		else
			cout << endl;
	}
	for (int i = k - 2; i >= 0; i--) {
		for (int j = 1; j <= rowNum; j++) {
			cout << people[matrix[i][j]].name;
			if (j != rowNum)
				cout << " ";
			else
				cout << endl;
		}
	}
	system("pause");
	return 0;
}
```

## 1056 组合数的和 ☆

```c++
#include <stdio.h>
int main()
{
	int a[10], n, i, j, sum = 0;
	scanf("%d", &n);
	for (i = 0; i < n; i++)
		scanf("%d", &a[i]);
	for (i = 0; i < n; i++)  // 就是两两逐一配对
	{
		for (j = 0; j < n; j++)
		{
			if (j != i)
				sum += a[i] * 10 + a[j];
		}
	}
	printf("%d", sum);
}

```

## ##1057 数零一

```c++
#include<iostream>
#include<string>
using namespace std;
int main()
{
	string s;//字符串数组
	getline(cin, s);
	int i,sum=0;
	for (i = 0; i < s.length(); i++)
	{
		if (s[i] >= 65 && s[i] <= 90)//算出大写字母的序号并加到和里去
			sum += s[i] - 64;
		if (s[i] >= 97 && s[i] <= 122)//算出小写字母的序号并加到和里去
			sum += s[i] - 96;
	}
	int result[2] = { 0 };//result[0]表示0的个数，result[1]表示1的个数
	while (sum)//利用除二取余法来统计出0,1的数量
	{
		result[sum % 2]++;  // !!!
		sum /= 2;
	}
	cout << result[0] << " " << result[1];
}

```

## 1061 判断题

```c++
#include <iostream>
#include <vector>
using namespace std;
int sum[101] = { 0 };

int main()
{
	int N = 0, M = 0;
	cin >> N >> M;
	vector <int> a(M);
	vector <int> b(M);
	vector <int> c(M);
	for (int i = 0; i < M; i++)
		cin >> a[i];
	for (int j = 0; j < M; j++)
		cin >> b[j];
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < M; j++) {
			cin >> c[j];
			if (c[j] != b[j])
				continue;
			else
				sum[i] += a[j];
		}
	}
	for (int j = 0; j < N; j++)
		cout << sum[j] << endl;
	return 0;
}

```

## 1066 图像过滤

```c++
#include <stdio.h>

int main()
{
    int N, M, A, B, C, D;

    scanf("%d %d %d %d %d", &M, &N, &A, &B, &C);

    for(int i = 0; i < M; i++)
        for(int j = 0; j < N; j++)
        {
            scanf("%d", &D);
            if(A <= D && D <= B)    D = C;
            printf("%03d%c", D, j == N - 1 ? '\n' : ' '); // !!!
        }

    return 0;
}
```

## 1071 小赌怡情

```c++
#include<iostream>
using namespace std;

int main() {
	int t1, k1, n1, b, t, n2, i, total = 0;
	cin >> t1 >> k1;
	total = t1;
	for (i = 0; i < k1; i++) {
		cin >> n1 >> b >> t >> n2;
		if (t > total)
			cout << "Not enough tokens.  Total = " << total << "." << endl;
		else if ((n1 > n2 && b == 0) || (n1 < n2 && b == 1)) {
			total += t;
			cout << "Win " << t << "!  Total = " << total << "." << endl;
		}
		else if ((n1 > n2 && b == 1) || (n1 < n2 && b == 0)) {
			total -= t;
			cout << "Lose " << t << ".  Total = " << total << "." << endl;
		}
		if (total == 0) {
			cout << "Game Over.";
			break;
		}
	}
	return 0;
}

```

## 1076 wifi密码

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include <cstdio>
int main() {
	int n;
	scanf("%d", &n);
	getchar();  // 吸收换行!!!
	for (int i = 0; i < n; i++) {
		char a, b;
		for (int j = 0; j < 4; j++) {
			scanf("%c-%c", &a, &b); 
			getchar();  // 吸收空格!!!
			if (a == 'A'&&b == 'T') printf("1");
			else if (a == 'B'&&b == 'T') printf("2");
			else if (a == 'C'&&b == 'T') printf("3");
			else if (a == 'D'&&b == 'T') printf("4");
		}
	}
	system("pause");
	return 0;
}

```

## 1081 检查密码

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include <cstdio>
#include<string>
using namespace std;

int main() {
	int N;
	cin >> N;
	getchar();
	string password;
	while (N--)
	{
		getline(cin, password);
		int len = password.length();
		if (len < 6) {
			printf("Your password is tai duan le.\n"); 
			continue;
		}
		else {
			int flag = 1; int num = 0; int letter = 0; int success = 0;
			for (int i = 0; password[i] != '\0'; i++)
			{
				if (password[i] >= '0'&&password[i] <= '9') num = 1;
				else if ((password[i] >= 'A'&&password[i] <= 'Z') || (password[i] >= 'a'&&password[i] <= 'z'))letter = 1;
				else if (password[i] != '.') {
					flag = 0;	break;
				}
			}
			if (flag == 0)printf("Your password is tai luan le.\n");
			else if (letter == 0) { printf("Your password needs zi mu.\n"); flag = 0; }
			else if (num == 0) { printf("Your password needs shu zi.\n"); flag = 0; }
			else if (flag == 1)printf("Your password is wan mei.\n");
		}
	}
}

```

## 1086 就不告诉你

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include <cstdio>
#include <string.h>
#include <algorithm>
using namespace std;
char str[101];

int main() {
	int a, b, c;
	cin >> a >> b;
	c = a * b;
	sprintf(str, "%d", c);
	int len = strlen(str);
	reverse(str, str + len);
	bool flag = true;
	for (int i = 0; i < len; i++) {
		if (flag && str[i] == '0')
			continue;
		if (flag && str[i] != '0')
			flag = false;
		cout << str[i];
	}
	system("pause");
	return 0;
}

```

## 1091 ☆ N-自守数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cmath>
#include <string>
using namespace std;

int main() {
	int M, j;	
	scanf("%d", &M);
	for (int i = 0; i < M; i++) {
		string s, str, sub;
		int tmp, res, lens;	
		cin >> s;
		tmp = stoi(s);  // !!!string 转int
		lens = s.length();
		for (j = 1; j < 10; j++) {
			str = to_string(int(j*pow(tmp, 2)));  // !!! int转string
			sub = str.substr(str.length() - lens);  // 从这一位才开始取子串
			if (sub == s) {
				printf("%ld %ld\n", j, int(j*pow(tmp, 2)));
				break;
			}
		}
		if (j == 10)	printf("No\n");
	}
	return 0;
}
```

## 1058 选择题 ☆ 🔺

1. set的巧用

2. 格式化输入，scanf
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <cstdio>
#include <vector>
#include <set>
using namespace std;

int main() {
	int n, m, temp, k;
	scanf("%d%d", &n, &m);
	vector<set<char>> right(m);   // !!!巧用set保存对的选项
	vector<int> total(m), wrongCnt(m);
	for (int i = 0; i < m; i++) {
		scanf("%d%d%d", &total[i], &temp, &k);
		for (int j = 0; j < k; j++) {
			char c;
			scanf(" %c", &c);
			right[i].insert(c);  // !!! 
		}
	}
	for (int i = 0; i < n; i++) {
		int score = 0;
		scanf("\n");
		for (int j = 0; j < m; j++) {
			if (j != 0) scanf(" ");
			scanf("(%d", &k);
			set<char> st;
			char c;
			for (int l = 0; l < k; l++) {
				scanf(" %c", &c);  // !!!吸收格式中的空格
				st.insert(c);
			}
			scanf(")");
			if (st == right[j]) {  // 直接用集合相等来判断是否全对!!!
				score += total[j];
			}
			else {
				wrongCnt[j]++;
			}
		}
		printf("%d\n", score);
	}
	int maxWrongCnt = 0;
	for (int i = 0; i < m; i++) {
		if (wrongCnt[i] > maxWrongCnt) {
			maxWrongCnt = wrongCnt[i];
		}
	}
	if (maxWrongCnt == 0)
		printf("Too simple");
	else {
		printf("%d", maxWrongCnt);
		for (int i = 0; i < m; i++) {  // !!!输出所有次数最大的题目
			if (wrongCnt[i] == maxWrongCnt) {
				printf(" %d", i + 1);
			}
		}
	}
	return 0;
}
```

## 1059 C语言竞赛

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <vector>
#include <string>
#include <map>
#include <cmath>
using namespace std;
map<string, int> record;
map<string, bool> checked;
bool isPrime(int n) {  // 判断n是否为素数
	if (n <= 1)
		return false;
	int sqr = (int)sqrt(1.0 * n);
	for (int i = 2; i <= sqr; i++) {
		if (n % i == 0)
			return false;
	}
	return true;
}

int main() {
	int k;
	string str;
	cin >> k;
	for (int no = 1; no <= k; no++) {
		cin >> str;
		record[str] = no;
		checked[str] = false;
	}
	cin >> k;
	while (k--) {
		cin >> str;
		map<string, int>::iterator it = record.find(str);
		if (it == record.end()) {
			printf("%s: Are you kidding?\n", str.c_str());
			continue;
		}
		else if (checked[str])
			printf("%s: Checked\n", str.c_str());
		else if (it->second == 1) 
			printf("%s: Mystery Award\n", str.c_str());
		else if(isPrime(it->second))
			printf("%s: Minion\n", str.c_str());
		else
			printf("%s: Chocolate\n", str.c_str());
		checked[str] = true;
	}
	system("pause");
	return 0;
}
```

## 1060 ☆ 爱丁堡数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <vector>
#include <string>
#include <map>
#include <cmath>
#include <algorithm>
using namespace std;

bool cmp(int a, int b) {
	return a > b;  // 从大到小排序
}

int main() {
	int k, tmp, ans;
	vector<int> record;
	cin >> k;
	for (int i = 0; i < k; i++) {
		cin >> tmp;
		record.push_back(tmp);
	}	
	sort(record.begin(), record.end(), cmp);  // !!! 核心思路
	//试想，如果给定10天骑行距离，并且每一天的骑行距离都超过了10，那么当扫描的时候，
	//不会有第E天的骑行距离没有超过E，那么就无法输出最终结果，
	//所以在扫之前判断一下最小的骑行距离是否超过给定的天数，如果超过，则直接输出结果即可。
	if (record[record.size() - 1] > k) {
		cout << k << endl;
	}
	else {
		for (int i = 0; i < record.size(); i++) {
			if (record[i] <= i + 1) {  // 注意是超过，要加等号，所给测试样例就是
				ans = i;
				break;
			}
		}
		cout << ans << endl;
	}
	system("pause");
	return 0;
}
```

## 1062. ☆ 最简分数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <vector>
#include <string>
#include <map>
#include <cmath>
#include <algorithm>
using namespace std;
struct Fraction {  // 分数
	int up, down;  // 分子、分母
	double value;
};

int gcd(int a, int b) {  // 求a与b的最大公约数
	return b == 0 ? a : gcd(b, a % b);  // !!!辗转相除法, 记住
}

int main() {
	int k;
	vector<int> res;
	double value;
	Fraction a, b, c;
	scanf("%d/%d %d/%d", &a.up, &a.down, &b.up, &b.down);
	cin >> k;
	a.value = (double)a.up / (double)a.down;
	b.value = (double)b.up / (double)b.down;
	if (a.value > b.value) {  // !!!两个输入的分数不一定是左小右大，要自己判断
		c = a;
		a = b;
		b = c;
	}
	for (int i = 1; ; i++) {
		int g = gcd(i, k);
		if (g != 1)
			continue;  // 不是最简分数
		value = (double)i / (double)k;
		if (value <= a.value)  // !!!不能等于边界!!!，虽然是double,等号生效2333
			continue;
		if (value < b.value)
			res.push_back(i);
		if (value >= b.value)
			break;
	}
	for (int j = 0; j < res.size(); j++) {
		if (j != 0)
			cout << " ";
		printf("%d/%d", res[j], k);
	}
	system("pause");
	return 0;
}
```

## 1063 计算谱半径

```c++
#include<cstdio>
#include<algorithm>
#include<cmath>
using namespace std;

int n;
double a,b,ans=0;
double f(double a,double b){
	return sqrt((a*a+b*b));
}

int main(){
	scanf("%d",&n);
	while(n--){
		scanf("%lf%lf",&a,&b);
		ans = max(ans,f(a,b));
	}
	printf("%.2f\n",ans);
	return 0;
}
```



## 1064 朋友数

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <vector>
#include <string>
#include <map>
#include <cmath>
#include <algorithm>
using namespace std;
bool friendNum[64] = { false };

int calc(int c) {
	int friendNum = 0;
	while (c) {
		friendNum += c % 10;
		c /= 10;
	}
	return friendNum;
}
// 题目保证所有数字小于 10^4
int main() {
	vector<int> v;
	int n;
	cin >> n;
	while(n--) {
		int m, sum = 0;
		cin >> m;
		int res = calc(m);
		if (friendNum[res] == false) {
			v.push_back(res);
			friendNum[res] = true;
		}
	}
	sort(v.begin(), v.end());
	cout << v.size() << endl;
	for (int i = 0; i < v.size(); i++) {
		if (i != 0)
			cout << " ";
		cout << v[i];
	}
	system("pause");
	return 0;
}
```

## 1067 试密码

```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
	string s;
	int N, cnt = 0;	
	cin >> s >> N;
	getchar();
	string tmp;
	getline(cin, tmp);
	while (cnt < N && tmp != "#") {
		if (tmp == s) {
			cout << "Welcome in" << endl;
			break;
		}
		else	
			cout << "Wrong password: " << tmp << endl;
		cnt++;
		getline(cin, tmp);
	}
	if (cnt == N)	
		cout << "Account locked" << endl;
	return 0;
}
```

## 1065 单身狗

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <string>
#include <map>
#include <vector>
#include <algorithm>
using namespace std;

map<string, string> couple;
map<string, bool> attend;
vector<string> guests;
vector<string> dog;

int main() {
	int k, sum = 0;
	cin >> k;
	string a, b;
	while (k--) {
		cin >> a >> b;
		couple[a] = b;
		couple[b] = a;
		attend[a] = attend[b] = false;
	}
	cin >> k;
	for (int i = 0; i < k; i++) {
		cin >> a;
		attend[a] = true;
		guests.push_back(a);
	}

	map<string, string>::iterator it;
	for (int i = 0; i < k; i++) {
		it = couple.find(guests[i]);
		if (it != couple.end() && attend[it->second] == true)
			continue;
		else {
			dog.push_back(guests[i]);
			sum++;
		}
	}
	sort(dog.begin(), dog.end());
	cout << dog.size() << endl;
	for (int i = 0; i < dog.size(); i++) {
		if (i != 0)
			cout << " ";
		cout << dog[i];
	}
	system("pause");
	return 0;
}
```

## 1068 万绿丛中一点红 

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<cstdio>
#include<iostream>
#include<cstring>
#include<map>
#include<cmath>
using namespace std;

map<int, int> vis;
int s[1001][1001];
int n, m, tol;
//int dir[8][2] = { 1,0, -1,0, 0,1, 0,-1, 1,1, 1,-1, -1,1, -1,-1 };  // !!! 以此遍历四个方向
int dir[8][2] = { {1,0}, {-1,0}, {0,1}, {0,-1}, {1,1}, {1,-1}, {-1,1}, {-1,-1}};  // !!! 以此遍历四个方向
//判断是否大于阈值 
bool check(int x, int y)
{
	for (int i = 0; i < 8; i++) {
		int xx = x + dir[i][0];
		int yy = y + dir[i][1];
		if (xx >= 0 && xx < n && yy < m && yy >= 0 && abs(s[xx][yy] - s[x][y]) <= tol) return false;
	}
	return true;
}

int main() {
	cin >> m >> n >> tol;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			cin >> s[i][j];
			vis[s[i][j]] ++;
		}
	}
	//cnt记录只出现一次的数字的个数
	//x y记录坐标 
	int cnt = 0;
	int x, y;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			if (vis[s[i][j]] == 1 && check(i, j)) {
				cnt++;
				x = i;
				y = j;
			}
		}
	}

	if (cnt == 1) {
		printf("(%d, %d): %d\n", y + 1, x + 1, s[x][y]);
	}
	else if (cnt > 1) {
		puts("Not Unique");
	}
	else {
		puts("Not Exist");
	}
	return 0;
}

```

## 1069 微博转发抽奖

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<cstdio>
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<map>
using namespace std;

map<string, bool> got;
int main() {
	int sum = 0;  // 领到礼品的人数
	string str;
	vector<string>  strs;
	int m, n, start;
	cin >> m >> n >> start;
	for (int i = 0; i < m; i++) {
		cin >> str;
		strs.push_back(str);
		got[str] = false;
	}
	if (m < start) {
		cout << "Keep going...";
		system("pause");
	}
	bool flag = false;
	for (int i = start - 1; i < m; i+=n) {
		while (got[strs[i]] == true) {
			i++;
			if (i == n) {
				flag = true;
				break;
			}
		}
		if (flag)
			break;
		cout << strs[i] << endl;
		got[strs[i]] = true;
	}
	system("pause");
	return 0;
}

```

## 1070 ☆ 结绳

```c++

#include<cstdio>
#include<algorithm>
#define MAXN 10001
using namespace std;
int n, a[MAXN];
int main()
{
	scanf("%d",&n);
	for(int i=0; i<n; i++) scanf("%d", &a[i]);
	sort(a,a+n);
	int ans = a[0];
	for(int i=1 ;i<n ;i++){
		ans = (ans + a[i])/2;
	}
	printf("%d\n",ans);
} 

/*
贪心法，一定要将这些绳子按照长度排序，
而选择身子的顺序无非有三种，从小到大，从大到小，从中间到两边。
绳子每进行一次连接就要对折一次，长度就会减少一半，那么应该让较长的绳子尽量少对折。
按照这样的想法，那么应该将绳子从小到大排列，两两进行串连。
*/
```

## 1072 开学寄语

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<cstdio>
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<map>
#include <set>
using namespace std;

int main() {
	int n, m, i, j, k;
	string x, y;
	set<string> s;  // !!! 用set存储这些string
	int stu = 0, cnt = 0;
	scanf("%d %d", &n, &m);
	for (i = 0; i < m; i++) {
		cin >> x;
		s.insert(x);
	}
	for (i = 0; i < n; i++) {
		int flag = 0;
		cin >> x >> k;
		for (j = 0; j < k; j++) {
			cin >> y;
			if (s.count(y)) {  // !!!set.count()
				if (!flag) {
					flag = 1;
					stu++;
					cout << x << ":";
				}
				cnt++;
				cout << " " << y;
			}
		}
		if (flag) {
			printf("\n");
		}
	}
	printf("%d %d\n", stu, cnt);
	return 0;
}
```





