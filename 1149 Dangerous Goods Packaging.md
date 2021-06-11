# 含危险品的打包题
其实这题吧, 个人认为主要考察几个数据结构(C++ STL): map, set, vector  
首先看题目给的测试用例, [题目链接](https://pintia.cn/problem-sets/994805342720868352/problems/1038429908921778176)  
```c++
6 3
20001 20002
20003 20004
20005 20006 //这里有20005
20003 20001
20005 20004 //这里也有20005
20004 20006
4 00001 20004 00002 20003
5 98823 20002 20003 20006 10010
3 12345 67890 23333
```  
我最初的想法是，把危险品和危险品的```pair```直接用```<int, int>```来表示，但是看到这组用例里面就有两个20005  
于是我把```pair```设计为```<int, vector<int>>```  
大致的逻辑如下:  
```c++
//step 1. 获取输入
// 把每个危险品都放进set<int> s, 然后再去更新map<int, vector<int>> m。一次输入的危险品有俩，所以互相要加入对方的危险品列表
//step 2. 判断输出
// 针对每个物品，先检查其是否在 本仓库不能出现的物品集合 set<int> incom中, 不在则进行下面的判断, 如果在, 这个仓库就是不合理的
// 再检查其是否在危险品集合s中, 如果在, 把它的危险品列表中每个元素都加入 incom 中
// 输出 Yes 或 No
```  
代码如下:  
```c++
#include<bits/stdc++.h>
#include<map>
#include<vector>
#include<set>
using namespace std;
int N,M,K;
vector<int> G;
set<int> s;
map<int, vector<int>> m;
int main(){
    scanf("%d %d\n",&N,&M);
    while(N--){
        int temp_1,temp_2;
        scanf("%d %d\n",&temp_1,&temp_2);
        if(s.find(temp_1)==s.end()){
            s.insert(temp_1);
            vector<int> v = vector<int>();
            v.emplace_back(temp_2);
            m[temp_1] = v;
        }
        else m[temp_1].emplace_back(temp_2);
        if(s.find(temp_2)==s.end()){
            s.insert(temp_2);
            vector<int> v = vector<int>();
            v.emplace_back(temp_1);
            m[temp_2] = v;
        }
        else m[temp_2].emplace_back(temp_1);
    }
    while(M--){
        scanf("%d",&K);
        getchar();
        set<int> incom = set<int>();
        bool could = true;
        while(K--){
            int t;
            scanf("%d",&t);
            getchar();
            if(incom.find(t) == incom.end()&&could == true){
                G.emplace_back(t);
                if(s.find(t)!=s.end())for(auto each: m[t])incom.insert(each);
            }
            else could = false;
        }
        if(could)printf("Yes\n");
        else printf("No\n");
    }
    return 0;
}
```
