## 题目描述
You are given an n * m grid where each cell contains one character A, B, C or D.
For each cell, you must change the character to A, B, C or D. The new character must be different from the old one.
Your task is to change the characters in every cell such that no two adjacent cells have the same character.

## 输入
The first line has two integers n and m: the number of rows and columns.
The next n lines each have m characters: the description of the grid.

## 输出
Print n lines each with m characters: the description of the final grid.
You may print any valid solution.
If no solution exists, just print IMPOSSIBLE.
Constraints

1 ≤ n, m ≤ 500

## 样例输入
```
3 4
AAAA
BBBB
CCDD
```
## 样例输出
```
CDCD
DCDC
ABAB
```
## 题解
贪心即可，挨个判断，只要是存在合理选择即赋值，若对应位置不存在合理选择则IMPOSSIBLE，煮啵把这个题想的太复杂了，刚开始还想每一行ab交叉，行间用dp，再转置，重复操作，所以煮啵挺不开心
## code
```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n,m;
	cin>>n>>m;
	vector<string> s(n);
	for(int i=0;i<n;i++){
		cin>>s[i];
	}
	vector<string> ans(n,string(m,'?'));
	string letters="ABCD";
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			bool cant[4]={false,false,false,false};
			for(int k=0;k<4;k++){
				if(letters[k]==s[i][j]){
					cant[k]=true;
				}
			}
			if(i>0){
				for(int k=0;k<4;k++){
					if(letters[k]==ans[i-1][j]){
						cant[k]=true;
					}
				}
			}
			if(j>0){
				for(int k=0;k<4;k++){
					if(letters[k]==ans[i][j-1]){
						cant[k]=true;
					}
				}
			}
			char pick='?';
			for(int k=0;k<4;k++){
				if(!cant[k]){
					pick=letters[k];
					break;
				}
			}
			if(pick=='?'){
				cout<<"IMPOSSIBLE";
				return 0;
			}
			ans[i][j]=pick;
		}
	}
	for(int i=0;i<n;i++){
		cout<<ans[i]<<'\n';
	}
	return 0;
} 
```
