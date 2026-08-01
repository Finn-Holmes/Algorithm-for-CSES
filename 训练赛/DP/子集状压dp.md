## 题目描述

给定一个长度为 `n` 的数组 `tasks`，其中 `tasks[i]` 表示完成第 `i` 个任务所需的时间。

每个工作时间段最多工作 `sessionTime` 小时，任务不能被拆分，并且任务可以按照任意顺序完成。

求完成所有任务所需的最少工作时间段数量。

## 输入

给定：

- 整数数组 `tasks`
- 整数 `sessionTime`

## 输出

返回完成所有任务所需的最少工作时间段数量。

## 样例输入

```text
tasks = [1,2,3], sessionTime = 3
```

## 样例输出

```text
2
```

## 数据范围

- `1 <= tasks.length <= 14`
- `1 <= tasks[i] <= 10`
- `max(tasks[i]) <= sessionTime <= 15`

## 题解

由于任务数量最多只有 `14`，可以使用状态压缩动态规划。

先预处理：

```text
sum[s]
```

表示状态 `s` 中所有任务的时间之和。如果：

```text
sum[s] <= sessionTime
```

那么状态 `s` 中的任务可以放在同一个工作时间段内完成。

定义：

```text
dp[s]
```

表示完成状态 `s` 中所有任务所需的最少工作时间段数量。

枚举 `s` 的所有非空子集 `sub`。如果 `sub` 可以在一个时间段内完成，那么将 `sub` 作为最后一个工作时间段，其余任务状态为 `s ^ sub`，转移为：

$$
dp[s]=\min(dp[s],dp[s\mathbin{\char94}sub]+1)
$$

枚举一个集合的所有非空子集可以使用：

```cpp
for(int sub=s;sub;sub=(sub-1)&s)
```

最终答案为 `dp[(1<<n)-1]`。

## 复杂度分析

- 时间复杂度：`O(3^n)`
- 空间复杂度：`O(2^n)`

## code

```C++
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int n=tasks.size();
        int m=1<<n;

        vector<int> sum(1<<n);

        // 预处理每个任务集合的总用时
        for(int i=0;i<n;i++){
            for(int j=0,bit=1<<i;j<bit;j++){
                sum[bit|j]=sum[j]+tasks[i];
            }
        }

        vector<int> dp(1<<n,n);
        dp[0]=0;

        for(int s=0;s<(1<<n);s++){
            // 枚举 s 的所有非空子集
            for(int sub=s;sub;sub=(sub-1)&s){
                if(sum[sub]<=sessionTime){
                    dp[s]=min(dp[s],dp[s^sub]+1);
                }
            }
        }

        return dp[(1<<n)-1];
    }
};
```
