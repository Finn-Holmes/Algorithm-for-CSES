## 题目描述
Let p(n,k) denote the kth permutation (in lexicographical order) of 1 ...n. For example, p(4,1)=[1,2,3,4] and p(4,2)=[1,2,4,3].
Your task is to process two types of tests:

Given n and k, find p(n,k)
Given n and p(n,k), find k

## 输入
The first line has an integer t: the number of tests.
Each test is either "1 n k" or "2 n p(n,k)".
## 输出
For each test, print the answer according to the example.

Constraints

1 ≤ t ≤ 1000

1 ≤ n ≤ 20

1 ≤ k ≤ n!

## 样例输入
```
6
1 4 1
1 4 2
2 4 1 2 3 4
2 4 1 2 4 3
1 5 42
2 5 2 4 5 3 1
```
## 样例输出
```
1 2 3 4
1 2 4 3
1
2
2 4 5 3 1
42
```
## 题解
康托展开与逆康托展开

#### 逆康托展开(ni_kangtuo)

设有 n 个不同数字（从 1 到 n），排列总数为 n!。

要获得第 k 个字典序的排列，假设剩下的还没选的数放在 vis 中。

在第 i 位（i = 0...n-1）时，剩下还有 n-i 个数字。对于每个数字为头，有 (n-1-i)! 种组合。

所以第一个数字选的是 idx = k / (n-1)! 的位置（从没被选过的数 vis 里选）。

选完第一个数后再从 vis 中移除，被选过的数以后都不会再出现。

然后更新 k，循环选下一个数，直到所有位置选完。
#### 康托展开(kangtuo)

perm 是一个 n 长的排列，vis 是所有没被用过的数。

对于每一位 perm[i]，统计有几个更小的且还没用过的数（index）。

这些 index 个数如果在当前位被提前选，每种情况都会产生 (n-1-i)! 个不同的全排列都会排在 nums 之前。

类似“字典序计数”，每一位累加贡献。

最终 k 就是 num 在所有排列中的字典序（从1计）。
## code
```C++
#include <bits/stdc++.h>
using namespace std;
const int N=21;
long long pre[N];
void init(){
    pre[0]=1;
    for(int i=1;i<N;i++){
        pre[i]=pre[i-1]*i;
    }
}
void ni_kangtuo(int n,long long k){
    vector<int> vis;
    for(int i=1;i<=n;i++) vis.push_back(i);//还未使用过的数
    vector<int> res;
    k--;
    for(int i=1;i<=n;i++){
        long long index=k/pre[n-i];
        res.push_back(vis[index]);
        vis.erase(vis.begin()+index);
        k=k%pre[n-i];
    }
    for(int i=0;i<res.size();i++){
        cout<<res[i]<<" ";
    } 
    cout<<'\n';
}
void kangtuo(int n,vector<int> nums){
    vector<int> vis;
    for(int i=1;i<=n;i++) vis.push_back(i);
    long long k=1;
    for(int i=0;i<n;i++){
        long long index=lower_bound(vis.begin(),vis.end(),nums[i])-vis.begin();
        k+=pre[n-i-1]*index;
        vis.erase(vis.begin()+index);
    }
    cout<<k<<'\n';
}
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    init();
    int t;
    cin>>t;
    while(t--){
        int op;
        cin>>op;
        if(op==1){
            int n;
            long long k;
            cin>>n>>k;
            ni_kangtuo(n,k);
        }else{
            int n;
            cin>>n;
            vector<int> nums(n);
            for(int i=0;i<n;i++){
                cin>>nums[i];
            }
            kangtuo(n,nums);
        }
    }
}
```
