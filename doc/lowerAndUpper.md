
## C++ algorithm 中的 lower_bound() upper_bound() 函数的使用

他们都是在一个前闭后开的区间查找一个元素，返回一个迭代器指针，未查到则返回end。但由于二者查询是以二分方式查询，要求待查序列是一个非降序序列。


**lower_bound()** 返回第一个大于等于查找值的元素所在的位置。


**upper_bound()** 返回第一个大于查找值的元素所在的位置。


**【换句话说】**


lower_bound()的意思就是说对于一个排好序的序列num，将index最**早** 能插入的位置。

```C++
int num[10]={5,7,9,11,13,16,17,17,17,21};
low = lower_bound(num,num+10,17) - num;
cout << "low: " << low << endl;
5, 7, 9, 11, 13, 16, *, 17, 17, 17,21
//6
```

lower_bound()的意思就是说对于一个排好序的序列num，将index最**晚** 能插入的位置。

```C++
int num[10]={5,7,9,11,13,16,17,17,17,21};
upp = upper_bound(num, num + 10, 17) - num;
cout << "upper: " << upp << endl;
5, 7, 9, 11, 13, 16, 17, 17, 17, *,21
//9
```
#### 【例】

```C++
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	int num[10]={5,7,9,11,13,16,17,17,17,21};
	int low = lower_bound(num,num+10,10) - num;
	cout << "low: " << low << endl; //3
	low = lower_bound(num,num+10,11) - num;
	cout << "low: " << low << endl; //3
	int upp = upper_bound(num, num + 10, 12) - num;
	cout << "upper: " << upp << endl; //4
	upp = upper_bound(num, num + 10, 13) - num;
	cout << "upper: " << upp << endl; //5
	cout << endl;
	low = lower_bound(num,num+10,17) - num;
	cout << "low: " << low << endl;//6
	upp = upper_bound(num, num + 10, 17) - num;
	cout << "upper: " << upp << endl; //9
	return 0;
}
```
