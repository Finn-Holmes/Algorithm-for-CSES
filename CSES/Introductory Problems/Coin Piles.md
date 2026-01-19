## 题目描述
You have two coin piles containing a and b coins. On each move, you can either remove one coin from the left pile and two coins from the right pile, or two coins from the left pile and one coin from the right pile.
Your task is to efficiently find out if you can empty both the piles.
## 输入
The first input line has an integer t（1 ≤ t ≤ 10^5）: the number of tests.
After this, there are t lines, each of which has two integers a and b（0 ≤ a, b ≤ 10^9）: the numbers of coins in the piles.
## 输出
For each test, print "YES" if you can empty the piles and "NO" otherwise.
## 样例输入
```
3
2 1
2 2
3 3
```
## 样例输出
```
YES
NO
YES
```
## 题解
先交换保证x>y,方便操作，首先分三种情况：

（1）x/y>2，明显就算不成立，即使每次x拿2y拿1都不行，即x-=d*2<0；

（2）x/y==2，刚好可以；

（3）x/y<2，先分到x==y，剩余的一次x拿2一次y拿2，所以要保证x%3==0；
## code
```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t,o,n,i,p=0,x,y,d;
    cin>>t;
    for(o=1;o<=t;o++){
        cin>>x>>y;
        if(x<y){
            swap(x,y);
        }
        d=x-y;
        x-=d*2;
        y-=d;
        if(x>=0&&x%3==0){
            cout<<"YES"<<'\n';
        }else{
            cout<<"NO"<<'\n';
        }
    }
}
```
