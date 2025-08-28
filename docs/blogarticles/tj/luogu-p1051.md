# 题解：P1051 [NOIP 2005 提高组] 谁拿了最多奖学金

## 题目分析

本题难度不大，但是要细心，要有好的码风，如：

- 不要把 $>$ 写成 $\geq$；
- 一个人可能拿多个奖，也可能不拿奖；
- 变量名不要定义得自己都看不懂，如 `a`、`b`、`c`。

本题 $1 \leq N \leq 100$，无需快读，`cin` 即可。定义一个结构体，存储学生信息，一些数据可用 `bool`。

最后输出获得最多奖金的学生的姓名、这名学生获得的奖金总数、这 $N$ 个学生获得的奖学金的总数。

注意用打擂台法找最大值是从前到后是 `>`，从后到前则是 `>=`，因为奖学金相同时顺序输出靠**前**的。

## 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
struct stu{
	string name; // 姓名
	int score, cscore, mon; // 期末平均成绩、班级评议成绩、奖学金总数
	bool ganBu, xiBu, lunWen; // 即为拼音
};
int main(){
	int n = 0;
	char g, x;
	int l;     // g、x、l 都是临时变量
	cin >> n;
	stu sts[n];
	for(int i = 0; i < n; i++){
		cin >> sts[i].name >> sts[i].score
			>> sts[i].cscore >> g >> x >> l;
		if(g == 'Y') sts[i].ganBu = 1;
		else sts[i].ganBu = 0;
		if(x == 'Y') sts[i].xiBu = 1;
		else sts[i].xiBu = 0;
		if(l >= 1) sts[i].lunWen = 1;
		else sts[i].lunWen = 0;       // 这三个都是判断
	} // 输入
	for(int i = 0; i < n; i++){
		sts[i].mon = 0; // 初始化这个人的奖学金
		if(sts[i].score > 80 && sts[i].lunWen) sts[i].mon += 8000;
		if(sts[i].score > 85 && sts[i].cscore > 80) sts[i].mon += 4000;
		if(sts[i].score > 90) sts[i].mon += 2000;
		if(sts[i].score > 85 && sts[i].xiBu) sts[i].mon += 1000;
		if(sts[i].cscore > 80 && sts[i].ganBu) sts[i].mon += 850;
	} // 判断是否获奖，是则增加奖学金
	int maxx = -1, maxS = -1, sum = 0;
	for(int i = 0; i < n; i++){
		if(sts[i].mon > maxx){
			maxx = sts[i].mon;
			maxS = i;
		} // 用打擂台法找最大值
		sum += sts[i].mon;
	} // 找最大值，同时加和
	cout << sts[maxS].name << '\n' << sts[maxS].mon << '\n' << sum;
}
```

请勿复制提交，否则会变棕（作弊者），代码可能动过手脚。
