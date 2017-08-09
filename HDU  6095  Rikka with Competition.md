### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6095)

### <font color=blue>**题目意思**</font>
给你一组n个数，代表n个选手的能量高低，现在再给你一个k，任意在n个选手中挑取两个选手比赛，如果 |ai−aj|>K那么能量高的选手获胜，另一个将被淘汰，否则两个人都有机会获胜，现在要你求有多少人有获胜的可能。

### <font color=blue>**解题思路**</font>
如果把n个选手的能量值进行排序，那么两两相减，如果大于k，就结束一轮的循环，否则两人都有可能获胜，那么让ans++即可。具体过程看代码吧。

### <font color=blue>**代码部分**</font>

```
#include <iostream>
#include <cstdio>
#include <string.h>
#include <algorithm>
#define ll long long
using namespace std;
ll a[1000010];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        ll n,k;
        int ans=1;
        scanf("%lld%lld",&n,&k);
        for(int i=0; i<n; i++)
        {
            scanf("%lld",&a[i]);
        }
        sort(a,a+n);
        for(int i=n-1; i>0; i--)
        {
            //cout<<a[i]<<" ";
            if((a[i]-a[i-1])>k)
                break;
                ans++;
        }
        printf("%d\n",ans);
    }
    return 0;
}
```