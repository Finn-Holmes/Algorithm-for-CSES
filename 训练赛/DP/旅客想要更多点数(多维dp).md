## 题目描述

给定两个整数 `n` 和 `k`，以及两个二维数组 `stayScore` 和 `travelScore`。

一名旅客将在拥有 `n` 座城市的国家中旅行恰好 `k` 天，并且可以选择任意城市作为起点。

每天可以选择：

- 留在当前城市 `curr`，获得 `stayScore[i][curr]` 分；
- 从当前城市 `curr` 前往另一座城市 `dest`，获得 `travelScore[curr][dest]` 分。

求旅行 `k` 天能够获得的最大分数。

## 输入

函数接收以下参数：

- `n`：城市数量；
- `k`：旅行天数；
- `stayScore`：每天留在各城市获得的分数；
- `travelScore`：城市之间旅行获得的分数。

## 输出

返回旅行恰好 `k` 天能够获得的最大分数。

## 样例输入

```text
n = 2
k = 1
stayScore = [[2,3]]
travelScore = [[0,2],[1,0]]
```

## 样例输出

```text
3
```

## 数据范围

- 1 <= n <= 200
- 1 <= k <= 200
- stayScore 的行数为 k，每行长度为 n
- travelScore 的大小为 n × n
- 1 <= stayScore[i][j] <= 100
- 0 <= travelScore[i][j] <= 100
- travelScore[i][i] = 0

## 题解

定义：

`dp[i][j]` 表示旅行完第 `i` 天，并且当天结束时位于城市 `j`，能够得到的最大分数。

到达城市 `j` 有两种方式。

第一种是前一天已经在城市 `j`，第 `i` 天继续停留：

$$
dp[i-1][j]+stayScore[i][j]
$$

第二种是前一天位于城市 `u`，第 `i` 天从 `u` 前往 `j`：

$$
dp[i-1][u]+travelScore[u][j]
$$

因此状态转移为：

$$
dp[i][j]=\max\left(dp[i-1][j]+stayScore[i][j],\max_{u\ne j}\left(dp[i-1][u]+travelScore[u][j]\right)\right)
$$

由于旅客可以选择任意城市作为起点，第 `0` 天可以：

- 直接从城市 `j` 出发并留在 `j`；
- 从任意城市 `u` 出发并前往城市 `j`。

最后枚举第 `k-1` 天结束时所在的城市，取最大值即可。

## 复杂度分析

时间复杂度为 `O(k × n²)`。

空间复杂度为 `O(k × n)`。

## code

```C++
class Solution {
public:
    int maxScore(int n, int k, vector<vector<int>>& stayScore, vector<vector<int>>& travelScore) {
        vector<vector<int>> dp(k,vector<int>(n,0));

        for(int i=0;i<k;i++){
            for(int j=0;j<n;j++){ 
                for(int u=0;u<n;u++){
                    if(i==0){
                        if(u==j){
                            dp[i][j]=max(dp[i][j],stayScore[i][j]);
                        }else{
                            dp[i][j]=max(dp[i][j],travelScore[u][j]);
                        }
                    }else{
                        if(u==j){
                            dp[i][j]=max(dp[i][j],dp[i-1][j]+stayScore[i][j]);
                        }else{
                            dp[i][j]=max(dp[i][j],dp[i-1][u]+travelScore[u][j]);
                        }
                    }
                }
            }
        }

        int ans=0;

        for(int j=0;j<n;j++){
            ans=max(ans,dp[k-1][j]);
        }

        return ans;
    }
};
```
