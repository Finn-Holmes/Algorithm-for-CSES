## 题目描述

对于从 `1` 到 `n` 的每个整数，删除它十进制表示中的所有数字 `0`，记录删除后得到的整数。

求最终记录到的不同整数数量。

## 输入

给定一个正整数 `n`。

## 输出

返回记录到的不同整数数量。

## 样例输入

```text
n = 10
```

## 样例输出

```text
9
```

## 数据范围

- `1 <= n <= 10^15`

## 题解

删除一个整数中的所有 `0` 后，得到的结果一定是不含数字 `0` 的正整数。

同时，删除数字 `0` 不会让数值变大，因此对于任意 `x<=n`，删除零得到的结果 `y` 一定满足：

```text
1 <= y <= n
```

反过来，任意一个不含数字 `0` 且不超过 `n` 的整数 `y`，都可以直接选择 `x=y`，删除零后仍然得到 `y`。

因此，原问题等价于：

> 统计 `[1,n]` 中十进制表示不含数字 `0` 的整数数量。

使用数位 DP，令：

```text
dfs(i,cnt0,limit_low,limit_high)
```

表示当前处理到第 `i` 位，已经出现 `cnt0` 个零，并且受到上下界限制时的合法方案数。

本题要求数字中不含零，因此调用：

```cpp
digitDP(1,n,0)
```

即统计零的数量恰好为 `0` 的整数。

代码中的“不填数字”分支用于处理位数比 `n` 少的数字，跳过的位置属于前导零，不能计入数字中零的数量。

## 复杂度分析

设 `n` 的十进制位数为 `L`。

- 时间复杂度：`O(L)`
- 空间复杂度：`O(L)`

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int n,diff;
string low_s,high_s;
vector<vector<long long>> memory;

long long dfs(int i,int cnt0,bool limit_low,bool limit_high,int target){
    if(cnt0>target){
        return 0;
    }

    if(i==n){
        return cnt0==target;
    }

    if(!limit_low&&!limit_high&&memory[i][cnt0]>=0){
        return memory[i][cnt0];
    }

    int low_num=0,high_num=9;

    if(limit_low&&i>=diff){
        low_num=low_s[i-diff]-'0';
    }

    if(limit_high){
        high_num=high_s[i]-'0';
    }

    long long res=0;
    int d=low_num;

    // 不填当前位，前导零不计入零的数量
    if(limit_low&&i<diff){
        res=dfs(i+1,0,true,false,target);
        d=1;
    }

    for(;d<=high_num;d++){
        res+=dfs(
            i+1,
            cnt0+(d==0),
            limit_low&&d==low_num,
            limit_high&&d==high_num,
            target
        );
    }

    if(!limit_low&&!limit_high){
        memory[i][cnt0]=res;
    }

    return res;
}

long long digitDP(long long low,long long high,int target){
    low_s=to_string(low);
    high_s=to_string(high);

    n=high_s.size();
    diff=n-low_s.size();

    memory.assign(n,vector<long long>(target+1,-1));

    return dfs(0,0,true,true,target);
}

class Solution {
public:
    long long countDistinct(long long n) {
        return digitDP(1,n,0);
    }
};
```
