# 使 x×m 成为完全平方数的最小 m

## 模板用途

求最小正整数 `m`，使得 `x×m` 是完全平方数。

## 核心思路

- 先预处理每个整数的最小质因数。
- 分解 `x` 的质因数。完全平方数中每个质因子的指数必须为偶数。
- 若某个质因子在 `x` 中出现奇数次，就在 `m` 中补上一个该质因子。

## 复杂度分析

预处理复杂度约为 `O(Nlog log N)`，单次分解复杂度为 `O(log x)`，空间复杂度为 `O(N)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>//�˺��������ڲ���x����С������m������������x*mΪ��ȫƽ���� 
using namespace std;
const int MX=10000000;
void pmf(int n){//Ԥ����ÿ��������С������ 
    mpf.resize(n+1);
    for(int i=0;i<=n;i++)mpf[i]=i;
    for(int i=2;i*i<=n;i++){
        if(mpf[i]==i){
            for(int j=i*i;j<=n;j+=i){
                if(mpf[j]==j){
                    mpf[j]=i;
                }
            }
        }
    }
}

int fun(int x){//����С����m 
    int r=1;
    while(x>1){
        int p=mpf[x];
        int c=0;
        while(mpf[x]==p){
            c++;
            x/=p;
        }
        if(c%2==1){//��������p�Ĵ���Ϊ��������һ����r�� 
            r*=p;
        }
    }
    return r;
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	pmf(MX);
}
```
