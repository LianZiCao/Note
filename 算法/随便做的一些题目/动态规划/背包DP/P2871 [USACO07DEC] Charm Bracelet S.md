[P2871 [USACO07DEC] Charm Bracelet S](https://www.luogu.com.cn/problem/P2871)
难度
普及−

经典0-1背包问题，本来也是按照之前的想法用二维数组记忆化记录状态的，结果竟然内存消耗太大MLE了

```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>
#include<cstring>
#include<unordered_map>

using namespace std;
int mem[3044][12882];

int n, m;
int w[3500];
int d[3500];

int dfs(int pos, int w_left) {
	if (mem[pos][w_left] != -1)
		return mem[pos][w_left];

	if (pos == n + 1)
		return 0;
	int dfs1 = dfs(pos + 1, w_left);
	int dfs2 = -INT_MAX;
	if (w_left >= w[pos])
		dfs2 = dfs(pos + 1, w_left - w[pos]) + d[pos];
	return mem[pos][w_left] = max(dfs1, dfs2);


}

int main() {
	memset(mem, -1, sizeof(mem));
	cin >> n >> m;
	for (int i = 1; i <= n; i++) {
		cin >> w[i] >> d[i];
	}
	cout << dfs(1, m);

}
```

所以得用降低空间复杂度，可以优化掉pos，采用一层for循环，对于每个可能的$i$（$i \in [1,n]$），从空间$m$开始，一直递减到$w_i$，如果有空间拿起来则尝试拿起来，取不拿和拿的最值，最后输出$f[m]$即可

转移方程$f_j=\max \left(f_j,f_{j-w_i}+v_i\right)$，其中$j$为空间，$w_i$为重量，$v_i$为价值

注意枚举顺序不能弄反，是从$m$开始到$w_i$，反了的话因为在 $j\geqslant w_{i}$ 时，$f_{i,j}$ 是会被 $f_{i,j-w_{i}}$ 所影响，所以就相当于一个物品可以被拿起多次。

（还有不要忘记在main函数外面声明的数组是因为在静态存储期的零初始化规则，所以其实f数组全是0，差点没转过来）

AC代码
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>

using namespace std;

int n, m;
int w[3500];
int d[3500];

int f[13000];

int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i++) {
		cin >> w[i] >> d[i];
	}
	for (int i = 1; i <= n; i++) {
		for (int l = m; l >= w[i]; l--)
			f[l] = max(f[l], f[l - w[i]] + d[i]);
	}
	cout << f[m];
}
```