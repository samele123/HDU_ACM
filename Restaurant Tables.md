[题目链接](https://vjudge.net/contest/172026#problem/A)

### <font color=green>题目意思</font>

该题意思是给你a张单人桌，b张双人桌，接待客人，如果客人是一个人，就做单人桌，如果没有单人桌就坐双人桌，如果也没双人桌，就坐双人桌坐了一个人的地方，如果还是没有，则拒绝接待。同样如果来的是两个人，就坐双人座，如果没有，就拒绝接待。最后求没接待的人数。

### <font color=green>解题思路</font>

来的人可以分为单人和双人，如果是单人就判断是否有单人桌，如果没有就判断双人桌，这样下次来单人的时候就判断有没有只坐了一个人的双人桌就好。如果来的是双人，就判断有没有双人桌就行。用总人数减去接待的人数就是所求的人数。

### <font color=green>代码部分</font>

```
#include <iostream>
#include <cstdio>
#include <cstring>
#include <string.h>
#include <algorithm>
#include <stack>
typedef long long LL;
using namespace std;
LL num[222222];
int main()
{
    LL n,a,b;
    while(cin>>n>>a>>b)
    {
       LL sum=0;///sum定义的是总人数
        for(int i=0; i<n; i++)
        {
            scanf("%lld",&num[i]);
            sum+=num[i];
        }
        LL s=0;///s记录接待的人数
        int f=0;///f记录双人桌坐了一个人的数目
        for(int i=0; i<n; i++)
        {
            if(num[i]==1)
            {
                if(a!=0)
                {
                    a--;
                    s++;
                }
                else if(b!=0)
                {
                    b--;
                    s++;
                    f++;
                }
                else if(f)
                {
                    s++;
                    f--;
                }
            }
            else if(num[i]==2&&b!=0)
            {
                b--;
                s+=2;
            }
        }
        cout<<(sum-s)<<endl;
    }
    return 0;
}
```
