
## C++ algorithm 中的 find 函数的使用

```C++
find(begin, end, index);
```

**find()** 是在指定区间中查找index元素第一次出现的位置，返回该位置的迭代器，没找到则返回end。
**特别地，** 在string类中也有自己的find函数，可用于字符串的匹配，若查到则返回查找子串首字母在该串中的位置，否则返回string::nops.这是一个非常有帮助的函数，string类中还有很多特别好用的字符串处理函数，后期会陆续更新。

```C++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int main()
{
	int num[5] = {3, 5, 7, 4, 2};
	if(find(num,num+5,7) != num+5)
		cout << "Yes!" << endl;
	else
		cout << "No" << endl;

	vector<int> vec(3);
	vec.push_back(3);
	vec.push_back(2);
	vec.push_back(1);
	if(find(vec.begin(),vec.end(),2) != vec.end())
		cout << "Yes" << endl;
	else
		cout << "No";

	string s="helloworld";
	if(find(s.begin(),s.end(),'w') != s.end())
		cout << "Yes " << endl;
	else
		cout << "No" << endl;

	if(s.find("wor")!= string::npos)
		cout << "Yes " << s.find("wor") << endl;
	//yes 5
	else
		cout << "No" << endl;
	return 0;
}
```
