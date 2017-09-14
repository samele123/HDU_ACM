### [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=1754)

### 题目意思 ###
给你n个学生的成绩然后m个询问，Q时询问学生A到B的最高成绩，U时将A学生的成绩改为B

### 解题思路 ###
一道简单的线段树题，具体的看代码吧！

### 代码部分 ###
```
#include <iostream>
#include <stdio.h>
#include <string.h>
#define lchild left,mid,root<<1
#define rchild mid+1,right,root<<1|1
using namespace std;
const int maxn=200000;
int Max[maxn<<2];
///更新线段树节点
void push_up(int root)
{
    Max[root]=max(Max[root<<1],Max[root<<1|1]);
}
///构建线段树
void build(int left,int right,int root)
{
    if(left==right)
    {
        scanf("%d",&Max[root]);
        return;
    }
    int mid=(left+right)>>1;
    build(lchild);
    build(rchild);
    push_up(root);
}
///更改操作
void updata(int aim,int num,int left,int right,int root)
{
    if(left==right)
    {
        Max[root]=num;
        return;
    }
    int mid=(left+right)>>1;
    if(aim<=mid)
        updata(aim,num,lchild);
    else
        updata(aim,num,rchild);
    push_up(root);
}
///查询操作
int query(int L,int R,int left,int right,int root)
{
    if(L<=left&&right<=R)
        return Max[root];
    int mid=(left+right)>>1;
    int ans=0;
    if(L<=mid)
        ans=max(ans,query(L,R,lchild));
    if(R>mid)
        ans=max(ans,query(L,R,rchild));
    return ans;
}
int main()
{
    int n,m,n1,n2;
    string str;
    while(~scanf("%d%d",&n,&m))
    {
        build(1,n,1);
        while(m--)
        {
            cin>>str;
            if(str[0]=='Q')
            {
                scanf("%d%d",&n1,&n2);
                printf("%d\n",query(n1,n2,1,n,1));
            }
            else
            {
                scanf("%d%d",&n1,&n2);
                updata(n1,n2,1,n,1);
            }
        }
    }
    return 0;
}

```