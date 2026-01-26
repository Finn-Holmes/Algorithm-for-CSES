## 题目描述
Given a string, your task is to calculate the number of different strings that can be created using its characters.
## 输入
The only input line has a string of length n. Each character is between a–z.

Constraints

1 ≤ n ≤ 10^6
## 输出
Print the number of different strings modulo 109+7.
## 样例输入
```
aabac
```
## 样例输出
```
20
```
## 题解
很简单的组合问题，主要考察的就是阶乘预处理和用除法时取模需要用逆元的问题
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int N=1e6+5;
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
    pre[0]=1;//注意
    for(int i=1;i<N;i++){
        pre[i]=i*pre[i-1]%MOD;
        inv[i]=inverse(pre[i]);
    } 
}
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    init();
    string s;
    cin>>s;
    vector<int> word(26,0);
    for(int i=0;i<s.size();i++){
        word[s[i]-'a']++;
    }
    int len=s.size();
    long long res=pre[len];
    for(int i=0;i<26;i++){
        if(word[i]){
            res=res*inv[word[i]]%MOD;
        }
    }
    cout<<res;
    return 0;
}
```
