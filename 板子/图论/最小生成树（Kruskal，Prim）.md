# 最小生成树（Kruskal 与 Prim）

## 模板用途

在连通无向带权图中选出 `n-1` 条边，使所有节点连通且边权和最小。

> 本文件包含多个相近算法或同一算法的不同实现，保留在同一篇笔记中便于对照学习。

## 核心思路

- Kruskal 将所有边按权值排序，使用并查集依次加入不会形成环的最小边，适合稀疏图。
- Prim 从任意节点出发，不断选择连接当前生成树和树外节点的最小边，适合稠密图。
- 最终选边数不足 `n-1` 时，原图不连通。

## 复杂度分析

Kruskal 时间复杂度为 `O(mlog m)`；朴素 Prim 为 `O(n^2+m)`；空间复杂度为 `O(n+m)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>//Kruskal�㷨��������ϡ��ͼ������mС��n^2������С��������������ͨͼ�������С��Ȩ֮�ͣ� 
using namespace std;//��Ҫ���ò��鼯���������б�Ȩ�������򣬴�������ѡ���õ���СȨ�����ӣ���merge 
struct edge{
	int u,v,w;
};
bool cmp(edge&p,edge&q){
	return p.w<q.w;
}
vector<edge> e;
vector<int> fa;
int cc;
void unionfind(int n){
	fa.resize(n+1);//������1��ʼ 
	iota(fa.begin(),fa.end(),0);
	cc=n;
}
int find(int x){
	if(fa[x]==x) return x;
	return fa[x]=find(fa[x]);
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n,m;
	cin>>n>>m;
	for(int i=1;i<=m;i++){
		cin>>e[i].u>>e[i].v>>e[i].w;
	}
	sort(e.begin()+1,e.end(),cmp);
	long long ans=0;
	int cnt=0;
	for(int i=1;i<=m;i++){
		int x=find(e[i].u),y=find(e[i].v);//merge������������ 
		if(x==y) continue;
		fa[x]=y;
		cc--;
		cnt++;
	}
	cnt==n-1//˵����m���߿��Խ�n���ڵ��ͼ��ͨ 
}
//Prim�㷨�������ڳ���ͼ���е���dijkstra�����˴���d[u]�������Ǿ���u(δ����ͨͼ�ڵĵ�)������ͨͼ�е���������
int d[100010];
struct edge{
	int v,w;
}; 
vector<bool> vis;
vector<edge> e;
void Prim(int s){//��ѡһ������Ϊ��� 
	for(int i=0;i<=n;i++){
		d[i]=INT_MAX;
	}
	int cnt=1;
	d[s]=0;
	for(int i=1;i<=n;i++){
		int u=0;
		for(int j=1;j<=n;j++){
			if(vis[j]==false&&d[j]<d[u]){
				u=j;
			}
		}
		vis[u]=true;
		ans+=d[u];
		if(d[u]!=INT_MAX) cnt++;
		for(auto it:e[u]){
			int v=it.v,w=it.w;
			if(d[v]>w&&!vis[v]){
				d[v]=w;
			}
		}
	}
	cnt==n//��˵��������ͨ 
}
```
