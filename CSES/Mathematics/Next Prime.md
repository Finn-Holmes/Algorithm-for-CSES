## 题目描述
Given a positive integer n, find the next prime number after it.

## 输入
The first line has an integer t: the number of tests.
After that, each line has a positive integer n.
## 输出
For each test, print the next prime after n.

Constraints

1 ≤ t ≤ 20

1 ≤ n ≤ 1012

## 样例输入
```
5
1
2
3
42
1337
```
## 样例输出
```
2
3
5
43
1361
```
## 题解
首先用埃氏筛，预处理获得小于1e6的所有指数，存在数组p中，每有一个数num<=1e12，遍历质数判断是否整除，可以快速判断是否为素数,此时就可枚举了.
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int sqrtN=1e6+5;
bool prime[sqrtN];
vector<long long> p;
void init_prime(){
	memset(prime,true,sizeof(prime));
	for(int i=2;i*i<=sqrtN;i++){
		if(prime[i]){
			for(int j=2*i;j<sqrtN;j+=i){
				prime[j]=false;
			}
		}
	}
	for(int i=2;i<sqrtN;i++){
		if(prime[i]){
			p.push_back(i);
		}
	}
}
bool is_prime(long long num){
	for(int i=0;i<p.size()&&p[i]<num;i++){
		if(num%p[i]==0){
			return false;
		}
	}
	return true;
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	init_prime();
	int n;
	cin>>n; 
	for(int i=1;i<=n;i++){
		long long x;
		cin>>x;
		x++; 
		while(!is_prime(x)){
			x++;
		}
		cout<<x<<'\n';
	}
	return 0;
}

```
