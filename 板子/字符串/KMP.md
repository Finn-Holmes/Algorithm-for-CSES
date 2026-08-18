## 题目描述

给定文本串 `text` 和模式串 `pattern`，找出 `pattern` 在 `text` 中所有成功匹配的起始下标。

字符串允许出现重叠匹配。

## 题解

### next 数组

定义 `next[i]` 表示：

> `pattern[0..i]` 的最长相等真前缀和真后缀的长度。

例如：

```text
pattern = "abab"
next    = [0, 0, 1, 2]
```

处理位置 `i` 时，使用 `cnt` 表示当前已经匹配的前后缀长度。

如果 `pattern[cnt]` 与 `pattern[i]` 不相等，就利用之前计算出的 `next` 数组回退：

```cpp
cnt=next[cnt-1];
```

如果相等，则当前前后缀可以继续扩展：

```cpp
cnt++;
```

### 匹配过程

枚举文本串中的每个字符，`cnt` 表示当前已经匹配的模式串长度。

当前需要比较：

```cpp
text[i] == pattern[cnt]
```

如果不相等，同样利用 `next` 数组回退 `cnt`：

```cpp
while(cnt&&pattern[cnt]!=text[i]){
    cnt=next[cnt-1];
}
```

文本串下标 `i` 不需要回退。

当 `cnt==m` 时，说明模式串已经完整匹配，起始下标为：

$$
i-m+1
$$

记录答案后令：

```cpp
cnt=next[cnt-1];
```

从而继续寻找后续匹配，同时支持重叠匹配。

## 复杂度分析

设文本串长度为 `n`，模式串长度为 `m`。

- 时间复杂度：`O(n + m)`
- 空间复杂度：`O(m)`

## code

```C++
#include <bits/stdc++.h>
using namespace std;

vector<int> kmp_search(const string& text,const string& pattern){
    int n=text.size();
    int m=pattern.size();

    if(m==0){
        return {};
    }

    vector<int> next(m);
    int cnt=0;

    for(int i=1;i<m;i++){
        while(cnt&&pattern[cnt]!=pattern[i]){
            cnt=next[cnt-1];
        }

        if(pattern[cnt]==pattern[i]){
            cnt++;
        }

        next[i]=cnt;
    }

    vector<int> pos;
    cnt=0;

    for(int i=0;i<n;i++){
        while(cnt&&pattern[cnt]!=text[i]){
            cnt=next[cnt-1];
        }

        if(pattern[cnt]==text[i]){
            cnt++;
        }

        if(cnt==m){
            pos.push_back(i-m+1);
            cnt=next[cnt-1];
        }
    }

    return pos;
}
```
