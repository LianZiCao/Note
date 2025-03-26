[Strategic game](http://poj.org/problem?id=1463)

这题目看起来有点像图的着色的问题，实际上你自己看的话其实是树状dp，这两个还是要区分一下的
>图着色问题通常要求相邻节点颜色不同，而本题只要求边被覆盖，不需要保证相邻节点颜色不同。

poj不好的地方就是他只能c++98，不能c++11，有点难绷

还有注意下这个dfs的实现，初始化
```c++
dp[p][0] = 0;  
dp[p][1] = 1; 
```
然后就是根据子节点的状态来确定当前节点的状态

还没提交呢，或者说提交了还在等待，poj在维护吗还是怎么样？
看[这边](http://poj.org/status?bottom=24921391)后面之后全是Waiting。还有记得转换成c++98的代码，`constexpr`和范围`for`都不给用

代码
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

constexpr int MAXN = 1.5e3 + 3;
constexpr int INF = 0x3f3f3f3f;
int n;
vector<int> graph[MAXN];
int dp[MAXN][2];
int vis[MAXN];

void dfs(int p){
	vis[p] = 1;
	dp[p][0] = 0;  
	dp[p][1] = 1;  
	for (int v : graph[p]) {
		if (vis[v]) continue;  
		dfs(v);
		dp[p][0] += dp[v][1];  // 当前节点不放置，子节点必须放置
		dp[p][1] += min(dp[v][0], dp[v][1]);  // 当前节点放置，子节点取最小
	}


}

int main() {

	while (cin >> n) {
		for (int i = 0; i < n; i++) {
			graph[i].clear();
		}

		for (int i = 0; i < n; i++) {
			int u;
			cin >> u;
			cin.get();
			cin.get();
			int t;
			cin >> t;
			cin.get();
			for (int i = 0; i < t; i++) {
				int v;
				cin >> v;
				graph[u].push_back(v);
				graph[v].push_back(u);
			}
		}
		memset(vis, 0, sizeof(vis));
		memset(dp, 0, sizeof(dp));
		dfs(0);
		cout << min(dp[0][0], dp[0][1]) << endl;
	}
}
```