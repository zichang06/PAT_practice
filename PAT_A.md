# PAT (Advanced Level) Practice

## 略的题

06、36、05、35、62、

28、16、55、75、83、

80、62

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
```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

int a[100001];
// 法2：
// !!! 二分法，二分查找
// left 和 right 初始分别为0为n-1，key即m-a[i]
int bin(int left, int right, int key) {
	int mid;
	while (left <= right) {
		mid = (left + right) / 2; 
		if (a[mid] == key)
			return mid;
		else if (a[mid] > key)
			right = mid - 1;
		else
			left = mid + 1;
	}
	return -1;  // 如果没有找到Key,返回-1
}

int main() {
	int n, m, i;
	cin >> n >> m;
	for (i = 0; i < n; i++) {
		cin >> a[i];		
	}
	sort(a, a + n);
	for (i = 0; i < n; i++) {
		int pos = bin(0, n - 1, m - a[i]);
		if (pos != -1 && i != pos) { // 找到一对数，它们和为ms
			cout << a[i] << ' ' << m - a[i];
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

## 1022  ☆ Digital Library

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <string>
#include <map>
#include <set>
using namespace std;

// 5个map变量分别建立书名、作者、关键词、出版社及出版年份与id的映射关系!!!
map<string, set<int>> mpTitle, mpAuthor, mpKey, mpPub, mpYear;

void query(map<string, set<int>>& mp, string& str) {
	if (mp.find(str) == mp.end())
		printf("Not Found\n");
	else {
		for (set<int>::iterator it = mp[str].begin(); it != mp[str].end(); it++) {
			printf("%07d\n", *it);
		}
	}
}
int main() {
	int n, m, id, type;
	string title, author, key, pub, year;
	scanf("%d", &n);
	for (int i = 0; i < n; i++) {
		scanf("%d", &id);
		getchar();
		getline(cin, title);
		mpTitle[title].insert(id);
		getline(cin, author);
		mpAuthor[author].insert(id);
		// 每本书对应多个关键词!!!
		while (cin >> key) {  // 每次读入单个关键词key
			mpKey[key].insert(id);  // 
			char c = getchar();  // 接收关键词Key之后的字符
			if (c == '\n') break;
		}
		getline(cin, pub);
		mpPub[pub].insert(id);
		getline(cin, year);
		mpYear[year].insert(id);
	}

	string temp;
	scanf("%d", &m);  // 查询次数
	for (int i = 0; i < m; i++) {
		scanf("%d: ", &type);  // 查询类型!!!
		getline(cin, temp);
		cout << type << ": " << temp << endl;
		if (type == 1) query(mpTitle, temp);
		else if (type == 2) query(mpAuthor, temp);
		else if (type == 3) query(mpKey, temp);
		else if (type == 4) query(mpPub, temp);
		else query(mpYear, temp);
	}
	system("pause");
	return 0;
}
```

## 1051 ☆ 🔺 Pop Sequence

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <stack>
using namespace std;
const int maxn = 1001;
int arr[maxn];  // 保存题目给定的出栈序列
stack<int> st;  // 定义栈st

int main() {
	int m, n, t;
	scanf("%d%d%d", &m, &n, &t);
	while (t--) { // 循环执行k次
		while (!st.empty()) {  // 清空栈
			st.pop();
		}
		for (int i = 1; i <= n; i++) {
			scanf("%d", &arr[i]);
		}
		int current = 1;  // 指向栈序列中的待出栈元素
		bool flag = true;
		for (int i = 1; i <= n; i++) {
			st.push(i);
			if (st.size() > m) {  // 如果此时栈中元素个数大于容量m,则序列非法
				flag = false;
				break;
			}
			while (!st.empty() && st.top() == arr[current]) {
				st.pop();  // 反复弹出栈 !!!
				current++;
			}
		}
		if (st.empty() == true && flag == true) {
			printf("YES\n");
		}
		else {
			printf("NO\n");
		}
	}
	system("pause");
	return 0;
}
```

## 1056 ☆ 🔺 Mice and Rice

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <queue>
using namespace std;
const int maxn = 1001;
struct mouth {
	int weight;  // 质量
	int R;  // 排名
}mouse[maxn];

int main() {
	int np, ng, order;
	scanf("%d%d", &np, &ng);
	for (int i = 0; i < np; i++) {
		scanf("%d ", &mouse[i].weight);
	}
	queue<int> q;
	for (int i = 0; i < np; i++) {
		scanf("%d", &order);
		q.push(order);  // 按顺序把老鼠们的标号入队
	}
	int temp = np, group;  // temp为当前轮比赛总老鼠数，group为组数
	while (q.size() != 1) {
		// 计算group,即当前轮分为几组进行比赛
		if (temp % ng == 0) group = temp / ng;
		else group = temp / ng + 1;
		// 枚举每一组，选出该组老鼠中质量最大的
		for (int i = 0; i < group; i++) {
			int k = q.front();
			for (int j = 0; j < ng; j++) {
				// 在最后一组老鼠数不足NG时，j超过最大老鼠数量，退出循环
				if (i * ng + j >= temp) break;  
				int front = q.front();
				if (mouse[front].weight > mouse[k].weight)
					k = front; 
				mouse[front].R = group + 1;
				q.pop();
			}
			q.push(k);  // 胜利的老鼠晋级
		}
		temp = group;  // group只老鼠晋级，因此下轮老鼠总数为group
	}
	mouse[q.front()].R = 1;  // 当队列中只剩1只老鼠时，令其排名为1.
	// 输出所有所有的信息
	for (int i = 0; i < np; i++) {
		printf("%d", mouse[i].R);
		if (i < np - 1)
			printf(" ");
	}
	system("pause");
	return 0;
}
```

## 1032 Sharing

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <queue>
using namespace std;
const int maxn = 100001;

struct node {
	char data;  // 数字域
	int next;  // 指针域
	bool flag;  // 结点是否在第一条链表中出现
}nodes[maxn];

int main() {
	for (int i = 0; i < maxn; i++) {
		nodes[i].flag = false;
	}
	int s1, s2, n;
	scanf("%d %d %d", &s1, &s2, &n);
	int address, next;  // 结点地址与后继结点地址
	char data;  // 数据
	for (int i = 0; i < n; i++) {
		scanf("%d %c %d", &address, &data, &next);
		nodes[address].data = data;
		nodes[address].next = next;
	}
	// 这题的两条链表都是按照顺序来的，所以采用下述方法：!!!
	int p;
	for (p = s1; p != -1; p = nodes[p].next) {
		nodes[p].flag = true;  // 枚举第一条链表所有节点，表示啊啊啊我已经属于第一条链表噢！
	}
	for (p = s2; p != -1; p = nodes[p].next) {
		if (nodes[p].flag == true)  // 找到第一个已经在第一条链表中出现的节点
			break;
	}
	if (p != -1)
		printf("%05d\n", p);
	else
		printf("-1\n");
	system("pause");
	return 0;
}
```

## 1044 ☆ 🔺 ??? Shopping in Mars

端点有点问题，晕

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <queue>
#include <climits>
using namespace std;
const int maxn = 100001;
int sum[maxn];

// 返回[l, r]内第一个大于x的位置 !!!
int upper_bound(int l, int r, int x) {
	int left = l, right = r, mid;
	while (left < right) {
		mid = (left + right) / 2;
		if (sum[mid] > x) {
			right = mid;
		}
		else {
			left = mid + 1;
		}
	}
	return left;
}

int main() {
	int n, s, nears = INT_MAX;
	scanf("%d%d", &n, &s);  
	sum[0] = 0;
	for (int i = 1; i <= n; i++) {
		scanf("%d", &sum[i]);
		sum[i] += sum[i - 1];
	}
	for (int i = 1; i <= n; i++) {  // 枚举左端点 !!!
		int j = upper_bound(i, n, sum[i - 1] + s);  // 求右端点
		if (sum[j - 1] - sum[i - 1] == s) {  // 查找成功
			nears = s;
			break;
		}
		else if (j <= n && sum[j - 1] - sum[i - 1] < nears) {
			// 存在大于s的解并且小于nears
			nears = sum[j] - sum[i - 1];  // 更新当前nears
		}
	}
	// 根据上述找到的nears，再次遍历找到这对端点值
	for (int i = 1; i <= n; i++) {
		int j = upper_bound(i, n, sum[i - 1] + s);
		if (sum[j - 1] - sum[i - 1] == nears) {
			printf("%d-%d\n", i, j - 1);  // 输出左端点和右端点（注意是j - 1）
		}
	}
	system("pause");
	return 0;
}
```

## 1089 ☆ 🔺 ??? Insert or Merge

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 101;
int origin[maxn], tempOri[maxn], changed[maxn];  // 原始数组，原始备份数组，目标数组
int n; // 元素个数

bool isSame(int a[], int b[]) {
	for (int i = 0; i < n; i++) {
		if (a[i] != b[i])
			return false;
	}
	return true;
}

void showArray(int a[]) {
	for (int i = 0; i < n; i++) {
		cout << a[i];
		if (i < n - 1)
			cout << " ";
	}
	cout << endl;
}

bool insertSort() {  // 插入排序
	bool flag = false; // 记录是否存在数组中间步骤与changed相同
	for (int i = 1; i < n; i++) {  // 进行n-1趟排序
		if (i != 1 && isSame(tempOri, changed)) {
			flag = true;  // 中间步骤与目标相同，且不是初始序列
		}
		// 以下是插入部分
		int temp = tempOri[i], j = i;
		while (j > 0 && tempOri[j - 1] > temp) {
			tempOri[j] = tempOri[j - 1];
			j--;
		}
		tempOri[j] = temp;
		if (flag == true) {
			return true;  // 如果flag 为true，则说明已达到目标数组，返回true
		}
	}
	return false;  // 无法达到目标数组，返回false
}

void mergeSort() {  // 归并排序
	bool flag = false;  // 记录是否存数组中间步骤与changed数组相同
	// 以下为归并排序部分
	for (int step = 2; step / 2 <= n; step *= 2) {
		if(step != 2 && isSame(tempOri, changed))
			flag = true;  // 中间步骤与目标相同，且不是初始序列
		for (int i = 0; i < n; i += step) {
			sort(tempOri + i, tempOri + min(i + step, n));
		}
		if (flag == true) {  // 已到达目标数组，输出tempOri数组
			showArray(tempOri);
			return;
		}
	}
	
}
	
int main() {
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> origin[i];
		tempOri[i] = origin[i];  // tempOri数组为备份，排序过程在tempOri上进行
	}
	for (int i = 0; i < n; i++) {
		cin >> changed[i];  // 目标数组
	}
	if (insertSort()) {  // 如果插入排序中找到目标数组
		cout << "Insertion Sort" << endl;
		showArray(tempOri);
	}
	else {  // 到达此处时一定是归并排序
		cout << "Merge Sort" << endl;
		for (int i = 0; i < n; i++) {
			tempOri[i] = origin[i];  //还原tempOri数组
		}
		mergeSort();  // 归并排序
	}
	system("pause");
	return 0;
}
```

## 1093 ☆ Count PAT's

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 100001;
const int mod = 1000000007;
// !!! 常用小技巧，不然这题会有样例WA
// !!! 1000000007 是最小的十位质数。模1000000007，可以保证值永远在int的范围内。
int leftNumP[maxn] = { 0 };  // 每一位左边（含）P的个数

int main() {
	string str;
	getline(cin, str);
	int len = str.length();  // 长度
	for (int i = 0; i < len; i++) {  // 从左到右遍历字符串
		if (i > 0) {  // 如果不是0号位
			leftNumP[i] = leftNumP[i - 1];
		}
		if (str[i] == 'P') {  // 当前位是P
			leftNumP[i]++;
		}
	}
	// ans位答案，rightNumT记录右边T的个数
	int ans = 0, rightNumT = 0;
	for (int i = len - 1; i >= 0; i--) {  // 从右到左遍历字符串
		if (str[i] == 'T') {  // 当前位是T
			rightNumT++;  // 右边T的个数加1
		}
		else if(str[i] == 'A'){  // 当前位是A
			ans = (ans + leftNumP[i] * rightNumT) % mod;  // 累计乘积
		}
	}
	printf("%d\n", ans);  // 输出结果
	system("pause");
	return 0;
}
```

## 1101 Quick Sort

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdio.h>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
const int maxn = 100001;
// a为序列，leftMax和rightMin分别为每一位左边最大的数和右边最小的数
int a[maxn], leftMax[maxn], rightMin[maxn];
// ans记录所有的主元，num为主元个数
int ans[maxn], num = 0;

int main() {
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	leftMax[0] = 0;  // A[0]左边没有比它大的数
	for (int i = 1; i < n; i++) {
		leftMax[i] = max(leftMax[i - 1], a[i - 1]);  // !!!
	}
	rightMin[n - 1] = INT_MAX;
	for (int i = n - 2; i >= 0; i--) {
		rightMin[i] = min(rightMin[i + 1], a[i + 1]);
	}
	for (int i = 0; i < n; i++) {
		// 左边的数比它小，右边所有数比它大
		if (leftMax[i] < a[i] && rightMin[i] > a[i]) {
			ans[num++] = a[i];  // 记录主元
		}
	}
	cout << num << endl;
	for (int i = 0; i < num; i++) {
		cout << ans[i];
		if (i < num - 1)
			cout << " ";
	}
	cout << endl;
	system("pause");
	return 0;
}
```

## 1069 ☆ 数字黑洞

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

bool cmp(int a, int b) {  
	return a > b;
}

void toArray(int n, int num[]) {  // 将n的每一位存到num数组中
	for (int i = 0; i < 4; i++) {
		num[i] = n % 10;
		n /= 10;
	}
}

int to_number(int num[]) {
	int sum = 0;
	for (int i = 0; i < 4; i++) {
		sum = sum * 10 + num[i];
	}
	return sum;
}
int main() {
	// MIN和MAX分别表示递增排序和递减排序后得到的最小值和最大值
	int n, min, max;
	cin >> n;
	int num[5];
	while (1) {
		toArray(n, num);  
		sort(num, num + 4); // 从小到大排序!!!
		min = to_number(num);
		sort(num, num + 4, cmp);  // 从大到小排序!!!
		max = to_number(num);
		n = max - min;
		printf("%04d - %04d = %04d\n", max, min, n);
		if (n == 0 || n == 6174)
			break;  // 如果下一个数是0或者6174则退出
	}
	system("pause");
	return 0;
}
```

## 1069 ☆Sum of Number Segments

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
	int n;
	double v, ans = 0;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> v;
		ans += v * i * (n + 1 - i);
	}
	printf("%0.2f\n", ans);
	system("pause");
	return 0;
}
```

## 1008 Elevator

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
int num[101] = {0};

int main() {
	int n, sum = 0, now, last = 0;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> now;
		if (now > last) {
			sum += (now - last) * 6;
		}
		else {
			sum += (last - now) * 4;
		}
		sum += 5;
		last = now;
	}
	cout << sum;
	system("pause");
	return 0;
}
```

## 1049 ☆ ??? Counting ones

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
int num[101] = {0};

int main() {
	int n, a = 1, ans = 0;
	int left, now, right;
	cin >> n;
	while (n / a != 0) {
		left = n / (a * 10);
		now = n / a % 10;
		right = n % a;
		if (now == 0)
			ans += left * a;
		else if (now == 1)
			ans += left * a + right + 1;
		else
			ans += (left + 1) * a;
		a *= 10;
	}
	cout << ans << endl;
	system("pause");
	return 0;
}
```

## 1081 ☆ 🔺 Rational Sum

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
typedef long long ll;
ll gcd(ll a, ll b) {  // 求a与b的最大公约数
	return b == 0 ? a : gcd(b, a % b);  // !!!辗转相除法, 记住
}
struct Fraction {  // 分数
	ll up, down;  // 分子、分母
 };

Fraction reduction(Fraction result) {  // 化简!!!
	if (result.down < 0) {  // 分母为负数，令分子分母都变为相反数
		result.down = -result.down;
		result.up = -result.up;
	}
	if (result.up == 0) { // 如果分子为0
		result.down = 1;  // 令分母为1
	}
	else {  // 如果分子不为0，进行约分
		int d = gcd(abs(result.up), abs(result.down));
		result.up /= d;
		result.down /= d;
	}
	return result;
}

Fraction add(Fraction f1, Fraction f2) {
	Fraction result;
	result.up = f1.up * f2.down + f2.up * f1.down;
	result.down = f1.down * f2.down;
	return reduction(result);
}

void showResult(Fraction r) {
	reduction(r);
	if (r.down == 1)
		cout << r.up << endl;  // 整数
	else if (abs(r.up) > r.down) {  // 假分数
		printf("%lld %lld/%lld\n", r.up / r.down, abs(r.up) % r.down, r.down);
	}
	else {
		printf("%lld/%lld\n", r.up, r.down);
	}
}
int main() {
	int n;
	cin >> n;
	Fraction sum, temp;
	sum.up = 0, sum.down = 1;  // ！!!!先这么设置
	for (int i = 0; i < n; i++) {
		scanf("%lld/%lld", &temp.up, &temp.down);
		sum = add(sum, temp);
	}
	showResult(sum);
	system("pause");
	return 0;
}
```

## 1088 ☆ 🔺 Rational Arithmetic

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
typedef long long ll;
ll gcd(ll a, ll b) {  // 求a与b的最大公约数
	return b == 0 ? a : gcd(b, a % b);  // !!!辗转相除法, 记住
}
struct Fraction {  // 分数
	ll up, down;  // 分子、分母
 }a, b;

Fraction reduction(Fraction result) {  // 化简!!!
	if (result.down < 0) {  // 分母为负数，令分子分母都变为相反数
		result.down = -result.down;
		result.up = -result.up;
	}
	if (result.up == 0) { // 如果分子为0
		result.down = 1;  // 令分母为1,该数成为整数
	}
	else {  // 如果分子不为0，进行约分
		int d = gcd(abs(result.up), abs(result.down));
		result.up /= d;
		result.down /= d;
	}
	return result;
}

Fraction add(Fraction f1, Fraction f2) {
	Fraction result;
	result.up = f1.up * f2.down + f2.up * f1.down;
	result.down = f1.down * f2.down;
	return reduction(result);
}
Fraction minu(Fraction f1, Fraction f2) {
	Fraction result;
	result.up = f1.up * f2.down - f2.up * f1.down;
	result.down = f1.down * f2.down;
	return reduction(result);
}
Fraction multi(Fraction f1, Fraction f2) {
	Fraction result;
	result.up = f1.up * f2.up;
	result.down = f1.down * f2.down;
	return reduction(result);
}
Fraction divide(Fraction f1, Fraction f2) {
	Fraction result;
	result.up = f1.up * f2.down;
	result.down = f1.down * f2.up;
	return reduction(result);
}

void showResult(Fraction r) {  // !!!
	r = reduction(r);
	if (r.up < 0)
		printf("(");
	if (r.down == 1)
		cout << r.up;  // 整数
	else if (abs(r.up) > r.down) {  // 假分数
		printf("%lld %lld/%lld", r.up / r.down, abs(r.up) % r.down, r.down);
	}
	else {
		printf("%lld/%lld", r.up, r.down);
	}
	if (r.up < 0)
		printf(")");
}
int main() {
	scanf("%lld/%lld %lld/%lld", &a.up, &a.down, &b.up, &b.down);
	// 加法
	showResult(a);
	printf(" + ");
	showResult(b);
	printf(" = ");
	showResult(add(a, b));
	printf("\n");
	// 减法
	showResult(a);
	printf(" - ");
	showResult(b);
	printf(" = ");
	showResult(minu(a, b));
	printf("\n");
	// 乘法
	showResult(a);
	printf(" * ");
	showResult(b);
	printf(" = ");
	showResult(multi(a, b));
	printf("\n");
	// 除法
	showResult(a);
	printf(" / ");
	showResult(b);
	printf(" = ");
	if (b.up == 0)
		printf("Inf");  // !!!
	else
		showResult(divide(a, b));
	printf("\n");
	system("pause");
	return 0;
}
```

## 1015 ☆ 🔺 Reversing Primes

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <cmath>
#include <algorithm>
using namespace std;
int d[111];

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
	int n, radix;
	while (scanf("%d", &n) != EOF) {
		if (n < 0)
			break;  // 当n为负数时，退出循环
		scanf("%d", &radix);
		if (isPrime(n) == false) {  // n不是素数，输出No,结束算法
			cout << "No" << endl;
		}
		else {
			int len = 0;
			d[len++] = n % radix;
			n /= radix;
			while (n != 0) {  // 进制转换
				d[len++] = n % radix;
				n /= radix;
			}
			for (int i = 0; i < len; i++) {  // 按逆序转换进制!!!
				n = n * radix + d[i];
			}
			if (isPrime(n) == true) {
				cout << "Yes" << endl;
			}
			else {
				cout << "No" << endl;
			}

		}
	}
	system("pause");
	return 0;
}
```

## 1078 ☆ Hashing

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <cmath>
#include <algorithm>
using namespace std;
const int maxn = 10001;
bool hashTable[maxn] = { 0 };
// hashTable[x] == false 表示x号位未被占用

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
	int n, tsize, a;
	cin >> tsize >> n;
	while (isPrime(tsize) == false) {  // 寻找第一个大于等于原来tsize的质数
		tsize++;
	}
	for (int i = 0; i < n; i++) {
		cin >> a;
		int m = a % tsize;
		if (hashTable[m] == false) {
			hashTable[m] = true;
			if (i == 0)
				cout << m;
			else
				cout << " " << m;
		}
		else {
			int step;  // 二次方探查法步长
			for (step = 1; step < tsize; step++) {
				m = (a + step * step) % tsize;  // 下一个检测值
				if (hashTable[m] == false) {  // 下一个检测值
					hashTable[m] = true;
					if (i == 0)
						cout << m;
					else
						cout << " " << m;
					break;
				}
			}
			if (step >= tsize) {  // 找不到插入的地方
				if (i > 0)
					cout << " ";
				cout << "-";
			}
		}
	}
	system("pause");
	return 0;
}
```

## 1096 ☆ Consecutive Factors

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <cmath>
#include <algorithm>
using namespace std;
typedef long long ll;

int main() {
	ll n;
	cin >> n;
	ll sqr = (ll)sqrt(1.0 * n), ansI = 0, ansLen = 0;
	for (ll i = 2; i <= sqr; i++) {  // 遍历连续的第一个整数
		ll temp = 1, j = i;  // temp为当前连续整数的乘积
		while (1) {  
			temp *= j;
			if (n % temp != 0)  // !!!
				break;  // 如果不能整除，结束计算
			if (j - i + 1 > ansLen) {  //发现了更长的长度
				ansI = i;  // 更新第一个整数
				ansLen = j - i + 1;
			}
			j++;
		}
	}
	if (ansLen == 0) {  // 无解
		printf("1\n%lld", n);
	}
	else {
		printf("%lld\n", ansLen);
		for (ll i = 0; i < ansLen; i++) {
			printf("%lld", ansI + i);
			if (i < ansLen - 1) {
				printf("*");
			}
		}
	}
	system("pause");
	return 0;
}
```

## 1059 ☆ 🔺 Prime Factors

质因数分解

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <string>
#include <cmath>
#include <algorithm>
using namespace std;
typedef long long ll;
const int maxn = 100001;

bool isPrime(int n) {  
	if (n <= 1)
		return false;
	int sqr = (int)sqrt(1.0 * n);
	for (int i = 2; i <= sqr; i++) {
		if (n % i == 0)
			return false;
	}
	return true;
}
int prime[maxn], pNum = 0;
void findPrime() {  // 求素数表
	for (int i = 1; i < maxn; i++) {
		if (isPrime(i) == true)
			prime[pNum++] = i;
	}
}
struct factor {
	int x, cnt;  // x为质因子，cnt为其指数!!!
}fac[10];

int main() {
	findPrime();
	int n, num = 0;  // num为n的不同质因子的个数
	cin >> n;
	if (n == 1)
		cout << "1=1";  // 特判1的情况
	else {
		printf("%d=", n);
		int sqr = (int)sqrt(1.0 * n);
		// 枚举根号n以内的质因子
		//for (int i = 0; i < pNum && prime[i] <= sqr; i++) {  
		for (int i = 0; prime[i] <= sqr; i++) {  
			if (n % prime[i] == 0) {
				fac[num].x = prime[i];
				fac[num].cnt = 0;
				while (n % prime[i] == 0) {   // 计算除质因子prime[i]的个数
					fac[num].cnt++;
					n /= prime[i];  // !!!
				}
				num++;
			}
			if (n == 1)  // 及时退出循环!!!
				break;
		}
		if (n != 1) {  // 如果无法被根号n以内的质因子除尽,n就是最后一个因子!!!
			fac[num].x = n;
			fac[num++].cnt = 1;
		}
		// 按格式输出结果
		for (int i = 0; i < num; i++) {
			if (i > 0)
				printf("*");
			printf("%d", fac[i].x);
			if (fac[i].cnt > 1) {
				printf("^%d", fac[i].cnt);
			}
		}
	}
	system("pause");
	return 0;
}
```

## 1090 ???

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
const int maxn = 100001;
vector<int> child[maxn];  // 存放树
double p, r;
// maxDepth 为最大深度，num为最大深度的叶节点个数
int n, maxDepth = 0, num = 0;
void DFS(int index, int depth) {
	if (child[index].size() == 0) {  // 到达叶节点
		if (depth > maxDepth) {
			maxDepth = depth;
			num = 1;
		}
		else if (depth == maxDepth) {
			num++;
		}
		return;
	}
	for (int i = 0; i < child[index].size(); i++) {
		DFS(child[index][i], depth + 1);
	}
}
int main() {
	int father, root;
	cin >> n >> p >> r;
	r /= 100;  // 将百分数除以100
	for (int i = 0; i < n; i++) {
		cin >> father;
		if (father != -1) {
			child[father].push_back(i);
		}
		else {
			root = i;
		}
	}
	DFS(root, 0);  // DFS入口
	printf("%.2f %d\n", p * pow(1 + r, maxDepth), num);
	system("pause");
	return 0;
}
```



## 1079 ???

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
const int maxn = 100001;
struct node {
	double data;  // 数据域
	vector<int> child;  // 指针域
}Node[maxn];  // 存放树
int n;
double p, r, ans = 0;  // ans为叶节点货物的价格之和

void DFS(int index, int depth) {
	if (Node[index].child.size() == 0) {  // 到达叶节点
		ans += Node[index].data * pow(1 + r, depth);  // 累加叶节点货物的价格
		return;
	}
	for (int i = 0; i < Node[index].child.size(); i++) {
		DFS(Node[index].child[i], depth + 1);  // 递归访问子节点
	}
}
int main() {
	int k, child;
	cin >> n >> p >> r;
	r /= 100;  // 将百分数除以100
	for (int i = 0; i < n; i++) {
		cin >> k;
		if (k != 0) {
			for (int j = 0; j < k; j++) {
				cin >> child;
				Node[i].child.push_back(child);
			}
		}
		else {
			cin >> Node[i].data;
		}
	}
	DFS(0, 0);  // DFS入口
	printf("%.1f\n", p * ans);
	system("pause");
	return 0;
}
```

## 1074 ☆ 🔺 Reversing Linked List 

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
const int maxn = 100001;
struct Node {
	int address, data, next;
	int order;  // 结点在链表上的序号，无效结点记为maxn
}node[maxn];

bool cmp(Node a, Node b) {
	return a.order < b.order;
}

int main() {
	for (int i = 0; i < maxn; i++) {
		node[i].order = maxn;  
	}
	int begin, n, k, address;
	cin >> begin >> n >> k;
	for (int i = 0; i < n; i++) {
		cin >> address;
		cin >> node[address].data >> node[address].next;
		node[address].address = address;
	}
	int p = begin, count = 0;
	while (p != -1) {
		node[p].order = count++;
		p = node[p].next;
	}
	sort(node, node + maxn, cmp);
	n = count;
	for (int i = 0; i < n / k; i++) {
		for (int j = (i + 1) * k - 1; j > i * k; j--) {
			printf("%05d %d %05d\n", node[j].address, node[j].data, node[j - 1].address);
		}
		printf("%05d %d ", node[i * k].address, node[i *k].data);
		if (i < n / k - 1) {
			printf("%05d\n", node[(i + 2) * k - 1].address);
		}
		else {
			if (n % k == 0)
				printf("-1\n");
			else {
				printf("%05d\n", node[(i + 1) * k].address);
				for (int i = n / k * k; i < n; i++) {
					printf("%05d %d ", node[i].address, node[i].data);
					if (i < n - 1) {
						printf("%05d\n", node[i + 1].address);
					}
					else {
						printf("-1\n");
					}
				}
			}
		}
	}
	system("pause");
	return 0;
}
```

## 1097 ☆ 🔺 Deduplication on a Linked List

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
const int maxn = 100001;
const int tableNum = 1000001;
struct Node {
	int address, data, next;
	int order;  // 结点在链表上的序号，无效结点记为maxn
}node[maxn];

bool cmp(Node a, Node b) {
	return a.order < b.order;
}

bool isExist[tableNum] = { false };

int main() {
	memset(isExist, false, sizeof(isExist));
	for (int i = 0; i < maxn; i++) {
		node[i].order = maxn * 2;
	}
	int n, begin, address;
	cin >> begin >> n;
	for (int i = 0; i < n; i++) {
		cin >> address;
		cin >> node[address].data >> node[address].next;
		node[address].address = address;
	}
	int countValid = 0, countRemoved = 0, p = begin;
	while (p != -1) {
		if (!isExist[abs(node[p].data)]) {
			isExist[abs(node[p].data)] = true;
			node[p].order = countValid++;
		}
		else {
			node[p].order = maxn + countRemoved++;
		}
		p = node[p].next;
	}
	sort(node, node + maxn, cmp);
	int count = countValid + countRemoved;
	for (int i = 0; i < count; i++) {
		if (i != countValid - 1 && i != count - 1) {
			printf("%05d %d %05d\n", node[i].address, node[i].data, node[i + 1].address);
		}
		else {
			printf("%05d %d -1\n", node[i].address, node[i].data);
		}
	}
	system("pause");
	return 0;
}
```

## 1103 ☆ 🔺 Integer Factorization

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
int n, k, p, maxFacSum = -1;
vector<int> fac, ans, temp;

int power(int x) {
	int ans = 1;
	for (int i = 0; i < p; i++) {
		ans *= x;
	}
	return ans;
}

void initFac() {
	int i = 0, temp = 0;
	while (temp <= n) {
		fac.push_back(temp);
		temp = power(++i);
	}
}

void DFS(int index, int nowK, int sum, int facSum) {
	if (sum == n && nowK == k) {
		if (facSum > maxFacSum) {
			ans = temp;
			maxFacSum = facSum;
		}
		return;
	}
	if (sum > n || nowK > k) return;
	if (index - 1 >= 0) {
		temp.push_back(index);
		DFS(index, nowK + 1, sum + fac[index], facSum + index);
		temp.pop_back();
		DFS(index - 1, nowK, sum, facSum);
	}
}

int main() {
	cin >> n >> k >> p;
	initFac();
	DFS(fac.size() - 1, 0, 0, 0);
	if (maxFacSum == -1)
		cout << "Impossible" << endl;
	else {
		printf("%d = %d^%d", n, ans[0], p);
		for (int i = 1; i < ans.size(); i++) {
			printf(" + %d^%d", ans[i], p);
		}
	}
	system("pause");
	return 0;
}
```

## ☆  🔺 1091 Acute Stroke

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <climits>
#include <vector>
#include <queue>
using namespace std;
struct node {
	int x, y, z;
}Node;

int n, m, slice, T;
int pixel[1290][130][61];
bool inq[1290][130][61] = { false };
int X[6] = { 0, 0, 0, 0, 1, -1 };
int Y[6] = { 0, 0, 1, -1, 0, 0 };
int Z[6] = { 1, -1, 0, 0, 0, 0 };

bool judge(int x, int y, int z) {
	if (x >= n || x < 0 || y >= m || y < 0 || z >= slice | z < 0)
		return false;
	if (pixel[x][y][z] == 0 || inq[x][y][z] == true)
		return false;
	return true;
}

int BFS(int x, int y, int z) {
	int tot = 0;
	queue<node> Q;
	Node.x = x, Node.y = y, Node.z = z;
	Q.push(Node);
	inq[x][y][z] = true;
	while (!Q.empty()) {
		node top = Q.front();
		Q.pop();
		tot++;
		for (int i = 0; i < 6; i++) {
			int newX = top.x + X[i];
			int newY = top.y + Y[i];
			int newZ = top.z + Z[i];
			if (judge(newX, newY, newZ)) {
				Node.x = newX, Node.y = newY, Node.z = newZ;
				Q.push(Node);
				inq[newX][newY][newZ] = true;
			}
		}
	}
	if (tot >= T)
		return tot;
	else
		return 0;
}
int main() {
	cin >> n >> m >> slice >> T;
	for (int z = 0; z < slice; z++) {
		for (int x = 0; x < n; x++) {
			for (int y = 0; y < m; y++) {
				cin >> pixel[x][y][z];
			}
		}
	}
	int ans = 0;
	for (int z = 0; z < slice; z++) {
		for (int x = 0; x < n; x++) {
			for (int y = 0; y < m; y++) {
				if (pixel[x][y][z] == 1 && inq[x][y][z] == false) {
					ans += BFS(x, y, z);
				}
			}
		}
	}
	printf("%d\n", ans);
	system("pause");
	return 0;
}
```

## 1020 ☆ 🔺 Tree Traversals

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue>
using namespace std;
const int maxn = 50;

struct node {
	int data;
	node* lchild;
	node* rchild;
};
int pre[maxn], in[maxn], post[maxn];
int n;

node* create(int postL, int postR, int inL, int inR) {
	if (postL > postR) {
		return NULL;
	}
	node* root = new node;
	root->data = post[postR];
	int k;
	for (k = inL; k <= inR; k++) {
		if (in[k] == post[postR]) {
			break;
		}
	}
	int numLeft = k - inL;
	root->lchild = create(postL, postL + numLeft - 1, inL, k - 1);
	root->rchild = create(postL + numLeft, postR - 1, k + 1, inR);
	return root;
}

int num = 0;
void BFS(node* root) {
	queue<node*> q;
	q.push(root);
	while (!q.empty()) {
		node* now = q.front();
		q.pop();
		cout << now->data;
		num++;
		if (num < n)
			cout << " ";
		if (now->lchild != NULL)
			q.push(now->lchild);
		if (now->rchild != NULL)
			q.push(now->rchild);
	}
}

int main() {
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> post[i];
	}
	for (int i = 0; i < n; i++) {
		cin >> in[i];
	}
	node* root = create(0, n - 1, 0, n - 1);
	BFS(root);
	system("pause");
	return 0;
}
```

## ☆ 🔺1086 Tree Traversals Again 

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
#include <queue>
#include <stack>
using namespace std;
const int maxn = 50;

struct node {
	int data;
	node* lchild;
	node* rchild;
};
int pre[maxn], in[maxn], post[maxn];
int n;

node* create(int preL, int preR, int inL, int inR) {
	if (preL > preR) {
		return NULL;
	}
	node* root = new node;
	root->data = pre[preL];
	int k;
	for (k = inL; k <= inR; k++) {
		if (in[k] == pre[preL]) {
			break;
		}
	}
	int numLeft = k - inL;
	root->lchild = create(preL + 1, preL + numLeft, inL, k - 1);
	root->rchild = create(preL + numLeft + 1, preR, k + 1, inR);
	return root;
}

int num = 0;
void postorder(node* root) {
	if (root == NULL) {
		return;
	}
	postorder(root->lchild);
	postorder(root->rchild);
	cout << root->data;
	num++;
	if (num < n)
		cout << " ";
}

int main() {
	cin >> n;
	string str;
	stack<int> st;
	int x, preIndex = 0, inIndex = 0;
	for (int i = 0; i < 2 * n; i++) {
		cin >> str;
		if (str == "Push") {
			cin >> x;
			pre[preIndex++] = x;
			st.push(x);
		}
		else {
			in[inIndex++] = st.top();
			st.pop();
		}
	}
	node* root = create(0, n - 1, 0, n - 1);
	postorder(root);
	system("pause");
	return 0;
}
```

## 1102 ☆ 🔺 Invert a Binary Tree

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
#include <queue>
#include <stack>
using namespace std;
const int maxn = 110;

struct node {
	int lchild;
	int rchild;
}Node[maxn];
bool notRoot[maxn] = { false };
int n, num = 0;

void print(int id) {
	cout << id;
	num++;
	if (num < n)
		cout << " ";
	else
		cout << endl;
}

void inOrder(int root) {
	if (root == -1)
		return;
	inOrder(Node[root].lchild);
	print(root);
	inOrder(Node[root].rchild);
}

void BFS(int root) {
	queue<int> q;
	q.push(root);
	while (!q.empty()) {
		int now = q.front();
		q.pop();
		print(now);
		if (Node[now].lchild != -1)
			q.push(Node[now].lchild);
		if (Node[now].rchild != -1)
			q.push(Node[now].rchild);
	}
}

void postOrder(int root) {
	if (root == -1)
		return;
	postOrder(Node[root].lchild);
	postOrder(Node[root].rchild);
	swap(Node[root].lchild, Node[root].rchild);
}

int strToNum(char c) {
	if (c == '-')
		return -1;
	else {
		notRoot[c - '0'] = true;
		return c - '0';
	}
}

int findRoot() {
	for (int i = 0; i < n; i++) {
		if (notRoot[i] == false) {
			return i;
		}
	}
}

int main() {
	char lchild, rchild;
	cin >> n;
	for (int i = 0; i < n; i++) {
		getchar();
		cin >> lchild >> rchild;
		Node[i].lchild = strToNum(lchild);
		Node[i].rchild = strToNum(rchild);
	}
	int root = findRoot();
	postOrder(root);
	BFS(root);
	num = 0;
	inOrder(root);
	system("pause");
	return 0;
} 
```

