# 1139 First Contact
找出两个点之间长为4的路径。  
还剩两个测试点没通过，虽然不知道为什么!!!  
```cpp
#include<bits/stdc++.h>
#include<vector>
#include<unordered_map>
using namespace std;

struct person{
    vector<string> boys,girls;
};

int N,M,K;
unordered_map<string,person> all;
vector<vector<string>> ans;
vector<string> t_route;//暂存路径

string wash_num(string t){
    string s="";
    for(int i = 0;i<t.size();i++)if(t[i]!='-')s+=t[i];
    return s;
}

//对ans进行清洗，选每组答案的中间两个数，并转换成大于0的数
void wash(){
    for(int i = 0;i<ans.size();i++){
        vector<string> t_ans = vector<string>();
        for(int j = 1;j<3;j++)t_ans.emplace_back(wash_num(ans[i][j]));
        ans[i] = t_ans;
    }
}

//用于排序比较
bool comp(vector<string> a, vector<string> b){
    if(a[0]!=b[0])return a[0]<b[0];
    return a[1]<b[1];
}

void dfs(string cur, string target,int remain){
    t_route.emplace_back(cur);
    if(cur == target && remain == 0)ans.emplace_back(t_route);
    else if(cur!=target&&remain!=0){//没找到目标
        person tp = all[cur];
        if(remain==3){//找的是刚开始的同性别好友
            if(cur[0]=='-'){for(string next:tp.girls)if(next!=target)dfs(next,target,remain-1);}
            else for(string next:tp.boys)if(next!=target)dfs(next,target,remain-1);
        }
        else if(remain == 2){//如果目标跟当前异性，那就换性别查找
            if(target[0]=='-'){for(string next:tp.girls)if(next!=target)dfs(next,target,remain-1);}
            else for(string next:tp.boys)if(next!=target)dfs(next,target,remain-1);
        }
        else{//还有最后一步，直接找目标
            if(target[0]=='-'){for(string next:tp.girls)if(next==target)dfs(next,target,remain-1);}
            else for(string next:tp.boys)if(next==target)dfs(next,target,remain-1);
        }
    }
    t_route.pop_back();
}

int main(){
    scanf("%d %d\n",&N,&M);
    while(M--){
        string a,b;
        cin>>a>>b;
        auto it1 = all.find(a);
        auto it2 = all.find(b);
        if(it1==all.end()){
            person p;
            if(b[0]!='-')p.boys.emplace_back(b);
            else p.girls.emplace_back(b);
            all[a] = p;
        }
        else{
            if(b[0]!='-')all[a].boys.emplace_back(b);
            else all[a].girls.emplace_back(b);
        }
        if(it2==all.end()){
            person p;
            if(a[0]!='-')p.boys.emplace_back(a);
            else p.girls.emplace_back(a);
            all[b] = p;
        }
        else{
            if(a[0]!='-')all[b].boys.emplace_back(a);
            else all[b].girls.emplace_back(a);
        }
    }
    scanf("%d\n",&K);
    while(K--){
        ans = vector<vector<string>>();
        t_route = vector<string>();
        string a,b;
        cin>>a>>b;
        dfs(a,b,3);
        cout<<ans.size()<<endl;
        if(ans.size()){
            wash();
            sort(ans.begin(),ans.end(),comp);
            for(int i = 0;i<ans.size();i++)cout<<ans[i][0]<<" "<<ans[i][1]<<endl;
        }
    }
    return 0;
}
```
