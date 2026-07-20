## 题目描述
小C终于被小X感动了，于是决定与他看电影，然而小X距离电影院非常远。现在假设每条道路需要花费小X的时间为1，由于有数以万计的好朋友沿路祝贺，导致小X在通过某些路时不得不额外耗费1的时间来和他们聊天。

尽管他希望尽早见到小C，所以他希望找到一条最快到达电影院的路。

一开始小X在1号点，共有n个点、m条双向道路，电影院为t号点。

## 输入
第一行包含3个正整数n、m和t，分别表示点的数量、道路的数量以及电影院所在的点。

接下来m行，每行包含3个整数u、v和w，表示点u和点v之间存在一条权值为w的双向道路。

注意，图中可能存在重边。

对于30%的数据：

n≤10，m≤20

对于60%的数据：

n≤1000，m≤20000

对于100%的数据：

n≤5000000，m≤10000000

## 输出
输出一行一个整数，表示从1号点到t号点的最短路长度。

如果无法到达t号点，输出-1。

## 样例输入
```
10 12 6
3 9 2
6 9 2
6 2 1
3 1 1
1 9 2
2 8 2
7 10 1
7 2 1
10 0 1
8 1 1
1 5 2
3 7 2
```

## 样例输出
```
4
```

## 题解
根据题意，一条道路原本需要花费1的时间，如果需要和朋友聊天则会额外花费1的时间，因此道路的权值只有1和2。

本题要求从1号点到t号点的最短路，可以使用Dijkstra算法。但是n和m的数据范围非常大，如果使用普通的优先队列，会产生较大的时间与空间开销。

由于边权只有1和2，可以使用桶优化的Dijkstra。

设当前处理的最短距离为len，因为从当前节点出发，新产生的距离只可能是len+1或者len+2，所以只需要准备3个桶：

```
q[0]
q[1]
q[2]
```

距离为d的节点放入：

```
q[d%3]
```

每次找到当前距离len对应的非空桶，从中取出节点并更新相邻节点的最短距离。

由于同一个节点可能被多次放入桶中，所以取出节点后需要判断：

```C++
if(d[u]!=len) continue;
```

如果当前记录不是节点u的最新最短距离，说明这是一个已经失效的状态，直接跳过即可。

另外，由于n和m非常大，不能使用`vector<vector<pair<int,int>>>`存图。代码使用链式前向星：

- head[u]：点u的第一条边
- to[i]：第i条边到达的节点
- next[i]：第i条边之后的下一条边
- we[i]：第i条边的权值

道路是双向的，所以每条输入道路需要保存两个方向。

重边不会影响算法。如果两点之间有多条边，在松弛操作时会自动选择能够得到更短距离的边。

时间复杂度为O(n+m)，空间复杂度为O(n+m)。

## code
```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,m,t;
    cin>>n>>m>>t;
    vector<int> head(n+1,-1);
    vector<int> to(2*m);
    vector<int> next(2*m);
    vector<int> we(2*m);
    int cnt=0;
    for(int i=0;i<m;i++){
        int u,v,w;
        cin>>u>>v>>w;
        to[cnt]=v;
        we[cnt]=w;
        next[cnt]=head[u];
        head[u]=cnt++;
        to[cnt]=u;
        we[cnt]=w;
        next[cnt]=head[v];
        head[v]=cnt++;
    }
    vector<int> d(n+1,INT_MAX);
    vector<vector<int>> q(3,vector<int>());
    d[1]=0;
    q[0].push_back(1);
    int num=1;
    int len=0;
    while(num>0){
        while(q[len%3].empty()){
            len++;
        }
        int u=q[len%3].back();
        q[len%3].pop_back();
        num--;
        if(d[u]!=len) continue;
        if(u==t){
            break;
        }
        for(int edge=head[u];edge!=-1;edge=next[edge]){
            int v=to[edge];
            int newd=len+we[edge];
            if(newd<d[v]){
                d[v]=newd;
                q[newd%3].push_back(v);
                num++;
            }
        }
    }
    if(d[t]==INT_MAX){
        cout<<-1;
    }else{
        cout<<d[t];
    }
    return 0;
}
```
