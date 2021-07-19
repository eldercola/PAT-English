# 一道根据序列生成二叉搜索树以及寻找两个节点共同祖先的题

[题目链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805343727501312).  
1151 题也是一道非常类似的题，只不过这道题相比1151，更加简单。 
------
题意是给定M个查询对，N个数字组成的序列。根据N个数字组成的序列生成一个BST(二叉搜索树)，再对M个查询对进行查找，搜索离它们最近的共同祖先。  
那具体的输出原题都有，这里就不说了  
思路如下:  
输入M,N->输入N个数字的序列->生成树->1->输入查询对->查找查询对的共同祖先->输出->如果未执行M次,回到1;否则下一步->结束, return 0;  
  
如何构造树?  
> 根据BST的特性,对于根节点A, 如果有另一个节点B的value大于A的value, 那么另外一个节点B就在A的右子树。  
  
根据这个特性, 我们对N个数字的序列进行拆分, 首先序列头就是根节点的值。接着，将剩余的序列拆分为 根节点的左子树、根结点的右子树。划分依据就是上面提到的这个性质。  
然后，我们要做的，就是不断对左子树和右子树进行递归，直到某节点的左边和右边中只有一个节点(也就是不可再分)为止。这样就完成了一颗BST的构造。  
  
如何查询共同祖先?  
在说明这个问题之前，不妨先介绍如何查询一个节点是否在BST中。  
> 根据要查找的value不断与当前根结点比较，大于找右边，小于找左边，等于就是根节点。如果某个节点已经没有子树了也没找到该value，那就报错。
  
查询共同祖先的道理也类似。  
在1151中, 我利用了查找每个点的搜寻路径上最末尾的共同点这个方法找共同祖先。这里介绍另一种方法。  
对于两个查询点，设置当前根节点，查看它们的值是否 “不” 被分在同一边，如果不被分在同一边，那么这个点就是最近的共同祖先。  
> 为什么是 “不” 被分在同一边?
> 因为有可能，要查询的点，一个是另一个的祖先！这种情况下也是要直接返回的。
  
源代码如下:  
```cpp
#include<bits/stdc++.h>
using namespace std;
int N, M;
int order[10005];
struct node{
    int value;
    node* left = nullptr;
    node* right = nullptr;
};

node* make_tree(int start, int end){
    node* p = new node();
    p->value = order[start];
    if(start!=end){
        int i = start+1;
        for(;i<=end;i++){
            if(order[i]>=order[start])break;
        }
        if(i == start+1)p->right = make_tree(start+1,end);
        else if(i == end+1)p->left = make_tree(start+1,end);
        else{
            p->right = make_tree(i,end);
            p->left = make_tree(start+1,i-1);
        }
    }
    return p;
}

bool find_node(int num){
    for(int i = 1;i<=N;i++)if(order[i]==num)return true;
    return false;
}

int find_ancestor(node* current_root, int v1, int v2){
    if((current_root->value-v1)*(current_root->value-v2) <= 0)return current_root->value;//异侧或有一个是根结点
    else{//同侧
        if(current_root->value>v1)return find_ancestor(current_root->left, v1, v2);
        else return find_ancestor(current_root->right,v1,v2);
    }
}

int main(){
    scanf("%d %d\n",&M,&N);
    for(int i = 1;i<=N;i++)cin>>order[i];
    node* head = new node();
    head = make_tree(1,N);
    while(M--){
        int num_1,num_2;
        scanf("%d %d\n",&num_1,&num_2);
        bool find_1 = find_node(num_1);
        bool find_2 = find_node(num_2);
        if(find_1 == false && find_2 == false)printf("ERROR: %d and %d are not found.\n", num_1, num_2);
        else if(find_1 == false)printf("ERROR: %d is not found.\n", num_1);
        else if(find_2 == false)printf("ERROR: %d is not found.\n", num_2);
        else{
            int ancestor = find_ancestor(head, num_1, num_2);
            //如果num_2是祖先,为了方便输出，跟num_1换个位置
            if(ancestor == num_2){
                int t = num_1;
                num_1 = num_2;
                num_2 = t;
            }
            if(ancestor == num_1)printf("%d is an ancestor of %d.\n", num_1, num_2);
            else printf("LCA of %d and %d is %d.\n",num_1,num_2,ancestor);
        }
    }
    return 0;
}
```
