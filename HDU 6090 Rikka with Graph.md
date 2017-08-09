### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6090)

### <font color=blue>**题目意思**</font>

给你一个包含n个点m条边的无向图，现在要求这个无向图的最小权值。无向图的权值等于每对点经过的边的条数，如果两点之间没有路径，那么权值就等于n。

### <font color=blue>**解题思路**</font>

这是一道思路题，我们很容易找到规律，可以分成三种情况。包含n个顶点的无向图最多有n*(n-1)/2条边。当m大于n*(n-1)/2时，是一种情况，那时我们直接让m等于n*(n-1)/2；此外，当m小于(n-1)时，就会有孤立的点，这是就有点之间没有路径，这是另一种情况，还有一种就是m刚好大于等于（n-1），这是有一种情况。具体的看代码吧！

### <font color=blue>**代码部分**</font>

```
#include <iostream>
#include <cstdio>
#include <string.h>
#include <algorithm>
#define ll long long
using namespace std;
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        ll n,m,ans=0;
        scanf("%lld%lld",&n,&m);
        if(m>n*(n-1)/2)///如果m大于无向图的最大边数值
            m=n*(n-1)/2;
        if(m<(n-1))///如果有孤立的点
        {
            ans=(n-m-1)*(n+m)*n+(m*m*2);
        }
        else
        {
             ans=(n-1)*(n-1)*2-(m-n+1)*2;
        }
        printf("%lld\n",ans);
    }
    return 0;
}

```