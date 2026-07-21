## 题目描述

给定一个整数数组 `nums` 和一个整数 `k`，需要将数组分割成若干个连续非空子数组。

对于一个子数组，将其中所有只出现一次的数字删除，剩余数组记为 `trimmed(subarray)`。

子数组的重要性为：

$$
k+trimmed(subarray).length
$$

所有子数组重要性之和就是本次分割的代价。

求所有分割方案中的最小代价。

## 输入

给定一个整数数组 `nums` 和一个整数 `k`。

## 输出

返回分割数组能够得到的最小代价。

## 样例输入

```text
nums = [1,2,1,2,1,3,3], k = 2
```

## 样例输出

```text
8
```

可以分割成：

```text
[1,2]
[1,2,1,3,3]
```

两个子数组的重要性分别为 `2` 和 `6`，总代价为 `8`。

## 数据范围

- 1 ≤ nums.length ≤ 1000
- 0 ≤ nums[i] < nums.length
- 1 ≤ k ≤ 10^9

## 题解

令 `dp[i]` 表示分割数组前 `i` 个元素，即 `nums[0...i-1]` 的最小代价。

枚举最后一个子数组为 `nums[j...i]`。

设 `unique` 表示这个子数组中只出现一次的数字数量。因为这些数字会被全部删除，所以：

$$
trimmed.length=(i-j+1)-unique
$$

选择 `nums[j...i]` 作为最后一个子数组时，总代价为：

$$
dp[j]+k+(i-j+1)-unique
$$

整理得到：

$$
k+i+1+(dp[j]-j-unique)
$$

对于固定的右端点 `i`，`k+i+1` 是固定的，因此只需要从所有左端点 `j` 中找出最小的：

$$
dp[j]-j-unique
$$

枚举右端点 `i`，再从 `i` 向左枚举 `j`，使用 `state[x]` 记录数字 `x` 在当前子数组中的出现次数：

- 从 `0` 次变成 `1` 次时，`unique` 增加 `1`；
- 从 `1` 次变成 `2` 次时，`unique` 减少 `1`；
- 出现次数超过 `2` 后，`unique` 不再变化。

这样可以在常数时间内得到每个子数组的 `trimmed` 长度。

## 复杂度分析

需要枚举最后一个子数组的左右端点。

时间复杂度为 `O(n²)`。

空间复杂度为 `O(n)`。

## code

```C++
class Solution {
public:
    int minCost(vector<int>& nums, int k) {
        int n=nums.size();

        // dp[i]表示分割前i个元素的最小代价
        vector<int> dp(n+1);
        dp[0]=0;

        vector<int> state(n);

        for(int i=0;i<n;i++){
            fill(state.begin(),state.end(),0);

            int unique=0;
            int res=INT_MAX;

            // 枚举最后一个子数组的左端点
            for(int j=i;j>=0;j--){
                int x=nums[j];

                if(state[x]==0){
                    state[x]++;
                    unique++;
                }else if(state[x]==1){
                    state[x]++;
                    unique--;
                }

                res=min(
                    res,
                    dp[j]-j-unique
                );
            }

            dp[i+1]=k+i+1+res;
        }

        return dp[n];
    }
};
```
