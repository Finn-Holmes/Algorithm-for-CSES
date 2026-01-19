## 题目描述
Given two arrays of integers, find their longest common subsequence.
A subsequence is a sequence of array elements from left to right that can contain gaps. A common subsequence is a subsequence that appears in both arrays.

## 输入
The first line has two integers n and m: the sizes of the arrays.
The second line has n integers a1,a2,...,an: the contents of the first array.
The third line has m integers b1,b2,...s,bm: the contents of the second array.

## 输出
First print the length of the longest common subsequence.
After that, print an example of such a sequence. If there are several solutions, you can print any of them.
Constraints

1 ≤ n,m ≤ 1000
1 ≤ ai, bi ≤ 10^9

## 样例输入
```
8 6
3 1 3 2 7 4 8 2
6 5 1 2 3 4
```
## 样例输出
```
3
1 2 4
```
## 题解
用经典的动态规划：dp[i][j] 表示 a 的前 i 个元素与 b 的前 j 个元素的最长公共子序列长度。

状态转移：

若 a[i-1] == b[j-1]，dp[i][j] = dp[i-1][j-1] + 1

否则 dp[i][j] = max(dp[i-1][j], dp[i][j-1])

计算完 dp 后从 (n,m) 反向回溯得到一个具体的 LCS（遇到相等就加入答案并同时 i--, j--，否则沿较大的方向移动）。
## code
```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n,m;
	cin>>n>>m;
	vector<int> a(n);
	vector<int> b(m);
	for(int i=0;i<n;i++){
		cin>>a[i];
	}
	for(int i=0;i<m;i++){
		cin>>b[i];
	}
	//dp
	vector<vector<int>> dp(n+1,vector<int>(m+1,0));
	for(int i=1;i<=n;i++){
		for(int j=1;j<=m;j++){
			if(a[i-1]==b[j-1]){
				dp[i][j]=dp[i-1][j-1]+1;//取该字符
			}else{
				dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
			}
		}
	}
	//回溯
	int len=dp[n][m];
	cout<<len<<'\n';
	vector<int> ans;
	int i=n,j=m;
	while(i>0&&j>0){
		if(a[i-1]==b[j-1]){
			ans.push_back(a[i-1]);
			i--;
			j--; 
		}else{
			if(dp[i-1][j]>=dp[i][j-1]){
				i--;
			}else{
				j--;
			}
		} 
	}
	reverse(ans.begin(),ans.end());
	for(int i=0;i<ans.size();i++){
		cout<<ans[i]<<" ";
	}
	return 0;
}

```
