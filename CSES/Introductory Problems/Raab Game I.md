## 题目描述
Consider a two player game where each player has n cards numbered 1,2,…,n. On each turn both players place one of their cards on the table. The player who placed the higher card gets one point. If the cards are equal, neither player gets a point. The game continues until all cards have been played.

You are given the number of cards n and the players' scores at the end of the game, a and b. Your task is to give an example of how the game could have played out.

## 输入
The first line contains one integer t: the number of tests.

Then there are t lines, each with three integers n, a and b.

## 输出
For each test case print YES if there is a game with the given outcome and NO otherwise.
If the answer is YES, print an example of one possible game. Print two lines representing the order in which the players place their cards. You can give any valid example.
Constraints

1 ≤ t ≤ 1000

1 ≤ n ≤ 100

0 ≤ a,b ≤ n
## 样例输入
```
5
4 1 2
2 0 1
3 0 0
2 1 1
4 4 1
```
## 样例输出
```
YES
1 4 3 2
2 1 3 4
NO
YES
1 2 3
1 2 3
YES
1 2
2 1
NO
```
## 题解
一眼构造，先看平局，前ping局均相等即可，member1直接均为i即可，member2将ping+1~n的序列向左循环平移a
## 注意
需要仔细思考“NO”的情况，煮啵自己写的时候就是这里错了

(1) 除去平局仅剩一局

(2) a+b超过n

(3) 非平局非0但是a或b为0
## code 
```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int t;
	cin>>t;
	while(t--){
		int n,a,b;
		cin>>n>>a>>b;
		int ping=n-a-b;
		vector<int> member1(n+1),member2(n+1);
		if(a+b==1||a+b>n||a+b>0&&(a==0||b==0)){//NO情况
			cout<<"NO"<<'\n';
			continue;
		}
		for(int i=1;i<=ping;i++){//平局构造
			member1[i]=i;
			member2[i]=i;
		}
		
		for(int i=ping+1;i<=n;i++){//member1构造
			member1[i]=i;
		}
		for(int i=ping+1;i<=ping+b;i++){//左移member2序列
			member2[i]=i+a;
		}
		for(int i=ping+b+1;i<=n;i++){
			member2[i]=i-b;
		}
		cout<<"YES"<<'\n';
		for(int i=1;i<=n;i++){
			cout<<member1[i]<<" ";
		}
		cout<<'\n';
		for(int i=1;i<=n;i++){
			cout<<member2[i]<<" ";
		}
		cout<<'\n';
	}
}
```
