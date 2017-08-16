> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6129)

#### <font color=blue>**题目意思**</font>
给你一个包含n个数的序列A和一个数m，序列B中的数是序列A经过异或得到的，比如：b[i]=a[1]^a[2]^.....^a[i]。现在让你求经过m次异或后，序列B的值。

#### <font color=blue>**解题思路**</font>

我们写下其前五项的值可以发现我们设定ans【i】【j】表示进行到第i次，第j个位子的答案的话，ans【i】【j】有推导式：

```
ans【i】【j】=ans【i-1】【j】^ans【i】【j-1】；
```

![这里写图片描述](http://img.blog.csdn.net/20170816104503572?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc0MTIyMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

从图中我们可以看出对于每一项，他的系数就是杨辉三角的值，那么如果当前位子系数为奇数的话，结果就会有贡献。

对于杨辉三角，第x次变换第y项是C（x+y-2，y-1）;
C(n,m)，如果n&m==m则C（n,m）为奇数，考虑第一项对后面每一项的贡献是奇数还是偶数，依次类推后面的项数产生的贡献情况。

具体情况看代码吧！

#### <font color=blue>**代码部分**</font>

```
#include <iostream>
#include <string.h>
#include <stdio.h>
#include <algorithm>
using namespace std;
int a[2050000];
int b[2050000];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,m;
        scanf("%d%d",&n,&m);
        memset(b,0,sizeof(b));
        for(int i=1; i<=n; i++)
            scanf("%d",&a[i]);
        for(int i=1; i<=n; i++)
        {
            int y=i-1;
            int x=i+m-2;
            if((x&y)==y)///判断为奇数
            {
                for(int j=i; j<=n; j++)
                    b[j]^=a[j-i+1];
            }
        }
        for(int i=1; i<=n; i++)
        {
            if(i>1)
                printf(" ");
            printf("%d",b[i]);
        }
        printf("\n");
    }
}

```