### **[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6097)**

### <font color=blue>**题目意思**</font>
在圆内有两点P，Q，满足OP=OQ。现在要在圆上找一点D，使得DP+DQ最短。

### <font color=blue>**解题思路**</font>

我们可以分为两种情况来讨论：
1、OP = OQ = r 的情况。这是最短的距离应该就是PQ的连线距离了。
2、OP = OQ < 的情况。这时我们引进一个反演点的知识。

<font color=red>**反演点**</font>：已知圆O的半径为R，从圆心O出发任作一射线，在射线上任取两点M,N。若OM=m，ON=n，且mn=R^2，则称点M,N是关于圆O的反演点


![这里写图片描述](http://img.blog.csdn.net/20170814155306376?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc0MTIyMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


    OQ×OQ1 =r×r=|OD|²
    易知△ODQ∽△ODQ1   △ODP∽△ODP1
    故可以推出OP/OD=DP/DP1  OQ/OD=DQ/DQ1
    两式结合DP+DQ=OP/OD DP1+OQ/OD DQ1
    又∵OP =OQ
    ∴DP+DQ=OP/OD（DP1+DQ1）

然后求出DP1+DQ1的最小值即可
此时已转换成圆上找一点使得该点与圆外两点连线和最短
具体的过程看代码吧！

### 代码部分 ###

```
#include<cstdio>
#include<cstring>
#include<cmath>
#include<iostream>
using namespace std;
const double esp=0.000000005;
double dis(double x1,double y1,double x2,double y2)
{
    return sqrt((x1-x2) *(x1-x2) +(y1-y2) *(y1-y2));
}
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        double r,x[2],y[2],a,b;
        scanf("%lf%lf%lf%lf%lf",&r,&x[0],&y[0],&x[1],&y[1]);
        double op=dis(0.0,0.0,x[0],y[0]);
        if(x[0]==x[1]&&y[0]==y[1])
        {
            printf("%.7lf\n",2.0*(r-op));
            continue;
        }
        if(op==r)
        {
            printf("%.7lf\n",dis(x[0],y[0],x[1],y[1]));
            continue;
        }
        double o=acos((x[0]*x[1]+y[0]*y[1])/(op*op))/2.0;///OP与OD的夹角
        double d=r*r/op;///d表示的是圆心到反演点的距离
        double h=d*cos(o);///h表示原点到两反演点连线的距离

        if(h<=r)
        {
            printf("%.7lf\n",2.0*r*sin(o));
            continue;
        }
        b=h-r,a=d*sin(o);
        printf("%.7lf\n",op*sqrt(a*a+b*b)*2.0/r);
    }
    return 0;
}

```