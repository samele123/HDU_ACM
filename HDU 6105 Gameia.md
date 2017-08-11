### **[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6105)**

### <font color=blue>**题目意思**</font>

Bob和Alice玩游戏，要将一颗没有颜色的树进行涂色。Alice先走，图白色，Bob图黑色，并且可以将与它直接相连的点染成黑色，不管这点以前是空白的还是白色的。由于Bob是VIP玩家，可以随时在游戏中切断一条边。

### <font color=blue>**解题思路**</font>

只有当n为偶数且Bob可以根据他的特权将这棵树切成两两相连的时候，才可以获得胜利，否则都是Alice赢。

### <font color=blue>**代码部分**</font>

```
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e3;
int main()
{
    int t;
    cin >> t;
    while(t --)
    {
        int n, k, father[maxn], size[maxn];///size表示当前节点的子节点个数
        bool flag = true;
        cin >> n >> k;
        for(int i = 2; i <= n; ++ i)
            cin >> father[i];
        for(int i = 1; i <= n; ++ i)
            size[i] = 1;
        for(int i = n; i >= 1; -- i)
        {
            if(size[i] >= 3)
                flag = false;
            size[father[i]] += size[i] & 1;
        }
        if(flag && n % 2 == 0 && k >= n / 2 - 1)
            cout << "Bob" << endl;
        else
            cout << "Alice" << endl;
    }
    return 0;
}

```