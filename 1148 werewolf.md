# 狼人杀 不过目前卡在最后一个测试点
```cpp
#include<bits/stdc++.h>
#include<vector>
#include<set>
using namespace std;
vector<vector<int>> ans;
vector<int> t_ans;
int N;
int sentence[101];
bool comp(vector<int> x, vector<int> y){
    if(x[0]!=y[0])return x[0]<y[0];
    return x[1]<y[1];
}
int main(){
    scanf("%d\n",&N);
    for(int i = 1;i<=N;i++)scanf("%d\n",&sentence[i]);
    //i和j分别是两个说谎者，题目要求说谎者中要有一个是狼人，遍历看哪两个是说谎者，然后反推出两条狼
    for(int i = 1;i<N;i++){
        for(int j = i+1;j<=N;j++){
            int k = 1;
            set<int> human = set<int>();
            set<int> wolf = set<int>();
            for(;k<=N;k++){
                int temp = sentence[k];
                if(k == i||k == j)temp = -temp;
                if(temp>0){
                    if(wolf.find(temp)==wolf.end())human.insert(temp);
                    else break;
                }
                else{//temp<0
                    if(human.find(-temp)==human.end())wolf.insert(-temp);
                    else break;
                }
            }
            if(k<N+1)continue;//说法矛盾
            if(human.size()>N-2||wolf.size()>2)continue;//好人过多或狼人过多
            if(human.size()==N-2)for(int s = 1;s<=N;s++)if(human.find(s)==human.end())wolf.insert(s);//好人刚好，那就把狼人找出来
            if(wolf.size()==2){//狼人刚好
                if((wolf.find(i)!=wolf.end()&&wolf.find(j)==wolf.end())||(wolf.find(i)==wolf.end()&&wolf.find(j)!=wolf.end())){
                    //合法解
                    t_ans=vector<int>();
                    for(auto p = wolf.begin();p!=wolf.end();p++)t_ans.emplace_back(*p);
                    ans.emplace_back(t_ans);
                }
            }
        }
    }
    if(!ans.size()){
        printf("No Solution\n");
    }
    else{
        sort(ans.begin(),ans.end(),comp);
        printf("%d %d\n",ans[0][0],ans[0][1]);
    }
    return 0;
}
```
