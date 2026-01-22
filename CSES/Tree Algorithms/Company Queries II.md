## 题目描述
A company has n employees, who form a tree hierarchy where each employee has a boss, except for the general director.
Your task is to process q queries of the form: who is the lowest common boss of employees a and b in the hierarchy?
## 输入
The first input line has two integers n and q: the number of employees and queries. The employees are numbered 1,2,...,n, and employee 1 is the general director.
The next line has n-1 integers e2,e3,...,en: for each employee 2,3,...,n their boss.
Finally, there are q lines describing the queries. Each line has two integers a and b: who is the lowest common boss of employees a and b?

Constraints

1 ≤ n,q ≤ 2*10^5

1 ≤ ei ≤ i-1

1 ≤ a,b ≤ n
## 输出
Print the answer for each query.
## 样例输入
```
5 3
1 1 3 3
4 5
2 5
1 4
```
## 样例输出
```
3
1
1
```
## 题解
LCA+倍增

倍增预处理，然后开始处理查询，先将a,b调整到相同深度

比较关键的一个地方是
```C++
for(int i=num-1;i>=0;i--){
  if(d[a][i]!=d[b][i]){
    a=d[a][i];
    b=d[b][i];
  }
}
```
i要倒序，确保每一步都是走的必要，从大到小，该I能用就是一定会用，不能用就一定用不上，
加入说要正序走的话，如果要走4步，二进制为100，i为1时满足d[a][i]!=d[b][i],会取i=1.必然不行.
a,b在循环中走的前提时d[a][i]!=d[b][i](走后不相等)，所以最后要去的是d[a][0].一定可以取到(1.可以走的步数(2^num-1)大于等于深度,2.所有人都有一共同祖先(1号)).
## code 
```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n,q;
	cin>>n>>q;
	vector<int> fa(n+1,0);
	for(int i=2;i<=n;i++){
		cin>>fa[i];
	}
	int num=0;
	vector<int> depth(n+1,0);
	for(int i=2;i<=n;i++){
		depth[i]=depth[fa[i]]+1;
	}
	while((1<<num)<=n) num++;
	vector<vector<int>> d(n+1,vector<int>(num,0));
	for(int i=1;i<=n;i++){
		d[i][0]=fa[i];
	}
	for(int i=1;i<num;i++){
		for(int j=1;j<=n;j++){
			if(d[j][i-1]==0){
				d[j][i]=0;
			}else{
				d[j][i]=d[d[j][i-1]][i-1];
			}
		}
	}
	while(q--){
		int a,b;
		cin>>a>>b;
		if(depth[a]<depth[b]){
			swap(a,b);
		}
		int diff=depth[a]-depth[b];
		for(int i=0;i<num;i++){
			if(diff&(1<<i)){
				a=d[a][i];
			}
		}
		if(a==b){
			cout<<a<<'\n';
			continue;
		}
		
		cout<<d[a][0]<<'\n';
	}
	return 0;
}
```
