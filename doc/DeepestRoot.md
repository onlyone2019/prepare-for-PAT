## PTA练习之 Deepest Root

![first](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/DeepestRoot1.PNG)

![second](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/DeepestRoot2.PNG)

### 【解题思路】
判断图是否为连通图，若不连通则输出连通子图个数，否则找出可以使图（树）深度最大的根节点，若答案不唯一按升序输出。判断连通子图直接用并查集很好解决。找目标根节点我最初思路是对每一个节点都检查一次，查找最深的节点并记录。但是提交发现第三个测试点运行超时，还以为是使用递归的原因，但是改写为非递归算法还是超时。偷瞄了一下大神的思路发现，只需检查两次就够。第一次随便找一个节点对其检查，找到距离它最远的叶子节点记录下来，不管这之间路径有多长这些叶子节点必定是答案的一部分。第二次检查从第一次检查的结果中随机找一个节点作为根便可找出所有满足条件的结果，该结果就是第一次与第二次检查结果的并集。如下图，第一次随机去节点1为根节点，找到离它最远的节点5（记录），第二次用节点5作为根节点再深度优先遍历便可找到节点3,4，最终找到结果。

![third](https://github.com/onlyone2019/prepare-for-PAT/blob/master/doc/images/DeepestRoot3.PNG)

### 【测试样例】
```C++
输入：
5
1 2
1 3
1 4
2 5
输出：
3
4
5

输入：
5
1 3
1 4
2 5
3 4
输出：
Error: 2 components

注意读题：题目明确给出只给出n-1条边，也就是说如果是连通图则必定是树，递归时不需要考虑环。
```

### 【代码】
```C++
#include<iostream>
#include<set>
#include<vector>
#include<algorithm>
using namespace std;
int father[10005];
int n;
int mmax=0;
set<int> result;//存放每次查找到的结果
set<int> ans;  //存放最终结果
vector<int> a[10005]; //邻接表
//初始化
void init()
{
    for(int i=0;i<10005;i++) father[i]=i;
}

//查找父亲
int findf(int x)
{
    int r=x;
    while(father[r]!=r) r=father[r];//找父亲
    int tmp;
    while(father[x]!=r)//路径压缩
    {
        tmp=father[x];
        father[x]=r;
        x=tmp;
    }
    return r;
}

//连接到一起
void join(int x,int y)
{
    int fx=findf(x),fy=findf(y);
    if(fx!=fy) father[fx]=fy;
}

void getHeight(int from,int len,int pre)
{
    if(len>mmax) //有更深的图 则清空原来存的较短的结果 存新的结果
    {
        mmax=len;
        result.clear();
        result.insert(from+1);
    }
    else if(len==mmax)//答案有多个
    {
        result.insert(from+1);
    }
    for(int j=0;j<(int)a[from].size();j++)
    {
        if(a[from][j]==pre) //防止在两邻接点之间无限递归
            continue;
        getHeight(a[from][j],len+1,from);
    }
}

int main()
{
    init();
    int cnt=0;
    cin>>n;
    int from,to;
    for(int i=0;i<n-1;i++)
    {
        cin>>from>>to;
        join(from,to);//添加到同一集合
        a[from-1].push_back(to-1);//计入邻接表
        a[to-1].push_back(from-1);
    }
    for(int i=0;i<n;i++)
    {
        if(findf(i+1)==i+1) cnt++; //检查集合（连通子图）个数
    }
    if(cnt>1) cout<<"Error: "<<cnt<<" components"<<endl;
    else
    {
        getHeight(0,0,-1);//第一次遍历
        ans=result;//记录第一次的结果
        set<int>::iterator iter;
        getHeight(*ans.begin()-1,0,-1);//第二次遍历
        for(iter = result.begin() ; iter != result.end() ; ++iter)
        {
            ans.insert(*iter); //求交集 set会自动去除重复元素并升序排列
        }
        for(iter = ans.begin() ; iter != ans.end() ; ++iter)
        {
            cout<<(*iter)<<endl;//输出结果
        }
    }
    return 0;
}
```
