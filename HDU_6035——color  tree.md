【题目链接】[http://acm.hdu.edu.cn/showproblem.php?pid=6035](http://acm.hdu.edu.cn/showproblem.php?pid=6035 "color  tree")

### <font color=blue>**题目意思**</font>
给你一个包含n个节点的树，用一个数字代表一种颜色，树上的路径权值为在这条路径上包含的颜色数量，这棵树总共有n*(n-1)/2条路径。求这棵树上所有路径的总权值。


### <font color=blue>**解题思路**</font> 
我们要考虑每条路上有多少颜色经过，这样求所有路径上的权值和，但是这种方法不好实现。所以我们反过来思考，我们假设每条路径上都包含了所有的颜色，那么总的路径权值和就是用颜色数*路径数，那么我们现在只要找到每种颜色没有经过那条路径，再用总数减去每种颜色没有经过的路径和就能求出这棵树中所有路径的权值和。

### <font color=blue>**代码部分**</font>

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 4e5+100;
typedef long long ll;
int c[N],vis[N];///c[i]表示第i个点的颜色，vis[i]表示的是i这个颜色有没有出现过
vector<int> e[N];///存储这个图呢
ll sum[N],size[N];///sum[i]表示的是这个颜色i所能管理的点，size[i]表示的是以i为根的点的个数
ll ans;
void dfs(int x,int y)
{

    size[x]=1;/// 指的是x为根的树的点的个数
    sum[c[x]]++;/// sum是指 当前颜色的所能管理的点（包括自身的总个数） 那么每种颜色 最后都是一个接近根的值
    ///到不了根的部分 就要在后面剔除
    ll pre=sum[c[x]];/// 指从根到 x ，c[x] 管辖的点的总个数
   // printf("x====%d,y===%d,,pre==%d\n\n",x,y,pre);
    for(int i=0; i<e[x].size(); i++)
    {
        if(e[x][i]==y) continue;
        dfs(e[x][i],x);
        size[x]+=size[e[x][i]];/// 以x为根的树的点的总个数，当前的这个点还要加上他的子数上的点
        //printf("x====%d,sum[c[x]]===%d,,pre==%d\n\n",x,sum[c[x]],pre);
        ll count=size[e[x][i]]-(sum[c[x]]-pre);/// 总的减去被管理的部分 剩下的都是自由的联通块 直接求路径后面剔除
        ans=ans+(1LL*count*(count-1))/2;
        sum[c[x]]+=count;/// 加上这些被剔除的，因为这些被剔除被合并到当前点被管理了
        pre=sum[c[x]];///更新这个颜色的所有的管理点
        //printf("后来sum[c[x]]===%d,,pre==%d\n\n",sum[c[x]],pre);
    }
}

int main()
{
    int n,cas=1;
    while(scanf("%d",&n)!=EOF)
    {
        int num=0;
        ans=0;
        memset(sum,0,sizeof(sum));
        memset(vis,0,sizeof(vis));
        for(int i=1; i<=n; i++)
        {
            e[i].clear();
            scanf("%d",&c[i]);
            if(!vis[c[i]])
            {
                vis[c[i]]=1;///标记这个颜色有没有出现过
                num++;///不同的颜色数
            }
        }
        for(int i=1; i<n; i++)
        {
            int u,v;
            scanf("%d%d",&u,&v);
            e[u].push_back(v);
            e[v].push_back(u);
        }
        dfs(1,0);
        ll ANS = 1LL*num*((1LL)*n*(n-1))/2;
        /// 因为根只有一种颜色 这里要剔除所有到不了根的颜色
        for(int i=1; i<=n; i++)
        {
            if(vis[i])
            {
                ll ct=n-sum[i];
                ans+=ct*(ct-1)/2;
            }
        }
        printf("Case #%d: %lld\n", cas++, ANS-ans);
    }
}
```