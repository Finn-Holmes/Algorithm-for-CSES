## 题目描述

给定一个 `n×n` 的网格，其中部分格子是陷阱。

需要计算从左上角走到右下角的路径数量，每次只能向右或向下移动，并且不能进入有陷阱的格子。

## 输入

第一行包含一个整数 `n`，表示网格大小。

接下来 `n` 行，每行包含 `n` 个字符：

- `.` 表示可以经过的格子；
- `*` 表示陷阱。

## 输出

输出从左上角到右下角的路径数量，答案对 `10^9+7` 取模。

## 样例输入

```text
4
....
.*..
...*
*...
```

## 样例输出

```text
3
```

## 数据范围

- 1 ≤ n ≤ 1000

## 题解

定义 `dp[i][j]` 表示从左上角走到格子 `(i,j)` 的路径数量。

因为只能向右或向下移动，所以到达 `(i,j)` 的上一步只能来自：

- 上方格子 `(i-1,j)`；
- 左方格子 `(i,j-1)`。

如果当前格子不是陷阱，转移方程为：

$$
dp[i][j]=dp[i-1][j]+dp[i][j-1]
$$

如果当前格子是陷阱，则不能到达，`dp[i][j]=0`。

当左上角不是陷阱时，初始化 `dp[1][1]=1`。

## 复杂度分析

时间复杂度为 `O(n²)`，空间复杂度为 `O(n²)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

const int MOD=1e9+7;

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n;
    cin>>n;

    vector<vector<char>> a(n+1,vector<char>(n+1));

    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            cin>>a[i][j];
        }
    }

    vector<vector<long long>> dp(n+1,vector<long long>(n+1,0));

    if(a[1][1]=='*'){
        dp[1][1]=0;
    }else{
        dp[1][1]=1;
    }

    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            if(a[i][j]!='*'){
                dp[i][j]=(dp[i][j]+dp[i-1][j]+dp[i][j-1])%MOD;
            }
        }
    }

    cout<<dp[n][n];

    return 0;
}
```
