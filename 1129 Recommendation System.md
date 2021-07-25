# Recent Use, 跟操作系统的页面置换算法有点像
但是!运行超时，很奇怪  
```cpp
#include<bits/stdc++.h>
#include<set>
using namespace std;

int N,K;
int order[50001];//记录出现的顺序
int t[50001]={0};//记录使用频率

bool comp(int a, int b){//根据使用频率排序
    if(t[a]!=t[b])return t[a]>t[b];
    return a<b;
}

int main(){
    scanf("%d%d",&N,&K);
    for(int i = 1;i<=N;i++)scanf("%d",&order[i]);
    vector<int> ans = vector<int>();
    for(int i = 1;i<=N;i++){
        if(i>1){
            if(t[order[i-1]]==1)ans.emplace_back(order[i-1]);
            sort(ans.begin(),ans.end(),comp);
            printf("%d: ",order[i]);
            for(int k = 0;k<K;k++){
                if(ans[k]==0)break;
                if(k)printf(" ");
                printf("%d",ans[k]);
            }
            printf("\n");
        }
        t[order[i]]++;
    }
    return 0;
}
```
