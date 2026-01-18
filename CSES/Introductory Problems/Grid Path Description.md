## 题目描述
There are 88418 paths in a 7 * 7 grid from the upper-left square to the lower-left square. Each path corresponds to a 48-character description consisting of characters D (down), U (up), L (left) and R (right).
For example, the path

corresponds to the description DRURRRRRDDDLUULDDDLDRRURDDLLLLLURULURRUULDLLDDDD.
You are given a description of a path which may also contain characters ? (any direction). Your task is to calculate the number of paths that match the description.
## 输入
The only input line has a 48-character string of characters ?, D, U, L and R.
## 输出
Print one integer: the total number of paths.
## 样例输入
```
??????R??????U??????????????????????????LD????D?
```
## 样例输出
```
201
```
## 题解
DFS+剪枝，我一开始以为DFS会超时，不过一剪枝确实没超

通过DFS遍历路径，并判断是否满足条件，DFS过程中通过剪枝提前优化掉一些无效路径

## code
```C++
#include <bits/stdc++.h>
using namespace std;
string s;
bool vis[7][7];
int dr[4]={0,1,0,-1};
int dc[4]={1,0,-1,0};
char dirs[4]={'R','D','L','U'};
bool is(int r,int c){
	return r<0||r>=7||c<0||c>=7||vis[r][c];
}
int dfs(int r,int c,int num){
	if(r==6&&c==0){
		if(num==48) return 1;
		else return 0;
	}
	if(num==48) return 0;
	//剪枝，这里是一个特殊情况
///
上、下都被堵住，当前位置在垂直方向上形成了一个“死板”（像一个狭窄的通道的横截点）。
左右都可走说明两侧都有未访问的空间。如果继续向左或向右走，当前格子会成为把格子平面分成左右两部分的“分割点”。
由于上/下被堵，无法从另一侧绕回来穿过这一格，于是左右两侧的未访问格会被隔断成两个互相不可达的区域。
但我们的目标是找一条访问全部格子的路径（总共 49 个格子、移动 48 步），一旦分割成不连通的两部分就不可能同时从一条连续路径访问两边所有格。
换言之，进入这种“被垂直封闭但水平开放”的状态，必然导致无法把剩下所有格子都走完，属于死路，安全剪掉。
///
	if(is(r-1,c)&&is(r+1,c)&&!is(r,c-1)&&!is(r,c+1)) return 0;//上下都不可走，左右都可走
	if(!is(r-1,c)&&!is(r+1,c)&&is(r,c-1)&&is(r,c+1)) return 0;//左右都不可走，左右都可走
	char ch=s[num];
	int ans=0;
	for(int k=0;k<4;k++){
		if(ch!='?'&&ch!=dirs[k]) continue;
		int newr=r+dr[k];
		int newc=c+dc[k];
		if(newr<0||newr>=7||newc<0||newc>=7) continue;//违法坐标
		if(vis[newr][newc]) continue;//走过了
		vis[newr][newc]=true;
		ans+=dfs(newr,newc,num+1);
		vis[newr][newc]=false;//记得回溯
	}
	return ans;
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin>>s; 
	memset(vis,0,sizeof(vis));
	vis[0][0]=true;
	cout<<dfs(0,0,0);
	return 0;
}
```
