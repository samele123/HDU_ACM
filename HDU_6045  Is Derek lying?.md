### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6045)

### <font color=blue>**题目意思** </font>
 给你总共有几道题，和两个人的分数，接下来给你两个人各自的答案，每道题一分，让你判断第一个人有没有说谎。

### <font color=blue>**解题思路** </font>
一道题一分，那么两个人的分数总和减去总题数就是两个人至少做的相同的题的数目，当然同时两个人不同的题数目至少也得是两人分数的差值。

### <font color=blue>**代码部分** </font>

```
#include<cstdio>
#include<algorithm>
#include<string.h>
#include <iostream>
#include <math.h>
using namespace std;
char a[800005],b[800005]; 
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n ,x,y,s,l;
        s=0;l=0;///s表示两人对应答案相同的个数，l表示两个人对应答案不同的个数
        scanf("%d %d %d",&n,&x,&y);
        scanf("%s",a);
        scanf("%s",b);
        for(int i=0;i<n;i++)
        {
            if(a[i]==b[i])
                s++;
            else l++;
        }
        if (s>=x+y-n&&l>=abs(x-y))
        {
            printf("Not lying\n");
        }
        else printf("Lying\n");
    }
}

```
