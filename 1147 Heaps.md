# 堆判断题+树输出题
题意很简单，给定一个二叉堆，判断是大堆还是小堆  
>大堆:父节点的value比子节点的value要大  
小堆:父节点的value比子节点的value要小  

判断完成之后，大堆输出```Max Heap```,小堆输出```Min Heap```,都不是则输出```Not Heap```  
然后紧接着给输入的树做一个**post order**(即 左-右-根)的输出  

思路很简单，先输入，再判断，最后输出  
输入：第一行输入```M```和```N```, 分别表示 M颗树以及每棵树N个节点  
判断：判断当前节点与其父节点之间的大小关系，然后把该关系传递出来  
输出：先遍历左子树,再遍历右子树，最后轮到自己  
  
我把判断和输出杂在一起，做递归。该函数思路如下:  
函数输入为 ```cur 当前节点序号``` 。  
逻辑如下：  
1. 如果当前节点有父节点,那么判断当前节点与其父节点的关系：  
```cpp
if(tree[cur]>tree[cur/2])smaller=true;
else if(tree[cur]<tree[cur/2])larger=true;//不能直接else,因为有相等的情况
```  
2. 如果当前节点有子节点,那么先遍历左子节点,再遍历右子节点,最后把自己放入输出队列:  
```cpp
post_order(cur*2);
post_order(cur*2+1);
ans.emplace_back(tree[cur]);
```  
全部代码如下:
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int N,M;
int tree[1001];
vector<int> ans;
bool larger=false,smaller=false;
//judge & push the order into vactor
void post_order(int cur){
    if(cur>=2&&cur<=N){
        if(tree[cur]<tree[cur/2])larger = true;
        else if(tree[cur]>tree[cur/2])smaller = true;
    }
    if(cur<=N){
        post_order(cur*2);
        post_order(cur*2+1);
        ans.emplace_back(tree[cur]);
    }
}

int main(){
    scanf("%d %d\n",&M,&N);//M<=100,1<=N<=1001
    while(M--){
        for(int i = 1;i<=N;i++)cin>>tree[i];
        //judge & print
        ans = vector<int>();
        larger=false;
        smaller=false;
        post_order(1);
        if(larger&&!smaller)printf("Max Heap\n");
        else if(!larger&&smaller)printf("Min Heap\n");
        else printf("Not Heap\n");
        for(int i = 0;i<ans.size();i++){
            if(i)printf(" ");
            printf("%d",ans[i]);
        }
        printf("\n");
    }
    return 0;
}
```
