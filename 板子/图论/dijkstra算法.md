# Dijkstra 最短路（朴素版与堆优化版）

## 模板用途

求非负权图中的单源最短路。

> 本文件包含多个相近算法或同一算法的不同实现，保留在同一篇笔记中便于对照学习。

## 核心思路

- 朴素版每轮选择一个尚未确定、距离源点最近的节点，再用它松弛所有出边。
- 堆优化版使用优先队列维护当前距离最小的节点，避免每轮扫描所有节点。
- Dijkstra 不能直接处理负权边。

## 复杂度分析

朴素版时间复杂度为 `O(n^2+m)`；堆优化版时间复杂度为 `O((n+m)log n)`；空间复杂度为 `O(n+m)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>
using namespace std;
const int N=1e5;
struct edge{
	int v,w;//v���ڵ㣬w��Ȩ�� 
};
vector<edge> e[N];//�õ�����г��� 
int d[N],vis[N];//d��Դ��s��i�ľ��룬vis[]�ж��Ƿ���� 
void dijkstra(int s){
	for(int i=0;i<=n;i++) d[i]=INT_MAX;
	d[s]=0;
	for(int i=1;i<n;i++){
		int u=0;
		for(int j=1;j<=n;j++){
			if(!vis[j]&&d[j]<d[u]){//֪��ĿǰΪֹδ�����Ҿ���s�����һ���� 
				u=j;
			}
		}
		vis[u]=1;
		for(auto& ed:e[u]){//���¸ó���������ڵ���� 
			int v=ed.v;
			int w=ed.w;
			if(d[v]>d[u]+w){ 
				d[v]=d[u]+w;
			}
		}
	}
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
}
//���ô�����Ż���dijkstra�㷨
struct edge{
	int v,w;
};
vector<edge> e[N];
int d[N],vis[N];
priority_queue<pair<int,int>> pq;
void dijkstra(int s){//���ô������Ҫ��Ϊ�˱���ÿ��ѭ����Ѱ��Ŀǰ������С���ڵ� 
	for(int i=0;i<=n;i++){
		d[i]=INT_MAX;
	}
	d[s]=0;
	q.push({0,s});
	while(!q.empty()){
		auto t=q.top();
		q.pop();
		int u=t.second;
		if(vis[u]) continue;//��ʱ����֮ǰ�Ѿ����ڵ�����˵�v�ľ��룬�ҵ�v�Ѿ����ߣ������ֱ������ 
		vis[u]=1;
		for(auto ed:e[u]){
			int v=ed.v,w=ed.w;
			if(d[v]>d[u]+w){
				d[v]=d[u]+w;
				q.push({-d[v],v});//����洢������top����Է���С����ĵ� 
			}
		}
	}
}
```
