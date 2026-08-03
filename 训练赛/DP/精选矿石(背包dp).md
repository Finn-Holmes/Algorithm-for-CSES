## 题目描述

有 \(n\) 块矿石，第 \(i\) 块矿石的重量为 \(w_i\)，价值为 \(v_i\)。

所有矿石的最大重量与最小重量之差不超过 10。在总重量不超过 \(m\) 的情况下，求能够获得的最大总价值。

## 输入

第一行两个整数 `n, m`。

接下来 `n` 行，每行两个整数 `w[i], v[i]`，表示矿石的重量和价值。

## 输出

输出能够获得的最大总价值。

## 样例输入

```text
4 6
2 1
3 4
4 10
3 8
```

## 样例输出

```text
12
```

## 提示或数据范围

- 1 ≤ n ≤ 100
- 1 ≤ m ≤ 10^9
- 1 ≤ w[i] ≤ 10^9
- 1 ≤ v[i] ≤ 10^7
- 最大重量与最小重量之差不超过 10

## 题解

由于容量 `m` 很大，不能直接使用普通的背包 DP。

设所有矿石中的最小重量为 `base`。每块矿石的重量可以写成：

$$
w_i=base+(w_i-base)
$$

因为最大重量与最小重量之差不超过 10，所以：

$$
0\le w_i-base\le 10
$$

如果选择了 `j` 块矿石，并且这些矿石的重量偏移量之和为 `k`，那么实际总重量就是：

$$
j\cdot base+k
$$

定义：

`dp[j][k]` 表示选择 `j` 块矿石，重量偏移量之和为 `k` 时的最大总价值。

最多选择 `n` 块矿石，每块矿石的偏移量最多为 10，因此 `k` 最大只有 `10n`。

对于每块矿石，按照 01 背包的方式倒序枚举：

$$
dp[j][k]=\max(dp[j][k],dp[j-1][k-(w_i-base)]+v_i)
$$

当满足：

$$
j\cdot base+k\le m
$$

说明当前状态没有超过载重限制，可以用它更新答案。

## 复杂度分析

时间复杂度：`O(n^3)`，其中偏移量范围为 `10n`，实际为 `O(10n^3)`。

空间复杂度：`O(n^2)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;

    vector<long long> w(n + 1), v(n + 1);
    long long base = LLONG_MAX;

    for (int i = 1; i <= n; i++) {
        cin >> w[i] >> v[i];
        base = min(base, w[i]);
    }

    vector<vector<long long>> dp(
        n + 1, vector<long long>(10 * n + 1, -1)
    );

    dp[0][0] = 0;
    long long ans = 0;

    for (int i = 1; i <= n; i++) {
        long long offset = w[i] - base;

        for (int j = n; j >= 1; j--) {
            for (int k = 10 * n; k >= offset; k--) {
                if (dp[j - 1][k - offset] == -1) {
                    continue;
                }

                dp[j][k] = max(
                    dp[j][k],
                    dp[j - 1][k - offset] + v[i]
                );

                if (1LL * j * base + k <= m) {
                    ans = max(ans, dp[j][k]);
                }
            }
        }
    }

    cout << ans;

    return 0;
}
```
