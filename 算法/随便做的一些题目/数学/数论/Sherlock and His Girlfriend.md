[Sherlock and His Girlfriend](https://ac.nowcoder.com/acm/problem/50556)

有点数学了（x）数论题就是这样。这题目关键就是
>使得一件珠宝的价格是另一件的质因子时，两件珠宝的颜色不同

这句话要理解清楚，“质因子”这个词太多信息了
先考虑所有的质数，自然是质数之间互质，可以为同一种颜色，而非质数与质数存在质因子，为另一种颜色，思路出来就可以了

还有质数筛的循环`j+=i`不要写惯写成`j++`了（

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
const int MAXN = 1e5 + 5;

bool is_prime[MAXN];

void pre() {
    fill(is_prime, is_prime + MAXN, 1);
    is_prime[0] = is_prime[1] = 0;
    for (int i = 2; i < MAXN; i++) {
        if (is_prime[i]) {
            for (int j = i * 2; j < MAXN; j+=i)
                is_prime[j] = 0;
        }
    }
}

int main() {
    int n;
    cin >> n;
    pre();

    vector<int> ans(n);
    bool has_not_prime = 0;
    for (int i = 0; i < n; i++) {
        int v = i + 2;
        if (is_prime[v]) {
            ans[i] = 1;
        }
        else {
            ans[i] = 2;
            has_not_prime = 1;
        }
    }
    int k = has_not_prime ? 2 : 1;
    cout << k << endl;
    for (int i = 0; i < n; i++) {
        cout << ans[i] << ' ';
    }

}
```
