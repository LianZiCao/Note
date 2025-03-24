[C. Kevin and Puzzle](https://codeforces.com/problemset/problem/2061/C)

>1. 状态定义：
prev_not_lying：记录前一个人诚实时的撒谎人数及其对应的方案数。
prev_lying：记录前一个人撒谎时的撒谎人数及其对应的方案数。
>2. 状态转移：
当前人诚实：若当前人的声明 a[i] 等于左边的实际撒谎人数 c，则从上一个状态转移而来，保持撒谎人数不变。
当前人撒谎：必须满足前一个人诚实，此时撒谎人数增加1，并更新到撒谎状态。
>3. 初始状态：
初始时，假设没有前一个人（视为诚实），撒谎人数为0，方案数为1。
>4. 转移规则：
从前一个诚实状态转移时，当前可以是诚实或撒谎。
从前一个撒谎状态转移时，当前必须是诚实（避免相邻两人同时撒谎）。

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

typedef unsigned long long ull;
constexpr int MOD = 998244353;


int main() {

	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;

		vector<int> v(n);
		for (auto& x : v)
			cin >> x;

		//记录前一个人诚实/撒谎时的撒谎人数（人数还是真实的）及其对应的方案数。
		map<int, int> past_lying, past_not_lying;
		past_not_lying[0] = 1;

		for (int i = 0; i < n; i++) {//dp
			map<int, int> curr_lying, curr_not_lying;

			//从未说谎的转移状态
			for (auto& [c, cnt] : past_not_lying) {
				//如果前一个（诚实的）和这个说的对，此刻的人没说谎
				if (v[i] == c)
					curr_not_lying[c] = (curr_not_lying[c] + cnt) % MOD;
				//此刻的人说谎（"说谎的人说的话可真可假"
				curr_lying[c + 1] = (curr_lying[c + 1] + cnt) % MOD;
			}

			//从说谎的转移状态
			for (auto& [c, cnt] : past_lying) {
				//没有两个说谎的人站在一起，这个一定是不说谎的，但是也要当v[i] == c再转移状态
				if (v[i] == c)
					curr_not_lying[c] = (curr_not_lying[c] + cnt) % MOD;
				
			}

			curr_lying.swap(past_lying);
			curr_not_lying.swap(past_not_lying);
		}
		int ans = 0;
		for (auto& [c, cnt] : past_not_lying) {
			ans = (ans + cnt) % MOD;
		}

		for (auto& [c, cnt] : past_lying) {
			ans = (ans + cnt) % MOD;
		}
		cout << ans << endl;
	}


}
```