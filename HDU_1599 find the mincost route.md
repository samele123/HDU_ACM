[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=1599)

#### <font color=blue>**题目意思**</font>
 给你n个景区，求经过最小三个景区形成的环的最小花费。

#### <font color=blue>**解题思路**</font>
 这是一道求最短路的题，用Floyd算法，Floyd本身只是比较暴力的dp求各点之前的最短路径。但是，在Floyd更新的过程中也是有顺序的。可以借助这一点来解决这个题目。
 首先先说一下如何来确定环的花费最小。开始先将ans赋值为无穷大。然后我们可以先选取两个点，两点之间的最短路径为<font color=red>dis[i][j]</font>,再找一个大于i，j的一个点k，做松弛操作;进行下面的操作<font color=red>ans=min(ans,dis[i][j] + mp[i][k] + mp[k][j])</font>;来不断更新环的最小花费。
当然Floyd的操作在松弛操作之后，可以保证i到j之间的最短距离不经过k。这样也可以保证经过的最小花费的路径为一个环。

#### <font color=blue>**代码部分**</font>

```
#include <iostream>
#include <stdio.h>
#define maxn 110
#define INF 1000000
using namespace std;
int n,m;
int mp[maxn][maxn];///邻接矩阵用来存储
int dis[maxn][maxn];///用于存储两点之间的最短路径
void initmp()///对邻接矩阵的初始化
{
    for(int i=1; i<=n; i++)
    {
        for(int j=1; j<=n; j++)
        {
            if(i==j)
                mp[i][j]=mp[j][i]=0;
            else
                mp[i][j]=mp[j][i]=INF;
        }
    }
}
void Floyd()
{
    for(int i=1; i<=n; i++)
    {
        for(int j=1; j<=n; j++)
        {
            dis[i][j]=mp[i][j];
        }
    }

    int ans=INF;
    for(int k=1; k<=n; k++)
    {
        for(int i=1; i<k; i++)
        {
            for(int j=i+1; j<k; j++)
                ans=min(ans,dis[i][j]+mp[i][k]+mp[k][j]);///对求环最小花费的松弛
        }
        for(int i=1; i<=n; i++)///普通的Floyd算法
        {
            for(int j=1; j<=n; j++)
            {
                if(dis[i][j]>dis[i][k]+dis[k][j])
                    dis[i][j]=dis[i][k]+dis[k][j];
            }
        }
    }
    if(ans==INF)
        cout<<"It's impossible."<<endl;
    else
        cout<<ans<<endl;
}
int main()
{
    int a,b,c;
     while(cin>>n>>m)
    {
        initmp();
        for(int i = 1; i <= m; i++)
        {
            cin >> a >> b >> c;
            if(mp[a][b] > c)/// 在给定的边可能会有重复，需要求最小的花费，所以需要更新重边
            {
                mp[a][b] = mp[b][a] = c;
            }
        }
        Floyd();
    }

    return 0;
}

```