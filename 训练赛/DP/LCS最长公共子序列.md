## 题目描述

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长公共子序列的长度。如果不存在公共子序列，返回 `0`。

子序列是在不改变字符相对顺序的情况下，删除原字符串中的部分字符或不删除任何字符后得到的新字符串。

例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

## 输入

给定两个只包含小写英文字母的字符串 `text1` 和 `text2`。

## 输出

返回两个字符串的最长公共子序列长度。

## 样例输入

```text
text1 = "abcde", text2 = "ace"
```

## 样例输出

```text
3
```

最长公共子序列为 `"ace"`。

## 数据范围

- 1 ≤ text1.length, text2.length ≤ 1000
- `text1` 和 `text2` 只包含小写英文字母

## 题解

使用动态规划。

令 `dp[i][j]` 表示 `text1` 的前 `i` 个字符与 `text2` 的前 `j` 个字符的最长公共子序列长度。

当其中一个字符串为空时，不存在公共子序列，因此：

$$
dp[0][j]=dp[i][0]=0
$$

如果当前两个字符相同，即：

```C++
text1[i-1]==text2[j-1]
```

可以将这个字符加入公共子序列：

$$
dp[i][j]=dp[i-1][j-1]+1
$$

如果当前两个字符不同，它们不能同时加入公共子序列，需要放弃其中一个字符：

$$
dp[i][j]=\max(dp[i-1][j],dp[i][j-1])
$$

最终 `dp[n][m]` 就是两个完整字符串的最长公共子序列长度。

## 复杂度分析

时间复杂度为 `O(n × m)`。

空间复杂度为 `O(n × m)`。

## code

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n=text1.size();
        int m=text2.size();

        vector<vector<int>> dp(
            n+1,
            vector<int>(m+1,0)
        );

        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(text1[i-1]==text2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=max(
                        dp[i-1][j],
                        dp[i][j-1]
                    );
                }
            }
        }

        return dp[n][m];
    }
};
```
