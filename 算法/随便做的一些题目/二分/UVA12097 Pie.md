[UVA12097 Pie](https://www.luogu.com.cn/problem/UVA12097)

思路：二分搜索，对于一个蛋糕的体积除以划分的体积得到一个蛋糕最多可切整的数量，再将这些数量加起来和总人数比较（换句话说就是切下来的蛋糕，剩下的不能混在一起
太坐牢了，debug那么久估计还是精度问题，摆了
WA
```c++
#include<iostream>
#include<vector>
#include<cmath>
#include<iomanip>

using namespace std;


int main() {

	int sample;
	cin >> sample;
	while (sample--) {
		int n, f;
		cin >> n >> f;
		vector<long double> v(n);

		long double v_max = 0;

		for (int i = 0; i < n; i++) {
			int r;
			cin >> r;
			v[i] = r * r * 3.14159265358979323846;
			v_max = fmax(v[i], v_max);
		}

		long double l = 0;
		long double r = v_max;
		while (r - l > 1e-6) {
			long double mid = (l + r) / 2;

			int count = 0;
			for (int i = 0; i < n; i++)
				count += floor(v[i] / mid);
			if (count > f)
				l = mid;
			else
				r = mid;
		}
		cout << fixed << setprecision(4) << l;
		if (sample)
			cout << endl;

	}

}
```

精度要求太高了，又是pi的精度，又是1e-6，又是long double
第一次用，用C++20标准写的，没做改动
```c++
#include<iostream>
#include<vector>
#include<set>
#include<limits.h>
#include<cctype>
#include<algorithm>
#include<map>
#include<functional>
#include<cmath>
#include<numbers>

using namespace std;


int main() {

	int sample;
	cin >> sample;
	while (sample--) {
		int n, f;
		cin >> n >> f;
		vector<double> v(n);

		double total = 0;

		for (auto& x : v) {
			int r;
			cin >> r;
			x = r * r * numbers::pi;
			total += x;
		}

		double l = 0;
		double r = total / (f + 1);
		while (r - l > 1e-5) {
			double mid = (l + r) / 2;

			int count = 0;
			for (auto& x : v)
				count += floor(x / mid);
			if (count > f)
				l = mid;
			else
				r = mid;
		}
		cout << l << endl;
	}

}
```
