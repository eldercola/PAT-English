# 两次搜索最短路径
## 方法一 DFS  
然而运行超时(最后一个测试点,3分)  
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

struct road{
    int distance;
    int timecost;
    vector<int> p;
};

int N,M,Q1,Q2;
int dis[501][501], t[501][501], min_dis[501], min_t[501];
vector<road> dis_ans,t_ans;

vector<int> dis_t,t_t;

int total_dis(vector<int> a){
    int sum = 0;
    for(int i = 0;i<a.size()-1;i++)sum += dis[a[i]][a[i+1]];
    return sum;
}

int total_t(vector<int> a){
    int sum = 0;
    for(int i = 0;i<a.size()-1;i++)sum += t[a[i]][a[i+1]];
    return sum;
}

void dfs_dis(int cur, int target, int cur_dis){
    if(cur_dis>min_dis[cur])return;//及时止损
    else min_dis[cur] = cur_dis;
    dis_t.push_back(cur);
    if(cur == target){
        road ro;
        ro.distance = total_dis(dis_t);
        ro.timecost = total_t(dis_t);
        ro.p = dis_t;
        dis_ans.push_back(ro);
        dis_t.pop_back();
        return;
    }
    for(int i = 0;i<N;i++){
        if(dis[cur][i]!=0)dfs_dis(i,target,cur_dis+dis[cur][i]);
    }
    dis_t.pop_back();
}

void dfs_t(int cur, int target, int cur_t){
    if(cur_t>min_t[cur])return;//及时止损
    else min_t[cur] = cur_t;
    t_t.push_back(cur);
    if(cur == target){
        road ro = {total_dis(t_t),total_t(t_t),t_t};
        t_ans.push_back(ro);
        t_t.pop_back();
        return;
    }
    for(int i = 0;i<N;i++){
        if(t[cur][i]!=0)dfs_t(i,target,cur_t + t[cur][i]);
    }
    t_t.pop_back();
}

bool comp_dis(road a, road b){
    if(a.distance!=b.distance)return a.distance<b.distance;
    return a.timecost<b.timecost;
}

bool comp_t(road a, road b){
    if(a.timecost!=b.timecost)return a.timecost<b.timecost;
    return a.p.size() < b.p.size();
}

bool sameV(road a, road b){
    if(a.p.size()!=b.p.size())return false;
    for(int i = 0;i<a.p.size();i++)if(a.p[i]!=b.p[i])return false;
    return true;
}

int main(){
    scanf("%d %d",&N,&M);
    for(int i = 0;i<N;i++)min_dis[i]=min_t[i]=1000000;
    while(M--){
        int e1,e2,one,d,tt;
        scanf("%d %d %d %d %d",&e1,&e2,&one,&d,&tt);
        dis[e1][e2] = d;
        t[e1][e2] = tt;
        if(one==0){
            dis[e2][e1] = dis[e1][e2];
            t[e2][e1] = t[e1][e2];
        }
    }
    scanf("%d %d",&Q1,&Q2);
    dfs_dis(Q1,Q2,0);
    dfs_t(Q1,Q2,0);
    sort(dis_ans.begin(),dis_ans.end(),comp_dis);
    sort(t_ans.begin(),t_ans.end(),comp_t);
    if(sameV(dis_ans[0],t_ans[0])==true){
        printf("Distance = %d; Time = %d: ",dis_ans[0].distance, t_ans[0].timecost);
        for(int i = 0;i<dis_ans[0].p.size();i++){
            if(i)printf(" -> ");
            printf("%d",dis_ans[0].p[i]);
        }
        printf("\n");
    }
    else{
        printf("Distance = %d: ",dis_ans[0].distance);
        for(int i = 0;i<dis_ans[0].p.size();i++){
            if(i)printf(" -> ");
            printf("%d",dis_ans[0].p[i]);
        }
        printf("\n");
        printf("Time = %d: ",t_ans[0].timecost);
        for(int i = 0;i<t_ans[0].p.size();i++){
            if(i)printf(" -> ");
            printf("%d",t_ans[0].p[i]);
        }
        printf("\n");
    }
    return 0;
}
```
------
## 方法二 Dijkstra  
明天写, 累了。  
