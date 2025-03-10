[P1531 I Hate It](https://www.luogu.com.cn/problem/P1531)
难度
普及/提高−

草了，这题目应该用线段树做，但是我用st表做的，然后成功AC了，这数据是不是有点水（x）

虽然查询元素的话st表常数时间就可以做到，但是单点修改的话最好还是线段树比较好吧，或者树状数组也行，复杂度都是$O(\log n)$，但是st表要修改的话肯定是比这个复杂度要高的，复杂度应该介于$\log n$ 到$n$之间，但是我还是用st表过了（x）

一开始是st表的更新有点问题导致TLE，问了d指导改了一下（其实自己也能发现问题）就AC了
AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<set>
#include<deque>
#include<cstring>
#include<map>

using namespace std;
typedef long long ll;

const int MAXN = 200010;
const int logN = 18;
int st[MAXN + 1][logN + 1];
int logn[MAXN + 1];
int n, m;


int main() {
	
	logn[1] = 0;
	for (int i = 2; i <= MAXN; i++) {
		logn[i] = logn[i / 2] + 1;
	}
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
		cin >> st[i][0];

	for (int j = 1; j <= logN; j++) {
		for (int i = 1; i + (1 << j) - 1 <= n; i++) {
			st[i][j] = max(st[i][j - 1],st[i + (1 << (j - 1))][j - 1]);
		}
	}

	for (int k = 0; k < m; k++) {
		char ch;
		int x, y;
		cin >> ch >> x >> y;
		if (ch == 'Q') {
			int t = logn[y + 1 - x];
			cout << max(st[x][t], st[y + 1 - (1 << t)][t]) << endl;

		}
		else if (st[x][0] < y) {
			//st[x][0] = y;
			//for (int j = 1; j <= logN; j++) {
			//	for (int i = max(1, x + 1 - (1 << j)); i + (1 << j) - 1 <= n && i <= (x - 1 + (1 << j)); i++) {
			//		st[i][j] = max(st[i][j - 1], st[i + (1 << (j - 1))][j - 1]);
			//	}
			//}
			st[x][0] = y;
			for (int j = 1; j <= logN; j++) {
				int s = 1 << j; // 当前层的区间长度
				int i_start = max(1, x - (s - 1));
				int i_end = min(x, n - s + 1);
				if (i_start > i_end) continue;
				for (int i = i_start; i <= i_end; ++i) {
					int mid = i + (1 << (j - 1));
					st[i][j] = max(st[i][j - 1], st[mid][j - 1]);
				}
			}
		
		}
	}
}
```
下面是用线段树的解法，话说我竟然把那个`ch==U`的条件写错了，改了一下就行了

线段树用重复贡献子问题还没见过，之间见到的线段树都是用来做区间求最大值和区间值修改的
AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<set>
#include<deque>
#include<cstring>
#include<map>

using namespace std;
typedef long long ll;

const int MAXN = 200010;

int a[MAXN];
int d[4 * MAXN];
int b[4 * MAXN];
int m, n;

void build(int s,int t,int p) {
	if (s == t) {
		d[p] = a[s];
		return;
	}
	int mid = s + ((t - s) >> 1);
	build(s, mid, 2 * p);
	build(mid + 1, t, 2 * p + 1);

	d[p] = max(d[2 * p], d[2 * p + 1]);
}

int query(int l, int r, int s, int t, int p) {//[l,r]查询区间,[s,t]该节点包含区间
	if (l <= s && t <= r) {
		return d[p];
	}
	int mid = s + ((t - s) >> 1);
	int t1 = 0, t2 = 0;
	
	if (l <= mid)t1 = query(l, r, s, mid, 2 * p);
	if (mid + 1 <= r)t2 = query(l, r, mid + 1, t, 2 * p + 1);
	return max(t1, t2);
}

void update(int l, int r, int c, int s, int t, int p) {
	if (l <= s && t <= r) {
		d[p] = c;
		return;
	}
	int mid = s + ((t - s) >> 1);
	if (l <= mid)
		update(l, r, c, s, mid, 2 * p);
	if (mid + 1 <= r)
		update(l, r, c, mid + 1, t, 2 * p + 1);
	d[p] = max(d[p * 2], d[p * 2 + 1]);
}

int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
		cin >> a[i];

	build(1, n, 1);

	for (int k = 0; k < m; k++) {
		char ch;
		int x, y;
		cin >> ch >> x >> y;
		if (ch == 'Q') {
			cout << query(x, y, 1, n, 1) << endl;
		}
		else if(query(x, x, 1, n, 1) < y) {
			update(x, x, y, 1, n, 1);
		}
	}
}
```
