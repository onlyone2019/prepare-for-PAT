
### PTA练习之 The Best Rank

![first](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/TheBestRank1.PNG)

![second](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/TheBestRank2.PNG)

##### 【解题思路】

这题主要把思路理清，就可以一气呵成。


**要把这几个点想清楚：**


1、采用什么方式来存储每一个学生的信息，这个应该都能想到结构体或者类。


2、如何输入一个学号有很快地找到该学号对应的学生成绩信息。这里我采用浪费内存的方式来获得时间上的节省，直接将学号作为结构体数组的下标，空间浪费较大，当然访问也比较快。


3、如何确定一个学号是否出现过（有记录）。我用了一个数组来存储已出现过的学号，以后每次查找信息时先找找这个数组中是否有该学号信息记录。


4、对于每一个成绩，如何快速获得该成绩的排名。这里我还是采用一个数组，预先排好序，以后直接查找该成绩在数组的哪个位置即可知道对应的名次。

##### 【代码】

```C++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

bool cmp(int a,int b)
{
	return a > b;
}

struct stud
{
	int gradec;
	int gradem;
	int gradee;
	int gradea;
}stude[1000000];
int main()
{
	int n, m, x=0;
	cin >> n >>m ;
	vector<int> C;
	vector<int> M;
	vector<int> E;
	vector<int> A;
	vector<int> ID;
	for (int i = 0; i < n;i++)
	{
		cin >> x;
		ID.push_back(x);
		cin >> stude[x].gradec >> stude[x].gradem >> stude[x].gradee;
		stude[x].gradea = (stude[x].gradec + stude[x].gradem + stude[x].gradee) / 3;
		C.push_back(stude[x].gradec);
		M.push_back(stude[x].gradem);
		E.push_back(stude[x].gradee);
		A.push_back(stude[x].gradea);
	}
	sort(ID.begin(), ID.end(), cmp);
	sort(C.begin(), C.end(), cmp);
	sort(M.begin(), M.end(), cmp);
	sort(E.begin(), E.end(), cmp);
	sort(A.begin(), A.end(), cmp);
	for (int i = 0; i < m;i++)
	{
		int index;
		cin >> index;
		if(find(ID.begin(),ID.end(),index) != ID.end())
		{
			int a[4];
			a[0] = find(A.begin(), A.end(), stude[index].gradea) - A.begin();
			a[1] = find(C.begin(), C.end(), stude[index].gradec) - C.begin();
			a[2] = find(M.begin(), M.end(), stude[index].gradem) - M.begin();
			a[3] = find(E.begin(), E.end(), stude[index].gradee) - E.begin();
			int i = min_element(a, a + 4) - a;
			switch(i)
			{
				case 0:
				{
					cout << a[0]+1 << " "
						 << "A" << endl;
					break;
				}
				case 1:
				{
					cout << a[1]+1 << " "
						 << "C" << endl;
					break;
				}
				case 2:
				{
					cout << a[2]+1 << " M" << endl;
					break;
				}
				case 3:
				{
					cout << a[3]+1 << " E" << endl;
					break;
				}
			}
		}
		else
		{
			cout << "N/A" << endl;
		}
	}
}
```
