
# C++ algorithm 中的 sort 排序函数的使用

**sort()** 所使用的排序方法类似于快速排序的方法，同时也结合了其他排序方法，时间复杂度为 n*log2(n)，执行效率非常快，在考试或者竞赛过程中是一个非常有帮助的工具函数。sort()是非稳定排序，稳定排序可以用stable_sort()。
**【稳定排序】** ：通俗讲就是两个相等的元素在排序前和排序后的相对位置不变，比如排序前第一个3在第二个3前面，那么排序后也应该是第一个3在第二个3前面。
**稳定**：冒泡排序、插入排序、归并排序、基数排序
**不稳定**：选择排序、快速排序、希尔排序、堆排序

## 1、默认方式排序

```C++
sort(begin , end);
```

默认以非降序方式排序

## 2、自定义方式排序

```C++
sort(begin , end , cmp);

bool cmp1(int a,int b)
{
	return a > b; //非升序
}

struct
{
	bool operator()(int a,int b)
	{
		return a > b; //非升序
	}
}cmp2;
```

## 3、使用函数greater与less来指明排序方式

```C++
sort(begin, end, greater<int>());//非升序
sort(begin, end, less<int>());//非降序
```

#### 【例】

```C++
#include<iostream>
using namespace std;
#include<vector>
#include<algorithm>

bool cmp1(int a,int b)
{
	return a > b; //大到小排序
}

struct
{
	bool operator()(int a,int b)
	{
		return a > b; //大到小排序
	}
}cmp2;

int main()
{
	int a[10]={2,5,3,8,12,15,12,8,9,10};
	cout << "begin sort: " << endl;
	for (int i = 0; i < 10;i++)
		cout<<a[i]<<" ";
	cout << endl;
	sort(a,a+10);
	cout << "after sort:" << endl;
	for (int i = 0; i < 10;i++)
		cout<<a[i]<<" ";
	//2 3 5 8 8 9 10 12 12 15
	cout << endl <<endl;

	vector<int> vec;
	vec.push_back(3);
	vec.push_back(9);
	vec.push_back(2);
	vec.push_back(6);
	vector<int>::iterator it;
	cout << "begin sort: " << endl;
	for (it = vec.begin(); it != vec.end(); it++)
		cout << *it <<" ";
	//3 9 2 6
	cout << endl;
	sort(vec.begin(), vec.end());
	cout << "after sort:" << endl;
	for (it = vec.begin(); it != vec.end(); it++)
		cout << *it <<" ";
	//2 3 6 9
	cout << endl <<endl;

	vector<int> cmp1_vec;
	cmp1_vec.push_back(7);cmp1_vec.push_back(9);
	cmp1_vec.push_back(2);cmp1_vec.push_back(4);
	cmp1_vec.push_back(6);cmp1_vec.push_back(3);
	cout << "begin sort: " << endl;
	for (it = cmp1_vec.begin(); it != cmp1_vec.end(); it++)
		cout << *it <<" ";
	//7 9 2 4 6 3
	cout << endl;
	sort(cmp1_vec.begin(), cmp1_vec.end(), cmp1);
	cout << "after sort:" << endl;
	for (it = cmp1_vec.begin(); it != cmp1_vec.end(); it++)
		cout << *it <<" ";
	//9 7 6 4 3 2
	cout << endl << endl;

	vector<int> cmp2_vec;
	cmp2_vec.push_back(7);cmp2_vec.push_back(9);
	cmp2_vec.push_back(2);cmp2_vec.push_back(4);
	cmp2_vec.push_back(6);cmp2_vec.push_back(3);
	cout << "begin sort: " << endl;
	for (it = cmp2_vec.begin(); it != cmp2_vec.end(); it++)
		cout << *it <<" ";
	//7 9 2 4 6 3
	cout << endl;
	sort(cmp2_vec.begin(), cmp2_vec.end(), cmp2);
	cout << "after sort:" << endl;
	for (it = cmp2_vec.begin(); it != cmp2_vec.end(); it++)
		cout << *it <<" ";
	//9 7 6 4 3 2
	cout << endl << endl;

	int arr[5] = {7, 9, 5, 6, 8};
	sort(arr, arr + 5, greater<int>());
	cout<<"after sort:" << endl;
	for (int i = 0; i < 5; i++)
		cout << arr[i] << " ";
	//9 8 7 6 5
	cout << endl;
	sort(arr,arr+5,less<int>());
	cout<<"after sort:" << endl;
	for (int i = 0; i < 5; i++)
		cout << arr[i] << " ";
	//5 6 7 8 9
	cout << endl;
	return 0;
}
```
