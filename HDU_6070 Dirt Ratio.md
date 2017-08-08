### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6070)

### <font color=blue>**题目意思**</font>

给出n个数，求任意区间【left，right】的AC率中最小的那个值。
区间AC率=distinct【left，right】/(right-left+1)
distinct的中文意思是唯一的，特殊的，表示区间【left，right】中不同数字的个数

### <font color=blue>**解题思路**</font>

我们求AC率，它无非就是0~1之间的一个数字，因此采用二分答案的方法求解。
现在假设答案为mid，则 `distinct【left，right】/（right-left+1）<=mid`，说明答案是小于mid的。
上边的式子可以进行变形得：

    distinct【left，right】<=mid*(right-left+1)


    distinct【left,right】+mid*left<=mid*(right+1)

因此确定一个mid值，我们构建一棵空的线段树，每个节点的值先赋值为`mid*left`，其中left为当前节点的左边界，对于线段树的每个节点我们存放的是：`distinct【left，right】+mid*left`，对于这些节点我们维护的是节点所代表区间的最小值，因为固定右边界的时候，右侧`mid*（right+1）`是固定的，我们需要让左侧尽量小。

 那么需要更新的就是`distinct【left，right】`区间中的值。我们可以通过遍历右边界，数组中的每个数都可以作为右边界，对于新加入线段树的a[i]。

pre[a[i]]记录前一次a[i]这个数字出现的位置，因此我们知道在这次遇见a[i]之前，pre[a[i]]+1~i，这些位置都没有出现过a[i]这个数字，所以在线段树中区间pre[a[i]]~i，应该加上1，就是a[i]这个数字。

更新完后，我们就可以查询【1，i】这个区间，原来我比较不理解这里，后来想明白了，1到i 这个区间求解的时候，这个区间包含许多子区间，例如【2，i】,【3，i】，【4，i】~【i,i,】。这样的话每次插入第i个数，相当于从这些区间中求了一个distinct【left,right】+mid*left 的最小值。

在循环过程中，只要有一个值小于mid*(right+1),就说明我们假设的值偏大了，循环完了也没出现这种情况
说明假设的值偏小了。

### <font color=blue>**代码部分**</font>

```cpp
#include <iostream>
#include <cstdio>
#include <string.h>
#include <algorithm>
using namespace std;
#define eps 1e-6
#define inf 0x3f3f3f3f
#define lchild left,mid,root<<1
#define rchild mid+1,right,root<<1|1

const int maxn=60010;
int a[maxn];
double Min[maxn<<2];///线段树节点值
double lazy[maxn<<2];///懒惰标记数组
int pre[maxn];///pre[i]用来存放i这个数上一次出现的时候在数组中的位置
///更新当前节点
void push_up(int root)
{
    Min[root]=min(Min[root<<1],Min[root<<1|1]);
}
///懒惰标记下推
void push_down(int root)
{
    if(lazy[root]>eps)
    {
        Min[root<<1]+=lazy[root];
        lazy[root<<1]+=lazy[root];
        Min[root<<1|1]+=lazy[root];
        lazy[root<<1|1]+=lazy[root];
        lazy[root]=0;
    }
}
///构建线段树
void build(int left,int right,int root,double temp)
{
    Min[root]=left*temp;
    lazy[root]=0;
    if(left==right)
        return;
    int mid=(left+right)>>1;
    build(lchild,temp);
    build(rchild,temp);
    push_up(root);
}
///区间更新，同时加上add
void updata(int L,int R,int add,int left,int right,int root)
{
    if(L<=left&&right<=R)
    {
      Min[root]+=add;
      lazy[root]+=add;
      return;
    }
    push_down(root);
    int mid=(left+right)>>1;
    if(L<=mid)
        updata(L,R,add,lchild);
    if(R>mid)
        updata(L,R,add,rchild);
    push_up(root);
}
///区间查找操作
double query(int L,int R,int left,int right,int root)
{
    if(L<=left&&right<=R)
        return Min[root];
    push_down(root);
    double ans=inf*1.0;
    int mid=(left+right)>>1;
    if(L<=mid)
        ans=min(ans,query(L,R,lchild));
    if(R>mid)
        ans=min(ans,query(L,R,rchild));
    return ans;
}
///判断二分答案偏大还是偏小
bool check(int n,double temp)
{
    build(1,n,1,temp);
    memset(pre,0,sizeof(pre));
    for(int i=1; i<=n; i++)
    {
        updata(pre[a[i]]+1,i,1,1,n,1);
        double minimum=query(1,i,1,n,1);
        pre[a[i]]=i;
        if(temp*(i+1)-minimum>=eps)
            return true;
    }
    return false;
}
int main()
{
    int t,n;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d",&n);
        for(int i=1; i<=n; i++)
        {
            scanf("%d",&a[i]);
        }
        double L,R,mid,ans;
        L=0;
        R=1;
        for(int i=0; i<30; i++)
        {
            mid=(L+R)/2.0;
            if(check(n,mid))
                R=ans=mid;
            else
                L=ans=mid;
        }
        printf("%.10lf\n",ans);
    }
    return 0;
}

```
