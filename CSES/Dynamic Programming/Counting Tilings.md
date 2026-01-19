## 题目描述
Your task is to count the number of ways you can fill an n* m grid using 1 * 2 and 2 * 1 tiles.
## 输入
The only input line has two integers n and m.

Constraints

1 ≤ n ≤ 10

1 ≤ m ≤ 1000

## 输出
Print one integer: the number of ways modulo 10^9+7.
## 样例输入
```
4 7
```
## 样例输出
```
781
```
## 题解
煮啵开始有一些喜欢状态压缩了，这个题我觉得是没有那么好想的，反正我在想的时候很难受，最后也借助了一些神秘的力量

状态压缩dp，dfs预处理

为了方便理解，这里我们先解释一下未处理当前列：未处理不代表当前列每一行都没有铺砖，可能存在左一列某几行铺了横砖，导致当前列某些位置是有砖的

#### 预处理
row为行，cur_mask为在把当前列row行以上铺满之后的状态，即半处理状态，next_mask为下一列的状态，随cur_mask铺横砖更新，init_mask为初始状态的cur_mask，即上文所说未处理当前列
预处理是为了得到trans[init_mask]，即将任意状态的未处理当前列处理后（铺满后）下一列可能出现的所有状态
#### dp 
dp[mask]为目前有多少种方案能使未处理当前列状态达成mask状态（注意此时mask仍是未处理，仅靠左侧列铺横砖实现当前列mask状态）
```C++
for(int next:trans[mask]){
  newdp[next]+=dp[mask];
```
未处理当前列状态为mask时，对于每一种方案来实现未处理当前列为mask，都有一种方案，来铺满当前列并使未处理下一列状态为next，有dp[mask]中方案来实现当前未处理列为mask，所以newdp[next]+=dp[mask]

#### 为什么最后输出dp[0]
在所有列都处理完（m 列后），我们要求没有留到下一列的“半个横向多米诺”，即最后一列处理后下一列的占位应为空——也就是状态mask==0。所以最终答案是dp[0]。
## code
```C++
#include <bits/stdc++.h>
using namespace std;
int n,m;
int full;
const int MOD=1e9+7;
vector<vector<int>> trans;
void dfs(int row,int cur_mask,int next_mask,int init_mask){
	if(row==n){//当前列铺满了
		trans[init_mask].push_back(next_mask); //存储下一列状态
		return; 
	}
	if(cur_mask&(1<<row)){//该列该行已经铺过了，上一列铺横砖导致，跳转至下一行
		dfs(row+1,cur_mask,next_mask,init_mask);
		return;
	}
	if(row+1<n&&!(cur_mask&(1<<row))&&!(cur_mask&(1<<(row+1)))){//尝试铺竖砖
		dfs(row+2,cur_mask|(1<<row)|(1<<(row+1)),next_mask,init_mask);
	}
	dfs(row+1,cur_mask|(1<<row),next_mask|(1<<row),init_mask);//铺横砖
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin>>n>>m;
	full=(1<<n)-1;
	trans.assign((1<<n),vector<int>());
	for(int mask=0;mask<=full;mask++){
		dfs(0,mask,0,mask);
	}
	vector<long long> dp(1<<n,0),newdp(1<<n,0);//
	dp[0]=1;
	for(int col=0;col<m;col++){
		fill(newdp.begin(),newdp.end(),0);//初始化newdp
		for(int mask=0;mask<=full;mask++){//遍历任意状态
			if(dp[mask]==0) continue;//
			for(int next:trans[mask]){
				newdp[next]+=dp[mask];
				if(newdp[next]>=MOD) newdp[next]-=MOD;
			}
		}
		dp.swap(newdp);//更新dp
	}
	cout<<dp[0];
}


```
