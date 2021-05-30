# 初次提交，样例4答案错误
1. 思路很简单 heap数组下标从1到N存放逐个输入的数据  
2. dfs函数用于从右到左遍历一个节点到叶子节点的路径  
3. temp 用于暂时存放路径，一旦遇到叶子节点，就会把当前的temp存放到total中  
4. total用于存放所有路径  
5. 输出路径的同时，检查当前节点数与根节点数的大小关系，如果当前节点更大，那么min_heap = true, 如果当前节点更小，那么max_heap = true.  
6. 如果是合法的，那么肯定只有一个是true, 如果是不合法的，两个都是true.  
7. 当然，有个特殊情况：所有节点大小一样，min_heap和max_heap都是false, 这种情况我当作not heap处理(至少我错误的那个样例，输出了Max Heap，可以证明 max_heap为true, 与这个特殊情况无关)
# 更新，已解决
原因在于 比较的时候，应该是每一个路径上的某点和它的上一个点比较，而不是直接和根节点比较，憨了憨了
```
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int Heap[1001];
int N;
vector<vector<int>> total;
vector<int> temp;

void dfs(int cur){
    temp.emplace_back(Heap[cur]);
    //无子节点
    if(2*cur>N)total.emplace_back(temp);
    //有子节点
    else{
        for(int i = 2*cur+1;i>=2*cur;i--){
            if(i<=N)dfs(i);
        }
    }
    temp.pop_back();
}

int main(){
    cin>>N;
    for(int i = 1;i<=N;i++)cin>>Heap[i];
    dfs(1);
    bool min_heap = false, max_heap = false;
    for(auto v:total){
        printf("%d",v[0]);
        for(int i = 1;i<v.size();i++){
            printf(" %d",v[i]);
            if(v[i]>v[i-1])min_heap = true; // 修改了这里
            if(v[i]<v[i-1])max_heap = true; // 修改了这里
            // if(v[i]>v[0])min_heap = true;
            // if(v[i]<v[0])max_heap = true;
        }
        printf("\n");
    }
    if(min_heap == true && max_heap == false)cout<<"Min Heap\n";
    else if(min_heap == false && max_heap == true)cout<<"Max Heap\n";
    else cout<<"Not Heap\n";
    return 0;
}
```
