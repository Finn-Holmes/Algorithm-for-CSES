## 题目描述

有一个长度为 `n` 的数组，每个元素的取值范围为 1 到 `m`，并且相邻两个元素的绝对差不超过 1。

现在给出这个数组的部分信息，其中 0 表示该位置的值未知。

请计算有多少个数组满足给定信息和相邻元素限制，答案对 `10^9+7` 取模。

## 输入

第一行包含两个整数 `n,m`，分别表示数组长度和元素的最大值。

第二行包含 `n` 个整数 `x1,x2,...,xn`，其中 0 表示该位置的值未知。

## 输出

输出满足条件的数组数量，答案对 `10^9+7` 取模。

## 样例输入

```text
3 5
2 0 2
```

## 样例输出

```text
3
```

## 数据范围

- 1 ≤ n ≤ 10^5
- 1 ≤ m ≤ 100
- 0 ≤ xi ≤ m

## 题解

定义 `dp[i][j]` 表示前 `i+1` 个位置满足条件，并且第 `i` 个位置的值为 `j` 的方案数。

由于相邻元素的绝对差不超过 1，如果当前位置为 `j`，那么前一个位置只能是：

```text
j-1、j、j+1
```

因此转移为：

$$
dp[i][j]=dp[i-1][j-1]+dp[i-1][j]+dp[i-1][j+1]
$$

如果 `a[i]=0`，当前位置可以选择 1 到 `m` 中的任意值；否则只计算 `j=a[i]` 的状态。

最后对所有可能的结尾值求和。

## 复杂度分析

时间复杂度为 `O(nm)`，空间复杂度为 `O(nm)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

const int MOD=1e9+7;

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n,m;
    cin>>n>>m;

    vector<int> a(n);
    vector<vector<long long>> dp(n,vector<long long>(m+1,0));

    for(int i=0;i<n;i++){
        cin>>a[i];
    }

    if(a[0]==0){
        for(int j=1;j<=m;j++){
            dp[0][j]=1;
        }
    }else{
        dp[0][a[0]]=1;
    }

    for(int i=1;i<n;i++){
        for(int j=1;j<=m;j++){
            if(a[i]==0||a[i]==j){
                dp[i][j]=dp[i-1][j];

                if(j>1){
                    dp[i][j]=(dp[i][j]+dp[i-1][j-1])%MOD;
                }

                if(j<m){
                    dp[i][j]=(dp[i-1][j+1]+dp[i][j])%MOD;
                }
            }
        }
    }

    long long ans=0;

    for(int j=1;j<=m;j++){
        ans=(ans+dp[n-1][j])%MOD;
    }

    cout<<ans;
}
```
