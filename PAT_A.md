# PAT (Advanced Level) Practice

## 略的题

06、36、05、35、62、

28、16、55、75、83、

80、

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

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <string>
#include <vector>
using namespace std;

string  num[10] = { "ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu" };
string wei[5] = { "Shi", "Bai", "Qian", "Wan", "Yi" };

int main() {
	string str;
	cin >> str;
	int len = str.length();
	int left = 0, right = len - 1;
	if (str[0] == '-') {
		cout << "Fu";
		left++;
	}
	while (left + 4 <= right) {
		right -= 4;  // 将right每次左移4位，直到left与right现同在最高节。 
	}

	while (left < len) {
		bool flag = false;
		bool isPrint = false;
		while (left <= right) {
			if (left > 0 && str[left] == '0') {
				flag = true;
			}
			else {
				if (flag == true) {
					cout << " ling";
					flag = false;
				}
				if (left > 0)
					cout << " ";
				cout << num[str[left] - '0'];
				isPrint = true;
				if (left != right) {
					cout << " " << wei[right - left - 1];
				}
			}
			left++;
		}
		if (isPrint == true && right != len - 1) {
			cout << " " << wei[(len - 1 - right) / 4 + 2];
		}
		right += 4;
	}
	system("pause");
	return 0;
}
```

## 1025 PAT Ranking

主要思想：

先组内排序，存储好顺序后，再总体排序（反过来亦可）。两种排名方式逐个击破。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

struct student{
	string id;
	int score;
	int location_num;
	int final_rank;
	int local_rank;
};

bool cmp(student a, student b) {  // 排序比较函数
	if (a.score != b.score) return a.score > b.score;  // 先按分数从高到低排序
	else return a.id < b.id;  // 分数相同按准考证号从小到大排序。
}

int main() {
	int location_sum[101] = {0};  // 每个考场的对应人数
	int location_num;
	vector<student> students;

	cin >> location_num;
	for (int i = 1; i <= location_num; i++) {
		cin >> location_sum[i];
		for (int j = 1; j <= location_sum[i]; j++) {
			student stu;
			stu.location_num = i;
			cin >> stu.id >> stu.score;
			students.push_back(stu);
		}
	}

	int sum = 0;
	for (int i = 1; i <= location_num; i++) {  // 对每个考场内考生分别进行成绩排序
		sort(students.begin() + sum, students.begin() + sum + location_sum[i], cmp);
		int rank = 1;
		for (int j = sum; j < sum + location_sum[i]; j++) {
			students[j].local_rank = rank++;
			if (j != location_sum[i - 1] && students[j].score == students[j - 1].score)
				students[j].local_rank = students[j - 1].local_rank;
		}
		sum += location_sum[i];
	}

	sort(students.begin(), students.end(), cmp);  // 对整个考场内考生进行成绩排序
	for (int i = 0; i < students.size(); i++) {
		students[i].final_rank = i + 1;
		if (i != 0 && students[i].score == students[i - 1].score)
			students[i].final_rank = students[i - 1].final_rank;
	}

	cout << students.size() << endl;
	for (int i = 0; i < students.size(); i++) {
		cout << students[i].id;
		printf(" %d %d %d\n", students[i].final_rank, students[i].location_num, students[i].local_rank);
	}
	system("pause");
	return 0;
}
```

## 1012 ☆ the best rank

利用全局变量row，对比较函数cmp进行“传参”，以实现对不同分数的比较，妙~~

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

struct student{
	string id;
	int grade[4];  // 四个分数——average，computer，math，English
	int rank[4];  // 对应四门课的排名 ACME
};

int now;  // 当前比较科目 

bool cmp(student a, student b) {  // 排序比较函数
	return a.grade[now] > b.grade[now];
}

char subjects[4] = { 'A','C','M','E' };

pair<int, char> minRank(student& a) {  // 找出该学生最靠前的排名和学科
	int subject, minRank = INT_MAX, minSubject = -1;
	for (subject = 0; subject < 4; subject++) {
		if (a.rank[subject] < minRank) {
			minSubject = subject;
			minRank = a.rank[subject];
		}
	}
	return make_pair(minRank, subjects[minSubject]);
}

int main() {
	int n, m;
	vector<student> students;
	student tmp;
	cin >> n >> m;

	/* input */
	for (int i = 0; i < n; i++) {
		cin >> tmp.id >> tmp.grade[1] >> tmp.grade[2] >> tmp.grade[3];
		tmp.grade[0] = tmp.grade[1] + tmp.grade[2] + tmp.grade[3];
		students.push_back(tmp);
	}
	/* processing —— sort */
	for (now = 0; now < 4; now++) {
		sort(students.begin(), students.end(), cmp);
		students[0].rank[now] = 1;
		for (int i = 1; i < students.size(); i++) {
			if (students[i].grade[now] == students[i - 1].grade[now])
				students[i].rank[now] = students[i - 1].rank[now];
			else
				students[i].rank[now] = i + 1;
		}
	}

	/* output */
	string id;
	int pos;
	for (int i = 0; i < m; i++) {
		cin >> id;
		pos = -1;
		for (int j = 0; j < students.size(); j++) {
			if (students[j].id == id) {
				pos = j;
				break;
			}
		}
		if (pos == -1) {
			cout << "N/A" << endl;
		}
		else {
			pair<int, char> res = minRank(students[pos]);
			cout << res.first << " " << res.second << endl;
		}
	}
	
	system("pause");
	return 0;
}
```

## 1095 ☆ Cars on Campus  ???

???

原来样例4超时，后来变成了答案错误，尚未解决。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <limits.h>
#include <string>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;


struct car {
	string id;  // 车牌号
	int time;  // 记录的时刻（以s为单位）
	string status;   // in 或 out
};

struct record {
	string id;  // 车牌号
	int inTime;  // 进入时间
	int outTime;  // 驶出时间
};

vector<car> allRecords;
vector<record> valid; // 所有有效记录, 用于最后统计特定时间内的校内车辆数
map<string, int> parkTime;  // 车牌号 → 总停留时间, 用以最后统计停车时间最长的车

// 先按车牌号字典序从小到大排序，若车牌号相同，则按时间从小到大排序
bool cmpByIdAndTime(car a, car b) {
	if (a.id != b.id)
		return a.id < b.id;
	else
		return a.time < b.time;
}
// 将有效记录按进入时间从小到大排序
bool cmpByInTime(record a, record b) {
	return a.inTime < b.inTime;
}

int timeToInt(int hh, int mm, int ss) {
	return hh * 3600 + mm * 60 + ss;
}

int main() {
	int n, k, hh, mm, ss;
	scanf("%d %d", &n, &k);
	car tmp;
	record recordTmp;
	for (int i = 0; i < n; i++) {
		cin >> tmp.id;  //  cin >> string 是可以的
		scanf("%d:%d:%d", &hh, &mm, &ss);
		tmp.time = timeToInt(hh, mm, ss);
		cin >> tmp.status;  // 转换为以s为计时单位
		allRecords.push_back(tmp);
	}
	sort(allRecords.begin(), allRecords.end(), cmpByIdAndTime);  // 按车牌号和时间排序

	int maxTime = -1;  // 最长停留时间
	for (int i = 0; i < n - 1; i++) {  // 遍历所有记录
		if (allRecords[i].id == allRecords[i].id
			&& allRecords[i].status == "in"   // 第i条是in记录
			&& allRecords[i + 1].status == "out") {  // 第i+1条是out记录

			recordTmp.id = allRecords[i].id;
			recordTmp.inTime = allRecords[i].time;
			recordTmp.outTime = allRecords[i + 1].time;
			valid.push_back(recordTmp);  // 这是一条有效的记录

			int inTime = allRecords[i + 1].time - allRecords[i].time;
			if (parkTime.count(allRecords[i].id) == 0) {  // map中还没这个车牌号，置零
				parkTime[allRecords[i].id] = 0;
			}
			parkTime[allRecords[i].id] += inTime;   // 增加这个车牌号的总停留时间
			maxTime = max(maxTime, parkTime[allRecords[i].id]);  // 更新最大总停留时间
		}
	}
	sort(valid.begin(), valid.end(), cmpByInTime);

	int now = 0;  // 所给输入的时间点是递增的，故可只对valid的遍历进行优化, 不然会超时。!!!
	//now 表示上次时间点之后的记录所在下标
	for (int i = 0; i < k; i++) {
		scanf("%d:%d:%d", &hh, &mm, &ss);
		int currentTime = timeToInt(hh, mm, ss);
		int numCar = 0;
		bool first = true;
		//for (int j = now; valid[j].inTime <= currentTime && j < valid.size();) {
		for (int j = now; j < valid.size() && valid[j].inTime <= currentTime; j++) {  // 此二者不能颠倒，不然会发生数组越界
			if (valid[j].outTime > currentTime) { 
				numCar++;
				if (first) {
					now = j;
					first = false;
				}
			}
		}
		printf("%d\n", numCar);
	}

	map<string, int>::iterator it;
	for (it = parkTime.begin(); it != parkTime.end(); it++) {
		if (it->second == maxTime)  // 输出所有最长总停留时间的车牌号
			printf("%s ", it->first.c_str());
	}
	printf("%02d:%02d:%02d\n", maxTime / 3600, maxTime % 3600 / 60, maxTime % 60);
	system("pause");
	return 0;
}
```

## 1041 Be Unique

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
using namespace std;

int hashTable[10001] = { 0 };
int num[100001];
// 注意此题应保存原来的数字输入顺序!!!


int main() {
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> num[i];
		hashTable[num[i]]++;
	}
	int ans = -1;
	for (int i = 0; i < n; i++) {
		if (hashTable[num[i]] == 1) {
			ans = num[i];
			break;
		}
	}
	if (ans == -1)
		cout << "None";
	else
		cout << ans;
	system("pause");
	return 0;
}
```

## String Subtraction

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
using namespace std;

int main() {
	bool hashTable[128];
	for (int i = 0; i < 128; i++) {
		hashTable[i] = true;
	}

	string s1, s2;
	int pos;
	getline(cin, s1);
	getline(cin, s2);

	for (int i = 0; i < s2.size(); i++) {
		pos = s2[i];
		hashTable[pos] = false;
	}

	for (int i = 0; i < s1.size(); i++) {
		pos = s1[i];
		if (hashTable[pos])
			cout << s1[i];
	}

	system("pause");
	return 0;
}
```

## 1048 🔺Find Coins

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
/* !!!
  如果申明的是全局/静态数组，系统会把数组的内容自动初始化为0。
  如果申明的是局部数组，数组的内容会是随机的，不一定是0。
  */
int hashTable[1005];

int main() {
	int n, m, a;
	//int hashTable[1005] = {0};  // !!! 不过这样也可以全部置零，不过只能置零，其他的数只是设置第一个。
	cin >> n >> m;
	for (int i = 0; i < n; i++) {
		cin >> a;
		hashTable[a]++;
	}
	
	for (int i = 1; i < m; i++) {
		if (hashTable[i] && hashTable[m - i]) { // 找到一对数，它们和为ms
			if (i == m - i && hashTable[i] <= 1)  // i == m - i 时必须保证数字i的个数大于等于2
				continue;
			cout << i << ' ' << m - i;
			system("pause");
			return 0;
		}
	}

	cout << "No Solution";
	system("pause");
	return 0;
}

```

## 1037 ☆ magic coupon

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 100001;

int main() {
	int n, m, coupon[maxn], product[maxn];
	cin >> n;
	for (int i = 0; i < n; i++) 
		cin >> coupon[i];
	cin >> m;
	for (int i = 0; i < m; i++)
		cin >> product[i];
	sort(coupon, coupon + n);
	sort(product, product + m);
	int i, j, ans = 0;
	for (i = 0; i < n && i < m && coupon[i] < 0 && product[i] < 0; i++) {
		ans += coupon[i] * product[i];
	}
	for (i = n - 1, j = m - 1; i >= 0 && j >= 0 && coupon[i] > 0 && product[j] > 0; i--, j--) {
		ans += coupon[i] * product[j];
	}
	cout << ans;
	system("pause");
	return 0;
}

```

## 1067 🔺 ??? Sort with swap(0, i)

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 100001;
int pos[maxn];

int main() {
	int n, ans = 0;  // ans 表示
	cin >> n;
	int left = n - 1, num;
	for (int i = 0; i < n; i++) {
		cin >> num;  // num 所在位置为i
		pos[num] = i;
		if (num == i && num != 0) {
			left--;
		}
	}
	int k = 1;
	while (left > 0) {
		if (pos[0] == 0) {
			while (k < n) {
				if (pos[k] != k) {
					swap(pos[0], pos[k]);
					ans++;
					break;
				}
				k++;
			}
		}
		while(pos[0] != 0) {
			swap(pos[0], pos[pos[0]]);
			ans++;
			left--;
		}
	}
	cout << ans;
	system("pause");
	return 0;
}
```

## 1038 ☆Recover the Smallest Number

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

bool cmp(string a, string b) {
	return a + b < b + a;  // 如果a+b<b+a，就把a排在前面
}
string nums[10001];

int main() {
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> nums[i];
	}
	sort(nums, nums + n, cmp);
	string ans;
	for (int i = 0; i < n; i++) {
		ans += nums[i];
	}
	while (ans[0] == '0' && ans.size() != 0) {
		ans.erase(ans.begin());
	}
	if (ans.size() == 0)
		cout << 0;
	else
		cout << ans;
	system("pause");
	return 0;
}
```

## 1063 Set Similarity

set的用法，并集，交集。

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <vector>
#include <cstring>
#include <set>
#include <algorithm>
using namespace std;
const int N = 51;
set<int> st[N];  // N 个集合

// 比较集合x和集合y
void compare(int x, int y) {
	int totalNum = st[y].size(), sameNum = 0;
	for (set<int>::iterator it = st[x].begin(); it != st[x].end(); it++) {
		if (st[y].find(*it) != st[y].end())
			sameNum++;
		else
			totalNum++;
	}
	printf("%.1f%%\n", sameNum * 100.0 / totalNum);  // 输出比率
}

int main() {
	int n, k, q, v, st1, st2;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> k;
		for (int j = 0; j < k; j++) {
			cin >> v;
			st[i].insert(v);
		}
	}
	cin >> q;
	for (int i = 0; i < q; i++) {
		cin >> st1 >> st2;
		compare(st1, st2);
	}
	system("pause");
	return 0;
}

```

## 1060 ☆ 🔺 Are They Equal  

保留三位的科学计数法

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
using namespace std;
int n;
string deal(string s, int &e) {
	int k = 0;  // s的下标
	while (s.length() > 0 && s[0] == '0') {
		s.erase(s.begin());  // 去掉s的前导零 !!!
	}
	if (s[0] == '.') {  // 若去掉前导零后是小数点，说明s是小于1的小数
		s.erase(s.begin());  // 去掉小数点
		while (s.length() > 0 && s[0] == '0') {
			s.erase(s.begin());  // 去掉小数点后非零位前的所有零
			e--;  // 每去掉一个0，指数e减1
		}
	}
	else {  // 若去掉前导零后不是小数点，则找到后面的小数点删除
		while (k < s.length() && s[k] != '.') {  // 寻找小数点
			k++;
			e++;  // 只要不遇到小数点，就让指数e++
		}
		if (k < s.length()) {  // while结束后k < s.length(), 说明遇到了小数点
			s.erase(s.begin() + k);  // 把小数点删除 
		}
	}
	if (s.length() == 0) {
		e = 0;  // 如果去除前导零后s的长度为0，则说明这个数本身为0
	}
	int num = 0;
	k = 0;
	string res;
	while (num < n) {  // 只要精度没有到n
		if (k < s.length())
			res += s[k++];
		else
			res += '0';
		num++;
	}
	return res;
}

int main() {
	string s1, s2, s3, s4;
	cin >> n >> s1 >> s2;
	int e1 = 0, e2 = 0;   // e1, e2为s1, s2的指数
	s3 = deal(s1, e1);
	s4 = deal(s2, e2);
	if (s3 == s4 && e1 == e2) {  // 主体相同且指数相同
		cout << "YES 0." << s3 << "*10^" << e1 << endl;
	}
	else {
		cout << "NO 0." << s3 << "*10^" << e1 << " 0." << s4 << "*10^" << e2 << endl;
	}
	system("pause");
	return 0;
}
```

## 1054 ☆ The Dominant Color

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
#include <map>
using namespace std;

int main() {
	int n, m, col;
	cin >> n >> m;
	map<int, int> count;  // 数字与出现次数的map映射
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			scanf("%d", &col);
			if (count.find(col) != count.end())
				count[col]++;
			else
				count[col] = 1;
		}
	}

	int k = 0, MAX = 0;
	for (map<int, int>::iterator it = count.begin(); it != count.end(); it++) {
		if (it->second > MAX) {
			k = it->first;
			MAX = it->second;
		}
	}
	cout << k << endl;
	system("pause");
	return 0;
}
```

## 1071 ☆ 🔺 speech patterns

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <string>
#include <map>
using namespace std;

bool check(char c) {  // 检查字符c是否是[0,9]、[A,Z]、[a,z]
	if (c >= '0' && c <= '9') return true;
	if (c >= 'a' && c <= 'z') return true;
	if (c >= 'A' && c <= 'Z') return true;
	return false;
}

int main() {
	map<string, int> count;   // 统计字符串出现的次数
	string str;
	getline(cin, str);
	int i = 0;  // 下标
	while (i < str.length()) {
		string word;
		//将一句话拆分成单词!!!
		while (i < str.length() && check(str[i]) == true) {   // 如果是单词的字符
			if (str[i] >= 'A' && str[i] <= 'Z') {
				str[i] += 32;  // 将大写字母转换成小写字母
			}
			word += str[i];
			i++;
		}
		if (count.find(word) == count.end())
			count[word] = 1;
		else
			count[word]++;
		while (i < str.length() && check(str[i]) == false)  // 跳过非单词字符!!!
			i++;
	}
	string ans;
	int MAX = 0;
	for (map<string, int>::iterator it = count.begin(); it != count.end(); it++) {
		if (it->second > MAX) {
			MAX = it->second;
			ans = it->first;
		}
	}
	cout << ans << " " << MAX << endl;
	system("pause");
	return 0;
}
```

