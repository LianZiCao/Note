[P1507 NASA的食物计划](https://www.luogu.com.cn/problem/P1507)
难度
普及−

纯粹的二维背包问题，和一维一样的思路，再加一个维度即可

AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<string>
#include<iomanip>
#include<set>
#include<deque>

using namespace std;

const int MAXN = 4e2 + 5;
int dp[MAXN][MAXN];
int h, t, n;



int main() {
	cin >> h >> t >> n;
	for (int k = 0; k < n; k++) {
		int hi, ti, ki;
		cin >> hi >> ti >> ki;
		for (int i = h; i >= hi; i--) {
			for (int j = t; j >= ti; j--)
				dp[i][j] = max(dp[i][j], dp[i - hi][j - ti] + ki);
		}

	}
	cout << dp[h][t];
}
```
