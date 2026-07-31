# Bellman-Ford、SPFA 与负环判断

## 模板用途

处理含负权边的单源最短路，并判断从源点可达的负环以及负环是否会影响目标点。

> 本文件包含多个相近算法或同一算法的不同实现，保留在同一篇笔记中便于对照学习。

## 核心思路

- Bellman-Ford 重复松弛所有边；若第 `n` 轮仍能松弛，则存在源点可达的负环。
- SPFA 使用队列只处理距离刚被更新的节点，是 Bellman-Ford 的队列优化。
- 若只关心负环是否影响终点，可先标记仍能被松弛的节点，再判断这些节点能否到达终点。

## 复杂度分析

Bellman-Ford 时间复杂度为 `O(nm)`；SPFA 最坏时间复杂度仍为 `O(nm)`；空间复杂度为 `O(n+m)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>//�и�Ȩ�ĵ�Դ���·���ж��Ƿ��и��� 
using namespace std;
struct edge{
	int v,w;
};
vector<edge> e[N];
int d[N];
bool bellman(int s){
	memset(d,INT_MAX,sizeof(d));
	d[s]=0;
	bool flag;//���ڼ�¼�Ƿ��ɳ� 
	for(int i=1;i<=n;i++){//���n��ѭ������ʵ��ȫ���ɳ� 
		flag=false;
		for(int u=1;u<=n;u++){
			if(d[u]==INT_MAX) continue;
			for(auto& ed:e[u]){
				int v=ed.v,w=ed.w;
				if(d[v]>d[u]+w){
					d[v]=d[u]+w;
					flag=true;
				}
			} 	
		}
		if(flag==false) break;//�������ɳڣ����˳�ѭ�� 
	}
	return flag;//����n��=true�����л� 
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	
}
struct edge{//SPFA�㷨����bellmanʹ�ö����Ż� 
	int v,w;
};
vector<edge> e[N];//�����ڵ�ͱ�Ȩ�� 
int d[N],cnt[N],vis[N];//d��������Դ��s���룬cnt[]Դ��s��i�ı�����vis[]i�Ƿ��ڶ����� 
queue<int> q;
bool spfa(int s){
	memset(d,inf,sizeof(d));
	d[s]=0;
	vis[s]=1;
	q.push(s);
	while(!q.empty()){
		int u=q.front();
		q.pop();
		vis[u]=0;
		for(auto ed:e[u]){
			int v=ed.v,w=ed.w;
			if(d[v]>d[u]+w){
				d[v]=d[u]+w;//���¾��� 
				cnt[v]=cnt[u]+1;//���±��� 
				if(cnt[v]>=n) return true;//�л�  
				if(!vis[v]) q.push(v),vis[v]=1;
			}
		}
	}
	return false;
}
//��Ҫ�ж�Դ�㣨1����ĳһ�㣨n��·�����Ƿ��и���
//����bellmanѭ������n-1�� 
vector<bool> cycle(n+1,false);//�ж�˭�ڻ��� 
	for(int u=1;u<=n;u++){//��n��ѭ�����������ɳڣ���õ��ڻ��� 
		if(d[u]==-INT_MAX) continue;
		for(auto&it :e[u]){
			if(d[it.v]<d[u]+it.w){
				cycle[u]=true;
				cycle[it.v]=true;
			}
		}
	}
	vector<bool> is(n+1,false);//�������жϵ���Դ�㣨1���ܵ�������и�����������ͨ���жϸ����ܷ񵽴��յ㣬��֪·�����Ƿ���Ծ�������������������Ƿ���������Ը���ȨֵΪ��������Ҫ�ж��ڱ�֤�ܵ����յ��ǰ�����ܷ񾭹������� 
	queue<int> q;
	for(int i=1;i<=n;i++){//�ж����и����ĵ����Ƿ���ڿɴ��յ�n�ĵ� 
		if(cycle[i]==true){
			q.push(i);
			is[i]=true;
		}
	}
 	bool reach=false;
 	while(!q.empty()){
 		int u=q.front();
 		q.pop();
 		if(u==n){
 			reach=true;
 			break;
		}
		for(auto& it:e[u]){
			if(is[it.v]==false){
				is[it.v]=true;
				q.push(it.v);
			}
		}
	}
	if(reach==true) cout<<-1;//�����ܵ����յ㣬����������޴� 
	else cout<<d[n];
	return 0;
```
