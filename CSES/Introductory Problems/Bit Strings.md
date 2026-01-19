## 题目描述
Your task is to calculate the number of bit strings of length n.
For example, if n=3, the correct answer is 8, because the possible bit strings are 000, 001, 010, 011, 100, 101, 110, and 111.
## 输入
The only input line has an integer n（1 ≤ n ≤ 10^6）.
## 输出
Print the result modulo 10^9+7.
## 样例输入
```
3
```
## 样例输出
```
8
```
## 题解
很简单，2^n，注意取模
## code
```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n,i,o=1;
    cin>>n;
    for(i=1;i<=n;i++){
        o=o*2%1000000007;
    }
    cout<<o;
}
```
