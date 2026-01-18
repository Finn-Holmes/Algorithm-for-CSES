## 题目描述
Given a string, you want to reorder its characters so that no two adjacent characters are the same. What is the lexicographically minimal such string?
## 输入
The only input line as a string of length n consisting of characters A–Z.

Constraints

1 ≤ n ≤ 106
## 输出
Print the lexicographically minimal reordered string where no two adjacent characters are the same. If it is not possible to create such a string, print -1.
## 样例输入
```
HATTIVATTI
```
## 样例输出
```
AHATITITVT
```
## 题解
贪心，每次都优先考虑字典序小的字符，上一个选择prev，假设本位置已选定字符c，保证c!=prev，除去该c后，剩余字符个数l,其中最大的字符是ch，对应个数为m，
若ch与c相同，则c之后不能放ch，最早只能放在c后两位，所以为了把ch放完，需保证m<=l/2，若ch!=c，需保证m<=(l+1)/2,若满足，则确定c可放，更新个数数组num，进行下一个循环，不满足则试用下一个字符。
## code
```C++
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	string s;
	cin>>s;
	int n=s.size();
	vector<int> num(26,0);
	for(int i=0;i<n;i++){
		num[s[i]-'A']++;
	}
	string ans;
	ans.reserve(n);
	int prev=-1;
	for(int i=0;i<n;i++){
		bool is=false;
		for(int c=0;c<26;c++){
			if(num[c]==0) continue;
			if(c==prev) continue;
			num[c]--;//假设放c
			int l=n-i-1;//假设放c后剩余次数
			int m=0,ch=-1;
			for(int j=0;j<26;j++){//找次数最大的字符ch
				if(num[j]>m){
					m=num[j];
					ch=j;
				}
			}
			bool vis;
			if(l==0){
				vis=true;
			}else{
				if(ch==c){
					vis=(m<=l/2);
				}else{
					vis=(m<=(l+1)/2);
				}
			}
			if(vis){//确定可以放c
				ans.push_back(char('A'+c));
				prev=c;
				is=true;
				break;
			}else{//
				num[c]++;//恢复，试用下一个字符
			}
		}
		if(!is){//无满足条件的字符串
			cout<<-1;
			return 0;
		}
	}
	cout<<ans;
	return 0;
}

```
