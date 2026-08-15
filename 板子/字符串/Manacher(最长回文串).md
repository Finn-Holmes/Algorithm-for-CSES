## 题目描述

给定一个字符串 `s`，返回 `s` 中最长的回文子串。

回文串是正着读和反着读完全相同的字符串。

## 题解

直接枚举每个位置作为回文中心并向两边扩展，最坏时间复杂度为 `O(n^2)`。

Manacher 算法利用已经计算过的回文信息，将时间复杂度优化到 `O(n)`。

### 预处理字符串

原字符串的回文长度可能是奇数，也可能是偶数。为了统一处理，在每两个字符之间插入 `#`，并在两端加入哨兵：

```text
s = abba
t = ^#a#b#b#a#$
```

处理后，所有回文串的长度都变成奇数，因此每个回文串都有一个明确的中心。

其中：

- `^` 和 `$` 是哨兵，避免扩展时越界；
- `#` 用来统一奇数长度和偶数长度回文串。

### 状态定义

定义：

```cpp
len[i]
```

表示以 `t[i]` 为中心的回文半径，不包括中心本身。

例如：

```text
t = ^#a#b#a#$
```

以字符 `b` 为中心时：

```text
#a#b#a#
```

可以向左右扩展 3 个字符，因此：

```cpp
len[i]=3
```

同时维护：

```cpp
mid
```

表示当前右边界最靠右的回文串中心。

```cpp
r
```

表示这个回文串能够到达的最右位置。

### 利用对称位置

对于当前位置 `i`，它关于 `mid` 的对称位置为：

```cpp
2*mid-i
```

如果 `i<r`，说明 `i` 位于当前已知回文串内部，可以利用对称位置的信息初始化：

```cpp
len[i]=min(len[2*mid-i],r-i);
```

不能直接全部复制镜像位置的半径，因为当前回文串的已知范围最多只能到达 `r`。

然后继续暴力向两侧扩展：

```cpp
while(t[i-len[i]-1]==t[i+len[i]+1]){
    len[i]++;
}
```

如果当前回文串超过了原来的右边界，就更新：

```cpp
mid=i;
r=i+len[i];
```

### 还原原字符串答案

假设最长回文中心是 `index`，半径是 `len[index]`。

对应到原字符串中的起点为：

```cpp
(index-len[index])/2
```

原字符串中的回文长度正好为：

```cpp
len[index]
```

所以答案为：

```cpp
s.substr((index-len[index])/2,len[index]);
```

## 复杂度分析

虽然代码中存在向两侧扩展的 `while`，但右边界 `r` 在整个算法中只会不断向右移动，因此总时间复杂度为 `O(n)`。

空间复杂度为 `O(n)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    string longestPalindrome(string s) {
        string t="^";

        for(char c:s){
            t+='#';
            t+=c;
        }

        t+="#$";

        vector<int> len(t.size());

        int mid=0;
        int r=0;
        int index=0;

        for(int i=1;i+1<(int)t.size();i++){
            if(i<r){
                len[i]=min(len[2*mid-i],r-i);
            }

            while(t[i-len[i]-1]==t[i+len[i]+1]){
                len[i]++;
            }

            if(i+len[i]>r){
                mid=i;
                r=i+len[i];
            }

            if(len[i]>len[index]){
                index=i;
            }
        }

        return s.substr(
            (index-len[index])/2,
            len[index]
        );
    }
};
```
