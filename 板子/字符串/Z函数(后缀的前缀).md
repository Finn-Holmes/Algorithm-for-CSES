## 题目描述

给定一个长度为 `n` 的字符串 `s`，求它的 Z 函数。

`z[i]` 表示字符串 `s` 与从位置 `i` 开始的后缀 `s[i...]` 的最长公共前缀长度。

例如：

```text
s = "abacaba"
z = [0,0,1,0,3,0,1]
```

其中 `z[4]=3`，因为：

```text
s[0...2] = "aba"
s[4...6] = "aba"
```

本模板规定 `z[0]=0`。

## 题解

### Z-box

如果 `z[i]=len`，那么：

$$
s[i\ldots i+len-1]=s[0\ldots len-1]
$$

称区间：

$$
[i,i+z[i]-1]
$$

为一个 Z-box。

计算过程中维护右端点最靠右的 Z-box `[l,r]`，因此有：

$$
s[l\ldots r]=s[0\ldots r-l]
$$

### 状态转移

依次计算 `z[i]`。

如果 `i>r`，说明当前位置不在已有的 Z-box 中，没有可以利用的信息，直接从 `z[i]=0` 开始比较。

如果 `i<=r`，令：

$$
k=i-l
$$

由于 `[l,r]` 与字符串前缀相同，位置 `i` 对应前缀中的位置 `k`，所以可以先得到：

$$
z[i]=\min(z[i-l],r-i+1)
$$

其中：

- `z[i-l]` 是前缀对应位置的已知答案；
- `r-i+1` 是当前位置到 Z-box 右端点的长度；
- 超过 `r` 的部分尚未确定，需要继续暴力扩展。

然后从当前的 `z[i]` 开始比较：

```cpp
s[z[i]] == s[i+z[i]]
```

直到字符不同或到达字符串末尾。

如果新的匹配段右端点超过了 `r`，就更新：

$$
l=i,\qquad r=i+z[i]-1
$$

### 为什么是线性复杂度

当 `i` 位于 `[l,r]` 中时，可以直接利用已经计算出的 Z 函数值。

只有匹配超过当前右端点 `r` 时，`while` 才会继续向右扩展。由于 `r` 在整个过程中只会向右移动，最多移动 `n` 次，因此总复杂度为线性。

## 复杂度分析

时间复杂度：`O(n)`。

空间复杂度：`O(n)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

vector<int> z_function(const string& s){
    int n=s.size();
    vector<int> z(n,0);

    int l=0,r=0;

    for(int i=1;i<n;i++){
        if(i<=r){
            z[i]=min(z[i-l],r-i+1);
        }

        while(i+z[i]<n&&s[z[i]]==s[i+z[i]]){
            z[i]++;
        }

        if(i+z[i]-1>r){
            l=i;
            r=i+z[i]-1;
        }
    }

    return z;
}
```

如果需要匹配模式串 `pattern` 在文本串 `text` 中的所有出现位置，可以构造：

```text
pattern + "#" + text
```

计算新字符串的 Z 函数。若某个位置满足：

```cpp
z[i]>=pattern.size()
```

说明从该位置开始出现了一次完整的 `pattern`。

```C++
#include <bits/stdc++.h>
using namespace std;

vector<int> z_function(const string& s){
    int n=s.size();
    vector<int> z(n,0);

    int l=0,r=0;

    for(int i=1;i<n;i++){
        if(i<=r){
            z[i]=min(z[i-l],r-i+1);
        }

        while(i+z[i]<n&&s[z[i]]==s[i+z[i]]){
            z[i]++;
        }

        if(i+z[i]-1>r){
            l=i;
            r=i+z[i]-1;
        }
    }

    return z;
}

vector<int> z_search(const string& text,const string& pattern){
    string s=pattern+"#"+text;
    vector<int> z=z_function(s);
    vector<int> pos;

    int m=pattern.size();

    for(int i=m+1;i<(int)s.size();i++){
        if(z[i]>=m){
            pos.push_back(i-m-1);
        }
    }

    return pos;
}
```
