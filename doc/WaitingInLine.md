
## PTA练习之 Waiting in Line

![first](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/WaitingInLine1.PNG)

![second](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/WaitingInLine2.PNG)

#### 【解题思路】

这题类似仿真的问题，模拟一个排队过程。题目大致意思是告诉你银行业务办理窗口数量，然后等待办理业务的人分为两拨，一波处在银行划定的黄区内，这个区域中每个窗口排队人数是规定了的，进入这个区域的人每次会在该区域的窗口中找一个队伍最短的排队，如果有多个最短的那就在序号较小的队排着。黄区满后，剩下的人在第二个区域等候，这个区域不需要过多处理，而且题目假设每个人是同时到达的。在五点后才轮到他来办理业务的则输出sorry表示已下班，不能办理。其他一些输入输出方式就不多赘述了。


这里我用的方式也是仿真的方式，时间每次一分钟递增。每个窗口都有一个队列que，每个人都有一个序号，从1-K，每一个人进入队列后，que中记录的信息是每个人的序号，这样也是为了方便得到每个人办理业务需要花的时间以及业务结束时间。任意一个时间点如果yello区间人数没满且还有顾客业务没有办理，则会有客户进入到yello区间，每次都找最短的队进去排队，如果该顾客进入的队原来没人，那么他进入后就成为第一个，需要记录一下他服务完成的时间。每一个时间点都需要检查每一队的第一个人的业务完成时间，如果该用户业务已完成则将其出队，并且记录下一个人的业务办理完时间。

#### 【测试样例】

```C++
input:
2 2 7 5
1 2 6 4 3 534 2
3 4 5 6 7

output:
08:07
08:06
08:10
17:00
Sorry
```

#### 【代码】

```C++
#include<iostream>
#include<queue>
#include<cstdio>
using namespace std;

int serveTime[1005];
int finishTime[1005];

queue<int> que[25];//一个窗口一条队列

int main()
{
	int window,person,cap,q;
	cin>>window>>cap>>person>>q;//窗口数 每个队能排的最多人数 待办理业务人数 待查询人数

	for(int i=1;i<=person;i++)
		cin>>serveTime[i];

	for(int i=0;i<window;i++)
	{
		if(que[i].empty()==false ) que[i].pop();
	}

	int count=1; //已入队的顾客数（在yellow区间以及办完了业务的顾客） ，也是顾客id
	int sum=0;  //在yellow区间中的顾客数

	for(int time=0;time<540;time++) //模拟时间
	{
		for(int i=0;i<window;i++)
		{
			for(int j=0;j<que[i].size();j++)
			{
				if(time == finishTime[que[i].front()])
				{
					que[i].pop();
					sum--;
					if(que[i].empty()==false)
					{
						finishTime[que[i].front()]=time+serveTime[que[i].front()];
					}
				}
			}
		}

		while(sum < cap*window && count <= person)
		{
			int min=0;
			for(int i=0;i<window;i++)
			{
				if(que[min].size()>que[i].size())
				{
					min=i; //找最短的队
				}
			}
			if(que[min].size() == 0 )
				finishTime[count]=time+serveTime[count];
			if(que[min].size()<cap && count<=person)
			{
				que[min].push(count);
				count++;
				sum++;  //yellow区间顾客数 +1
			}
		}

	}
	for(int i=0;i<q;i++)
	{
		int t;
		cin>>t;
		if(finishTime[t]==0) cout<<"Sorry"<<endl;
		else
		{
			int hour=8+ finishTime[t] / 60;
			int min = finishTime[t]%60;
			printf("%02d:%02d\n",hour,min);
		}
	}
	return 0;
}
```
