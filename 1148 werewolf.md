## 狼人杀 船新AC版本(百度搜索结果找到的解答的一般套路) 先假设狼人，找说谎者  
大致思路如下:  
总共```N```个玩家  
一个存放所有玩家真实身份的数组```r```，先全置```1```，表示好人(狼人则设为```-1```)  
一个存放所有玩家口嗨的数组```sentence```  
两个变量```i```和```j```,```i从1到N-1,步长1: j从i+1到N,步长1```  
这两个序号为```i```和```j```的就是(假设的)狼人，```r[i]=r[j]=-1```。  
遍历```sentence```找```liars```,如果liars刚好是两人，并且其中一人恰好是狼，这就是**最小**合法解  
>最小合法解:  
>如果解A和解B的前```i```位大小都一样,而```A[i+1]<B[i+1]```,那么则有```解A<解B```  
```cpp
#include<bits/stdc++.h>
#include<vector>
#include<cmath>
using namespace std;

int N;
int r[101],sentence[101];
vector<int> liars;

bool isInVec(vector<int> v, int num){
    for(int each:v){
        if(each == num)return true;
    }
    return false;
}

bool isValid(int i, int j, vector<int> l){
    if(isInVec(l,i)&&!isInVec(l,j))return true;
    if(!isInVec(l,i)&&isInVec(l,j))return true;
    return false;
}

int main(){
    scanf("%d\n",&N);
    for(int i = 1;i<=N;i++)scanf("%d\n",&sentence[i]);
    for(int i = 1;i<=N-1;i++){
        for(int j = i+1;j<=N;j++){
            //假设i和j是狼人
            liars = vector<int>();
            fill(r,r+N+1,1);//real数组填充1,即全好人
            r[i]=r[j]=-1;
            for(int k = 1;k<=N;k++){
                if(sentence[k]*r[abs(sentence[k])]<0)liars.emplace_back(k);//k是一个说谎者
            }
            if(liars.size()==2&&isValid(i,j,liars)){
                printf("%d %d\n",i,j);
                return 0;
            }
        }
    }
    printf("No Solution\n");
    return 0;
}
```
## 狼人杀 先假设说谎者然后找狼人的版本 不过目前卡在最后一个测试点
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
