[P1216 [IOI 1994] 数字三角形 Number Triangles](https://www.luogu.com.cn/problem/P1216)
难度
普及−

一看这题属实有点不会？仔细一想还是想出来了，在中间的时候每次加上上次路径的最大值然后一直往下求就可以了，最后扫一遍输出最大值。看了下今天的讲义好像也就是这么做的，所以这就是dp？（x）所谓转移方程原来就是"上层相邻的最大值加上这个位置的值"？

AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<numeric>

using namespace std;


int main() {
	int n;
	cin >> n;
	vector<vector<int>> v(n);
	for (int i = 0; i < n; i++) {
		for (int j = 0; j <= i; j++) {
			int m;
			cin >> m;

			if (i > 0 && j > 0 && j < i) {
				m += max(v[i - 1][j - 1], v[i - 1][j]);
			}
			else if(i > 0 && j == 0) {
				m += v[i - 1][0];
			}
			else if (i > 0 && j == i) {
				m += v[i - 1][j - 1];
			}

			v[i].push_back(m);

		}
	}
	int max_v = 0;
	for (auto& x : v[n - 1])
		max_v = max(max_v, x);
	cout << max_v << endl;

}
```