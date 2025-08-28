# 题解：P12246 电 van

## 题意简述

给定一个字符串，仅包含 $\texttt{v}$、$\texttt{a}$、$\texttt{n}$ 三种字符。接下来进行 $m$ 次操作，每次操作给定 $x$，交换 $s_x$ 和 $s_{x-1}$，输出交换后的字符串中子序列 $\texttt{van}$ 的出现次数。注意子序列**不一定连续**。

## 做法

### 朴素做法

写一个计算用函数，计算一个字符串中子序列 $\texttt{van}$ 的出现次数，复杂度 $O(n^3)$，类似下面这样：

```cpp
int clc(string a){
	int ans = 0, len = a.length();
	for(int i = 0; i < len; i++){
		for(int j = i+1; j < len; j++){
			for(int k = j+1; k < len; k++){
				if(a[i] == 'v' && a[j] == 'a' && a[k] == 'n'){
					ans++;
				}
			}
		}
	}
	return ans;
}
```

主函数里面直接循环：

```cpp
for(int i = 0; i < m; i++){
	cin >> tmp;
	swap(str[tmp-1], str[tmp]);
	cout << clc(str) << '\n';
}
```

总复杂度达到 $O(n^3 m)$，只能通过 $1 \sim 5$ 的测试点，$25$ 分。

-----

### 正解

不难发现，每个字符 $\texttt{a}$ 对答案的贡献是其左边 $\texttt{v}$ 的数量乘以右边 $\texttt{n}$ 的数量（乘法原理）。而答案则是所有字符 $\texttt{a}$ 的贡献之和。

- 例如 $s=\texttt{vvvaannn}$，字符 $\texttt{a}$ 的的贡献是左边 $\texttt{v}$ 的数量 $3$ 乘以右边 $\texttt{n}$ 的数量 $3$；答案就是中间两个字符 $\texttt{a}$ 的贡献之和 $18$。

所以，先读入字符串并扫一遍，计算每一个字符 $\texttt{a}$ 的贡献，可以找 $\texttt{a}$ 然后往前后找，复杂度 $O(n^2)$，会 TLE，能通过 $1 \sim 9$ 的测试点，$45$ 分。

而正解是可以用两个变量记录当前前面有几个 $\texttt{v}$ 和后面有几个 $\texttt{n}$，扫到 $\texttt{v}$ 或者 $\texttt{n}$ 就让变量加或减，复杂度 $O(n)$。

接下来处理交换。很明显，如果 $\texttt{a}$ 与 $\texttt{v}$ 交换，则左边 $\texttt{v}$ 的数量就会 $+1$ 或 $-1$，而贡献则要加上或减去右边 $\texttt{n}$ 的数量（因为在计算贡献时左边 $\texttt{v}$ 的数量和右边 $\texttt{n}$ 的数量是相乘的）。接着将加减后的新答案输出即可，最后处理完贡献不要忘记把两个字符的顺序交换。

总复杂度 $O(n+m)$，能通过所有数据，$100$ 分。

## 最终实现

```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
struct chr{
	char typ;
	int xl, xr;
};
signed main(){
	int n, m, tmp, ans = 0, nowv = 0, nown = 0;
	string str; chr a[1234567];
	cin >> n >> m;
	cin >> str;
	for(int i = 0; i < n; i++){
		a[i].typ = str[i];
		if(a[i].typ == 'n'){
			nown++;
		}
	}
	for(int i = 0; i < n; i++){
		a[i].typ = str[i];
		a[i].xl = 0; a[i].xr = 0;
		if(a[i].typ == 'v'){
			nowv++;
		}else if(a[i].typ == 'n'){
			nown--;
		}else{
            a[i].xl = nowv;
    		a[i].xr = nown;
    		ans += a[i].xl * a[i].xr;
        }
	}
	for(int i = 0; i < m; i++){
		cin >> tmp;
		if(a[tmp-1].typ == 'a' && a[tmp].typ == 'n'){
			a[tmp-1].xr--;
			ans -= a[tmp-1].xl;
		}else if(a[tmp-1].typ == 'n' && a[tmp].typ == 'a'){
			a[tmp].xr++;
			ans += a[tmp].xl;
		}else if(a[tmp-1].typ == 'v' && a[tmp].typ == 'a'){
			a[tmp].xl--;
			ans -= a[tmp].xr;
		}else if(a[tmp-1].typ == 'a' && a[tmp].typ == 'v'){
			a[tmp-1].xl++;
			ans += a[tmp-1].xr;
		}
		swap(a[tmp-1], a[tmp]);
		cout << ans << '\n';
	}
	return 0;
}
```
