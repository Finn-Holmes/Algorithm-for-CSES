## 题目描述
Your task is to calculate the number of ways to get a sum n by throwing dice. Each throw yields an integer between 1...6.
For example, if n=10, some possible ways are 3+3+4, 1+4+1+4 and 1+1+6+1+1.
## 输入
The only input line contains an integer n.

Constraints

1 ≤ n ≤ 10^18

## 输出
Print the number of ways modulo 109+7.
## 样例输入
```
8
```
## 样例输出
```
125
```
## 题解
f(n)=f(n-1)+f(n-2)+f(n-3)+f(n-4)+f(n-5)+f(n-6)，满足线性递推关系，可以用矩阵快速幂解决.想要更详细的解释可以看同系列问题Fibonacci Numbers的相应题解.
## code
```C++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int MOD = 1e9+7;
void cheng(ll a[6][6], ll b[6][6], ll res[6][6]) {
    ll t[6][6] = {};
    for(int i=0;i<6;++i)
        for(int j=0;j<6;++j)
            for(int k=0;k<6;++k)
                t[i][j]=(t[i][j]+a[i][k]*b[k][j]%MOD)%MOD;
    for(int i=0;i<6;++i)
        for(int j=0;j<6;++j)
            res[i][j]=t[i][j];
}
void matrix_mi(ll mat[6][6], ll n, ll res[6][6]) {
    for(int i=0;i<6;++i) res[i][i]=1;
    while(n){
        if(n&1) cheng(res,mat,res);
        cheng(mat,mat,mat);
        n>>=1;
    }
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	ll n; 
	cin>>n;
    if(n==0){
		cout<<1<<endl; 
		return 0;
	}
    ll mat[6][6];
    memset(mat,0,sizeof(mat)); 
    for(int i=0;i<6;++i) mat[0][i]=1;
    for(int i=1;i<6;++i) mat[i][i-1]=1;
    ll f[7]={1,1,2,4,8,16,32};
    if(n<7){
        cout<<f[n]<<endl;
        return 0;
    }
    ll res[6][6];
    memset(res,0,sizeof(res)); 
    matrix_mi(mat,n-5,res);
    ll ans=0;
    for(int i=0;i<6;++i){
        ans=(ans+res[0][i]*f[5-i]%MOD)%MOD;
    }
    cout<<ans;
	return 0; 
}
```
