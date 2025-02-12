[P1601 A+B Problem（高精）](https://www.luogu.com.cn/problem/P1601)
难度
普及−

把add函数中的`c[i] += a[i] + b[i];`不要写成 `=` 运算了，因为可能前面的数字有进位，所以是加上操作，而不是直接赋值

AC
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<iomanip>
#include<cmath>
#include<limits.h>
#include<cstring>

using namespace std;

const int LEN = 1004;

void clear(int* a) {
	for (int i = 0; i < LEN; i++)
		a[i] = 0;
}

void read(int * a) {
	static char s[LEN + 1];//空终止字符结尾，故+1
	cin >> s;

	clear(a);
	int len = strlen(s);
	for (int i = 0; i < len; i++)
		a[i] = s[len - i - 1] - '0';

}

void print(int* a) {
	int i = 0;
	for (i = LEN - 1; i >= 1; i--)
		if (a[i] != 0)
			break;
	for (; i >= 0; i--)
		cout << a[i];
	
}

void add(int* a, int* b, int* c) {
	clear(c);

	for (int i = 0; i < LEN - 1; i++) {
		c[i] += a[i] + b[i];//+=而不是=，因为可能前面进位了
		if (c[i] >= 10) {
			c[i] -= 10;
			c[i + 1] += 1;
		}
	}

}

int main() {

	int a[LEN],b[LEN],c[LEN];
	read(a);
	read(b);
	add(a, b, c);
	print(c);

}
```