
## C++ algorithm 中的 count 函数的使用

```C++
count(begin, end, index);
```

**count()** 类似于find()，在一个序列中统计index出现的次数，返回次数。

#### 【例】

```C++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int  main()
{
	int a[10] = {3, 5, 7, 3, 1, 3, 2, 4, 6, 6};
	vector<int> vec(10);
	vec[0] = 2; vec[1] = 3;
	vec[2] = 9; vec[3] = 5;
	vec[4] = 2; vec[5] = 4;
	vec[6] = 8; vec[7] = 4;
	vec[8] = 2; vec[9] = 1;
	//数组中查找
	cout << count(a, a + 10, 3) << endl;
	cout << count(a, a + 10, 6) << endl;
	cout << count(a, a + 10, 7) << endl;
	//3 2 1

	//vector中查找
	cout << count(vec.begin(), vec.end(), 2) <<endl;
	cout << count(vec.begin(), vec.end(), 3) <<endl;
	cout << count(vec.begin(), vec.end(), 5) <<endl;
	// 3 1 1

	//字符串中字符查找
	string s = "helloworld";
	cout << count(s.begin(), s.end(), 'l') <<endl;
	cout << count(s.begin(), s.end(), 'o') <<endl;
	cout << count(s.begin(), s.end(), 'h') <<endl;
	//3 2 1
	return 0;
}
```
