[B3647 【模板】Floyd](https://www.luogu.com.cn/problem/B3647)
难度
普及−

第一次交WA没考虑是无向图，第二次交WA是没考虑重边（题目有要求），改一下就好了

AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>
#include<set>

using namespace std;

const int MAXN = 105;
const int INF = 0xffffff;
int f[MAXN][MAXN];

int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 0; i < m; i++) {
		int u, v, w;
		cin >> u >> v >> w;
		if (f[u][v] == 0) {
			f[u][v] = w;
			f[v][u] = w;
		}
		else{
			f[u][v] = min(f[u][v], w);
			f[v][u] = min(f[v][u], w);
		}
	}
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			if (i != j && f[i][j] == 0)
				f[i][j] = INF;
		}
	}
	for (int k = 1; k <= n; k++) {
		for (int x = 1; x <= n; x++) {
			for (int y = 1; y <= n; y++) {
				f[x][y] = min(f[x][y], f[x][k] + f[k][y]);
			}
		}
	}
	for (int x = 1; x <= n; x++) {
		for (int y = 1; y <= n; y++) {
			cout << f[x][y] << ' ';
		}
		cout << endl;
	}
}
```