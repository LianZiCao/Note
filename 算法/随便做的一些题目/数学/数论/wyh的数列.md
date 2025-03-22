[wyh的数列](https://ac.nowcoder.com/acm/problem/15448)

fibonacci数列取模的周期性题目而已，也就是“Pisano周期“，把这个知识点看明白了应该不会很难

首先搓了一个找摸c下的fibonacci周期性的函数，并且对该数字记忆化。但是这个函数一开始搓的时候非常智障用`vector`，其实直接`a`,`b`,`tem`递推就可以了（话说用do while还是少见，还有出口条件应该是`!(a == 0 && b == 1)`，逻辑清晰点，`a != 0 || b != 1`就一眼看不出来）

然后一个麻烦的问题，或者说导致WA的地方是算fibonacci，因为周期最高可能`1e6`（`1e3*1e3+1`），递推这么多次会溢出`long long`，所以得用`fib_mod`求模c下的值

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
typedef unsigned long long ull;
constexpr int MAXN = 1e6 + 5;
int T;
ull a, b, c;
int period[1005];

ull pow_mod(ull a, ull b,ull mod) {
	a %= mod;
	ull ans = 1;
	while (b > 0) {
		if (b & 1)
			ans = ans * a % mod;
		a = a * a % mod;
		b >>= 1;
	}
	return ans;

}

int find_cycle() {
	if (c == 1)return 1;
	ull a = 0, b = 1, cycle = 0;
	do {
		ull tem = (a + b) % c;
		a = b;
		b = tem;
		cycle++;
	} while (!(a == 0 && b == 1));
	return cycle;

}

pair<ull, ull> fib_mod(ull n, ull mod) {
	if (n == 0) return { 0, 1 };
	auto [a, b] = fib_mod(n >> 1, mod);
	ull c = a * ((2 * b % mod + mod - a) % mod) % mod;
	ull d = (a * a % mod + b * b % mod) % mod;
	if (n & 1)
		return { d, (c + d) % mod };
	else
		return { c, d };
}

int main(){

	cin >> T;
	while (T--) {
		cin >> a >> b >> c;
		int cycle = 0;
		if (period[c] != 0) {
			cycle = period[c];
		}
		else {
			cycle = period[c] = find_cycle();
		}
		int index = pow_mod(a, b, cycle);
		cout << fib_mod(index, c).first << endl;
	}
} 
```
