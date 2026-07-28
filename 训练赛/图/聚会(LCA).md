## 题目描述

Y 岛上有 `N` 个城市和 `N-1` 条道路，任意两个城市之间都可以通过道路互相到达，因此整个地图是一棵树。

每次聚会有三个人，分别位于城市 `a`、`b`、`c`。需要选择一个城市作为聚会地点，使三个人到达该城市经过的道路总数最少。

对于每次询问，输出最优聚会地点以及最小总费用。

## 输入

第一行包含两个正整数 `N,M`，分别表示城市数量和聚会次数。

接下来 `N-1` 行，每行包含两个正整数 `A,B`，表示城市 `A` 和城市 `B` 之间有一条道路。

接下来 `M` 行，每行包含三个正整数 `a,b,c`，表示三个人当前所在的城市。

## 输出

对于每次询问，输出一行两个整数 `Pos` 和 `Cost`：

- `Pos` 表示最优聚会地点；
- `Cost` 表示三个人到达该地点经过的道路总数。

## 样例输入

```text
6 4
1 2
2 3
2 4
4 5
5 6
4 5 6
6 3 1
2 4 4
6 6 6
```

## 样例输出

```text
5 2
2 5
4 1
6 0
```

## 数据范围

- 对于 40% 的数据：N, M ≤ 2000
- 对于 100% 的数据：N, M ≤ 500000

## 题解

在树上给定三个节点 `a,b,c`，使三点距离和最小的节点称为这三个点的树上中位点。

任意选择节点 `1` 作为根，计算三个两两最近公共祖先：

```C++
x=lca(a,b);
y=lca(a,c);
z=lca(b,c);
```

这三个节点中深度最大的节点就是树上中位点，也就是最优聚会地点。

原因是三个节点的路径会在中位点处汇合。三个两两 LCA 中，最深的那个恰好位于三条路径的公共连接位置。

总费用可以直接计算三个人到中位点的距离，也可以使用两两距离之和：

$$
Cost=\frac{dist(a,b)+dist(a,c)+dist(b,c)}{2}
$$

连接三个节点的最小子树中，每条边恰好被三条两两路径中的两条经过，因此两两距离之和是答案的两倍。

使用倍增预处理 LCA，即可在 `O(log N)` 时间内回答一次询问。

## 复杂度分析

- 预处理时间复杂度：`O(N log N)`
- 每次询问时间复杂度：`O(log N)`
- 总时间复杂度：`O((N+M) log N)`
- 空间复杂度：`O(N log N)`

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int n,m,LOG;
vector<vector<int>> graph;
vector<vector<int>> up;
vector<int> depth;

int lca(int u,int v){
    if(depth[u]<depth[v]){
        swap(u,v);
    }

    int diff=depth[u]-depth[v];

    for(int j=0;j<LOG;j++){
        if(diff&(1<<j)){
            u=up[j][u];
        }
    }

    if(u==v){
        return u;
    }

    for(int j=LOG-1;j>=0;j--){
        if(up[j][u]!=up[j][v]){
            u=up[j][u];
            v=up[j][v];
        }
    }

    return up[0][u];
}

int get_dist(int u,int v){
    int p=lca(u,v);
    return depth[u]+depth[v]-2*depth[p];
}

int get_deepest(int x,int y,int z){
    int pos=x;

    if(depth[y]>depth[pos]){
        pos=y;
    }

    if(depth[z]>depth[pos]){
        pos=z;
    }

    return pos;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    cin>>n>>m;

    graph.resize(n+1);

    for(int i=1;i<n;i++){
        int u,v;
        cin>>u>>v;

        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    LOG=1;
    while((1LL<<LOG)<=n){
        LOG++;
    }

    up.assign(LOG,vector<int>(n+1));
    depth.assign(n+1,0);

    vector<int> parent(n+1,0);
    stack<int> s;

    s.push(1);
    parent[1]=1;

    while(!s.empty()){
        int u=s.top();
        s.pop();

        for(int v:graph[u]){
            if(v==parent[u]){
                continue;
            }

            parent[v]=u;
            depth[v]=depth[u]+1;
            s.push(v);
        }
    }

    for(int u=1;u<=n;u++){
        up[0][u]=parent[u];
    }

    for(int j=1;j<LOG;j++){
        for(int u=1;u<=n;u++){
            up[j][u]=up[j-1][up[j-1][u]];
        }
    }

    while(m--){
        int a,b,c;
        cin>>a>>b>>c;

        int x=lca(a,b);
        int y=lca(a,c);
        int z=lca(b,c);

        int pos=get_deepest(x,y,z);

        long long cost=
            ((long long)get_dist(a,b)
            +get_dist(a,c)
            +get_dist(b,c))/2;

        cout<<pos<<" "<<cost<<'\n';
    }

    return 0;
}
```
