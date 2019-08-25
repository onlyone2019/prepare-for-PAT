
### PTA练习之 Battle Over Cities

![first](images/WorldCupBetting1.png)

![second](images/WorldCupBetting2.png)

##### 【解题思路】

这题怎么说呢，一看就是并查集吧，刷过天梯赛练习题里面红色警报那个题的朋友肯定知道，这两题差不太多。


需要明白的一点就是，连成一个无向的连通图，至少需要的边是顶点数减一。因此这题也就转换成了一个寻找顶点数的题了。对于已经连通了的城市，可以将它们看成一个城市块，也就是一个顶点，并查集查出顶点数也就得到了结果。


首先将边的信息保存下来，然后每次选择一个城市后，跳过所有与该城市相连的边，将剩余的城市连通的处理为相同祖先，最后遍历一下祖先数就可以找出顶点数，最终确定边数。


我的代码数组长度都是写死的，也就造成后面为了通过测试点将数组长度写得非常大了，这种方式非常不可取，还是非常建议动态方式来分配内存。

##### 【测试样例】

```C++
input:
3 2 3
1 2
1 3
1 2 3

output:
1
0
0
```

##### 【代码】

```C++
#include<iostream>
using namespace std;

struct edge
{
	int u;
	int v;
} e[1000000];

int father[100005];

void init()
{
	for (int i = 0; i < 100005;i++)
		father[i] = i;
}

int find(int x)
{
	int r=x;
	while(r!=father[r])
	{
		r = father[r];
	}
	int i = x, temp;
	while(father[i]!=r)
	{
		temp=father[i];
		father[i]=r;
		i = temp;
	}
	return r;
}

void join(int x,int y)
{
	int fx=find(x),fy=find(y);
	if(fx!=fy)
		father[fx] = fy;
}

int main()
{
	int n, m, k, u, v;
	cin >> n >> m >> k;
	for (int i = 0; i < m;i++)
	{
		cin >> u >> v;
		e[i].u = u;
		e[i].v = v;
	}
	for (int i = 0; i < k; i++)
	{
		int x,num=0;
		cin >> x;
		init();
		for (int j = 0; j < m;j++)
		{
			if(x== e[j].u || x==e[j].v)
				continue;
			else
				join(e[j].u,e[j].v);
		}
		for (int j = 1; j <= n;j++)
		{
			if(father[j]==j && j!=x)
				num++;
		}
		if(num<=1)
			cout << "0" << endl;
		else
			cout << num - 1 << endl;
	}
	return 0;
}
```
