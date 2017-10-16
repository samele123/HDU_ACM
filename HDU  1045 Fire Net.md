> #### **[题目链接](http://acm.split.hdu.edu.cn/showproblem.php?pid=1045)**

#### <font color=blue>**题目意思**</font>
给你一个n*n的矩阵，让你在空地放枪支，其中要求“.”代表空地，“X”代表墙。要求枪支不能出现在同一行，同一列，除非中间有墙阻隔。现在问你最多能放多少枪支。

#### <font color=blue>**解题思路**</font>

这道题就是一个普通的深搜问题，和之前做过的N皇后问题很相似。我们要从开始往后走，走到一个位置的时候往这一行，这一列的前边搜索是否有枪，如果有枪就不能再放，如果遇到墙就可以放置。

#### <font color=blue>**代码部分**</font> 

```
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <algorithm>
#include <queue>
using namespace std;
char map[9][9];
int n,ans;
bool judge(int x,int y)
{
    bool flag=true;
    for(int i=x; i>=0; i--)
    {
        if(map[i][y]=='#')
            flag=false;
        else
        {
            if(map[i][y]=='X')
                break;
            else
                continue;
        }
    }
    for(int i=y; i>=0; i--)
    {
        if(map[x][i]=='#')
            flag=false;
        else
        {
            if(map[x][i]=='X')
                break;
            else
                continue;
        }
    }
    return flag;
}
void dfs(int idx,int t)///idx表示当前位置，t表示放枪的数量
{
    ans=max(t,ans);
    if(idx>n*n)
        return;
    int x=idx/n;///当前位置的横坐标
    int y=idx%n;///当前位置的纵坐标
    int temp,tmp;///temp表示下一个位置，tmp表示枪支数目
    if(map[x][y]=='.'&&judge(x,y))///如果是空地并且能够放枪
    {
        map[x][y]='#';///将放枪的地方标记
        temp=idx+1;
        tmp=t+1;
        dfs(temp,tmp);
        map[x][y]='.';
        temp=idx+1;
        tmp=t;
        dfs(temp,tmp);
    }
    else
    {
        temp=idx+1;
        tmp=t;
        dfs(temp,tmp);
    }
}
int main()
{
    while(~scanf("%d",&n),n)
    {
        ans=-1;
        for(int i=0; i<n; i++)
        {
            for(int j=0; j<n; j++)
            {
                scanf(" %c",&map[i][j]);
            }
        }
        dfs(0,0);
        cout<<ans<<endl;
    }
}

```

