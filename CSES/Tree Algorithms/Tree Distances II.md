## 题目描述
You are given a tree consisting of n nodes.
Your task is to determine for each node the sum of the distances from the node to all other nodes.
## 输入
The first input line contains an integer n: the number of nodes. The nodes are numbered 1,2,...,n.
Then there are n-1 lines describing the edges. Each line contains two integers a and b: there is an edge between nodes a and b.

Constraints

1 ≤ n ≤ 2*10^5

1 ≤ a,b ≤ n
## 输出
Print n integers: for each node 1,2,...,n, the sum of the distances.
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
6 9 5 8 8
```
## 题解
先说一下主要思路：假设我们现在有的是一棵有向树，根为1，我们先求出1到所有结点路径之和（所有结点深度之和），ans[1]=sum_depth;

定义以结点i为根的树的结点个数(包括i本身)为num[i]

此时，比如说1有一个子节点2，则ans[2]=ans[1]-num[2]+(n-num[2]);

2的子结点到2的距离都比到结点1少1，其他到结点2的距离都比结点1多1(其他结点去2都要经过结点1)

现在已经明了很多了

那我们现在要做的就是求出把无向图转化为有向图，记录每个结点的深度，求每个结点的num[i]，最后求出ans[i]

深度记录d[i]在bfs无向图转有向图时就可以解决
一般思路是后序遍历求num[i]，递归求ans[i],求num[i]需要逆拓扑序，求ans需要拓扑序，但考虑到深度可能达到2e5，深度过深，会爆栈，我们可以在无向图转有向图时记录拓扑序并存在数组tuopu
，之后就可以根据tuopu倒序求num[i],正序求ans[i].
## code
```C++
#include <bits/stdc++.h>
using namespace std;
vector<long long> ans;
vector<vector<int>> newg;
vector<long long> num;
int n;
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin>>n;
	ans.assign(n+1,0);
	num.assign(n+1,1);
	newg.assign(n+1,vector<int>());
	vector<vector<int>> g(n+1,vector<int>());
	for(int i=1;i<n;i++){
		int a,b;
		cin>>a>>b;
		g[a].push_back(b);
		g[b].push_back(a);//无向图
	}
	queue<int> q;
	q.push(1);
	vector<bool> vis(n+1,false);
	vis[1]=true;
	vector<long long> d(n+1,0);//深度
	vector<int> tuopu;//拓扑序数组
	tuopu.push_back(1);
	while(!q.empty()){
		int u=q.front();
		q.pop();
		for(int v:g[u]){
			if(!vis[v]){
				tuopu.push_back(v);//记录拓扑序
				vis[v]=true;//避免重复访问
				q.push(v);
				newg[u].push_back(v);//有向图
				d[v]=d[u]+1;//记录深度
			}
		}
	}
	//逆拓扑统计各节点树大小
	for(int i=tuopu.size()-1;i>=0;i--){
		int u=tuopu[i];
		for(int v:newg[u]){
			num[u]+=num[v];
		}
	}
	long long sum_root=0;//记得开long long
	for(int i=1;i<=n;i++){
		sum_root+=(long long)d[i];
	}
	ans[1]=sum_root;
	for(int i=0;i<tuopu.size();i++){//拓扑序求解
		int u=tuopu[i];
		for(int v:newg[u]){
			ans[v]=ans[u]-num[v]+(n-num[v]);
		}
	}
	for(int i=1;i<=n;i++){
		cout<<ans[i]<<" "; 
	}
	return 0;
}

```
