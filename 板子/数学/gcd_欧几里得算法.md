## 题目描述

给定两个整数 `a` 和 `b`，求它们的最大公约数。

最大公约数是能够同时整除 `a` 和 `b` 的最大正整数，通常记作：

$$
\gcd(a,b)
$$

例如：

$$
\gcd(12,18)=6
$$

## 题解

### 欧几里得算法

欧几里得算法也叫辗转相除法，核心公式为：

$$
\gcd(a,b)=\gcd(b,a\bmod b)
$$

不断使用这个公式，直到第二个数变成 0。此时第一个数就是最大公约数：

$$
\gcd(a,0)=a
$$

例如计算：

$$
\gcd(48,18)
$$

过程如下：

```text
gcd(48,18)
= gcd(18,12)
= gcd(12,6)
= gcd(6,0)
= 6
```

### 为什么可以这样转移

设：

$$
a=kb+r
$$

其中：

$$
r=a\bmod b
$$

如果一个数 `d` 同时是 `a` 和 `b` 的因数，那么：

$$
d\mid a,\qquad d\mid b
$$

所以 `d` 也一定可以整除：

$$
a-kb=r
$$

反过来，如果 `d` 同时整除 `b` 和 `r`，那么它也可以整除：

$$
kb+r=a
$$

因此，`a,b` 和 `b,a mod b` 的公因数集合完全相同，从而有：

$$
\gcd(a,b)=\gcd(b,a\bmod b)
$$

## 复杂度分析

时间复杂度：`O(log(min(a,b)))`。

空间复杂度：

- 递归写法：`O(log(min(a,b)))`；
- 循环写法：`O(1)`。

## code

### 循环写法

```C++
#include <bits/stdc++.h>
using namespace std;

long long get_gcd(long long a,long long b){
    a=llabs(a);
    b=llabs(b);

    while(b){
        long long r=a%b;
        a=b;
        b=r;
    }

    return a;
}
```

### 递归写法

```C++
#include <bits/stdc++.h>
using namespace std;

long long get_gcd(long long a,long long b){
    if(b==0){
        return a;
    }

    return get_gcd(b,a%b);
}
```

如果输入可能为负数，可以先取绝对值：

```C++
long long get_gcd(long long a,long long b){
    a=llabs(a);
    b=llabs(b);

    if(b==0){
        return a;
    }

    return get_gcd(b,a%b);
}
```

### 最小公倍数

根据最大公约数，还可以计算最小公倍数：

$$
\mathrm{lcm}(a,b)=\frac{a}{\gcd(a,b)}\times b
$$

代码如下：

```C++
long long get_lcm(long long a,long long b){
    if(a==0||b==0){
        return 0;
    }

    return llabs(a/get_gcd(a,b)*b);
}
```

先计算 `a/get_gcd(a,b)` 再乘 `b`，可以在一定程度上降低乘法溢出的风险。
