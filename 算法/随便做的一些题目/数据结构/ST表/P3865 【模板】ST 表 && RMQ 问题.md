[P3865 【模板】ST 表 && RMQ 问题](https://www.luogu.com.cn/problem/P3865)
难度
普及/提高−

AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>
#include<cstdio>

using namespace std;

const int MAXN = 100010;
const int MAXLOGN = 18;

int st[MAXN][MAXLOGN + 1];
int logN[MAXN + 1] = { };

void pre(){
	logN[1] = 0;
	logN[2] = 1;
	for (int i = 3; i < MAXN; i++) {
		logN[i] = logN[i / 2] + 1;
	}

	
}

inline int read()
{
	int x = 0, f = 1; char ch = getchar();
	while (ch < '0' || ch>'9') { if (ch == '-') f = -1; ch = getchar(); }
	while (ch >= '0' && ch <= '9') { x = x * 10 + ch - 48; ch = getchar(); }
	return x * f;
}

int main() {

	int n = read();
	int m = read();

	pre();
	for (int i = 1; i <= n; i++) {
		st[i][0] = read();
	}

	for (int j = 1; j <= MAXLOGN; j++) {
		for (int i = 1; i + (1 << j) - 1 <= n; i++) {
			st[i][j] = max(st[i][j - 1], st[i + (1 << (j - 1))][j - 1]);

		}
	}
	for (int i = 1; i <= m; i++) {
		int l = read(); 
		int r = read();
		int s = logN[r - l + 1];
		printf("%d\n",max(st[l][s], st[r - (1 << s) + 1][s]));

	}

}
```
