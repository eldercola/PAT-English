# 哈密尔顿回路，一个NP问题
------
>NP问题 指 在多项式时间里只能判断 是 或 否 的一类问题  
P问题 指 能在多项式时间里解决的问题  
  
<font color = red>当然, 我有点忘了, 有错的地方还请各位大佬指正</font>  
  
但其实，题目跟这个……没有什么很大的关系，你只需读懂题目，知道题目想让你判断一条路是不是汉密尔顿回路即可。  
当然，路径在本题分很多类，主要有如下三种:  
1. TS simple cycle, 标准汉密尔顿回路，一条路径从一个起始点开始并回到起始点，中间经过其他任何节点一次。换句话说，其他节点必须到过一次，起始的那个点必须到过两次。  
2. TS cycle, 维持一个cycle该有的样子，即起始节点和终点必须相同，中间必须访问到所有节点，不过次数不做限制，即次数大于等于1。
3. Not a TS cycle, 这个好理解，有没到达的节点，即 不为cycle  
废话不多说，上代码！  
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;
int dis[201][201];
int N,M,K;
int visit[201];
int current_smallest_index = -1,current_smallest_dis = 10000000;//记录最短路径的下标及其总距离
vector<vector<int>> all;
int main(){
    for(int i = 0;i<201;i++)for(int j = 0;j<201;j++)dis[i][j]=10000000;
    scanf("%d %d\n",&N,&M);
    while(M--){
        int start,end_,dis_;
        scanf("%d %d %d\n",&start,&end_,&dis_);
        dis[start][end_] = dis[end_][start] = dis_;
    }
    cin>>K;
    for(int k = 1;k<=K;k++){
        for(int i = 1;i<=N;i++)visit[i]=0;
        int total_size;
        scanf("%d ",&total_size);
        vector<int> temp = vector<int>();
        while(total_size--){
            int current_node;
            scanf("%d",&current_node);
            getchar();
            temp.emplace_back(current_node);
        }
        all.emplace_back(temp);
        int calc_dis = 0;
        bool has_dis = true;
        for(int i = 0;i<temp.size()-1;i++){
            if(dis[temp[i]][temp[i+1]]==10000000){
                printf("Path %d: NA (Not a TS cycle)\n",k);
                has_dis = false;
                break;
            }
            visit[temp[i]]++;
            calc_dis+=dis[temp[i]][temp[i+1]];
        }
        if(!has_dis)continue;
        else{
            int i = 1;
            bool not_fully = false,redundant = false;
            for(;i<=N;i++)if(visit[i]!=1){
                if(visit[i]==0)not_fully=true;
                if(visit[i]>1)redundant = true;
            }
            //Not a cycle
            if(not_fully||temp[0]!=temp[temp.size()-1])printf("Path %d: %d (Not a TS cycle)\n",k,calc_dis);
            else{
                if(redundant)printf("Path %d: %d (TS cycle)\n",k,calc_dis);//abnormal situation
                else printf("Path %d: %d (TS simple cycle)\n",k,calc_dis);//normal situation
                if(calc_dis<current_smallest_dis){
                    current_smallest_index = all.size();
                    current_smallest_dis = calc_dis;
                }
            }
        }
    }
    printf("Shortest Dist(%d) = %d\n",current_smallest_index,current_smallest_dis);
    return 0;
}
```
