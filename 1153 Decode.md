# 解决了运行超时……不过是靠着不断重复提交卡过去的
![image](https://user-images.githubusercontent.com/52536689/120271553-1ffaf200-c2de-11eb-8a0b-b4db0425c3aa.png)
无非就把原来cin, cout改成了scanf,printf,逻辑没改，逻辑的注释可以看下面超时的代码  
代码如下:  
```cpp
#include<bits/stdc++.h>
#include<set>
#include<vector>
using namespace std;
string s[10001];
int score[10001],sub_persons[1000];
vector<int> queryResult;
set<int> p;
int N,M;

int string_to_int(string n){
    int num = 0;
    for(int i = n.size()-1,unit = 1;i>=0;i--,unit *= 10)num+=(unit*(n[i]-'0'));
    return num;
}

bool sort_type_1(int a, int b){
    if(score[a]!=score[b]) return score[a]>score[b];
    else return s[a]<s[b];
}

bool sort_type_3(int a, int b){
    if(sub_persons[a]!=sub_persons[b])return sub_persons[a]>sub_persons[b];
    else return a<b;
}

void query_type_1(string level){
    queryResult = vector<int>();
    for(int i = 1;i<=N;i++)if(s[i][0]==level[0])queryResult.emplace_back(i);
    if(queryResult.size())sort(queryResult.begin(),queryResult.end(),sort_type_1);
}

void query_type_2(string num){
    queryResult = vector<int>();
    for(int i = 1;i<=N;i++){
        string sub = s[i].substr(1,3);
        if(sub == num)queryResult.emplace_back(i);
    }
}

void query_type_3(string timeslice){
    queryResult = vector<int>();
    p = set<int>();
    for(int i = 100;i<1000;i++)sub_persons[i] = 0;
    for(int i = 1;i<=N;i++){
        string sub = s[i].substr(4,6);
        if(sub == timeslice){
            string sub_pNum = s[i].substr(1,3);
            int pNum = string_to_int(sub_pNum);
            sub_persons[pNum]++;
            if(p.find(pNum)==p.end()){//新的题目
                queryResult.emplace_back(pNum);
                p.insert(pNum);
            }
        }
    }
    if(queryResult.size())sort(queryResult.begin(),queryResult.end(),sort_type_3);
}

int main(){
    cin>>N>>M;
    char buf[1000];
    for(int i = 1;i<=N;i++){
        scanf("%s %d\n",buf,&score[i]);
        s[i] = buf;
    }
    for(int i = 1;i<=M;i++){
        int Type;
        string Term;
        scanf("%d %s\n",&Type,buf);
        Term = buf;
        printf("Case %d: %d %s\n",i,Type,buf);
        if(Type == 1){
            query_type_1(Term);
            if(queryResult.size()){
                for(int each:queryResult){
                    cout<<s[each]<<" "<<score[each]<<endl;
                }
            }
            else printf("NA\n");
        }
        else if(Type == 2){
            query_type_2(Term);
            if(queryResult.size()){
                int total = 0;
                for(int each: queryResult)total+=score[each];
                printf("%d %d\n",queryResult.size(),total);
            }
            else printf("NA\n");
        }
        else if(Type == 3){
            query_type_3(Term);
            if(queryResult.size()){
                for(int each: queryResult){
                    printf("%d %d\n",each,sub_persons[each]);
                }
            }
            else printf("NA\n");
        }
    }
    return 0;
}
```
# 运行超时了……待解决 5/31/2021 21:55
代码如下:
```cpp
#include<bits/stdc++.h>
#include<set>
#include<vector>
using namespace std;
string s[10001];
int score[10001],sub_persons[1000];
vector<int> queryResult;
set<int> p;
int N,M;

//string_to_int 用于把'107'转化为107
int string_to_int(string n){
    int num = 0;
    for(int i = n.size()-1,unit = 1;i>=0;i--,unit *= 10)num+=(unit*(n[i]-'0'));
    return num;
}

//针对Type 1 的排序方法
bool sort_type_1(int a, int b){
    if(score[a]!=score[b]) return score[a]>score[b];
    else return s[a]<s[b];
}

//针对Type 3 的排序方法
bool sort_type_3(int a, int b){
    if(sub_persons[a]!=sub_persons[b])return sub_persons[a]>sub_persons[b];
    else return a<b;
}

//针对Type 1 的查询方法
void query_type_1(string level){//其实level只有1位，比如"A"
    queryResult = vector<int>();
    for(int i = 1;i<=N;i++)if(s[i][0]==level[0])queryResult.emplace_back(i);
    if(queryResult.size())sort(queryResult.begin(),queryResult.end(),sort_type_1);
}

//针对Type 2 的查询方法
void query_type_2(string num){
    queryResult = vector<int>();
    for(int i = 1;i<=N;i++){
        string sub = s[i].substr(1,3);
        if(sub == num)queryResult.emplace_back(i);//插入一条提交记录的序号，因为题号匹配了
    }
}

void query_type_3(string timeslice){
    queryResult = vector<int>();
    p = set<int>();
    for(int i = 100;i<1000;i++)sub_persons[i] = 0;
    for(int i = 1;i<=N;i++){
        string sub = s[i].substr(4,6);//拿时间
        if(sub == timeslice){//时间对了
            string sub_pNum = s[i].substr(1,3);//拿到题号
            int pNum = string_to_int(sub_pNum);//题号转int
            sub_persons[pNum]++;//这道题又一个人提交，算上
            if(p.find(pNum)==p.end()){//新的题目
                queryResult.emplace_back(pNum);
                p.insert(pNum);
            }
        }
    }
    if(queryResult.size())sort(queryResult.begin(),queryResult.end(),sort_type_3);
}

int main(){
    cin>>N>>M;
    for(int i = 1;i<=N;i++)cin>>s[i]>>score[i];
    for(int i = 1;i<=M;i++){
        int Type;
        string Term;
        cin>>Type>>Term;
        cout<<"Case "<<i<<": "<<Type<<" "<<Term<<endl;
        if(Type == 1){
            query_type_1(Term);
            if(queryResult.size()){
                for(int each:queryResult)cout<<s[each]<<" "<<score[each]<<endl;
            }
            else cout<<"NA\n";
        }
        else if(Type == 2){
            query_type_2(Term);
            if(queryResult.size()){
                int total = 0;
                for(int each: queryResult)total+=score[each];
                cout<<queryResult.size()<<" "<<total<<endl;
            }
            else cout<<"NA\n";
        }
        else if(Type == 3){
            query_type_3(Term);
            if(queryResult.size()){
                for(int each: queryResult){
                    cout<<each<<" "<<sub_persons[each]<<endl;
                }
            }
            else cout<<"NA\n";
        }
    }
    return 0;
}
```
