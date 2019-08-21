
### PTA练习之 Maximum Subsequence Sum

![first](images/WorldCupBetting1.png)

![second](images/WorldCupBetting2.png)

#####【解题思路】

这个题来说个人认为拿满分还是有一定难度，可能用暴力三重循环是比较容易想到的思路，但是拿不到满分，在最后一个测试点会出现运行超时，也就是算法复杂度太高。我原来就是用暴力法写的，但是一路纠正后，最后一个测试点的运行超时还是解决不了。最后参考高人的思路后，了解到联机算法，在任意时刻，算法对要操作的数据只读入（扫描）一次，一旦被读入并处理，它就不需要在被记忆了。而在此处理过程中算法能对它已经读入的数据立即给出相应子序列问题的正确答案。这种算法很巧妙，记得曾经好像见过，但是并没有重视过。这种算法复杂度为O(n)，是最优的算法，相比三重或者两重循环速度快很多，这也体现了算法的魅力。

#####【低级代码】

```C++
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int sum = 0, i, j, start, end, max=0, n;
	cin>>n;
	int *a;
	a = new int[n];
	for (i = 0; i < n;i++)
	{
		cin>>a[i];
		max += a[i];
	}
	j = n - 1;
    start=0;
    end=n-1;
	for (i = 0; i < n - 1;i++)
	{
		j = n - 1;
		while (i <= j)
		{
			sum = 0;
			for (int k = i; k <= j; k++)
			{
				sum += a[k];
			}
			if (sum>max)
			{
				start=i;
				end = j;
				max = sum;
			}
			else if(sum==max)
			{
				if(i<start || j<end)
				{
					start = i;
					end = j;
				}
			}
			j--;
		}
	}
    if(max>=0)
	    cout<<max<<" "<<a[start]<<" "<<a[end];
    else cout<<"0 "<<a[0]<<" "<<a[n-1];
	return 0;
}
```

#####【联机算法】

```C++
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<vector>
using namespace std;
int main()
{
	int n;
	cin>>n;
	int *a;
	a = new int[n];
	for (int i = 0; i < n; i++)
	{
		cin >> a[i];
	}
	int start = 0, end = 0,nowSum=0,maxSum=-1,temp_start=0;
	for(int i=0;i<n;i++)
	{
		nowSum +=a[i];
		if(nowSum<0)
		{
			nowSum = 0;
			temp_start = i + 1;
		}
		else if(nowSum>maxSum)
		{
			maxSum = nowSum;
			start = temp_start;
			end = i;
		}
	}
	if(maxSum<0)
		cout << "0 " << a[0] << " " << a[n - 1];
	else
		cout << maxSum << " " << a[start] << " " << a[end];
	return 0;
}
```
