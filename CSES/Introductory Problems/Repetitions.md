## 题目描述
You are given a DNA sequence: a string consisting of characters A, C, G, and T. Your task is to find the longest repetition in the sequence. This is a maximum-length substring containing only one type of character.
## 输入
The only input line contains a string of n（1 ≤ n ≤ 106） characters.
## 输出
Print one integer: the length of the longest repetition.
## 样例输入
```
ATTCGGGA
```
## 样例输出
```
3
```
## 题解
暴力即可，一次循环就可完成
## code
```C++
#include <bits/stdc++.h>
using namespace std;
 
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    string s;
    int n,i,k=1,maxk=1;
    cin>>s;
    n=s.size();
    for(i=1;i<n;i++){
        if(s[i]==s[i-1]){
            k++;
        }else{
            k=1;
        }
        maxk=max(maxk,k);
    }
    cout<<maxk;
}
```
