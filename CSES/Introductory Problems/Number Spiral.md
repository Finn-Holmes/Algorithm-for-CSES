## 题目描述
A number spiral is an infinite grid whose upper-left square has number 1. Here are the first five layers of the spiral:

Your task is to find out the number in row y and column x.

![图片怎么没加载出来捏~(￣▽￣)~*](../images/20240227201624_61918.png)
## 输入
The first input line contains an integer t（1 ≤ t ≤ 105）: the number of tests.
After this, there are t lines, each containing integers y and x（1 ≤ y,x ≤ 109）.
## 输出
For each test, print the number in row y and column x.
## 样例输入
```
3
2 3
1 1
4 2
```
## 样例输出
```
8
1
15
```
## 题解
1e9，直接打表也不现实，我才用的方法是找一些小规律，按主对角线分为两半，按不同规则求，奇偶也按不同规则求，注意要开long long
## code
```C++
#include <bits/stdc++.h>
using namespace std;
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    long long int t,o,i,x,y,s=0;
    cin>>t;
    for(o=1;o<=t;o++){
        cin>>x>>y;
        if(x>=y){
            s=(x-1)*(x-1);
            if(x%2==0){
                s+=x+x-y;   
            }else{
                s+=y;
            }
        }else{
            s=(y-1)*(y-1);
            if(y%2==0){
                s+=x;
            }else{
                s+=y*2-x;
            }
        }
        cout<<s<<'\n';
    }
}
```
