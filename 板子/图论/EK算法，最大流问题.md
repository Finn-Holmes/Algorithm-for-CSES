# Edmonds-Karp 最大流

## 模板用途

使用最短增广路不断增广，求容量网络的最大流。

## 核心思路

- 在残量网络中用 BFS 寻找从源点到汇点的增广路。
- `mf[v]` 记录本次增广时到达 `v` 的最大可增广流量，`pre[v]` 记录前驱边。
- 找到汇点后沿前驱边回溯，减少正向边容量并增加反向边容量。

## 复杂度分析

时间复杂度为 `O(VE^2)`，空间复杂度为 `O(V+E)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>
using namespace std;
const int M,N;//sΪ��㣨Դ�㣩��TΪ�յ㣨��㣩
struct edge{//!!!ע��MҪ��������С���������� 
	long long v,c,ne;//vΪ�ñ���һ���ڵ㣬cΪ�ñ�������neΪ�ñ���һ���� 
}e[M];//�� 
int h[N],idx=1;//h[u]��ڵ�u�ĵ�һ�����ߣ�idx��Ϊ�ߵı�� 
long long mf[N],pre[N];//mf[v]Դ��s��v���������ޣ��������ߵ���С����pre[v]��ڵ�v��ǰ���� 
void add(int a,int b,int c){
	idx++;//��2Ϊ��㣬2�����ߣ�3�淴�ߣ�����i^1����i�ķ��ߣ�����һ�� 
	e[idx]={b,c,h[a]};//
	h[a]=idx;
}
bool bfs(){
	memset(mf,0,sizeof mf);
	queue<int> q;
	q.push(s);
	mf[s]=1e9;
	while(q.size()){
		int u=q.front();
		q.pop();
		for(int i=h[u];i;i=e[i].ne){
			long long v=e[i].v;
			if(mf[v]==0&&e[i].c>0){//v��û�з����Ҹñ߻������� 
				mf[v]=min(mf[u],e[i].c); 
				pre[v]=i;
				q.push(v);
				if(v==T) return true;
			}
		}
	}
	return false;
}
long long EK(){
	long long flow=0;
	while(bfs()==true){
		int v=T;
		while(v!=s){
			int i=pre[v];
			e[i].c-=mf[T];
			e[i^1].c+=mf[T];
			v=e[i^1].v;
		}
		flow+=mf[T];
	}
	return flow;
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	for(int i=1;i<=m;i++){
		cin>>a>>b>>c;
		add(a,b,c);
		add(b,a,0);
	}
}
```
