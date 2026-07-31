# Dinic 求二分图最大匹配

## 模板用途

将二分图最大匹配转化为最大流问题。

> 本文件包含多个相近算法或同一算法的不同实现，保留在同一篇笔记中便于对照学习。

## 核心思路

- 源点向左部每个节点连容量为 `1` 的边。
- 左部向右部按原二分图连容量为 `1` 的边。
- 右部每个节点向汇点连容量为 `1` 的边，最大流量就是最大匹配数。
- Dinic 使用 BFS 构造分层图，再用 DFS 发送阻塞流。

## 复杂度分析

在单位容量二分图网络中通常可达到 `O(m√n)` 量级；模板的一般 Dinic 上界取决于网络结构，空间复杂度为 `O(n+m)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>
using namespace std;
const int M=120000,N=2000;
int S,T;
struct edge{
	long long v,c,ne;//v��һ���ڵ�,c�ñ�����,ne��һ���� 
}e[M];//������ע�������������ߣ�����Ҫ������������ 
int d[N],h[N],cur[N];//d�Ǹõ���ȣ�h[u]�Ǹõ��һ���ߣ�cur[u]��u�㵱ǰ�� 
int idx=1;//�ߵ����� 
void add(int a,int b,int c){
	idx++;
	e[idx]={b,c,h[a]};
	h[a]=idx;
}
bool bfs(){//�ж�ÿ�������ȣ������dfs 
	queue<int> q;
	q.push(S);
	memset(d,0,sizeof d);
	d[S]=1;
	while(!q.empty()){
		int u=q.front();
		q.pop();
		for(int i=h[u];i;i=e[i].ne){
			int v=e[i].v;
			if(d[v]==0&&e[i].c>0){
				d[v]=d[u]+1;
				q.push(v);
				if(v==T) return true;
			}
		}
	}
	return false;
}
long long dfs(int u,long long mf){
	if(u==T)return mf;
	long long sum=0;//��ǰ�ڵ������ߵ������ 
	for(int i=cur[u];i;i=e[i].ne){
		cur[u]=i;//���Ż���u֮ǰ���ӱ߶��Ѿ������ˣ���һ��bfs��dfsʱ�Ͳ��������� 
		int v=e[i].v; 
		if(d[v]==d[u]+1&&e[i].c>0){
			long long f=dfs(v,min(e[i].c,mf));//�ӽڵ������ߵ������ 
			e[i].c-=f;
			e[i^1].c+=f;
			sum+=f;
			mf-=f;//ʣ��������ٸ������ӽڵ�� 
			if(mf==0) break;//�����Ż� 
		}
	}
	if(sum==0) d[u]=0;//��֦�Ż� 
	return sum;
}
long long dinic(){
	long long flow=0;
	while(bfs()==true){
		memcpy(cur,h,sizeof h);
		flow+=dfs(S,1e9);
	}
	return flow;
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int a,b;
	int n,m,k;
	cin>>n>>m>>k;
	while(k--){
		cin>>a>>b;
		add(a,b+n,1);
		add(b+n,a,0);
	}
	S=0,T=n+m+1;
	for(int i=1;i<=n;i++){
		add(S,i,1);
		add(i,S,0);
	}
	for(int i=1;i<=m;i++){
		add(i+n,T,1);
		add(T,i+n,0); 
	}
	cout<<dinic();
}
```
