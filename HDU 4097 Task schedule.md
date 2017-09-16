[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4907)

### 题目意思 ###

就是给你一个序列，再给你m个数字，让你找出序列中没有出现的且大于等于该数字的值。

### 解题思路 ###

这就相当于一道简单的dp题。我们从后边往前，将序列中出现的数进行标记，然后找到没有出现的数字，将没有出现的数字与给出的数字进行比较就好。

### 代码部分 ###
```
#include <iostream>
#include <stdio.h>
#include <algorithm>
#include <string.h>
#include <math.h>
using namespace std;
const int maxn=200005;
int main()
{
    int t,n,m,p,q;
    int flag[maxn],dp[maxn];
    scanf("%d",&t);
    while(t--)
    {
        memset(flag,0,sizeof(flag));
        scanf("%d%d",&n,&m);
        for(int i=1; i<=n; i++)
        {
            scanf("%d",&p);
            flag[p]=1;
        }
        int a;
        for(int i=maxn; i>=1; i--)
        {
            if(flag[i]!=1)
                a=i;
            dp[i]=a;
        }
        for(int i=1; i<=m; i++)
        {
            scanf("%d",&q);
            cout<<dp[q]<<endl;
        }
    }
    return 0;
}

```
