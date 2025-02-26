参考[oiwiki上的背包 DP](https://oi-wiki.org/dp/knapsack/)
### 0-1 背包
一个物品只能选一次，要么选或者不选，即0，1
第 $i$ 个物品的重量 $w_{i}$，价值 $v_{i}$，以及背包的总容量 $W$

对于每个$i$，当前重量对应价值$f_j$的更新依赖于上一次的重量对应价值$f_j$和**上一次**的重量对应价值减去重量加上价值$f_{j-w_i}+v_i$，取二者的最大值（为了保证是上一次，迭代的时候是自$W$至$w_i$）

转移方程$$ f_j=\max \left(f_j,f_{j-w_i}+v_i\right)$$

示例代码
```c++
for (int i = 1; i <= n; i++)
  for (int l = W; l >= w[i]; l--) f[l] = max(f[l], f[l - w[i]] + v[i]);
```

### 完全背包
一个物品可以选无穷次
状态转移方程：
$$f_{i,j}=\max_{k=0}^{+\infty}(f_{i-1,j-k\times w_i}+v_i\times k)$$
让$f_{i,j-w_i}$ 的更新依赖于$ f_{i,j-2\times w_i} $，进行优化后：
$f_{i,j}=\max(f_{i-1,j},f_{i,j-w_i}+v_i)$
```c++
#include <iostream>
using namespace std;
constexpr int MAXN = 1e4 + 5;
constexpr int MAXW = 1e7 + 5;
int n, W, w[MAXN], v[MAXN];
long long f[MAXW];

int main() {
  cin >> W >> n;
  for (int i = 1; i <= n; i++) cin >> w[i] >> v[i];
  for (int i = 1; i <= n; i++)
    for (int l = w[i]; l <= W; l++)
      if (f[l - w[i]] + v[i] > f[l]) f[l] = f[l - w[i]] + v[i];  // 核心状态方程
  cout << f[W];
  return 0;
}
```

### 多重背包
每种物品有 $k_i$ 个，而非一个
#### 朴素做法
k个物品给放出来，或者说对于k个物品中的任意一个，只能选一次
状态转移方程
$$f_{i,j}=\max_{k=0}^{k_i}(f_{i-1,j-k\times w_i}+v_i\times k)$$
这样做与0-1背包解决方法相同
代码
```c++
for (int i = 1; i <= n; i++) {
  for (int weight = W; weight >= w[i]; weight--) {
    // 多遍历一层物品数量
    for (int k = 1; k * w[i] <= weight && k <= cnt[i]; k++) {
      dp[weight] = max(dp[weight], dp[weight - k * w[i]] + k * v[i]);
    }
  }
}
```
#### 二进制分组优化
$A_{i,j}$ 代表第 $i$ 种物品拆分出的第 $j$ 个物品。
「同时选 $A_{i,1},A_{i,2}$」与「同时选 $A_{i,2},A_{i,3}$」完全等效，需化简拆分方式
##### 过程
「二进制分组」
令 $A_{i,j}\left(j\in\left[0,\lfloor \log_2(k_i+1)\rfloor-1\right]\right)$ 分别表示由 $2^{j}$ 个单个物品「捆绑」而成的大物品。特殊地，若 $k_i+1$ 不是 $2$ 的整数次幂，则需要在最后添加一个由 $k_i-2^{\lfloor \log_2(k_i+1)\rfloor-1}$ 个单个物品「捆绑」而成的大物品用于补足。

例子：

-   $6=1+2+3$
-   $8=1+2+4+1$
-   $18=1+2+4+8+3$
-   $31=1+2+4+8+16$

复杂度 
$O(W\sum_{i=1}^n\log_2k_i)$

代码：
p单件物品重量，h单件物品价值，k表示物品数
作用是成功**把组给划分**了出来放到list数组里面，最后再利用0-1背包的代码即可解决
```c++
index = 0;
for (int i = 1; i <= m; i++) {
  int c = 1, p, h, k;
  cin >> p >> h >> k;
  while (k > c) {
    k -= c;
    list[++index].w = c * p;
    list[index].v = c * h;
    c *= 2;
  }
  list[++index].w = p * k;
  list[index].v = h * k;
}
```
### 混合背包
有的只能取一次，有的能取无限次，有的只能取 $k$ 次。
伪代码：
```c++
for (循环物品种类) {
  if (是 0 - 1 背包)
    套用 0 - 1 背包代码;
  else if (是完全背包)
    套用完全背包代码;
  else if (是多重背包)
    套用多重背包代码;
}
```
### 二维费用背包
只需在状态中增加一维存放第二种价值即可
代码
```c++
for (int k = 1; k <= n; k++)
  for (int i = m; i >= mi; i--)    // 对经费进行一层枚举
    for (int j = t; j >= ti; j--)  // 对时间进行一层枚举
      dp[i][j] = max(dp[i][j], dp[i - mi][j - ti] + 1);
```
### 分组背包
从0-1背包的“每个物品选或不选”变成了“每组物品选一个，或者不选”
可以将 $t_{k,i}$ 表示第 $k$ 组的第 $i$ 件物品的编号是多少，再用 $\mathit{cnt}_k$ 表示第 $k$ 组物品有多少个
通过先循环背包容量，再循环物品，使得每个dp在每一轮循环中只选择每组物品的一个
![photo](../../photo/fenzubeibao.png)
第一行的循环把0-1背包的“每个物品”→“循环每个组”，使得每个组只选择一个物品或者不选
第二行按照重量递减，依次更新
第三行枚举这个组的所有物品
代码：
```c++
for (int k = 1; k <= ts; k++)          // 循环每一组
  for (int i = m; i >= 0; i--)         // 循环背包容量
    for (int j = 1; j <= cnt[k]; j++)  // 循环该组的每一个物品
      if (i >= w[t[k][j]])             // 背包容量充足
        dp[i] = max(dp[i],
                    dp[i - w[t[k][j]]] + c[t[k][j]]);  // 像0-1背包一样状态转移
```
### 有依赖的背包
![photo](../../photo/yilaibeibao.png)
把每个依赖的组看成一个组，变成分组背包的形式
可以参考例题[P1064 [NOIP 2006 提高组] 金明的预算方案](https://www.luogu.com.cn/problem/P1064)