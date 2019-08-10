
### PTA练习之 World Cup Betting

![first](images/WorldCupBetting1.png)

![second](images/WorldCupBetting2.png)

#####【解题思路】

这个题目是相对较为简单的题。题目是以足球博彩引出，但是总结起来很简单，就是给你三行数据，每行三列，你需要找出每列中最大的值，每个最大的值对应的字母"W"，"T"，"L"，输出一下，然后把三个最大值相乘再乘以0.65（65%的赔率）保留两位小数输出，所以真的很简单吧！


#####【话不多说，上代码】

```C++
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;
int main()
{
	double x;
	char max[3];
	double result=0.65;
	for (int i = 0; i < 3; i++)
	{
		double max_num = 0.0;
		int index = 0;
		for (int j = 0; j < 3; j++)
		{
			cin >> x;
			if(x>max_num)
			{
				max_num = x;
				index = j;
			}
		}
		result *= max_num;
		switch(index)
		{
			case 0:
				max[i] = 'W';
				break;
			case 1:
				max[i] = 'T';
				break;
			case 2:
				max[i] = 'L';
				break;
		}
	}
	for (int i = 0; i < 3; i++)
		cout << max[i] << " ";
	printf("%.2lf", (result - 1) * 2);
	return 0;
}
```
