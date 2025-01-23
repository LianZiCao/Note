[P1464 Function](https://www.luogu.com.cn/problem/P1464)
难度
普及−

弄了一会WA了，原来是数组开小了，弄到25就行了，之前是15
从第二点可知，a，b,c这三个数其中一个大于20就返回w(20,20,20)，并且注意得先判断a,b,c均>0且均<=20，这么就可以查询，保存相应的w(a,b,c)了

注意数据规模约定那边，因为在长整型范围内，所以得用long long 

AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>
#include<cstdio>
#include<unordered_map>

using namespace std;

int memo[25][25][25] = {};

typedef long long ll;

ll func(ll a, ll b, ll c) {

	if (a <= 0 || b <= 0 || c <= 0) {
		return 1;
	}else	if (a > 20 || b > 20 || c > 20) {
		return func(20, 20, 20);
	}

	if (memo[a][b][c]) {
		return memo[a][b][c];
	}
	int result;


	if (a < b && b < c) {
		result = func(a, b, c - 1) + func(a, b - 1, c - 1) - func(a, b - 1, c);
	}
	else {
		result = func(a - 1, b, c) + func(a - 1, b - 1, c) + func(a - 1, b, c - 1) - func(a - 1, b - 1, c - 1);
	}
	memo[a][b][c] = result;
	return result;
}

int main() {

	ll a, b, c;
	while (cin >> a >> b >> c) {
		if (a == -1 && b == -1 && c == -1) {
			break;
		}
		cout << "w(" << a << ", " << b << ", " << c << ") = " << func(a, b, c) << endl;

	}
	
}
```

