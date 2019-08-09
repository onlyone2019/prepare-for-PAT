
## C++ algorithm 中的 next_permutation(), prev_permutation 函数的使用

对于一个序列{a,b,c}，按照字符串大小比较规则有这样顺序的六种排列组合{abc,acb,bac,bca,cab,cba}。其中abc没有上一个排列组合，cba没有下一个排列组合。


**next_permutation()** 会取得[first,end)所标示的序列的下一个排列组合，如果没有下一个排列组合则返回false，有则返回true。

#### 【例】

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	int a[3]={1,2,3};
	sort(a,a+3);
	do
	{
		for (int i = 0; i < 3; i++)
			cout << a[i] << " ";
		cout << endl;
	} while (next_permutation(a, a + 3));
	/*
	1 2 3
	1 3 2
	2 1 3
	2 3 1
	3 1 2
	3 2 1
	*/
	string s="abc";
	do
	{
		cout << s << " ";
	} while (next_permutation(s.begin(), s.end()));
	//abc acb bac bca cab cba
	cout << endl;

	//{1,2,3,4,5……,n}的第m个排列
	int num[7] = {1, 2, 3, 4, 5, 6, 7};
	int cnt = 0;
	do
	{
		if(cnt==1654)
		{
			for (int i = 0; i < 7; i++)
				cout << num[i];
		}
		cnt++;
	} while (next_permutation(num, num + 7));//3267514
	return 0;
}
```
