# Tarjan 强连通分量

## 模板用途

在线性时间内求有向图的所有强连通分量。

## 核心思路

- `dfn[u]` 是节点首次被访问的时间戳，`low[u]` 是从 `u` 出发能到达的最小时间戳。
- DFS 过程中将尚未归入强连通分量的节点保存在栈中。
- 当 `dfn[u] == low[u]` 时，`u` 是一个强连通分量的根，持续弹栈直到弹出 `u`。

## 复杂度分析

时间复杂度为 `O(n+m)`，空间复杂度为 `O(n+m)`。

## 使用提示

这是学习模板，使用时需要根据具体题目补充节点数、边数、数组范围、输入输出以及必要的初始化。模板文件中若包含多个相近算法，应按对应小节选择其中一种实现，而不是把多个独立程序同时编译。

## code

```C++
#include <bits/stdc++.h>//ǿ��ͨ����Tarjan�㷨 
using namespace std;
vector<int> e[N];
int dfn[N],low[N],tot;//dfnʱ�����low[x]��x�������ܷ��ʵ��������ʱ�������ʼ��Ϊ���� 
int stk[N],instk[N],top;//stkջ��instk�ж��Ƿ���ջ�� 
int scc[N],siz[N],cnt;
void tarjan(int x){
	tot++;//�Ǵ�����ջ 
	dfn[x]=tot;
	low[x]=tot;
	top++;
	stk[top]=x;
	instk[x]=1;
	for(int y:e[x]){
		if(!dfn[y]){
			tarjan(y);//���yû�з��ʣ�������������ӽڵ� 
			low[x]=min(low[x],low[y]);//����x��low�������y�����ߵ�low[y],��ôxҲ����ͨ��y�ߵ�low[y] 
		}else if(instk[y]){//��y�Ѿ���������ջ�У���˵��y��x�����Ƚڵ�����������ڵ㣬��ôxҲ��ͨ��y����low[y] 
			low[x]=min(low[x],dfn[y]);//��ʱҲ��Ҫ����һ��low[x] ,�˴���ʵ������low[y]����˫��ͨ������low[y]�Ǵ���ģ�dfn[y]һ���ԣ��ҿ��Ա��ִ���һ���� 
		}
	}
	if(dfn[x]==low[x]){//��x��x��ǿ��ͨ�����ĸ� 
		int y;
		cnt++;
		do{
			y=stk[top];
			top--;
			instk[y]=0;
			scc[y]=cnt;
			++siz[cnt];
		}while(y!=x);
	}
}
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
}
```
