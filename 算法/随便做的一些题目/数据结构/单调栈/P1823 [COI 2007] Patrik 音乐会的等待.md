[P1823 [COI 2007] Patrik 音乐会的等待](https://www.luogu.com.cn/problem/P1823)
难度
普及+/提高

![photo](../../photo/P1823%20[COI%202007]%20Patrik%20音乐会的等待_已批注.png)
问了下d指导给出的方案，其实和题解是一样的思路
有点做数学题，考验思维了，详细可见图

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
#include<stack>

using namespace std;
typedef long long ll;

const int MAXN = 5e5 + 5;
int n;

int main() {
	cin >> n;
	stack<pair<int,int>> s;
	ll ans = 0;
	for (int i = 1; i <= n; i++) {
		int h;
		cin >> h;
		int count = 1;

		while (!s.empty() && s.top().first < h) {
			ans += s.top().second;
			s.pop();
		}

		if (!s.empty() && s.top().first == h) {
			ans += s.top().second;
			if (s.size() > 1) {
				ans++;
			}
			count += s.top().second;
			s.pop();
			s.push({ h,count });
		}
		else if (!s.empty()) {
			ans++;
			s.push({ h,1 });
		}
		else {
			s.push({ h,1 });
		}

	}

	cout << ans;
}
```
