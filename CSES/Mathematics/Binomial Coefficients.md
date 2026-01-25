## 题目描述
Your task is to calculate n binomial coefficients modulo 109+7.
A binomial coefficient  can be calculated using the formula a!/b!(a-b)! . We assume that a and b are integers and 0 ≤ b ≤ a.
## 输入
The first input line contains an integer n: the number of calculations.
After this, there are n lines, each of which contains two integers a and b.

Constraints

1 ≤ n ≤ 10^5

0 ≤ b ≤ a ≤ 10^6
## 输出
Print each binomial coefficient modulo 109+7.
## 样例输入
```
3
5 3
8 1
9 5
```
## 样例输出
```
10
8
126
```
## 题解
预处理阶乘，计算的时候因为是除法，所以要乘以逆元pow(num,MOD-2)%MOD;
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD=1e9+7;
const int N=1e6+5;
long long pre[N],inv[N];//一个存阶乘，一个存逆元

long long mi(long long x){
	long long res=1;
	long long mod=MOD-2;
	while(mod>0){
		if(mod&1){
			res=(res*x)%MOD;
		}
		x=(x*x)%MOD;
		mod>>=1;
	}
	return res;
}
void init(){
	pre[0]=inv[0]=1;
	for(int i=1;i<=N;i++){
		pre[i]=i*pre[i-1]%MOD;
		inv[i]=inverse(pre[i]);
	}
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	init();
	int n,a,b;
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>a>>b;
		cout<<pre[a]*inv[b]%MOD*inv[a-b]%MOD<<'\n';
	}
}

```
