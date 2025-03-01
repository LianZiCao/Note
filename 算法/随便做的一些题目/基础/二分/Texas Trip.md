题目在[poj](http://poj.org/problem?id=3301)上
寒假训练的题目有几题没做来补了

这题出现在二分里面，但是不知道是怎么二分，有点没思路，而且还是计算几何，这个真的难整

![photo](../../photo/Texas%20Trip.png)
对每个点旋转，求出左右投影边界，然后两个边界取最大值，返回平方
可能旋转这一步还要点理解，我的理解就是对基向量旋转，并且参考单位圆（看了3brown1blue的线性代数本质

角度设置`l-r`要`1e-8`，高了会WA

编译错误代码，但是实际如果能过编译会AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<string>
#include<set>
#include<map>

using namespace std;

const int times = 100;

struct point {
	double x, y;
};

double get_area(vector<point>& points, double angle) {
	double max_X = -1e4, max_Y = -1e4;
	double min_X = 1e4, min_Y = 1e4;
	double c = cos(angle), s = sin(angle);
	for (size_t i = 0; i < points.size(); ++i) {
		point& p = points[i];
		double rotation_x = p.x * c - p.y * s;
		double rotation_y = p.x * s + p.y * c;
		max_X = max(max_X, rotation_x);
		max_Y = max(max_Y, rotation_y);
		min_X = min(min_X, rotation_x);
		min_Y = min(min_Y, rotation_y);
	}
	double wide = max_X - min_X;
	double length = max_Y - min_Y;
	double max_len = std::max(wide, length);
	return max_len * max_len;
}

double solve(vector<point>& points) {
	double l = 0, r = asin(1);
	while (r - l > 1e-8) {
		double lmid = l + (r - l) / 3;
		double rmid = r - (r - l) / 3;
		double lv = get_area(points, lmid);
		double rv = get_area(points, rmid);
		if (lv > rv)
			l = lmid;
		else
			r = rmid;
	}
	return get_area(points, l);
}

int main() {

	int w;
	cin >> w;
	while (w--) {
		int n;
		cin >> n;
		vector<point> points(n);
		for (int i = 0; i < n; ++i) {
			cin >> points[i].x >> points[i].y;
		}
		double ans = solve(points);
		cout << fixed << setprecision(2) << ans << endl;
	}
}
```
把范围for循环改成一般的for循环，并且`max`改成`std::max`即可编译通过