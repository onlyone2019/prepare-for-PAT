
## C++ algorithm 中的 max(), min(), max_element(),min_element() 函数的使用

**min(),max()** 选两数中最小/大的那个。


**max_element(),min_element()** 找一个序列中最大/小的那个，返回一个迭代器。


如果有多个最大/小值，则返回第一个最大/小值所在位置。


#### 【例】

```C++
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	int a = max(5, 6);
	int b = min(3, 9);
	int num[10] = {5, 5, 9, 11, 13, 16, 17, 17, 17};
	cout << "a " << a << endl; //6
	cout << "b " << b << endl; //3
	cout << "max_element: " << *max_element(num, num + 9) << " at "
		 << max_element(num, num + 9) - num << endl;
	//17 at 6
	cout << "min_element: " << *min_element(num, num + 9) << " at "
		 << min_element(num, num + 9) - num << endl;
	//5 at 0
	return 0;
}
```
