# 链表题
一个难点是有访问不到的陷阱节点。  
一个难点是最后一个测试点数据量很大，容易超时。  
> 本想用 char[] 来接受中间的地址输入，无奈```scanf("%s %d %s\n",a,&b,c)```会产生很奇怪的结果。  
> 所以还是用了```string```，然后，意料之中的运行超时  
  
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

struct node{
    string p;
    string next;
    int val;
};

int N,K;
char start[5];
node all[100001];
vector<int> smaller, between, larger;

void arrange(){
    string s = start;
    for(int i = 0;i<N;i++){
        if(all[i].p!=s){
            int j = i+1;
            for(;j<N;j++)if(all[j].p==s)break;
            if(j<N){//交换i和j
                node t = all[i];
                all[i] = all[j];
                all[j] = t;
            }
            if(all[i].next == "-1"){
                N = i+1;
                return;
            }
            else s = all[i].next;
        }
    }
}

vector<node> resort(){
    for(int i = 0;i<N;i++){
        if(all[i].val<0)smaller.emplace_back(i);
        else if(all[i].val<=K)between.emplace_back(i);
        else larger.emplace_back(i);
    }
    vector<node> temp;
    for(int i = 0;i<smaller.size();i++)temp.emplace_back(all[smaller[i]]);
    for(int i = 0;i<between.size();i++)temp.emplace_back(all[between[i]]);
    for(int i = 0;i<larger.size();i++)temp.emplace_back(all[larger[i]]);
    for(int i = 0;i<temp.size();i++){
        if(i!=temp.size()-1)temp[i].next = temp[i+1].p;
        else temp[i].next = "-1";
    }
    return temp;
}

int main(){
    scanf("%s %d %d\n",start, &N, &K);
    for(int i = 0;i<N;i++)cin>>all[i].p>>all[i].val>>all[i].next;
    arrange();
    vector<node> t = resort();
    for(int i = 0;i<t.size();i++)cout<<t[i].p<<" "<<t[i].val<<" "<<t[i].next<<endl;
    return 0;
}
```
