# 咕咕咕 有空就写思路(~~如果我还记得的话~~(bushi
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int Cmax,N,Sp,Roads,bestSend=-1,bestTake=-1,bestTime=-1;
int bikes[501],dis[501][501],mindis[501];
vector<int> r[501];
vector<int> path,ans;

void dfs(int cur,int curTime,int cursend,int curtake){
    path.emplace_back(cur);
    if(curTime>mindis[cur]){//及时止损
        path.pop_back();
        return;
    }
    else if(cur == Sp){//终止条件
        //开始判别
        if(bestTime<0||bestTime>curTime){//第一条路径
            ans = path;
            bestTime=curTime;
            bestSend = cursend;
            bestTake = curtake;
        }
        else if(bestTime==curTime){//时间一样
            if(bestSend>cursend){//当前方案发送少
                ans = path;
                bestSend = cursend;
                bestTake = curtake;
            }
            else if(bestSend==cursend){//发送一样
                if(bestTake>curtake){//当前方案带回来少
                    ans = path;
                    bestTake = curtake;
                }
            }
        }
    }
    else{
        mindis[cur] = curTime;
        int compare_stand = Cmax/2;
        int temp_send = cursend;
        int temp_take = curtake;
        for(int next_s:r[cur]){
            if(mindis[next_s]!=100000000)continue;
            cursend = temp_send;
            curtake = temp_take;
            if(bikes[next_s]+curtake<compare_stand){
                cursend+=(compare_stand-bikes[next_s]-curtake);
                curtake=0;
            }
            else curtake = bikes[next_s]+curtake-compare_stand;
            //寻找下一个节点
            dfs(next_s,curTime+dis[cur][next_s],cursend,curtake);
        }
    }
    path.pop_back();
    //加上这一句是因为, 假设有A->B->C和A->C这两条路径, 先走了前面的路
    //如果不回到初始状态，假如后面的路步长比前面的小，是个更优解
    //但因为没回到初始状态，则不会被检验出来
    mindis[cur]=100000000;
}

int main(){
    //input
    cin>>Cmax>>N>>Sp>>Roads;
    for(int i = 1;i<=N;i++)cin>>bikes[i];
    while(Roads--){
        int a,b,temp_dis;
        scanf("%d %d %d\n",&a,&b,&temp_dis);
        dis[a][b]=dis[b][a]=temp_dis;
        r[a].emplace_back(b);
        r[b].emplace_back(a);
    }
    //find
    mindis[0]=0;
    for(int i = 1;i<=N;i++)mindis[i]=100000000;
    dfs(0,0,0,0);
    //output
    printf("%d ",bestSend);
    for(int i = 0;i<ans.size();i++){
        if(i>0)printf("->");
        printf("%d",ans[i]);
    }
    printf(" %d\n",bestTake);
    return 0;
}
```
