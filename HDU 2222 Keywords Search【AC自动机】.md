> [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=2222%E3%80%91)

### **题目意思**

给你几个字符串，接着给你一个文本串，问你文本串中有几个前边所给的字符串。

### **解题思路**

这就是一道简单的AC自动机的模板题，并没有什么难点。

### **代码部分**

```
#include <iostream>
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

const int allson=26;
char patten[60];///模式串
char text[1000010];///文本串
int ans;
struct TrieNode
{
    struct TrieNode *son[allson];///儿子们
    struct TrieNode *fail;///匹配失败时的指针方向
    int num;
}*root;
///创建节点
TrieNode* createNode()
{
    TrieNode *p;
    p=(TrieNode*)malloc(sizeof(TrieNode));
    for(int i=0; i<allson; i++)
        p->son[i]=NULL;
    p->num=0;
    p->fail=NULL;
    return p;
}
///插入模式串，构建字典树
void insertPatten()
{
    TrieNode *p;
    p=root;
    int index=0;
    while(patten[index]!='\0')
    {
        int lowercase=patten[index]-'a';
        if(p->son[lowercase]==NULL)
        {
            p->son[lowercase]=createNode();
        }
        p=p->son[lowercase];
        index++;
    }
    p->num++;
}
///求fail指针
void build_AC_automaton()
{
    TrieNode *p;
    p=root;
    queue<TrieNode*>Q;
    Q.push(p);
    while(!Q.empty())
    {
        p=Q.front();
        Q.pop();
        for(int i=0; i<allson; i++)
        {
            if(p->son[i]!=NULL)
            {
                if(p==root)
                {
                    p->son[i]->fail=root;
                }
                else
                {
                    TrieNode *node=p->fail;
                    while(node!=NULL)
                    {
                        if(node->son[i]!=NULL)
                        {
                            p->son[i]->fail=node->son[i];
                            break;
                        }
                        node=node->fail;
                    }
                    if(node==NULL)
                        p->son[i]->fail=root;
                }
                Q.push(p->son[i]);
            }
        }
    }
}
void find_in_AC_automaton()
{
    TrieNode *p;
    p=root;
    int index=0;
    while(text[index]!='\0')
    {
        int lowercase=text[index]-'a';
        while(p->son[lowercase]==NULL&&p!=root)
            p=p->fail;
        p=p->son[lowercase];
        if(p==NULL)
            p=root;
        TrieNode *temp=p;
        while(temp!=NULL&&temp->num!=-1)
        {
            ans+=temp->num;
            temp->num=-1;
            temp=temp->fail;
        }
        index++;
    }
}
void freeNode(TrieNode *node)
{
    if(node!=NULL)
    {
        for(int i=0; i<allson; i++)
        {
            freeNode(node->son[i]);
        }
    }
    free(node);
}
int main()
{
    int t,n;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d",&n);
        root=createNode();
        for(int i=0; i<n; i++)
        {
            scanf("%s",patten);
            insertPatten();
        }
        scanf("%s",text);
        build_AC_automaton();
        ans=0;
        find_in_AC_automaton();
        printf("%d\n",ans);
        freeNode(root);
    }
    return 0;
}

```
