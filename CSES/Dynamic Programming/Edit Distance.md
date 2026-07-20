## 题目描述

两个字符串之间的编辑距离，是将一个字符串转换成另一个字符串所需的最少操作次数。

每次可以进行以下一种操作：

- 添加一个字符；
- 删除一个字符；
- 替换一个字符。

请计算给定两个字符串之间的编辑距离。

## 输入

第一行包含一个长度为 `n` 的字符串。

第二行包含一个长度为 `m` 的字符串。

两个字符串都只包含大写英文字母。

## 输出

输出两个字符串之间的编辑距离。

## 样例输入

```text
LOVE
MOVIE
```

## 样例输出

```text
2
```

## 数据范围

- 1 ≤ n, m ≤ 5000

## 题解

定义 `dp[i][j]` 表示将 `s1` 的前 `i` 个字符转换成 `s2` 的前 `j` 个字符所需的最少操作次数。

当其中一个字符串为空时，只能通过添加或删除完成转换：

```C++
dp[i][0]=i;
dp[0][j]=j;
```

如果当前两个字符相同，则不需要额外操作：

$$
dp[i][j]=dp[i-1][j-1]
$$

如果当前字符不同，可以选择：

- 替换字符：`dp[i-1][j-1]+1`
- 删除字符：`dp[i-1][j]+1`
- 添加字符：`dp[i][j-1]+1`

因此：

$$
dp[i][j]=\min(dp[i-1][j-1],dp[i-1][j],dp[i][j-1])+1
$$

## 复杂度分析

时间复杂度为 `O(nm)`，空间复杂度为 `O(nm)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    string s1,s2;
    cin>>s1>>s2;

    int n=s1.size();
    int m=s2.size();

    vector<vector<int>> dp(n+1,vector<int>(m+1,0));

    for(int i=0;i<=n;i++){
        dp[i][0]=i;
    }

    for(int j=0;j<=m;j++){
        dp[0][j]=j;
    }

    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(s1[i-1]==s2[j-1]){
                dp[i][j]=dp[i-1][j-1];
            }else{
                dp[i][j]=min({
                    dp[i-1][j-1],
                    dp[i-1][j],
                    dp[i][j-1]
                })+1;
            }
        }
    }

    cout<<dp[n][m];
}
```
