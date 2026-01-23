## 题目描述
You are given a rooted tree consisting of n nodes. The nodes are numbered 1,2,...,n, and node 1 is the root. Each node has a value.
Your task is to process following types of queries:
1.change the value of node s to x
2.calculate the sum of values on the path from the root to node s
## 输入
The first input line contains two integers n and q: the number of nodes and queries. The nodes are numbered 1,2,\ldots,n.
The next line has n integers v1,v2,...,vn: the value of each node.
Then there are n-1 lines describing the edges. Each line contains two integers a and b: there is an edge between nodes a and b.
Finally, there are q lines describing the queries. Each query is either of the form "1 s x" or "2 s".

Constraints

1 ≤ n, q ≤ 2*10^5

1 ≤ a,b, s ≤ n

1 ≤ vi, x ≤ 10^9
## 输出
Print the answer to each query of type 2.
## 样例输入
```
5 3
4 2 5 2 1
1 2
1 3
3 4
3 5
2 4
1 3 2
2 4
```
## 样例输出
```
11
8
```
## 题解
懒标记线段树，主要流程就是扁平化树化为数组，扁平化处理时顺便将父节点的值加到子结点上
## code
```C++
#include <bits/stdc++.h>
using namespace std;
struct Tree{
	int n;
	vector<long long> lazy,base;
	void init(int nn){
		n=nn;
		lazy.assign(4*n+5,0);
		base.assign(n+1,0);
	}
	void range_add(int p,int l,int r,int ql,int qr,long long val){
		if(ql>r||qr<l) return;
		if(ql<=l&&qr>=r){
			lazy[p]+=val;
			return;
		}
		int m=(l+r)/2;
		range_add(2*p,l,m,ql,qr,val);
		range_add(2*p+1,m+1,r,ql,qr,val);
	}
	long long query(int p,int l,int r,int index,long long sum_lazy=0){
		sum_lazy+=lazy[p];
		if(l==r){
			return base[l]+sum_lazy;
		}
		int m=(l+r)/2;
		if(index<=m) return query(2*p,l,m,index,sum_lazy);
		else return query(2*p+1,m+1,r,index,sum_lazy);
	}
};
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n,q;
	cin>>n>>q;
	vector<long long> vals(n+1,0);
	for(int i=1;i<=n;i++){
		cin>>vals[i];
	} 
	vector<vector<int>> g(n+1,vector<int>());
	for(int i=1;i<n;i++){
		int a,b;
		cin>>a>>b;
		g[a].push_back(b);
		g[b].push_back(a); 
	}
	vector<int> st;
	vector<int> time(n+1,0);
	vector<int> tuopu;
	vector<long long> num(n+1);
	vector<int> fa(n+1,0);
	vector<int> sz(n+1,1);
	st.push_back(1);
	vector<long long> path=vals;
	while(!st.empty()){//扁平化处理
		int u=st.back();
		st.pop_back();
		tuopu.push_back(u);
		for(int v:g[u]){
			if(v==fa[u]) continue;
			fa[v]=u;
			st.push_back(v);
			path[v]+=path[u];//把父节点u的值加到v上，预处理
		}
	}
	for(int i=0;i<tuopu.size();i++){
		time[tuopu[i]]=i+1;
	}
	for(int i=tuopu.size()-1;i>=0;i--){
		int u=tuopu[i];
		if(fa[u]!=0) sz[fa[u]]+=sz[u];
	}
	
	Tree tree;
	tree.init(n);
	for(int u=1;u<=n;u++){
		tree.base[time[u]]=path[u];
	}
	vector<long long> cur=vals;
	while(q--){
		int type;
		cin>>type;
		if(type==1){
			int s;
			long long x;
			cin>>s>>x;
			long long d=x-cur[s];
			cur[s]=x;
			int ql=time[s],qr=time[s]+sz[s]-1;
			if(d!=0) tree.range_add(1,1,n,ql,qr,d);//懒标记记录delta值
		}else{
			int s;
			cin>>s;
			cout<<tree.query(1,1,n,time[s])<<'\n';
		}
	}
}
```
