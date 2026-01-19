## 题目描述
Your task is to calculate the number of trailing zeros in the factorial n!.
For example, 20!=2432902008176640000 and it has 4 trailing zeros.
## 输入
The only input line has an integer n（1 ≤ n ≤ 10^9）.
## 输出
Print the number of trailing zeros in n!.
## 样例输入
```
20
```
## 样例输出
```
4
```
## 题解
我一开始以为是大数乘法，稍微想一点初等数论的小脑筋发现只有5和2能产生0，2比5多，所以计数5就行，有多少5就有多少0。
## code
```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n,i,j,k=0;
    cin>>n;
    for(i=5;i<=n;i+=5){
        j=i;
        while(j%5==0){
            k++;
            j=j/5;
        }
    }
    cout<<k;
}
```
