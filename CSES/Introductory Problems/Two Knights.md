## 题目描述
Your task is to count for k=1,2,...,n the number of ways two knights can be placed on a k * k chessboard so that they do not attack each other.
## 输入
The only input line contains an integer n（1 ≤ n ≤ 10000）.
## 输出
Print n integers: the results.
## 样例输入
```
8
```
## 样例输出
```
0
6
28
96
252
550
1056
1848
```
## 题解
容斥原理，先求总的，再求可以攻击的方式，骑士（马）按日字/L字走，棋盘上的日字个数“(i-1)*(i-2)”一种日字有两种摆法，再算横竖两种，需✖4。
## code
```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    double i=1;
    while(i<=n){
        long long ans=i*i*(i*i-1)/2-4*(i-1)*(i-2);
        i+=1;
        cout<<ans<<'\n';
    } 
     
}
```
