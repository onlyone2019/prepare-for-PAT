## Digital Library

### 题意
输入规格：
每个输入文件包含一个测试用例。对于每种情况，第一行都包含一个正整数N(≤10^4)，这是书籍的总数。然后N个块紧随其后，每个块包含6行的书信息：
第1行：7位ID号；
第2行：书名-不超过80个字符的字符串；
第三行：作者-不超过80个字符的字符串；
第4行：关键字-每个单词是一个不超过10个字符的字符串，不带任何空格，关键字之间仅用一个空格隔开；
第5行：发布者-不超过80个字符的字符串；
第6行：发布的年份-4位数字，范围为[1000，3000]。
假定每本书仅属于一个作者，并且包含的​​关键词不超过5个；总共不超过1000个不同的关键字；并且不超过1000个不同的发布者。图书信息之后，有一行包含正整数 M(≤1000)，这是用户的搜索查询的数量。然后紧随其后的M行如下所示之一：
1：书名
2：作者姓名
3：一个关键词
4：发布者名称
5：代表年份的4位数字
输出规格：
对于每个查询，首先将原始查询打印在一行中，然后以升序输出结果书ID，每个书ID占据一行。如果找不到书，请打印Not Found。


**这题其实就是简单的数据存储的问题，难点在于大数据量怎么实现快速查找，一个提高运行效率的问题，用两个map映射，即节省空间有能快速定位待查内容，话不多说，看代码。**

### 输入样例
```C++
3
1111111
The Testing Book
Yue Chen
test code debug sort keywords
ZUCS Print
2011
3333333
Another Testing Book
Yue Chen
test code sort keywords
ZUCS Print2
2012
2222222
The Testing Book
CYLL
keywords debug book
ZUCS Print2
2011
6
1: The Testing Book
2: Yue Chen
3: keywords
4: ZUCS Print
5: 2011
3: blablabla
```

### 输出样例
```C++
1: The Testing Book
1111111
2222222
2: Yue Chen
1111111
3333333
3: keywords
1111111
2222222
3333333
4: ZUCS Print
1111111
5: 2011
1111111
2222222
3: blablabla
Not Found
```

### 代码
```C++
#include<iostream>
#include<set>
#include<map>
#include<cstdio>
using namespace std;
map<string, set<int> > title, author, key, pub, year;
void query(map<string, set<int> > &m, string &str)
{
    if(m.find(str) != m.end())
    {
        for(set<int>::iterator it = m[str].begin(); it != m[str].end(); it++)
            printf("%07d\n", *it);
    }
    else
        cout << "Not Found\n";
}
int main()
{
    int n;
    string this_title,this_author,this_publisher,this_year,this_key;
    char ch;
    cin>>n;
    int id;
    for(int i=0;i<n;i++)
    {
        cin>>id;
        getchar();
        getline(cin,this_title);
        title[this_title].insert(id);
        getline(cin,this_author);
        author[this_author].insert(id);
        while(cin>>this_key)
        {
            key[this_key].insert(id);
            ch=getchar();
            if(ch=='\n') break;
        }
        getline(cin,this_publisher);
        pub[this_publisher].insert(id);
        getline(cin,this_year);
        year[this_year].insert(id);
    }
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int tmp;
        string s;
        scanf("%d: ",&tmp);
        getline(cin,s);
        cout<<tmp<<": "<<s<<endl;
        switch(tmp)
        {
        case 1:
            query(title,s);
            break;
        case 2:
            query(author,s);
            break;
        case 3:
            query(key,s);
            break;
        case 4:
            query(pub,s);
            break;
        case 5:
            query(year,s);
            break;
        }
    }
    return 0;
}
```
