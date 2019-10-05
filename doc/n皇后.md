## n皇后

### 题目描述
会下国际象棋的人都很清楚：皇后可以在横、竖、斜线上不限步数地吃掉其他棋子。如何将8个皇后放在棋盘上（有8 * 8个方格），使它们谁也不能被吃掉！这就是著名的八皇后问题。


原题链接：http://codeup.cn/problem.php?cid=100000608&pid=3

### 输入
```C++
一个整数n（ 1 < = n < = 10 )
```

### 输出
```C++
每行输出对应一种方案，按字典序输出所有方案。每种方案顺序输出皇后所在的列号，相邻两数之间用空格隔开。如果一组可行方案都没有，输出“no solute!”
```

### 判断小技巧
把棋盘存储为一个n维数组a[n]，数组中第i个元素的值代表第i行的皇后位置，这样便可以把问题的空间规模压缩为一维O(n)，在判断是否冲突，首先每行只有一个皇后，且在数组中只占据一个元素的位置，行冲突就不存在了，其次是列冲突，判断一下是否有a[i]与当前要放置皇后的列j相等即可。至于斜线冲突，通过观察可以发现所有在斜线上冲突的皇后位置都有规律即它们所在的行列互减的绝对值相等，即| row – i | = | col – a[i] | 。这样某个位置是否可以放置皇后的问题就可以解决。

### 代码
```C++
#include<iostream>
using namespace std;
#include<cmath>
#include<functional>

int a[12];
int n;

bool check(int i,int j)
{
	for(int k=1;k<i;k++)
	{
		if(a[k]==j || abs(a[k]-j)==abs(k-i)) return false;
	}
	return true;
}

int flag;

void dfs(int index)
{
	if(index==n+1)
	{
		for(int i=1;i<=n;i++)
		{
			cout<<a[i];
			if(i<n) cout<<" ";
		}
		cout<<endl;
		flag=1;
	}
	for(int i=1;i<=n;i++)
	{
		if(check(index,i))
		{
			a[index]=i;
			dfs(index+1);
		}
	}
}

int main()
{
	cin>>n;
	dfs(1);
	if(flag==0) cout<<"no solute!";
	return 0;
}
```
