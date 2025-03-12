[First One](https://acm.hdu.edu.cn/showproblem.php?pid=5358)

![photo](../photo/First%20One_已批注.png)

这题要用奇妙的数学观察（x），观察$\lfloor \log _2 S(i,j) \rfloor +1$，题目给可以认为$\log_2 0=0=\log_2 1$，其实实际上是$S(i,j)$的二进制位数，记其为$k$

那么因为$a_i \in [0,10^5]$，所以实际上$S(i,j)\in [0,10^{10}]$，$S$的最低二进制位数可以为1，最高可以为35

枚举每一个二进制位$k\in[1,35]$，使得$k=1$，范围为$[0,1]$，$k\ne 1$，范围为$[2^{k-1},2^k-1]$。而确定每一个$k$，从$i=1 \rarr n$，双指针确定满足$S(i,j)$的最小与最大右边界范围（S单调递增）,算出其的贡献。

从这边就看出来了主要是要想到双指针，把$\displaystyle \Sigma_{i=1}^n\Sigma_{j=i}^n S(i,j)$转化为离散的$k$，然后用双指针算出对每个离散的k的满足的左右$i,j$，（其中$i : 1 \rarr n$）计算贡献$k$的将其累加（$k : 1\rarr n$）即可

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

const int MAXN = 100010;
ll sum[MAXN], p2[40];
int n, k;

void init() {
	p2[0] = 1;
	for (int i = 1; i < 40; i++) {
		p2[i] = p2[i - 1] * 2;
	}
}

ll solve(int k) {
	ll low = (k == 1) ? 0 : p2[k - 1];
	ll up = p2[k] - 1;
	ll res = 0;

	int r1 = 1, r2 = 0;

	for (int i = 1; i <= n; i++) {
		r1 = max(r1, i);
		while (r1 <= n && sum[r1] - sum[i - 1] < low)r1++;
		if (r1 > n || sum[r1] - sum[i - 1] > up) continue;

		r2 = max(r1, r2);
		//while (r2 <= n && sum[r2] - sum[i - 1] < up)r2++;
		while (r2 + 1 <= n && sum[r2 + 1] - sum[i - 1] <= up) r2++;
		if (r1 > r2)continue;

		ll cnt = r2 - r1 + 1;
		res += k * (i * cnt + (r1 + r2) * cnt / 2);
	}


	return res;
}

int main() {
	init();
	cin >> k;
	while (k--) {
		cin >> n;

		for (int i = 1; i <= n; i++) {
			cin >> sum[i];
			sum[i] += sum[i - 1];
		}

		ll ans = 0;
		for (int i = 1; i <= 35; i++) {
			ans += solve(i);
		}
		cout << ans << endl;
	}
}
```