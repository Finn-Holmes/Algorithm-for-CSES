## 题目描述
You are given a tree consisting of n nodes.
Your task is to determine for each node the maximum distance to another node.
## 输入
The first input line contains an integer n: the number of nodes. The nodes are numbered 1,2,...,n.
Then there are n-1 lines describing the edges. Each line contains two integers a and b: there is an edge between nodes a and b.
Constraints

1 ≤ n ≤ 2*105

1 ≤ a,b ≤ n
## 输出
Print n integers: for each node 1,2,...,n, the maximum distance to another node.
## 样例输入
```
5
1 2
1 3
3 4
3 5
```
## 样例输出
```
2 3 2 3 3
```
## 题解
首先我们需要先阐述一个结论，作为这个题的关键

选取树的直径的两个端点u,v,对于每个端点i,树上距离i最远的点必为u,v之一，即max(du[i],dv[i]);

简易证明：假设w为距离x最远的点，则在树上存在一点t，为u-x,v-x,w-x三条路径交点，则dist(x,u) = dist(x,t) + dist(t,u),dist(x,v) = dist(x,t) + dist(t,v),dist(x,w) = dist(x,t) + dist(t,w)
因为u-v为树的直径，则有dist(u,v)=dist(u,t)+dist(t,v)为最大值，假设w不为u,v其中一值，则有dist(t,w)>dist(t,u)&&dist(t,w)>dist(t,v),
此时树的直径应为dist(t,w)+dist(u,t)||dist(t,w)+dist(v,t),这与条件:u-v为直径矛盾.

                                                                                 # 证毕
此时，我们只需求出每个点到u,v的距离，在此之前我们应先找出u,v两点

任选一点（code选的是1）bfs求出距离1点最远的一点，根据结论，此点必为u,v之一,此处定为u

再从u点出发,bfs,找出v点，注意在bfs过程中储存遍历到的每个点到u的距离du

最后从v点出发,bfs,求出每个点到v点的距离dv

## code
```C++
#include <bits/stdc++.h>
using namespace std;
vector<int> d1,du,dv;
int n;
vector<vector<int>> g;
void bfs(int head,vector<int> &d){
	vector<bool> vis(n+1,false);
	queue<int> q;
	q.push(head);
	d[head]=0;
	vis[head]=true;
	while(!q.empty()){
		int node=q.front();
		q.pop();
		for(int y:g[node]){
			if(!vis[y]){
				vis[y]=true;
				d[y]=d[node]+1;
				q.push(y);
			}
		}
	}
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin>>n;
	g.assign(n+1,vector<int>());
	for(int i=1;i<n;i++){
		int a,b;
		cin>>a>>b;
		g[a].push_back(b);
		g[b].push_back(a);
	}
	d1.assign(n+1,0);
	du.assign(n+1,0);
	dv.assign(n+1,0);
	int u=1,v=1;
	bfs(1,d1);
	int dist=0;
	for(int i=1;i<=n;i++){
		if(d1[i]>dist){
			dist=d1[i];
			u=i;
		}
	}
	bfs(u,du);
	dist=0;
	for(int i=1;i<=n;i++){
		if(du[i]>dist){
			dist=du[i];
			v=i;
		}
	}
	bfs(v,dv);
	for(int i=1;i<=n;i++){
		cout<<max(du[i],dv[i])<<" ";
	}
	return 0;
}
```
