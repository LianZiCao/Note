[P2880 [USACO07JAN] Balanced Lineup G](https://www.luogu.com.cn/problem/P2880)
难度
普及/提高−

AC
```
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>
#include<cstdio>

using namespace std;

const int MAXN = 50010;
const int MAXLOGN = 18;

int st_max[MAXN][MAXLOGN + 1];
int st_min[MAXN][MAXLOGN + 1];
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

	int n, q;
	cin >> n >> q;

	pre();
	for (int i = 1; i <= n; i++) {
		st_max[i][0] = st_min[i][0] = read();
	}

	for (int j = 1; j <= MAXLOGN; j++) {
		for (int i = 1; i + (1 << j) - 1 <= n; i++) {
			st_max[i][j] = max(st_max[i][j - 1], st_max[i + (1 << (j - 1))][j - 1]);
			st_min[i][j] = min(st_min[i][j - 1], st_min[i + (1 << (j - 1))][j - 1]);

		}
	}
	for (int i = 1; i <= q; i++) {
		int l, r;
		cin >> l >> r;
		int s = logN[r - l + 1];
		printf("%d\n", max(st_max[l][s], st_max[r - (1 << s) + 1][s]) - min(st_min[l][s], st_min[r - (1 << s) + 1][s]));

	}

}
```