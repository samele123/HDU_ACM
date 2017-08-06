### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6050)

### <font color=blue>**题目意思**</font>

![](http://i.imgur.com/jpA1U3A.jpg)

根据上图中所给的规律，求给定的m,n的时候，F(m，1)的值为多少。在求解过程中的结果会比较大。所以求解过程要对`1e9+7`取余。


### <font color=blue>**解题思路**</font>

为了讲解方便，先将其打表：
```cpp
n=2                         
1   1   3   5   11  21  43   85
2   4   8   16  32  64  128  256
6   12  24  48  96  192 384  768
18  36  72  144 288 576 1152 2304
n=3                         
1   1   3   5    11   21    43    85
5   9   19  37   75   149   299   597
33  65  131 261  523  1045  2091  4181
229 457 915 1829 3659 7317  14635 29269
n=4                         
1    1    3    5     11    21    43     85
10   20   40   80    160   320   640    1280
150  300  600  1200  2400  4800  9600   19200
2250 4500 9000 18000 36000 72000 144000 288000
n=5                         
1      1       3       5       11      21       43       85
21     41      83      165     331     661      1323     2645
641    1281    2563    5125    10251   20501    41003    82005
19861  39721   79443   158885  317771  635541   1271083  2542165
615681 1231361 2462723 4925445 9850891 19701781 39403563 78807125
 
```
要求的是F（m,1），则肯定与上边的结果有关系，数据量过大，肯定是没有办法循环得到结果，由此看出，这道题肯定有规律。


当n为奇数个偶数的时候，求F（m,1）的规律是不一样的。

- **偶数：**打表可以看出当n为偶数时，从F（2,1）开始，第一列的下一个数是上一个数的k倍， 多打几组数据可以看出，k为`3,15,63,255……`k的规律就是2^n-1。由此看出，当n为偶数时，`F(m,1) = F(2,1)*(2^n-1)^(m-2)`。

- **奇数：**奇数的求解过程比较复杂。当n为奇数时，可以看出，F(m-1,1)*(2^n-1)与F（m,1）之间为一个常数k。我们可以进一步通过这种关系求解出F（m,1）的值。

**奇数求解过程：**通过打表可以看出，当n等于`3,5,7,9……`的时候，k的值为`2,10,42,170……`可以看出之间的规律：
```
2 = 2^1
10 = 2^1+2^3
42 = 2^1+2^3+2^5
170 = 2^1+2^3+2^5+2^7
……
//可以看出是等比为2^2的前k项和 
```
结合上边可以看出：`F(m-1,1)*(2^n-1)-F(m,1) = Sk`
    F(m,1) = F(m-1,1)*(2^n-1)-Sk;
又可以根据这个公式推出

![](http://i.imgur.com/3rUmkNm.png)

算到这里，就只剩下F（2,1）的求解问题了
先打表看看n:(1~n)第一行与F（2,1）的关系

```
S[i]: 1 1 3 5 11 21 43 85 171 341 683 1365 2731 5461 10923 21845 43691 87381 174763 349525
F[i]: 1 2 5 10 21 42 85 170 341 682 1365 2730 5461 10922 21845 43690 87381 174762 349525
 

```

F(i)表示当n为i时F(2,1)的值。
当i为奇数时`F[i]=s[i+1];`当i为偶数时`F[i]=s[i+1]-1;`
其中`s[i+1]=((-1)^i+2^(i+1))/3;`
接下来的求解过程就只需要注意边计算边取模就行。

注意：在对除数取余的时候为了防止精度的丢失，需要用到乘法的逆元，将除法变成乘法。这里使用的乘法逆元算法为费马小定理。

### <font color=blue>**费马小定理**</font>

在p是素数的情况下，对任意整数x都有`x^p≡x(mod)p。`
可以在p为素数的情况下求出一个数的逆元，`x∗x^(p−2)≡1(mod p)`，`x^(p−2)`即为逆元。

```
ll mult(ll a,ll n)  //求a^n%mod
{
    ll s=1;
    while(n)
    {
        if(n&1)s=s*a%mod;
        a=a*a%mod;
        n>>=1;
    }
    return s;
} //mult(a,n-2);
```

最后我们来理一下思路，其实就是一步一步往简单推公式的过程，如下图所示

![](http://i.imgur.com/Mt7KtWa.jpg)

### <font color=blue>**代码部分**</font>

```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
#include <vector>
#include<iostream>
using namespace std;
 
typedef long long LL;
const int mod = 1e9+7;
LL n,m;
LL mult(LL a,LL n)
{
    LL ans=1;
    while(n)
    {
        if(n&1)ans=(ans*a)%mod;
        a=(a*a)%mod;
        n>>=1;
    }
    return ans;
}
 
void solve()
{
    if(m==1)
    {
        cout<<"1"<<endl;
        return;
    }
    LL three=mult(3,mod-2);         // 3 的逆元 费马小定理 求乘法逆元
    LL one=((mult(2,n+1)+(n&1?-1:1)+mod)%mod*three)%mod;    // F[1][n+1]
    if(n%2==0)
        one=(one-1+mod)%mod;  // 计算 F[2][1]
    LL ans=0;
    LL mu=(mult(2,n)-1+mod)%mod;    // 2^n-1
    LL mun=mult(mu,m-2);            // (2^n-1)^(m-2)
    if(n%2==0)                      // 偶数时
        ans=(one*mun)%mod;
    else                            // 奇数时
    {
        LL cha=n/2;
        cha=((2*(mult(4,cha)-1+mod)%mod)%mod)*three%mod;
        LL add=((cha*(mun-1+mod)%mod)*mult((mu-1+mod)%mod,mod-2))%mod;
        ans=((one*mun)%mod-add+mod)%mod;
    }
    cout<<ans<<endl;
}
 
int main()
{
    ios::sync_with_stdio(false);
    int T;
    cin>>T;
    while(T--)
    {
        cin>>n>>m;
        solve();
    }
    return 0;
}
```