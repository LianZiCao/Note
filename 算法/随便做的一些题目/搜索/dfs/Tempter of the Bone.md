[Tempter of the Bone](https://acm.hdu.edu.cn/showproblem.php?pid=1010)
这题一开始最基本的思路是有的，就是DFS遍历，但是很显然复杂度太高，肯定TLE，无奈之下问下d指导。结果给了两个剪枝方案，一个是奇偶性剪枝，另外一个是距离剪枝（当距离大于剩余时间，直接返回）

思路有了，然后也做得太粗心了，WA了问问d指导改一下，思路大体是没问题的，细节真的差了点（不在状态了所以是
1. 起始点也要标记为访问过
2. `steps`状态重置了，但是`st`状态没重置
3. 数组可能越界，要判断是否在迷宫里面
4. 输出`NO`而非`No`

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
typedef long long ll;
const int MAX_SIZE = 10;
char graph[MAX_SIZE][MAX_SIZE];

int steps[MAX_SIZE][MAX_SIZE];
int op_x[] = { 0,0,1,-1 };
int op_y[] = { 1,-1,0,0 };

int st;
int n, m, t;

int s_x, s_y;
int e_x, e_y;

int manhattan(int x1, int y1, int x2, int y2) {
	return abs(x1 - x2) + abs(y1 - y2);
}

bool dfs(int x,int y) {
	if (graph[x][y] == 'D' && st == t)
		return 1;
	if (t - st < manhattan(x, y, e_x, e_y))
		return 0;

	for (int i = 0; i < 4; i++) {
		int x_ = x + op_x[i];
		int y_ = y + op_y[i];

		if (x_ < 1 || x_ > n || y_ < 1 || y_ > m) {
			continue;
		}

		if (steps[x_][y_] == 0 && (graph[x_][y_] == '.' || graph[x_][y_] == 'D')) {
			steps[x_][y_] = 1;
			st++;
			if (dfs(x_, y_))
				return 1;
			steps[x_][y_] = 0;
			st--;
		}
	}
	return 0;
}

int main() {
	while (cin >> n >> m >> t) {
		if (n == 0)
			break;
		memset(steps, 0, sizeof(steps));
		st = 0;

		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= m; j++) {
				cin >> graph[i][j];
				if (graph[i][j] == 'S') {
					s_x = i, s_y = j;
				}else if (graph[i][j] == 'D') {
					e_x = i, e_y = j;
				}
			}
		}
		steps[s_x][s_y] = 1;
		if (manhattan(s_x, s_y, e_x, e_y) % 2 == t % 2 && dfs(s_x, s_y))//奇偶剪枝
			cout << "YES" << endl;
		else
			cout << "NO" << endl;
	}

}
```
