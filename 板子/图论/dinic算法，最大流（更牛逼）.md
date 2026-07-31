# Dinic 最大流

## 模板用途

求有向容量网络从源点到汇点的最大流。

> 本文件包含多个相近算法或同一算法的不同实现，保留在同一篇笔记中便于对照学习。

## 核心思路

- 每条正向边都要建立一条初始容量为 `0` 的反向边。
- BFS 构造只沿剩余容量为正的边形成的分层图。
- DFS 只向下一层发送流量，并使用当前弧、余量和残枝优化。

## 复杂度分析

一般网络中的常用复杂度上界为 `O(V^2E)`，实际运行通常更快；空间复杂度为 `O(V+E)`。

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

	cout<<dinic();
}
```
