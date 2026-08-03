# meeting2019

## 题目描述

给定一棵包含 \(n\) 个节点的树，其中 \(k\) 个节点上有人。

选择一个节点作为聚会地点，聚会所需时间为所有人到该节点距离的最大值。求最小聚会时间。

## 输入

第一行两个整数 \(n,k\)。

接下来 \(n-1\) 行，每行两个整数，表示树中的一条边。

最后一行包含 \(k\) 个不同的整数，表示所有人所在的节点。

## 输出

输出最小聚会时间。

## 样例输入

```text
4 2
1 2
3 1
3 4
2 4
```

## 样例输出

```text
2
```

## 提示或数据范围

- \(1\le n\le 10^5\)
- 每条边的长度均为 1
- 输入的图是一棵树

## 题解

只需要找到所有人员所在节点之间的最长距离，也就是这些特殊节点构成的直径。

具体步骤：

1. 从任意一个人员所在节点开始 BFS。
2. 在所有人员所在节点中，找到距离最远的节点 \(u\)。
3. 从 \(u\) 再进行一次 BFS。
4. 在所有人员所在节点中，找到距离最远的节点 \(v\)。
5. \(d(u,v)\) 就是人员节点之间的最大距离。

设这个最大距离为 \(D\)。

如果选择直径中间的节点作为聚会地点，那么直径两端到聚会地点的最大距离为：

\[
\left\lceil\frac{D}{2}\right\rceil
\]

因此答案为：

\[
\frac{D+1}{2}
\]

注意，寻找最远节点时只在人员所在的节点中寻找，但 BFS 仍然需要遍历整棵树。

## 复杂度分析

- 时间复杂度：\(O(n)\)
- 空间复杂度：\(O(n)\)

```C++
#include <bits/stdc++.h>
using namespace std;

vector<vector<int>> graph;

// 计算起点 head 到所有节点的距离
vector<int> bfs(int head) {
    int n = graph.size() - 1;
    vector<int> distance(n + 1, -1);
    queue<int> q;

    distance[head] = 0;
    q.push(head);

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        for (int v : graph[u]) {
            if (distance[v] == -1) {
                distance[v] = distance[u] + 1;
                q.push(v);
            }
        }
    }

    return distance;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, k;
    cin >> n >> k;

    graph.resize(n + 1);

    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v;

        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    vector<int> people(k);

    for (int i = 0; i < k; i++) {
        cin >> people[i];
    }

    // 从任意一个人员节点出发
    vector<int> distance = bfs(people[0]);

    // 找到距离最远的人员节点 u
    int u = people[0];

    for (int x : people) {
        if (distance[x] > distance[u]) {
            u = x;
        }
    }

    // 从 u 再进行一次 BFS
    distance = bfs(u);

    // 找到距离 u 最远的人员节点 v
    int v = u;

    for (int x : people) {
        if (distance[x] > distance[v]) {
            v = x;
        }
    }

    int diameter = distance[v];

    cout << (diameter + 1) / 2 << '\n';

    return 0;
}
```
