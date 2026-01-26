## 题目描述
There are n children and m apples that will be distributed to them. Your task is to count the number of ways this can be done.
For example, if n=3 and m=2, there are 6 ways: [0,0,2], [0,1,1], [0,2,0], [1,0,1], [1,1,0] and [2,0,0].
## 输入
The only input line has two integers n and m.

Constraints

\1 ≤ n,m ≤ 10^6
## 输出
Print the number of ways modulo 10^9+7.
## 样例输入
```
3 2
```
## 样例输出
```
6
```
## 题解
阶乘预处理，逆元.这个系列已经有过好几道这种题了
主要说一下组合数的思路，相当于往苹果中间插隔板，需要分为n份，则需要n-1个隔板，苹果有m个，所以相当于在(n+m-1)个空位挑n-1个位置放隔板
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int N=2e6+5;
const int MOD=1e9+7;
long long pre[N],inv[N];
long long inverse(long long num){
    int mod=MOD-2;
    long long res=1;
    while(mod){
        if(mod&1){
            res=res*num%MOD;
        }
        num=num*num%MOD;
        mod>>=1;
    }
    return res; 
}
void init(){
    pre[0]=1;
    inv[0]=1; 
    for(int i=1;i<N;i++){
        pre[i]=i*pre[i-1]%MOD;
        inv[i]=inv[i-1]*inverse(i)%MOD;
    } 
}
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    init();
    int a,b;
    cin>>a>>b;
    long long res=pre[a+b-1]; 
    res=res*inv[a-1]%MOD*inv[b]%MOD;
    cout<<res;
    return 0;
}
```
