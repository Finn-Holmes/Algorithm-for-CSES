## 题目描述
Given an array of n integers, your task is to calculate the number of increasing subsequences it contains. If two subsequences have the same values but in different positions in the array, they are counted separately.
## 输入
The first input line has an integer n: the size of the array.

The second line has n integers x1,x2,...,xn: the contents of the array.

Constraints

1 ≤ n ≤ 2 * 10^5

1 ≤ xi ≤ 10^9

## 输出
Print one integer: the number of increasing subsequences modulo 10^9+7.
## 样例输入
```
3
2 1 3
```
## 样例输出
```
5
```
## 提示
The increasing subsequences are [2], [1], [3], [2,3] and [1,3].
## 题解
离散化，dp，线段树

状态定义：令 dp[i] 表示以 a[i] 结尾的严格递增子序列个数（只计长度 ≥1 的子序列）。


转移： dp[i] = 1 + sum{ dp[j] | 0 ≤ j < i 且 a[j] < a[i] }。 其中 1 表示仅包含 a[i] 的单元素子序列。

问题是如何快速计算 j<i 且 a[j]<a[i] 的 dp 和：

先对所有 a 值做离散化（压缩到 0..m-1），保持大小关系。

用线段树维护“以某个值（离散坐标）结尾的 dp 之和”。线段树支持点加（把 dp[i] 累加到对应坐标）和区间求和（查询 [0, idx-1] 得到小于当前值的所有 dp 的和）。

遍历数组，按上式计算 dp[i]，累加到答案，并把 dp[i] 加到线段树的对应坐标，所有运算都对 MOD 取模。
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD=1e9+7;
struct Tree{
	int n;
	vector<long long> st;
	Tree(int nn){
		init(nn);
	}
	void init(int nn){
		n=1;
		while(n<max(1,nn)){
			n<<=1;
		}
		st.assign(2*n,0);
	}
	void add(int p,int val){
		p+=n;
		st[p]=(st[p]+val)%MOD;
		for(p>>=1;p>=1;p>>=1){
			st[p]=(st[p*2]+st[p*2+1])%MOD;
		}
	}
	long long query(int l,int r){
		if(l>r) return 0;
		long long res=0;
		int ll=l+n,rr=r+n;
		while(ll<=rr){
			if(ll%2==1){
				res=(res+st[ll++])%MOD;
			}
			if(rr%2==0){
				res=(res+st[rr--])%MOD;
			}
			ll>>=1;
			rr>>=1;
		}
		return res;
	}
};
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n;
	cin>>n;
	vector<long long> a(n);
	for(int i=0;i<n;i++){
		cin>>a[i];
	}
	vector<long long> nums=a;
	sort(nums.begin(),nums.end());
	nums.erase(unique(nums.begin(),nums.end()),nums.end());
	int m=nums.size();
	Tree tree(m);
	long long ans=0;
	for(int i=0;i<n;i++){
		int index=int(lower_bound(nums.begin(),nums.end(),a[i])-nums.begin());
		long long num=0;
		if(index){
			num=tree.query(0,index-1);
		}
		num=(num+1)%MOD;
		ans=(ans+num)%MOD;
		tree.add(index,num);
	}
	cout<<ans;
	return 0;
}

```
