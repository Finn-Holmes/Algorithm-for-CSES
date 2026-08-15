## 题目描述

初始时，`n` 个人手中都有 `1` 块蛋糕。接下来进行 `m` 次操作：

- `C l r x`：将区间 `[l,r]` 中所有人的蛋糕数修改为 `x`。
- `P l r`：查询区间 `[l,r]` 中有多少种不同的蛋糕数。

## 输入

第一行输入三个整数 `n,m,k`，分别表示人数、操作数量和蛋糕数的最大值。

接下来 `m` 行，每行表示一次操作。

## 输出

对于每次查询操作，输出一行一个整数，表示区间中不同蛋糕数的种类数。

## 样例输入

```text
2 4 2
C 1 1 2
P 1 2
C 2 2 2
P 1 2
```

## 样例输出

```text
2
1
```

## 数据范围

对于 100% 的数据：

`1 <= l <= r <= n <= 100000`，`m <= 100000`，`k <= 30`。

## 题解

由于蛋糕数最多只有 `30` 种，可以使用一个二进制整数表示一个区间内出现过的蛋糕数。

蛋糕数 `x` 对应的状态为：

```cpp
1<<(x-1)
```

例如蛋糕数 `1、3` 同时出现时，区间状态为：

```text
001 | 100 = 101
```

合并左右区间时，对两个状态按位或：

```cpp
tree[p]=tree[p*2]|tree[p*2+1];
```

查询得到区间状态 `mask` 后，其中二进制位 `1` 的数量就是不同蛋糕数的种类数：

```cpp
__builtin_popcount(mask)
```

修改操作是将整个区间赋成相同的值，因此使用带懒标记的线段树。`lazy_tag[p]` 表示节点 `p` 对应区间还未下传的赋值状态，`0` 表示没有懒标记。

## 复杂度分析

每次区间修改和区间查询的时间复杂度均为 `O(log n)`。

总时间复杂度为 `O(m log n)`，空间复杂度为 `O(n)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

vector<int> tree;
vector<int> lazy_tag;

void build(int p,int l,int r){
    tree[p]=1;
    if(l==r) return;

    int mid=(l+r)/2;
    build(p*2,l,mid);
    build(p*2+1,mid+1,r);
}

void push_down(int p){
    if(lazy_tag[p]==0) return;

    tree[p*2]=lazy_tag[p];
    tree[p*2+1]=lazy_tag[p];

    lazy_tag[p*2]=lazy_tag[p];
    lazy_tag[p*2+1]=lazy_tag[p];

    lazy_tag[p]=0;
}

void update(int p,int l,int r,int ql,int qr,int value){
    if(ql<=l&&r<=qr){
        tree[p]=value;
        lazy_tag[p]=value;
        return;
    }

    push_down(p);

    int mid=(l+r)/2;

    if(ql<=mid){
        update(p*2,l,mid,ql,qr,value);
    }

    if(qr>mid){
        update(p*2+1,mid+1,r,ql,qr,value);
    }

    tree[p]=tree[p*2]|tree[p*2+1];
}

int query(int p,int l,int r,int ql,int qr){
    if(ql<=l&&r<=qr){
        return tree[p];
    }

    push_down(p);

    int mid=(l+r)/2;
    int res=0;

    if(ql<=mid){
        res|=query(p*2,l,mid,ql,qr);
    }

    if(qr>mid){
        res|=query(p*2+1,mid+1,r,ql,qr);
    }

    return res;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n,m,k;
    cin>>n>>m>>k;

    tree.assign(4*n+5,0);
    lazy_tag.assign(4*n+5,0);

    build(1,1,n);

    while(m--){
        char op;
        int l,r;

        cin>>op>>l>>r;

        if(op=='C'){
            int x;
            cin>>x;

            int value=1<<(x-1);
            update(1,1,n,l,r,value);
        }else{
            int mask=query(1,1,n,l,r);
            int ans=__builtin_popcount((unsigned)mask);

            cout<<ans<<'\n';
        }
    }

    return 0;
}
```
