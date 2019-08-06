
## C++ algorithm 中的 binary_search 函数的使用

```C++
binary_search(begin, end, index);
```

**binary_search()** 使用的是二分查找，因此查询速度较快，但是要求待查询的序列是有序的而且是非降序。
在区间[begin, end) 用二分法（折半）查找index，查到则返回true，否则返回false。查询的效率也非常高。

```C++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()
{
	int a[10] = {3, 7, 9, 5, 8, 4, 6, 2, 1, 10};
	sort(a, a + 10);//非降序
	if(binary_search(a,a+10,5))
		cout << "Yes 5 is found" << endl;
	else
		cout << "No 5 is not found" << endl;
	cout << endl;

	if(binary_search(a,a+10,11))
		cout << "Yes 11 is found" << endl;
	else
		cout << "No 11 is not found" << endl;
	return 0;
}

```
