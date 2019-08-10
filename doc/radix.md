
### PTA练习之Radix

![first](images/radix1.png)

![second](images/radix2.png)

题目我就直接截图不码字啦。


#####【解题思路】

题目大致的意思就是说给你四个数字输入，n1,n2,tag,radix。其中n1,n2分别是一个任意进制的正整数，如果tag为1 那么radix指的就是第一个数字n1的进制，为2的话那radix指的是n2的进制数，而你则需要找出n2/n1的进制数从而满足n1=n2，如果不存在满足条件的进制则输出“Impossible”。输入的数字中0-9代表数字0-9，a-z代表数字10-35.如果存在多个满足条件的进制数，则输出最小的那个。


这题乍看不难，但是它的测试点真的非常坑！题目没有明说数字是多长也就是说你不考虑长度基本是AC不了的，哈哈，这是PTA最喜欢的。所以这里n1，n2我都采用了字符串的方式读入。在有就是原来打算直接遍历2-35找符合条件的最小进制数，老是有几个测试点过不了。我最初的方向可能有问题，过于关注那个有多个满足条件的进制输出最小的条件，其实只有在只有一位，且两数相等的情况下才需要考虑这种情况。后来查了一下大牛们的思路后发现这个进制完全可能超过35！那么再用循环去查找显然已经不合适了（超时也是PTA的测试点！），于是采用了二分查找的方式来寻找进制数，终于过了……


#####【AC代码】

```C++
#include<iostream>
using namespace std;
#include<algorithm>
#include<cmath>
#include<cctype>

//将数字转换成十进制数
long long trans_ten(string s, long long radix)
{
	long long n=0;
	int len=s.length();
	reverse(s.begin(),s.end());
	for (int i = 0; i < len; i++)
	{
		if(isdigit(s[i]))
			n += pow(radix, i) * (s[i] - '0');
		else if(isalpha(s[i]))
			n += pow(radix, i) * (s[i] - 'a' + 10);
	}
	return n;
}

//二分法查找进制数
long long find_radix(long long n, string s, long long min_radix, long long max_radix)
{
	long long m = 0;
	long long low = min_radix, heigh = max_radix, mid;
	while(low<=heigh)
	{
		mid = (low + heigh) / 2;
		m = trans_ten(s, mid);
		if(m==n)
			return mid;
		//进制太大 这里m<0是由于进制太大越界造成的
		else if(m<0 || m>n)
		{
			heigh = mid - 1;
		}
		//进制太小
		else
			low = mid + 1;
	}
	return -1;
}

int main()
{
	string s1,s2;
	long long  tag,Radix;
	cin >> s1 >> s2 >> tag >> Radix;
	if(tag==2)
		swap(s1, s2);
	long long n1, min_radix=0,max_radix;

	//找s2中最小可能的进制数
	char ch=*max_element(s2.begin(),s2.end());
	if(isdigit(ch))
		min_radix = (ch - '0')+1;
	else if(isalpha(ch))
		min_radix = (ch - 'a') + 11;

	//将n1转换成10进制数
	n1 = trans_ten(s1, Radix);

	max_radix = max(n1, min_radix);
	long long my_radix = find_radix(n1, s2, min_radix, max_radix);
	if(my_radix!=-1)
		cout << my_radix;
	else
		cout << "Impossible";
	return 0;
}
```
