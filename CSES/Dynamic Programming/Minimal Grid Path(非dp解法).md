## 题目描述

给定一个 `n × n` 的网格，每个格子中都有一个大写字母。

你需要从左上角移动到右下角，每次只能向右或向下移动一格。经过的字母会组成一个长度为 `2n-1` 的字符串。

求所有可行路径中字典序最小的字符串。

## 输入

第一行包含一个整数 `n`，表示网格大小。

接下来 `n` 行，每行包含 `n` 个大写字母，表示网格中的字符。

## 输出

输出一行，表示能够得到的字典序最小字符串。

## 样例输入

```text
4
AACA
BABC
ABDA
AACA
```

## 样例输出

```text
AAABACA
```

## 数据范围

- 1 ≤ n ≤ 3000
- 网格中只包含 `A` 到 `Z` 的大写字母

## 题解

从左上角出发，移动相同步数后到达的所有格子一定满足相同的 `x+y`，也就是位于同一条斜线上。

使用 `cur` 保存能够组成当前最小前缀的所有位置。每次从这些位置向右或向下扩展，并找到下一层能够选择的最小字符 `min_char`。

然后只保留字符等于 `min_char` 的位置，其他位置不可能产生字典序更小的答案，可以直接舍弃。

不能只保留其中一个位置，因为这些位置虽然当前组成的前缀相同，但位置不同，后面能够选择的字符也可能不同。

同一个格子可能同时从上方和左侧到达，因此使用 `vis` 避免重复加入。

## 复杂度分析

每个格子最多加入一次候选集合，每次只检查右边和下边两个位置。

时间复杂度为 `O(n²)`。

空间复杂度为 `O(n²)`。

## code

```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;

    vector<vector<char>> a(n,vector<char>(n));
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            cin>>a[i][j];
        }
    }

    vector<pair<int,int>> cur;
    vector<vector<bool>> vis(n,vector<bool>(n,false));

    cur.push_back({0,0});

    int dx[2]={0,1};
    int dy[2]={1,0};

    string ans="";

    vis[0][0]=true;
    ans+=a[0][0];

    while(ans.size()<2*n-1){
        char min_char='Z';

        for(auto [x,y]:cur){
            for(int d=0;d<2;d++){
                int xx=x+dx[d];
                int yy=y+dy[d];

                if(xx<n&&yy<n){
                    min_char=min(min_char,a[xx][yy]);
                }
            }
        }

        vector<pair<int,int>> new_cur;

        for(auto [x,y]:cur){
            for(int d=0;d<2;d++){
                int xx=x+dx[d];
                int yy=y+dy[d];

                if(xx<n&&yy<n&&
                   a[xx][yy]==min_char&&
                   !vis[xx][yy]){
                    new_cur.push_back({xx,yy});
                    vis[xx][yy]=true;
                }
            }
        }

        cur=new_cur;
        ans+=min_char;
    }

    cout<<ans;
    return 0;
}
```
