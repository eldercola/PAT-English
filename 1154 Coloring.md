# 记录第一道自己掐着时间AC的题目！
# 555 还是太菜了
原题链接指路: [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/1071785301894295552)  
啊题目呢就是要你先构造一个图，然后再给你一串颜色，让你判断这是不是一个合法的coloring(即图中一边上连接的两点颜色不能一样)  
如果是，输出 总共的颜色数量-coloring, 例如 4-coloring  
  
当然，第一种最简单粗暴的方法，就是for循环了!  
  
思路很简单，针对每一个节点，判断它相连的点是不是都颜色不同……(没错，就这……)  
然后呢，每次for循环的时候，都判断一下，这个点是不是有相邻的点……(这是一个坑，比如题目的例子中就有个点 没有边连着它)  
如果有相邻的点，再用for循环遍历一遍邻居，当然，中间要把每一个点的颜色存储到set里面，方便最后输出(直接用set.size()就可以获得颜色数量)  
什么? 你说你用不来set? 那也不是没办法, C++ 官方文档!  
  
废话不多说，上代码!  
```cpp
#include<bits/stdc++.h>
#include<vector>
#include<set>
using namespace std;

int N,M,K;
int visited[10000],color[10000];
vector<int> g[10000];
set<int> used;

bool dfs(int cur){
    if(used.find(color[cur])==used.end())used.insert(color[cur]);
    for(int v:g[cur]){
        if(color[v]==color[cur])return false;
    }
    return true;
}

int main(){
    cin>>N>>M;
    int start,end_;
    while(M--){
        scanf("%d %d\n",&start,&end_);
        g[start].emplace_back(end_);
        g[end_].emplace_back(start);
    }
    cin>>K;
    while(K--){
        used = set<int>();
        for(int i = 0;i<N;i++){
            visited[i]=0;
            cin>>color[i];
        }
        bool result;
        vector<int> emp=vector<int>();
        for(int i = 0;i<N;i++){
            if(!g[i].size())emp.emplace_back(i);
            result = dfs(i);
            if(result==false){
                cout<<"No\n";
                break;
            }
        }
        if(result){
            int colors = used.size();
            cout<<colors<<"-coloring\n";
        }
        else continue;
    }
    return 0;
}
```
