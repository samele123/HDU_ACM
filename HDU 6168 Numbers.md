>**[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6168)**

### **题目意思**

给你两个序列A,B，序列B中的数是A中任意两个数的和。现在给出你A,B序列混在一起的数，让你找出A序列输出。

### **解题思路**

B序列中的数是A中任意两个数的和，那么给定的序列中最小的两个数一定是A序列中的，最大的两个数一定是B序列中的，现在就根据A中的两个数依次找，两数相加的和是B中的，就用map，每次将两数的和在map中除去，剩余的一个加入A中，再一次往后推即可。

### **代码部分**

```
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e4;
typedef long long ll;

ll a[maxn];
int main()
{
    ll num, m;
    while(~scanf("%lld", &m))
    {
        map <ll, ll> M;
        ll min1=1000000009,min2=1000000000;
        ll n=(sqrt(8*m+1)-1)/2;///b序列的长度

        for(ll i=0; i<m; i++)
        {
            scanf("%lld",&num);
            if(num<min2)
            {
                min1=min2;
                min2=num;
            }
            else if(num<min1)
            {
                min1=num;
            }
            M[num]++;///将数据全部存在map容器里
        }
        M[min1] --;
        M[min2] --;
        a[0] = min2;///找出最小的两个数
        a[1] = min1;
        ll ans = 1;
        while(ans != n - 1)
        {
            for(ll i = 0; i < ans; ++ i)
            {
                M[a[i] + a[ans]] --;
            }
            map<ll, ll>::iterator it = M.begin();
            while(it!= M.end())
            {
                if(it -> second == ll(0))
                {
                    M.erase(it ++);
                }
                else
                {
                    break;
                }
            }
            a[++ ans] = M.begin() -> first;
            M[a[ans]] --;
        }
        printf("%lld\n",n);
        printf("%lld",a[0]);
        for(int i=1; i<n; i++)
            printf("% lld",a[i]);
        printf("\n");
    }
    return 0;
}


```