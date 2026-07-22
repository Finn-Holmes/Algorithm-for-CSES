## 题目描述

有 `n` 个小孩从左到右排成一条队，第 `i` 个小孩原来的位置是 `i`，活跃度为 `A[i]`。

现在可以将这些小孩重新排序。对于一个原来位于 `x`、重新排序后位于 `y` 的小孩，他能够获得的开心值为：

$$A[i]\times |x-y|$$

求重新排序后，所有小孩的开心值之和最大是多少。

## 输入

第一行包含一个整数 `n`，表示小孩数量。

第二行包含 `n` 个整数 `A1,A2,...,An`，表示每个小孩的活跃度。

## 输出

输出一个整数，表示重新排序后能够获得的最大开心值。

## 样例输入

```text
4
1 3 4 2
```

## 样例输出

```text
20
```

## 数据范围

- 2 ≤ n ≤ 2000
- 1 ≤ Ai ≤ 10^9
- 所有输入均为整数

## 题解

活跃度越大的小孩，移动相同距离产生的开心值越大，因此先按照活跃度从大到小处理。

处理一个小孩时，将他放在当前剩余位置的最左端或者最右端。这样可以优先让活跃度较大的小孩获得更大的移动距离。

使用 `child[i]` 保存第 `i` 个小孩的活跃度和原始位置，并按照活跃度降序排序。

令 `dp[l][r]` 表示已经安排了前 `l+r` 个小孩，其中：

- `l` 个小孩被放在最终队列最左边的 `l` 个位置；
- `r` 个小孩被放在最终队列最右边的 `r` 个位置；
- `dp[l][r]` 表示当前能够获得的最大开心值。

当前小孩可以放在左边第一个空位 `l+1`：

$$dp[l+1][r]=\max\left(dp[l+1][r],dp[l][r]+value\times |pos-(l+1)|\right)$$

也可以放在右边第一个空位 `n-r`：

$$dp[l][r+1]=\max\left(dp[l][r+1],dp[l][r]+value\times |pos-(n-r)|\right)$$

处理完所有小孩后，所有满足 `l+r=n` 的状态中最大值就是答案。

## 复杂度分析

排序的时间复杂度为 `O(n log n)`，动态规划的时间复杂度为 `O(n²)`。

总时间复杂度为 `O(n²)`。

空间复杂度为 `O(n²)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

const long long NEG=-(1LL<<60);

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;

    vector<pair<long long,int>> child(n);

    for(int i=0;i<n;i++){
        cin>>child[i].first;
        child[i].second=i+1;
    }

    sort(
        child.begin(),
        child.end(),
        greater<pair<long long,int>>()
    );

    vector<vector<long long>> dp(
        n+1,
        vector<long long>(n+1,NEG)
    );

    dp[0][0]=0;

    for(int index=0;index<n;index++){
        long long value=child[index].first;
        int pos=child[index].second;

        for(int l=0;l<=index;l++){
            int r=index-l;

            if(dp[l][r]==NEG){
                continue;
            }

            int left_pos=l+1;

            dp[l+1][r]=max(
                dp[l+1][r],
                dp[l][r]+value*abs(pos-left_pos)
            );

            int right_pos=n-r;

            dp[l][r+1]=max(
                dp[l][r+1],
                dp[l][r]+value*abs(pos-right_pos)
            );
        }
    }

    long long ans=0;

    for(int l=0;l<=n;l++){
        int r=n-l;
        ans=max(ans,dp[l][r]);
    }

    cout<<ans;

    return 0;
}
```
