# 最大集团 Maximal Clique
什么是clique？  
> clique  
> 给定一个图，给定几个节点，如果能找到一个子图使得这几个节点之间互相有路，那么就是clique。  
> 如果这个clique不能通过加入一个节点变成一个更大的clique，那么这个clique就是 maximal clique

[原题指路](https://pintia.cn/problem-sets/994805342720868352/problems/994805343979159552).  
解题思路:  
输入->构造图->判断给定序列是否为clique?是否为maximal clique?  
  
> 如何判断序列是否为clique?  
> 这里的常规方法是先找到所有的maximal clique, 然后判断给定的序列是否在maximal clique中。  
> 但是，这么做不仅很思路复杂，还会导致```运行超时```.  
> 所以转换一下思路，找找maximal clique和clique有什么性质:  
> 在maximal clique中, 所有的点之间都互相有通路。  
> 假设有一个序列A,B,C  
> 它们各自的邻接点情况如下:  
> A: B,C,D. 
> B: A,C.  
> C: A,B,E.  
> 序列长度为3.  
> 其中A在三张邻接表中共出现了2次，B和C也是2次. 那么这个序列是一个maximal clique.  
> 如果邻接表如下:  
> A: B,C,D  
> B: A,C,D  
> C: A,B,D  
> 发现了吗,D在三张表中都出现了, 如果画出对应的图就会发现, D应当属于maximal clique。  
> 所以，这里的ABC是一个clique而不是maximal clique。
  
利用这个特性判断一个序列，不仅思路简单，时间也比较省。  
  
代码如下:  
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int Nv,Ne,M;
vector<int> v[201];
vector<int> points;

int judge(vector<int> ps){
    int times_occur[201] = {0};
    //统计频次
    for(int p:ps){
        for(int p_p:v[p])times_occur[p_p]++;
    }
    //如果要统计的几个点，有一个没出现size-1次，说明有点连不上，返回0
    for(int p:ps){
        if(times_occur[p]<ps.size()-1)return 0;
    }
    //遍历times_occur，如果有点也出现了size次，说明这个序列不是最大的clique
    for(int i = 1;i<=Nv;i++)if(times_occur[i]==ps.size())return 1;
    return 2;
}

int main(){
    scanf("%d %d\n",&Nv,&Ne);
    while(Ne--){
        int v1,v2;
        scanf("%d %d\n",&v1,&v2);
        v[v1].emplace_back(v2);
        v[v2].emplace_back(v1);
    }
    scanf("%d\n",&M);
    while(M--){
        int total;
        scanf("%d",&total);
        getchar();
        points = vector<int>();
        while(total--){
            int t;
            scanf("%d",&t);
            getchar();
            points.emplace_back(t);
        }
        int result = judge(points);
        if(!result)printf("Not a Clique\n");
        else if(result == 1)printf("Not Maximal\n");
        else printf("Yes\n");
    }
    return 0;
}
```
