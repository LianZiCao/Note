[题目pdf以及提交](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=378)或者看[洛谷](https://www.luogu.com.cn/problem/UVA437)上面的

这题目是[oiwiki上面](https://oi-wiki.org/dp/dag/)的例题DAG DP上面的例题，按照参考代码自己复现了一下

思路就是建立一个无向无环图。当前节点有一个width和length，对于每个砖块的三种状态，看看是否能够放置下，如果可以的话就继续递归下去搜索，然后边的权就是高度。自然当无法进行下去（递归）的时候，就是达到终点了。初始就是直接调`max(ans, dag_dp(i, 0, n) + x[i])`，`max(ans, dag_dp(i, 1, n) + x[i])`，`max(ans, dag_dp(i, 2, n) + x[i])`即可（因为地面可以放任意一块砖

同时采取了记忆化搜素的思路，`d[c][rot]`存储了第`i`块砖的第`rot`旋转方式能够得到的最大值，防止重复遍历

我一些没注意的地方：`width = y[c];`不要把`c`写成`rot`了，以及如果`d[c][rot] == -1`的话后面要改成`d[c][rot] = 0;`（要不然最后状态的时候返回-1了

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

constexpr int MAXN = 35;
int d[MAXN][3];
int x[MAXN], y[MAXN], z[MAXN];

int dag_dp(int c,int rot,int n) {
	if (d[c][rot] != -1) {//这边记忆化了 特定砖块 特定放置方式 的可能返回的最大值
		return d[c][rot];
	}

	//在当前第c块砖的rot方式为基放置
	d[c][rot] = 0;
	int width = 0, length = 0;
	if (rot == 0) {
		width = y[c];
		length = z[c];
	}else if (rot == 1) {
		width = x[c];
		length = z[c];
	}
	else if (rot == 2) {
		width = x[c];
		length = y[c];
	}

	for (int i = 0; i < n; i++) {//当前节点找所有可能的（第0~n-1个砖块，三个状态），判断是否可以放置，若是则递归下去
		if (y[i] < width && z[i] < length || z[i] < width && y[i] < length)
			d[c][rot] = max(d[c][rot], dag_dp(i, 0, n) + x[i]);
		if (x[i] < width && z[i] < length || z[i] < width && x[i] < length)
			d[c][rot] = max(d[c][rot], dag_dp(i, 1, n) + y[i]);
		if (y[i] < width && x[i] < length || x[i] < width && y[i] < length)
			d[c][rot] = max(d[c][rot], dag_dp(i, 2, n) + z[i]);
	}
	return d[c][rot];
}

int dp(int n) {
	for (int i = 0; i < n; i++) {
		d[i][0] = d[i][1] = d[i][2] = -1;
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		ans = max(ans, dag_dp(i, 0, n) + x[i]);
		ans = max(ans, dag_dp(i, 1, n) + y[i]);
		ans = max(ans, dag_dp(i, 2, n) + z[i]);
	}
	return ans;
}

int main() {
	int t = 0;
	int n;
	while (cin >> n) {
		if (n == 0)break;
		t++;
		for (int i = 0; i < n; i++) {
			cin >> x[i] >> y[i] >> z[i];
		}
		cout << "Case " << t << ":" << " maximum height = " << dp(n) << endl;
	}

}
```
