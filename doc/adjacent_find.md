
## C++ algorithm 中的 accumulate 函数的使用

#### 1、在迭代器区间[begin,end)上查找第一次出现的两个连续相等的元素，返回指向第一个元素的迭代器，未找到则返回end

```C++
adjacent_find(begin, end);
```

#### 2、在迭代器区间[begin,end)上，用二元谓词BinaryPredicate判断，查找第一次出现的两个连续满足BinaryPredicate的元素，返回指向第一个元素的迭代器，未找到则返回end

```C++
adjacent_find(begin, end, BinaryPredicate);
```

##### 【例】

```C++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
bool parity_judge(int a,int b)
{
	return (a - b)%2 == 0 ? 1 : 0;
}
int main()
{
	vector<int> vec;
	vec.push_back(1);
	vec.push_back(2);
	vec.push_back(3);
	vec.push_back(3);
	vec.push_back(4);
	vec.push_back(9);
	vec.push_back(12);
	vec.push_back(12);
	//打印数组
	vector<int>::iterator it;
	for (it = vec.begin(); it != vec.end();it++)
	{
		cout << *it << " ";
	}
	cout << endl;
	//查找邻接相等的元素
	it = adjacent_find(vec.begin(), vec.end());
	if(it != vec.end())
	{
		cout << "第一对相等的元素：";
		cout << *it++ << " ";
		cout << *it << endl;
	}
	//查找奇偶性相同的邻近元素
	it = adjacent_find(vec.begin(), vec.end(), parity_judge);
	if(it != vec.end())
	{
		cout<<"第一对奇偶性相同的元素为：";
		cout << *it++ << " ";
		cout << *it;
	}
	return 0;
}
```
