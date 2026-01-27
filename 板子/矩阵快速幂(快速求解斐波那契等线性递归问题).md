## 题目描述
The Fibonacci numbers can be defined as follows:
F0=0
F1=1
Fn = Fn-2+Fn-1

Your task is to calculate the value of Fn for a given n.
## 输入
The only input line has an integer n.

Constraints

0 ≤ n ≤ 10^18

## 输出
Print the value of Fn modulo 10^9+7.
## 样例输入
```
10
```
## 样例输出
```
55
```
## 题解
矩阵快速幂，可用于快速求解线性递推问题

由于煮啵不会写矩阵，以下矩阵以()代替，/为矩阵换行

设有一矩阵A，A*(f(0)/f(1))=(f(1)/f(2)),A*(f(1)/f(2))=(f(2)/f(3))...

我们可求解该矩阵A=(0 1/1 1).则有(f(n)/f(n+1))=A^n  *  (f(0)/f(1))..

接下来就转化成快速求解矩阵n次幂的问题，可以采用之前快速幂的方法，不过初始res不为1，而是E=(1 0/0 1).

其中快速幂的操作除矩阵乘法规则外与常规快速幂均一致.
## code
```C++
#include <bits/stdc++.h>
using namespace std;
long long xi[2][2];
const int MOD=1e9+7;
long long res[2][2]={{1,0},{0,1}};
void matrix_mi(long long n){
	xi[0][1]=1;
	xi[1][0]=1;
	xi[1][1]=1; 
	while(n){
		if(n&1){
			long long tmp[2][2];
		    tmp[0][0]=(xi[0][0]*res[0][0]%MOD+xi[0][1]*res[1][0]%MOD)%MOD;
		    tmp[0][1]=(xi[0][0]*res[0][1]%MOD+xi[0][1]*res[1][1]%MOD)%MOD;
		    tmp[1][0]=(xi[1][0]*res[0][0]%MOD+xi[1][1]*res[1][0]%MOD)%MOD;
		    tmp[1][1]=(xi[1][0]*res[0][1]%MOD+xi[1][1]*res[1][1]%MOD)%MOD;
		    res[0][0]=tmp[0][0];
		    res[0][1]=tmp[0][1];
		    res[1][0]=tmp[1][0];
		    res[1][1]=tmp[1][1];
		} 
		n>>=1;
		long long tmp[2][2];
	    tmp[0][0]=(xi[0][0]*xi[0][0]%MOD+xi[0][1]*xi[1][0]%MOD)%MOD;
	    tmp[0][1]=(xi[0][0]*xi[0][1]%MOD+xi[0][1]*xi[1][1]%MOD)%MOD;
	    tmp[1][0]=(xi[1][0]*xi[0][0]%MOD+xi[1][1]*xi[1][0]%MOD)%MOD;
	    tmp[1][1]=(xi[1][0]*xi[0][1]%MOD+xi[1][1]*xi[1][1]%MOD)%MOD;
	    xi[0][0]=tmp[0][0];
	    xi[0][1]=tmp[0][1];
	    xi[1][0]=tmp[1][0];
	    xi[1][1]=tmp[1][1];
	}
	cout<<res[0][1];
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	long long n;
	cin>>n;
	matrix_mi(n);
}

```
