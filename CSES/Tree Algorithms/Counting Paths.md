## 题目描述
You are given a tree consisting of n nodes, and m paths in the tree.
Your task is to calculate for each node the number of paths containing that node.
## 输入
The first input line contains integers n and m: the number of nodes and paths. The nodes are numbered 1,2,...,n.
Then there are n-1 lines describing the edges. Each line contains two integers a and b: there is an edge between nodes a and b.
Finally, there are m lines describing the paths. Each line contains two integers a and b: there is a path between nodes a and b.
Constraints

1 ≤ n, m ≤ 2*105

1 ≤ a,b ≤ n
## 输出
Print n integers: for each node 1,2,...,n, the number of paths containing that node.
## 样例输入
```
5 3
1 2
1 3
3 4
3 5
1 3
2 5
1 4
```
## 样例输出
```
3 1 3 1 1
```
## 题解
本题算是Tree Distances II与Company Queries II的结合，首先无向图g转有向图newg,记录fa[i],之后是倍增LCA(记录最近祖先dad)，距离差即为depth[a]-depth[dad]+depth[b]-depth[dad].
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
    vector<vector<int>> g(n+1,vector<int>());
    for(int i=2;i<=n;i++){
    	int a,b;
    	cin>>a>>b;
    	g[a].push_back(b);
    	g[b].push_back(a);
	}
	vector<vector<int>> newg(n+1,vector<int>());
	queue<int> qu;
	qu.push(1);
	vector<bool> vis(n+1,false);
	vis[1]=true;
	vector<int> fa(n+1,0);
	vector<int> depth(n+1,0);
	while(!qu.empty()){
		int u=qu.front();
		qu.pop();
		for(int v:g[u]){
			if(!vis[v]){
				vis[v]=true;
				newg[u].push_back(v);
				fa[v]=u;
				depth[v]=depth[u]+1;
				qu.push(v);
			}
		}
	}
    int num=0;
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
        int aa,bb;
        aa=a;
        bb=b;
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
            cout<<diff<<'\n';
            continue;
        }
        for(int i=num-1;i>=0;i--){
            if(d[a][i]!=d[b][i]){
                a=d[a][i];
                b=d[b][i];
            }
        }
        int dad=d[a][0];
        cout<<depth[aa]+depth[bb]-2*depth[dad]<<'\n';
    }
    return 0;
}
```
