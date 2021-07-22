# 并查集
并查集的一道应用题。  
主要思路是:  
  
迭代所有节点，对于每个节点,做以下判断:  
自己的兴趣中有没有和**前面的集合**重叠的部分。  
如果有，就把 前面这个集合 的**下标** 加入到 "可能的父亲节点" 这个集合中来。  
  
> 那么这个前面的集合是什么？  
> 它是一个存放兴趣集合的数组。  
> 每一组兴趣都被存放在这个集合中。每个兴趣集合都有一个对应的下标。  
> 这个下标就是父亲节点的序号。  
  
然后看看 "可能的父亲节点" 这个集合有多少人。  
如果是0，那么把自己的兴趣插入集合，把这个集合的最后一个下标(因为自己刚插入一个元素) 当作自己的父亲节点。  
如果是1，那么把自己的兴趣和前面这个 "父亲" 的兴趣融合一下，然后把自己的 "父亲" 标记为这个 "父亲" 节点。  
如果>=2，那么就把这里面所有的集合融合一下并**更新给最前面的 "可能的父亲节点"**，并且删除后面的这些集合。  
  
最后统计一下各集合里面有多少子节点就可以了。  
  
源代码如下:  
```cpp
#include<bits/stdc++.h>
#include<vector>
#include<set>
using namespace std;

int N;
int father[1001];
int nums[1001];
set<int> hobbies[1001];
vector<set<int>> total_set;

set<int> merge_set(set<int> a, set<int> b){
    for(auto it = b.begin();it!=b.end();it++)a.insert(*it);
    return a;
}

bool comp(int a, int b){
    return a>=b;
}

void make_set(){
    for(int i = 1;i<=N;i++){
        set<int> possible_father;//用于标记前面与自己喜好重叠的人
        int f;//最终的父亲节点
        //遍历前面的人
        for(int j = 0;j<total_set.size();j++){
            for(auto it = hobbies[i].begin();it!=hobbies[i].end();it++){
                if(total_set[j].find(*it)!=total_set[j].end())possible_father.insert(j);
            }
        }
        //根据多少人来判断
        if(possible_father.size()==0){
            total_set.emplace_back(hobbies[i]);
            f = total_set.size()-1;
        }
        else if(possible_father.size()==1){
            total_set[*possible_father.begin()] = merge_set(total_set[*possible_father.begin()],hobbies[i]);
            f = *possible_father.begin();
        }
        else{//有1个以上，需要把别的父节点也改掉
            for(int k = 1;k<i;k++)if(possible_father.find(father[k])!=possible_father.end())father[k] = *possible_father.begin();
            //然后并集
            auto it = possible_father.begin();
            it++;
            for(;it!=possible_father.end();it++){
                total_set[*possible_father.begin()] = merge_set(total_set[*possible_father.begin()],total_set[*it]);
            }
            it = possible_father.begin();
            it++;
            for(;it!=possible_father.end();it++){
                total_set.erase(total_set.begin()+(*it));
            }
            f = *possible_father.begin();
        }
        //标记自己的父亲
        father[i] = f;
    }
}

int main(){
    //输入
    scanf("%d\n",&N);
    for(int i = 1;i<=N;i++){
        int hs;
        scanf("%d: ",&hs);
        while(hs--){
            int h;
            scanf("%d",&h);
            getchar();
            hobbies[i].insert(h);
        }
    }
    //开始找集合
    make_set();
    cout<<total_set.size()<<endl;
    for(int i = 1;i<=N;i++)nums[father[i]]++;
    sort(nums,nums+total_set.size(),comp);
    for(int j = 0;j<total_set.size();j++){
        if(j)cout<<" ";
        cout<<nums[j];
    }
    cout<<endl;
    return 0;
}
```
