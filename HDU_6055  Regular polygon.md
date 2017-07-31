### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6055)
 
 **题目意思**
给你n个点，接着给你这n个点的坐标，求这些点能组成多少个正多边形。

**解题思路**
因为都是整数点，所以可以知道这些点只能组成正四边形。所以我们将这些点按照横纵坐标排序，然后枚举两个点，暴力找另外两个点的位置是否存在就行。

**代码部分**

```
#include <bits/stdc++.h>
using namespace std;
const int maxn=510;
struct Node///将横坐标和纵坐标按照从小到大的顺序排序
{
    int x,y;
    bool operator<(const Node& rhs)const
    {
        if(x == rhs.x)
            return y < rhs.y;
        else
            return x < rhs.x;
    }
} nodes[maxn];

int main()
{
    int n;
    while(~scanf("%d",&n))
    {
        int ans=0;///正方形个数
        for(int i=0; i<n; ++i)
            scanf("%d%d",&nodes[i].x,&nodes[i].y);
        sort(nodes,nodes+n);
        for(int i=0; i<n; ++i)
            for(int j=i+1; j<n; ++j)
            {
                ///看一下以i，j两个点作为正方形得一条边能不能找到其余的两个顶点
                Node tmp;//tmp作为正方形的第3或4个点
                tmp.x=nodes[i].x+nodes[i].y-nodes[j].y;
                
                tmp.y=nodes[i].y+nodes[j].x-nodes[i].x;
                
                if(!binary_search(nodes,nodes+n,tmp)) 
                    continue;
                tmp.x=nodes[j].x+nodes[i].y-nodes[j].y;
                
                tmp.y=nodes[j].y+nodes[j].x-nodes[i].x;
                
                if(!binary_search(nodes,nodes+n,tmp))
                 continue;
                ///只有两个点都找到才能够构成一个正方形
                ++ans;
            }
        printf("%d\n",ans/2);///注意这里/2，原因是因为一个正方形可以通过最最左下角得那个点向上和向右分别找到同一个正方形两次
    }
    return 0;
}
```

 
