# PAT (Basic Level) Practice （中文）

## 略的题目

06、21、31、02、48、

15、38、33、43、47、



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

