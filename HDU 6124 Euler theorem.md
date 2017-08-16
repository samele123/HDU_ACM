>### **[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6124)**

#### <font color=blue>**题目意思**</font>
有两个数a，b，计算a mod b。现在忘记了b，只知道a，问你可能的结果有多少种。

#### <font color=blue>**解题思路**</font>
这就是一道规律题，我们观察可以发现我们取模的结果要不是正好除尽为0，或者比他大余它本身，还有就是余下的不同个数，但是你会发现不管是什么数，它的结果总是自身的一半加上一或者二。因为奇数的时候要加上它自身的一半那个数和它自身那个数。偶数的时候直接加上它自身就够了。

#### <font color=blue>**代码部分**</font>

```
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <algorithm>
using namespace std;
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        int n;
        scanf("%d",&n);
        printf("%d\n",((n+1)/2)+1);
    }
}

```