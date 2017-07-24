 [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=2795)

### <font color=blue>**题目意思**</font>
##### 有一块h*w的广告牌，现在要往上边贴广告，所贴广告的高度都为1，要求所贴的广告尽量往上和往左贴，如果可以贴，输出你所帖广告的行号，否则输出“-1”。

### <font color=blue>**解题思路**</font>

##### 这道题用线段树来解。我们以广告牌的高度作为标准来构造线段树，另外用一个数组来存储线段树每个节点的宽度。贴广告的时候我们从左子树往右子树来贴，这样就是从上往下来贴的。具体过程看代码吧！

### <font color=blue>**代码部分**</font>

```
#include <iostream>
#include <stdio.h>
#define lchild left,mid,root<<1
#define rchild mid+1,right,root<<1|1
using namespace std;

const int maxn=1e8;
int Max[maxn<<2];
int height,width,n;
///构建线段树
void build(int left,int right,int root)
{
    Max[root]=width;
    if(left==right)
        return;
    int mid=(left+right)>>1;
    build(lchild);
    build(rchild);
}
///更新线段树
void push_up(int root)
{
    Max[root]=max(Max[root<<1],Max[root<<1|1]);///找出左右子树最大的宽度赋给父节点
}
///查询并插入
int query(int w,int left,int right,int root)
{
    if(left==right)
    {
        Max[root]-=w;
        return left;///返回行号
    }
    int mid=(left+right)>>1;
    int ans=0;
    if(w<=Max[root<<1])///如果左子树中有大于w的边，就将其插入左子树中
        ans=query(w,lchild);
    else
        ans=query(w,rchild);
    push_up(root);
    return ans;
}
int main()
{
    while(~scanf("%d%d%d",&height,&width,&n))
    {
        if(n<height)
            height=n;
        build(1,height,1);
        int w;
        while(n--)
        {
            scanf("%d",&w);
            if(Max[1]<w)///如果根的最大长度没有w那么长，直接输出-1
                printf("-1\n");
            else
            {
                int ans=query(w,1,height,1);
                printf("%d\n",ans);
            }
        }
    }
    return 0;
}

```