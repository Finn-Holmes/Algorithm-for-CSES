## 题目描述

有一个包含 `n` 个整数的序列，两名玩家轮流进行操作。

每次操作时，当前玩家可以取走序列最左边或者最右边的数字，并将该数字加入自己的得分。

两名玩家都会采取最优策略，求第一名玩家能够获得的最大得分。

## 输入

第一行包含一个整数 `n`，表示序列长度。

第二行包含 `n` 个整数 `x1,x2,...,xn`，表示序列中的数字。

## 输出

输出一个整数，表示双方都采取最优策略时，第一名玩家能够获得的最大得分。

## 样例输入

```text
4
4 5 1 3
```

## 样例输出

```text
8
```

## 数据范围

- 1 ≤ n ≤ 5000
- -10^9 ≤ xi ≤ 10^9

## 题解

使用区间动态规划。

令 `dp[i][j]` 表示只剩下区间 `[i,j]` 时，当前玩家最终得分减去另一名玩家最终得分的最大值。

如果当前玩家选择左端点 `nums[i]`，接下来对手会在区间 `[i+1,j]` 中取得 `dp[i+1][j]` 的得分优势，因此当前玩家的得分优势为：

$$
nums[i]-dp[i+1][j]
$$

同理，选择右端点时的得分优势为：

$$
nums[j]-dp[i][j-1]
$$

因此状态转移为：

$$
dp[i][j]=\max(nums[i]-dp[i+1][j],nums[j]-dp[i][j-1])
$$

当区间中只有一个数字时，当前玩家可以直接取走：

$$
dp[i][i]=nums[i]
$$

设所有数字之和为 `total`，第一名玩家得分为 `first`，第二名玩家得分为 `second`，则：

$$
first+second=total
$$

$$
first-second=dp[0][n-1]
$$

所以第一名玩家的最大得分为：

$$
first=\frac{total+dp[0][n-1]}{2}
$$

## 复杂度分析

时间复杂度为 `O(n²)`。

空间复杂度为 `O(n²)`。

## code

```C++
#include <iostream>
#include <vector>
using namespace std;
 
int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
     
    int n;
    cin >> n;

    vector<int> nums(n);
    for (int i = 0; i < n; i++) {
        cin >> nums[i];
    }

    vector<vector<long long>> dp(n, vector<long long>(n, 0));

    for (int i = 0; i < n; i++) {
        dp[i][i] = nums[i];
    }
     
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i + len - 1 < n; i++) {
            int j = i + len - 1;

            dp[i][j] = max(
                nums[i] - dp[i + 1][j],
                nums[j] - dp[i][j - 1]
            );
        }
    }

    long long total = 0;

    for (int i = 0; i < n; i++) {
        total += nums[i];
    }

    cout << (total + dp[0][n - 1]) / 2;

    return 0;
}
```
