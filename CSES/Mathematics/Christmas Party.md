## 题目描述
There are n children at a Christmas party, and each of them has brought a gift. The idea is that everybody will get a gift brought by someone else.
In how many ways can the gifts be distributed?
## 输入
The only input line has an integer n: the number of children.

Constraints

1 ≤ n ≤ 10^6
## 输出
Print the number of ways modulo 10^9+7.
## 样例输入
```
4
```
## 样例输出
```
9
```
## 题解
错位排列
以 n 个人编号为 1∼n，每人带了自己的礼物，我们要统计所有没有人拿到自己带来的礼物的分法。

随机选第1件礼物（原本属于1号），我们要让它去给其他某个 k 号（k ≠ 1）：

假设分配给了第 k 号（k≠1）小朋友

那么现在有两种情况：

第 k 号小朋友的礼物给了 1 号：这意味着1←k，k←1，这两个人直接互换，两个人的问题解决，还剩下 n−2 个人要错位，这对应于 D(n-2)。

第 k 号小朋友的礼物给了别人（不是 1 号）：此时我们就在 n−1 个人里继续递归地做全错位，这对应于 D(n-1)。

1号有 n−1 个选择（除了自己别人都可以）。

D(n)=(n-1)*[D(n-1)+D(n-2)].
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD=1e9+7;
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    if(n==0){
        cout<<1;
        return 0;
    }
    if(n==1){
        cout<<0;
        return 0;
    }
    long long a0=1;
    long long a1=0;
    for(int i=2;i<=n;i++){
        long long num=((long long)(i-1)*((a0+a1)%MOD))%MOD;
        a0=a1;
        a1=num;
    }
    cout<<a1%MOD;
}
```
