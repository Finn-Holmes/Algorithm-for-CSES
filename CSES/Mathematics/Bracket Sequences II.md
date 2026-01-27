## 题目描述
Your task is to calculate the number of valid bracket sequences of length n when a prefix of the sequence is given.
## 输入
The first input line has an integer n.
The second line has a string of k characters: the prefix of the sequence.

Constraints

1 ≤ k ≤ n ≤ 10^6
## 输出
Print the number of sequences modulo 109+7.
## 样例输入
```
6
(()
```
## 样例输出
```
2
```
## 提示
There are two possible sequences: (())() and (()()).
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int N=2e6+5;
long long pre[N];
const int MOD=1e9+7;
long long inv[N];
long long inverse(long long num){
    long long res=1;
    int mod=MOD-2;
    while(mod){
        if(mod&1) res=res*num%MOD;
        num=num*num%MOD;
        mod>>=1;
    }
    return res;
}
void init(){
    pre[0]=1;
    inv[0]=1;
    for(int i=1;i<N;i++){
        pre[i]=pre[i-1]*i%MOD;
        inv[i]=(inv[i-1]*inverse(i))%MOD;
    }
}
long long C(int n,int k){
    if(k<0||k>n) return 0;
    return pre[n]*inv[k]%MOD*inv[n-k]%MOD;
}
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    init();
    int n;
    cin>>n;
    string s;
    cin>>s;
    if(n&1){
        cout<<0;
        return 0;
    }
    int k=s.size();
    int left=0,right=0,diff=0;
    for(char c:s){
        if(c=='(') left++;
        else right++;
        if(right>left){
            cout<<0;
            return 0;
        }
    }
    diff=left-right;
    int add_left=n/2-left;
    int add_right=n/2-right;
    int remain=n-k;
    if(add_left<0||add_right<0){
        cout<<0;
        return 0;
    }
    long long ans=(C(remain,add_left)-C(remain,add_left-1)+MOD)%MOD;
    cout<<ans;
    return 0;
}
```
