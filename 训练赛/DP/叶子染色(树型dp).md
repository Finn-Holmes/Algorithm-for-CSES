## 题目描述

给定一棵包含 `m` 个节点的无根树，其中编号为 `1` 到 `n` 的节点是叶子。

可以选择一个度数大于 `1` 的节点作为根，并给任意节点染成黑色或白色，也可以不染色。

染色方案需要满足：

- 从根到每个叶子的路径上至少存在一个有色节点；
- 对于每个叶子 `u`，从根到 `u` 的路径上最后一个有色节点的颜色必须等于给定的 `c[u]`。

求最少需要染色多少个节点。

## 输入

第一行包含两个正整数 `m,n`，分别表示节点总数和叶子数量。

接下来 `n` 行，每行一个整数，依次表示叶子 `1,2,...,n` 要求的颜色：

- `0` 表示黑色；
- `1` 表示白色。

接下来 `m-1` 行，每行包含两个整数 `a,b`，表示节点 `a` 和节点 `b` 之间有一条边。

## 输出

输出一个整数，表示最少需要染色的节点数量。

## 样例输入

```text
5 3
0
1
0
1 4
2 5
4 5
3 5
```

## 样例输出

```text
2
```

## 数据范围

- m ≤ 10000
- n ≤ 5021
- 节点 `1` 到 `n` 是叶子

## 题解

由于最终的根可以是任意一个度数大于 `1` 的节点，因此使用树形 DP 配合换根 DP。

在从根走向叶子的过程中，只需要记录路径上最近一个有色节点的颜色，共有三种状态：

- `0`：最近颜色为黑色；
- `1`：最近颜色为白色；
- `2`：路径上还没有出现有色节点。

如果当前节点不染色，状态不变；如果染黑或染白，传给下面节点的状态就会变成 `0` 或 `1`。

### 向下的树形 DP

定义 `down[u][c]` 表示：

> 进入节点 `u` 前最近的颜色为 `c` 时，处理 `u` 的子树中所有叶子所需的最少染色数。

对于一个要求颜色为 `color[u]` 的叶子：

- 如果继承的颜色正好满足要求，不需要染色；
- 否则必须把叶子染成要求的颜色。

对于内部节点 `u`，设 `sum[c]` 为所有儿子继承颜色 `c` 时的答案之和。

节点 `u` 有三种选择：

- 不染色，花费为 `sum[c]`；
- 染成黑色，花费为 `1+sum[0]`；
- 染成白色，花费为 `1+sum[1]`。

因此转移为：

$$
down[u][c]=\min(sum[c],1+sum[0],1+sum[1])
$$

### 换根 DP

`down[u]` 只能计算儿子方向，不能计算父亲方向，因此再定义：

```text
up[u][c]
```

表示进入 `u` 的父亲方向前，最近颜色为 `c` 时，处理父亲方向所有节点所需的最少染色数。

计算儿子 `v` 的 `up[v]` 时，先统计节点 `u` 所有方向的贡献，再减去 `v` 自己的子树贡献：

```C++
res[c]=sum[c]-down[v][c];
```

然后同样考虑 `u` 不染色、染黑和染白三种情况：

$$
up[v][c]=\min(res[c],1+res[0],1+res[1])
$$

当尝试把 `u` 作为最终的根时，根的上方不存在有色节点，因此初始状态为 `2`。答案为：

$$
\min(sum[2],1+sum[0],1+sum[1])
$$

枚举所有度数大于 `1` 的节点作为根，取最小值即可。

## 复杂度分析

每条边只会被处理常数次。

- 时间复杂度：`O(m)`
- 空间复杂度：`O(m)`

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int m,n;
    cin>>m>>n;

    vector<int> color(m+1,-1);
    for(int i=1;i<=n;i++){
        cin>>color[i];
    }

    vector<vector<int>> graph(m+1);
    for(int i=1;i<m;i++){
        int u,v;
        cin>>u>>v;
        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    // 临时选择一个内部节点作为根
    int root=1;
    while(graph[root].size()<=1){
        root++;
    }

    vector<int> parent(m+1,0);
    vector<int> order;
    order.reserve(m);

    stack<int> s;
    s.push(root);
    parent[root]=-1;

    while(!s.empty()){
        int u=s.top();
        s.pop();

        order.push_back(u);

        for(int v:graph[u]){
            if(v==parent[u]){
                continue;
            }

            parent[v]=u;
            s.push(v);
        }
    }

    // down[u][c]表示处理u子树方向的最小染色数
    vector<vector<int>> down(m+1,vector<int>(3,0));

    for(int index=m-1;index>=0;index--){
        int u=order[index];

        // 叶子节点
        if(u<=n){
            down[u][0]=(color[u]!=0);
            down[u][1]=(color[u]!=1);
            down[u][2]=1;
            continue;
        }

        int sum[3]={0,0,0};

        for(int v:graph[u]){
            if(parent[v]==u){
                for(int c=0;c<3;c++){
                    sum[c]+=down[v][c];
                }
            }
        }

        for(int c=0;c<3;c++){
            down[u][c]=min({
                sum[c],
                sum[0]+1,
                sum[1]+1
            });
        }
    }

    // up[u][c]表示处理u父亲方向的最小染色数
    vector<vector<int>> up(m+1,vector<int>(3,0));

    int ans=m;

    for(int u:order){
        int sum[3]={0,0,0};

        // 加入父亲方向
        if(parent[u]!=-1){
            for(int c=0;c<3;c++){
                sum[c]+=up[u][c];
            }
        }

        // 加入所有儿子方向
        for(int v:graph[u]){
            if(parent[v]==u){
                for(int c=0;c<3;c++){
                    sum[c]+=down[v][c];
                }
            }
        }

        // 尝试把u作为最终的根
        if(graph[u].size()>1){
            int root_ans=min({
                sum[2],
                sum[0]+1,
                sum[1]+1
            });

            ans=min(ans,root_ans);
        }

        // 将父亲方向的信息传递给每个儿子
        for(int v:graph[u]){
            if(parent[v]!=u){
                continue;
            }

            int res[3];

            for(int c=0;c<3;c++){
                res[c]=sum[c]-down[v][c];
            }

            for(int c=0;c<3;c++){
                up[v][c]=min({
                    res[c],
                    res[0]+1,
                    res[1]+1
                });
            }
        }
    }

    cout<<ans;

    return 0;
}
```
