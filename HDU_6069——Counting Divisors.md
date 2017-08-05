### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6069)

### <font color=blue>**题目意思**</font>
给你三个数L,R,K让你求满足下面公式的答案


![这里写图片描述](http://img.blog.csdn.net/20170805110101658?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc0MTIyMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### <font color=blue>**解题思路**</font>

由题意我们可以知道L，R最大为1e12，所以我们可以用筛法筛选sqrt(1e12)之内的所有素数。
有数论中的结论我们知道，任何一个正整数x，都可以分解成若干个素数幂的积。
则 x = (p1^m1)* (p2^m2)* (p3^m3)* .....* (pn^mn);    其中p1,p2,p3...pn都是素数，m1,m2,m3...mn都是幂指数。
则 x 的因子个数d(x) = (m1+1)* (m2+1)*(m3+1)....(mn+1);

那么对于x^k = ((p1^m1)* (p2^m2)* (p3^m3)* .....*(pn^mn))^k;
 则 x^k = (p1^m1* k) * (p2^m2* k) *  (p3^m3* k)* .....* (pn^mn*k);
则x^k的因子个数d(x^k) = (m1* k+1) * (m2* k+1) * (m3* k+1) * .....*(mn * k+1);    

然后这道题就是让算一个区间的K次方的因数个数之和，首先先将`1e6`之内的素数打表，然后再然后枚举这些素数，在枚举 [L,R] 区间中这些素数的倍数，然后根据这些倍数进行素因子分解，其实就是统计 [L,R] 区间中有多少个枚举的素数，然后乘以 K， 在加 1 计算即可。在枚举 [L,R] 区间值的时候，我们需要先把 [L,R] 区间中的数用数组保存下来，然后通过数组进行素因子分解。

### <font color=blue>**代码部分**</font>

```
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const LL MOD = 998244353;
const LL MAXN = 1e6+5;
int prime[MAXN], cnt;///cnt代表1e6中素数的个数
LL p[MAXN];///存放素数从0开始
///筛法求素数
void isprime()
{
    memset(prime, 0, sizeof(prime));
    cnt = 0;
    prime[1] = 1;
    for(LL i=2; i<MAXN; i++)
    {
        if(!prime[i])
        {
            p[cnt++] = i;
            for(LL j=i*i; j<MAXN; j+=i)
                prime[j] = 1;
        }
    }
}
 
LL f[MAXN], num[MAXN];///f[i]用来存储L~R之间的数,在不断的对因数相除。累积数量
int main()
{
    isprime();
    int T;
    scanf("%d", &T);
    while(T--)
    {
        LL L, R, K;
        scanf("%lld%lld%lld", &L, &R, &K);
        LL ans = 0;
        if(L == 1) 
            ans = 1, L++;
        int t = R-L;
        for(int i=0; i<=t; i++) 
            f[i] = i+L, num[i] = 1;///初始化
 
        for(int i=0; i<cnt&&p[i]*p[i]<=R; i++)
        {
            LL tmp = L;
            if(L % p[i]) tmp = (L/p[i]+1)*p[i];///找到第一个可以整除素数p[i]的数，不进行这步会超时
            //cout << "tmp = " << tmp << endl;
            for(LL j=tmp; j<=R; j+=p[i])///素因子分解
            {
                LL cnt = 0;
                while(f[j-L]%p[i] == 0)
                {
                    cnt++;
                    f[j-L] /= p[i];
                }
                num[j-L] = num[j-L]*(cnt*K+1)%MOD;
            }
        }
 
 
        for(int i=0; i<=t; i++)///计算结果
        {
            //cout << "num = " << num[i] << "   f = " << f[i] << endl;
            if(f[i] == 1) 
                ans = (ans + num[i]) % MOD;
            else
                ans = (ans + num[i]*(K + 1)) % MOD;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
 
 
```