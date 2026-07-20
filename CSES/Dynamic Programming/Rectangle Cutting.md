## 题目描述

给定一个大小为 `a × b` 的长方形，需要将它切割成若干个正方形。

每次可以选择一个长方形，沿整数位置将其切成两个长方形。

求最少需要切割多少次。

## 输入

输入一行，包含两个整数 `a` 和 `b`，表示长方形的长和宽。

## 输出

输出一个整数，表示最少切割次数。

## 样例输入

```text
3 5
```

## 样例输出

```text
3
```

## 数据范围

- 1 ≤ a,b ≤ 500

## 题解

使用动态规划。

令 `dp[i][j]` 表示将大小为 `i × j` 的长方形全部切成正方形所需的最少次数。

如果 `i == j`，当前长方形本身就是正方形，不需要切割：

$$
dp[i][j]=0
$$

否则枚举第一刀的位置。

横着切，将长方形分成 `k × j` 和 `(i-k) × j`：

$$
dp[i][j]=\min(dp[i][j],dp[k][j]+dp[i-k][j]+1)
$$

竖着切，将长方形分成 `i × k` 和 `i × (j-k)`：

$$
dp[i][j]=\min(dp[i][j],dp[i][k]+dp[i][j-k]+1)
$$

最后取所有切割位置中的最小值。

## 复杂度分析

时间复杂度为 `O(ab(a+b))`。

空间复杂度为 `O(ab)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int a,b;
    cin>>a>>b;

    vector<vector<int>> dp(a+1,vector<int>(b+1,0));

    for(int i=1;i<=a;i++){
        for(int j=1;j<=b;j++){
            if(i==j){
                continue;
            }else{
                dp[i][j]=i*j;

                // 横着切
                for(int k=1;k<i;k++){
                    dp[i][j]=min(
                        dp[i][j],
                        dp[i-k][j]+dp[k][j]+1
                    );
                }

                // 竖着切
                for(int k=1;k<j;k++){
                    dp[i][j]=min(
                        dp[i][j],
                        dp[i][j-k]+dp[i][k]+1
                    );
                }
            }
        }
    }

    cout<<dp[a][b];
}
```
