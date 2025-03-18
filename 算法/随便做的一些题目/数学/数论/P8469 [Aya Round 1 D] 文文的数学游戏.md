[P8469 [Aya Round 1 D] 文文的数学游戏](https://www.luogu.com.cn/problem/P8469)
难度
普及−

要求$\gcd {b_i}$，而$b_i \leq a_i$，实际上的$\gcd$最大值就是$a_i$的最小值

而方案数，对每个元素而言就是$\lfloor \frac{a_i}{\min\{a_i\}} \rfloor$，即如果min是3，那么某个元素可以取它的整数倍，3，6，9，12...

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

const int MOD = 1e9 + 7;
int n;

int main() {
	cin >> n;
	vector<int> v;
	v.resize(n);
	for (auto& x : v)
		cin >> x;

	int min_v = *min_element(v.begin(), v.end());
	ll ans = 1;
	for (auto& x : v) {
		ans = ans * (x / min_v) % MOD;
	}
	cout << min_v << ' ' << ans;
}
```
