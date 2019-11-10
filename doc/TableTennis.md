## Table Tennis

### 题意
一个乒乓球俱乐部向公众提供N张桌子。这些表的编号从1到N。对于任何一对玩家，如果到达时有一些表打开，它们将分配给编号最小的可用表。如果所有表都被占用，它们将不得不在队列中等待。假设每对玩家最多可以玩2个小时。
您的工作是计算排队等待时间的每个人，并为每张桌子计算当天服务的球员数量。
使此过程有些复杂的一件事是俱乐部为VIP会员保留了一些桌子。当VIP表打开时，队列中的第一对VIP将具有使用它的特权。但是，如果队列中没有VIP，则下一对玩家可以使用它。另一方面，如果轮到VIP对，但没有VIP表可用，则可以将其分配为任何普通玩家。

输入规格：
每个输入文件包含一个测试用例。对于每种情况，第一行都包含一个整数N（≤ 10000） -对选手的总数。然后是几N行，每行包含2次和VIP标签：HH:MM:SS-到达时间，P-一对玩家的播放时间（以分钟为单位），以及tag-如果他们持有VIP卡，则为1，否则为0。俱乐部开放时，保证抵达时间在08:00:00和21:00:00之间。假设没有两个客户同时到达。以下球员的信息，有2个正整数：K（≤ 100） -表的数量，和M（< K）-VIP表的数量。最后一行包含M表号。
输出规格：
对于每个测试用例，首先以示例所示的格式打印每对玩家的到达时间，服务时间和等待时间。然后在一行中打印每张桌子所服务的玩家数量。请注意，必须按服务时间的时间顺序列出输出。等待时间必须四舍五入为整数分钟。如果不能在截止时间之前得到一张桌子，则不得打印其信息。

### 输入样例
```C++
9
20:52:00 10 0
08:00:00 20 0
08:02:00 30 0
20:51:00 10 0
08:10:00 5 0
08:12:00 10 1
20:50:00 10 0
08:01:30 15 1
20:53:00 10 1
3 1
2
```

### 输出样例
```C++
08:00:00 08:00:00 0
08:01:30 08:01:30 0
08:02:00 08:02:00 0
08:12:00 08:16:30 5
08:10:00 08:20:00 10
20:50:00 20:50:00 0
20:51:00 20:51:00 0
20:52:00 20:52:00 0
3 3 2
```

### 代码
```C++
#include<iostream>
using namespace std;
#include<cstdio>
#include<vector>
#include<algorithm>
#include<cmath>

struct person
{
    int arrive;//到达时间
    int servetime;//使用时间
    int start;//开始时间
    bool vip;//是否是会员
}tmpperson;

struct tablenode
{
    int endtime=8*3600;//结束时间
    int cnt;//服务人数
    int vip=false;//是否是会员桌
}tmptable;

vector<struct person> player;
vector<struct tablenode> table;

bool cmp1(struct person a,struct person b)
{
    return a.arrive<b.arrive;
}

bool cmp2(struct person a,struct person b)
{
    return a.start<b.start;
}

void allocatetable(int i,int index)
{
    if(table[index].endtime>=player[i].arrive) player[i].start=table[index].endtime;
    else player[i].start=player[i].arrive;
    table[index].endtime=player[i].start+player[i].servetime;
    table[index].cnt++;
}

int findnextvip(int x)
{
    x++;
    while(x<(int)player.size()&&player[x].vip==false)
        x++;
    return x;
}

int main()
{
    int n,k,t;
    int h,m,s,flag,cost;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        scanf("%d:%d:%d %d %d",&h,&m,&s,&cost,&flag);
        tmpperson.arrive=h*3600+m*60+s;
        tmpperson.start=21*3600;
        if(tmpperson.arrive>=21*3600) continue;
        tmpperson.servetime=cost*60;
        if(tmpperson.servetime>7200) tmpperson.servetime=7200;
        if(flag==1) tmpperson.vip=true;
        else tmpperson.vip=false;
        player.push_back(tmpperson);
    }
    cin>>m>>k;
    table.resize(m+1);
    for(int i=1;i<=k;i++)
    {
        scanf("%d",&t);
        table[t].vip=true;
    }
    sort(player.begin(),player.end(),cmp1);
    int i=0;
    int vipid=-1;
    vipid=findnextvip(vipid);
    while(i<(int)player.size())
    {
        int index=-1,minendtime=99999999;
        for(int j=1;j<=m;j++)
        {
            if(table[j].endtime<minendtime)
            {
                index=j;
                minendtime=table[j].endtime;
            }
        }
        if(table[index].endtime>=21*3600) break;
        if(player[i].vip==true && i<vipid)
        {
            i++;
            continue;
        }
        if(table[index].vip==true)
        {
            if(player[i].vip==true)
            {
                allocatetable(i,index);
                if(i==vipid)vipid=findnextvip(vipid);
                i++;
            }
            else
            {
                if(vipid<(int)player.size() && player[vipid].arrive<=table[index].endtime)
                {
                    allocatetable(vipid,index);
                    vipid=findnextvip(vipid);
                }
                else
                {
                    allocatetable(i,index);
                    i++;
                }
            }
        }
        else
        {
            if(player[i].vip==false)
            {
                allocatetable(i,index);
                i++;
            }
            else
            {
                int vipindex=-1;
                minendtime=99999999;
                for(int j=1;j<=m ;j++)
                {
                    if(table[j].endtime<minendtime && table[j].vip==true)
                    {
                        vipindex=j;
                        minendtime=table[j].endtime;
                    }
                }
                if(vipindex!=-1 && table[vipindex].endtime<=player[i].arrive)
                {
                    allocatetable(i,vipindex);
                    if(i==vipid) vipid=findnextvip(vipid);
                    i++;
                }
                else
                {
                    allocatetable(i,index);
                    if(i==vipid) vipid=findnextvip(vipid);
                    i++;
                }
            }
        }
    }
    sort(player.begin(),player.end(),cmp2);
    for(int i=0;i<(int)player.size() && player[i].start<21*3600;i++)
    {
        printf("%02d:%02d:%02d ",player[i].arrive/3600,(player[i].arrive%3600)/60,player[i].arrive%60);
        printf("%02d:%02d:%02d ",player[i].start/3600,(player[i].start%3600)/60,player[i].start%60);
        printf("%.0f\n",round((player[i].start-player[i].arrive)/60.0));
    }
    for(int i=1;i<=m;i++)
    {
        if(i==1) printf("%d",table[i].cnt);
        else printf(" %d",table[i].cnt);
    }
    return 0;
}
```
