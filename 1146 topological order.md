# Topological Order 判断题
输入: ```N 图的节点数, 节点编号范围 1~N```,```M 图的(有向)边数```,接下来的M行格式```point_start边的起始点 point_end边的终点```, 然后输入```K 表示有K个序列要判断```,接着K行输入```N个整型的序列，每两个数之间用空格隔开```  
输出: 非topological order的序列下标(下标范围从0到K-1),每两个数之间用空格隔开，末尾换行

逻辑如下:  
**双指针法**(~~投机取巧法~~ bushi): ```i```从序列末尾向序列开头遍历,子循环```j```从序列开头遍历到```i```,如果```i```到```j```存在有向边，那么这个序列不是topological order,因为topological order不存在```order[i]->order[j], when i>j```  
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int N,M,K;
vector<int> order[100],start,non_top;
int edges[1001][1001];

int main(){
    for(int i = 0;i<1001;i++)for(int j = 0;j<1001;j++)edges[j][i]=0;
    scanf("%d %d\n",&N,&M);
    while(M--){
        int e1,e2;
        scanf("%d %d\n",&e1,&e2);
        edges[e1][e2]=1;
    }
    scanf("%d\n",&K);
    int p = K;
    while(K--){
        int v;
        for(int i = 0;i<N;i++){
            scanf("%d",&v);
            getchar();
            order[p-K-1].emplace_back(v);
        }
        bool flag = true;
        for(int i = order[p-K-1].size()-1;i>=1;i--){
            for(int j = 0;j<i;j++){
                if(edges[order[p-K-1][i]][order[p-K-1][j]])flag = false;
                if(!flag)break;
            }
            if(!flag)break;
        }
        if(!flag)non_top.emplace_back(p-K-1);
    }
    for(int i = 0;i<non_top.size();i++){
        if(i)printf(" ");
        printf("%d",non_top[i]);
    }
    printf("\n");
    return 0;
}
```
