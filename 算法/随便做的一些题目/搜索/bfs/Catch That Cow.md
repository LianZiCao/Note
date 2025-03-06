[Catch That Cow](http://poj.org/problem?id=3278)

这题目也是超级简单，只要会bfs一下就做出来了

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

using namespace std;
const int MAXN = 1e5 + 5;
int n, k, ans;
int vis[MAXN];

void bfs() {
	if (n == k) {
		return;
	}

	deque<int> q;
	q.push_back(n);
	vis[n] = 1;
	while (!q.empty()) {
		int pos = q.front();
		q.pop_front();

		if (pos - 1 >= 0 && !vis[pos - 1]) {
			q.push_back(pos - 1);
			vis[pos - 1] = vis[pos] + 1;
		}
		if (pos + 1 <= 100000 && !vis[pos + 1]) {
			q.push_back(pos + 1);
			vis[pos + 1] = vis[pos] + 1;
		}
		if (2 * pos <= 100000 && !vis[2 * pos]) {
			q.push_back(2 * pos);
			vis[2 * pos] = vis[pos] + 1;
		}

		if (pos == k) {
			ans = vis[pos] - 1;
			break;
		}
	}

}


int main() {
	cin >> n >> k;
	
	bfs();
	cout << ans;
}
```
