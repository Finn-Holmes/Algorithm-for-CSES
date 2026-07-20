## 题目描述

Fanvree 很聪明，解决难题时他总会把问题简单化。

例如，他整天喜欢把图转化为树。但是他不会缩环，那他怎么转化呢？

现在有一个包含 $n$ 个点、$m$ 条无向边的图。Fanvree 会选择一个节点，删除这个节点以及所有与它相连的边。如果删除后剩余的图变成一棵树，那么这个节点就是可行的。

树是一个没有简单环的无向连通图。

请找出所有可行的节点。

## 输入

第一行包含两个正整数 $n,m$，表示图的节点数和边数，保证 $n\ge 2$。

接下来 $m$ 行，每行包含两个整数 $v,u$，表示节点 $v$ 和节点 $u$ 之间有一条无向边。

保证：

- $1\le v,u\le n$
- 没有重边
- 没有自环

## 输出

第一行输出一个整数 $ns$，表示可选择的节点数量。

第二行输出 $ns$ 个整数，表示所有可行节点的编号。

节点编号需要按照从小到大的顺序输出。

数据保证至少存在一个可行节点。

## 样例输入

```text
6 6
1 2
1 3
2 4
2 5
4 6
5 6
```

## 样例输出

```text
3
4 5 6
```

## 数据范围

- 对于 40% 的数据：n, m ≤ 1000
- 另外存在 10% 的数据：m = n - 1
- 另外存在 20% 的数据：m = n
- 对于 100% 的数据：n, m ≤ 100000

## 题解

要让删除节点 $u$ 后剩下的图成为一棵树，需要满足两个条件：

1. 剩下的图连通；
2. 剩下的边数等于剩下的节点数减一。

删除 $u$ 后还剩下 $n-1$ 个节点，所以应该剩下 $n-2$ 条边。

节点 $u$ 相连的边一共有 $degree[u]$ 条，因此删除它后剩下：

$$
m-degree[u]
$$

条边。

于是必须满足：

$$
m-degree[u]=n-2
$$

也就是：

$$
degree[u]=m-n+2
$$

接下来只需要判断删除这个节点后，剩下的图是否连通。

这里使用 Tarjan 算法。

对于 DFS 树中节点 $u$ 的儿子 $v$，如果：

$$
low[v]\ge dfn[u]
$$

说明 $v$ 的子树无法绕过 $u$ 到达上面的节点。删除 $u$ 后，这棵子树会变成一个新的连通块。

因此：

- 如果 $u$ 是 DFS 树的根，删除后产生的连通块数量就是它的 DFS 儿子数量；
- 如果 $u$ 不是根，删除后产生的连通块数量为满足 `low[v] >= dfn[u]` 的儿子数量再加一。这个一表示 $u$ 父亲方向的部分。

如果原图有 `component_cnt` 个连通块，那么删除 $u$ 后，整个图的连通块数量为：

```C++
component_cnt-1+split_cnt[u]
```

它等于 1 时，说明剩下的图连通。

所以节点 $u$ 是答案，当且仅当：

```C++
graph[u].size()==m-n+2
```

并且：

```C++
component_cnt-1+split_cnt[u]==1
```

## 复杂度分析

每个节点和每条边只会访问常数次。

时间复杂度为 $O(n+m)$，空间复杂度为 $O(n+m)$。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int n,m;
int timer_cnt;
int component_cnt;

vector<vector<int>> graph;
vector<int> dfn;
vector<int> low;
vector<int> father;
vector<int> split_cnt;

void tarjan(int u){
    dfn[u]=low[u]=++timer_cnt;

    int child=0;          // DFS树中u的儿子数量
    int split_child=0;    // 满足low[v]>=dfn[u]的儿子数量

    for(int v:graph[u]){
        if(dfn[v]==0){
            father[v]=u;
            child++;

            tarjan(v);

            low[u]=min(low[u],low[v]);

            // 删除u后，v的子树会成为一个独立连通块
            if(low[v]>=dfn[u]){
                split_child++;
            }
        }else if(v!=father[u]){
            // v已经访问过，并且不是u的父亲，说明遇到返祖边
            low[u]=min(low[u],dfn[v]);
        }
    }

    if(father[u]==0){
        // 根节点删除后，每个DFS儿子都是一个连通块
        split_cnt[u]=child;
    }else{
        // 非根节点还需要计算父亲方向的连通块
        split_cnt[u]=split_child+1;
    }
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    cin>>n>>m;

    graph.resize(n+1);
    dfn.assign(n+1,0);
    low.assign(n+1,0);
    father.assign(n+1,0);
    split_cnt.assign(n+1,0);

    for(int i=0;i<m;i++){
        int u,v;
        cin>>u>>v;

        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    // 原图不一定连通，所以每个连通块都要进行一次Tarjan
    for(int i=1;i<=n;i++){
        if(dfn[i]==0){
            component_cnt++;
            tarjan(i);
        }
    }

    vector<int> ans;

    // 删除节点后，剩余边数为n-2时所要求的度数
    int need_degree=m-n+2;

    for(int i=1;i<=n;i++){
        int remaining_component=
            component_cnt-1+split_cnt[i];

        bool edge_ok=
            (int)graph[i].size()==need_degree;

        bool connected_ok=
            remaining_component==1;

        if(edge_ok&&connected_ok){
            ans.push_back(i);
        }
    }

    cout<<ans.size()<<'\n';

    for(int x:ans){
        cout<<x<<' ';
    }

    return 0;
}
```
