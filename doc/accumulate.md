
# C++ algorithm 中的 accumulate 函数的使用

## 1、基本数据类型的数据的累加求和

```C++
accumulate("起始地址","终止地址","迭加初始值")
```

## 2、自定义数据的求和,需要写一个函数作为accumulate的第四个参数

### 【例】

```C++

#include<iostream>
#include<numeric>
#include<algorithm>
#include<vector>
using namespace std;
struct student
{
	string name;
	double grade;
};
double get_totalgrade(double x,student s)
{
	return x + s.grade;
}
int main()
{
	int int_sum = 0;
	double double_sum = 0.0;
	int arr[5]={1,2,3,4,5};
	vector<double> vec;
	student stus[3] = {
		{"Jack", 96},
		{"Jone", 84},
		{"Luton", 94}};
	for (int i = 1; i < 10;i++)
	{
		vec.push_back(1.0);
	}
	int_sum = accumulate(arr, arr + 5, 0);
	cout << "int_sum " << int_sum << endl; // 15
	double_sum = accumulate(vec.begin(),vec.end(),0);
	cout << "double_sum " << double_sum <<endl;  //9
	double grade = accumulate(stus, stus + 3, 0, get_totalgrade);
	cout << "grade " << grade << endl;//274
	return 0;
}

```
