## 题目描述

给定一个整数数组 `nums`，判断其中是否存在长度为 `3` 的严格递增子序列。

也就是说，判断是否存在三个下标 `i、j、k`，满足：

$$
i<j<k
$$

并且：

$$
nums[i]<nums[j]<nums[k]
$$

如果存在返回 `true`，否则返回 `false`。

## 输入

给定一个整数数组 `nums`。

## 输出

如果存在长度为 `3` 的严格递增子序列，返回 `true`，否则返回 `false`。

## 样例输入

```text
nums = [2,1,5,0,4,6]
```

## 样例输出

```text
true
```

其中一个满足条件的递增子序列是 `1,4,6`。

## 数据范围

- 1 ≤ nums.length ≤ 5 × 10^5
- -2^31 ≤ nums[i] ≤ 2^31-1

## 题解

使用求最长递增子序列的贪心方法。

数组 `g` 中：

```text
g[i]表示长度为i+1的递增子序列中，结尾元素的最小值
```

对于当前数字 `nums[i]`，使用 `lower_bound` 找到 `g` 中第一个大于等于它的位置。

如果找不到，就将当前数字添加到 `g` 的末尾，表示可以构造更长的递增子序列。

如果找到了，就用当前数字替换该位置。更小的结尾可以让后面更容易接上更大的数字。

当找到的位置下标为 `2` 时，说明已经存在一个长度为 `2` 的严格递增子序列，并且当前数字可以接在它的后面，因此能够得到长度为 `3` 的递增子序列，直接返回 `true`。

使用 `lower_bound` 而不是 `upper_bound`，可以保证相等的数字不会增加递增子序列的长度。

## 复杂度分析

由于只需要判断递增子序列长度能否达到 `3`，`g` 的长度最多为 `2`，每次查找可以看作常数时间。

时间复杂度为 `O(n)`。

空间复杂度为 `O(1)`。

## code

```C++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        vector<int> g;

        for(int i=0;i<nums.size();i++){
            auto it=lower_bound(
                g.begin(),
                g.end(),
                nums[i]
            );

            // 当前数字可以成为递增子序列的第三个数字
            if(it-g.begin()==2){
                return true;
            }

            if(it==g.end()){
                g.push_back(nums[i]);
            }else{
                *it=nums[i];
            }
        }

        return false;
    }
};
```
