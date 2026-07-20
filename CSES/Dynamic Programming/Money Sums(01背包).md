## 题目描述

给定 `n` 枚硬币，每枚硬币都有一个确定的价值，并且每枚硬币最多只能使用一次。

求使用这些硬币能够组成的所有不同的正整数金额。

## 输入

第一行包含一个整数 `n`，表示硬币数量。

第二行包含 `n` 个整数 `x1,x2,...,xn`，表示每枚硬币的价值。

## 输出

第一行输出一个整数 `k`，表示能够组成的不同正整数金额数量。

第二行按照从小到大的顺序输出所有能够组成的金额。

## 样例输入

```text
4
4 2 5 2
```

## 样例输出

```text
9
2 4 5 6 7 8 9 11 13
```

## 数据范围

- 1 ≤ n ≤ 100
- 1 ≤ xi ≤ 1000

## 题解

这是一道 01 背包可行性问题，因为每枚硬币最多只能使用一次。

令 `dp[j]` 表示能否使用已经处理过的硬币组成金额 `j`。

初始时不选择任何硬币可以组成金额 `0`：

```C++
dp[0]=true;
```

处理价值为 `a[i]` 的硬币时，如果原来能够组成 `j-a[i]`，加入当前硬币后就能够组成 `j`：

```C++
dp[j]=dp[j]||dp[j-a[i]];
```

金额必须从大到小枚举，防止当前硬币在同一轮中被重复使用。

处理完所有硬币后，遍历 `dp[1...sum]`，统计并输出所有值为 `true` 的金额。

## 复杂度分析

设所有硬币价值之和为 `sum`。

时间复杂度为 `O(n × sum)`。

空间复杂度为 `O(sum)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n;
    cin>>n;

    vector<int> a(n);
    int sum=0;

    for(int i=0;i<n;i++){
        cin>>a[i];
        sum+=a[i];
    }

    vector<bool> dp(sum+1,false);
    dp[0]=true;

    for(int i=0;i<n;i++){
        for(int j=sum;j>=a[i];j--){
            if(dp[j-a[i]]==true){
                dp[j]=true;
            }
        }
    }

    int num=0;

    for(int i=1;i<=sum;i++){
        if(dp[i]==true){
            num++;
        }
    }

    cout<<num<<'\n';

    for(int i=1;i<=sum;i++){
        if(dp[i]==true){
            cout<<i<<" ";
        }
    }
}
```
