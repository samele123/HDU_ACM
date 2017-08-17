[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6127)

### **题目意思**

平面上有(n)个点，已知每个点的坐标为((x,y))，以及该点的权值为(val)，任意两点之间可以连接成一条不经过原点的边，该边的权值是两个端点的权值的乘积。现在要求画一条经过原点的直线，定义这条直线的权值为这条直线穿过的所有线段的值的和，问权值的最大值。

### **解题思路**

我们将所有的点按照极角排序，分别散落在y轴的左右两侧，y轴左端的点的和为suml，右端的点的和为sumr，则权值和为suml*sumr 。我们先以y轴为起始点，不断偏移，更新suml，sumr。以便刷新ans，找到最大的权值。因为两点不经过原点，所以只能偏移一个点。

### **代码部分**

```
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <algorithm>
typedef long long ll;
using namespace std;
#define eps 1e-9
#define maxn 200005
struct node
{
    double x,y,ang;///横纵坐标和极角
    int val;///点的权值
}p[maxn];
bool cmp(node a,node b)///极角从小到大排序
{
    return a.ang<b.ang;
}
int n,T;
ll suml,sumr,ans;
void work()
{
    scanf("%d",&n);
    for(int i=1; i<=n; i++)
    {
        scanf("%lf%lf%d",&p[i].x,&p[i].y,&p[i].val);
        p[i].ang=atan(p[i].y/p[i].x);///极角的计算
    }
    sort(p+1,p+n+1,cmp);
    suml=sumr=ans=0;
    for(int i=1; i<=n; i++)
    {
        if(p[i].x>eps)
            suml+=p[i].val;
        else
            sumr+=p[i].val;
    }
    ans=suml*sumr;
    for(int i=1; i<=n; i++)///以y轴不断偏移
    {
        if(p[i].x>0)
            suml-=p[i].val,sumr+=p[i].val;
        else
            suml+=p[i].val,sumr-=p[i].val;
        ans=max(ans,suml*sumr);
    }
    printf("%lld\n",ans);
}
int main()
{
    scanf("%d",&T);
    while(T--)
    {
        work();
    }
    return 0;
}

```