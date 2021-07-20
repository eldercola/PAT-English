# 给每个学校的PAT分数排序
刚开始的存储改用了map, 运行不超时了，但是最后一个点答案错误了  
```cpp
//向map屈服
#include<bits/stdc++.h>
#include<algorithm>
#include<map>
#include<vector>

using namespace std;

string lower(string s){
    for(int c = 0;c < s.size();c++)if(s[c]<'a')s[c]=s[c]+32;
    return s;
}

struct node{
    string school_name;
    vector<int> score;
    int total_score;
};

int hash_score(string id, int score){
    if(id[0]=='A')return score;
    else if(id[0]=='B')return int((double)score/1.5);
    return int((double)score*1.5);
}

int totalScore(vector<int> scores){
    int sum = 0;
    for(int s:scores)sum+=s;
    return sum;
}

bool comp_total_score(node a, node b){
    if(a.total_score!=b.total_score)return a.total_score>b.total_score;
    else if(a.score.size()!=b.score.size())return a.score.size()<b.score.size();
    return a.school_name < b.school_name;
}

int N;
map<string, node> um;
vector<node> schools;

int main(){
    scanf("%d\n",&N);
    while(N--){
        string id,school;
        //char数组 用于scanf
        char id_c[6], school_c[6];
        int score;
        scanf("%s %d %s\n",id_c,&score,school_c);
        id = id_c;
        //char[] 转 string
        school = school_c;
        school = lower(school);
        //找到学校的号次
        map<string, node>::iterator it = um.find(school);
        //如果没有这个学校 新建一个
        if(it == um.end()){
            node new_school = node();
            new_school.school_name = school;
            new_school.score.emplace_back(hash_score(id,score));
            um[school] = new_school;
        }
        //如果有了，加入就行
        else it->second.score.emplace_back(hash_score(id,score));
    }
    for(auto it = um.begin();it!=um.end();it++)schools.emplace_back(it->second);
    //算每个学校得分
    for(int i = 0;i<schools.size();i++)schools[i].total_score=totalScore(schools[i].score);
    //按照题目要求排序
    sort(schools.begin(),schools.end(),comp_total_score);
    //输出总共多少个学校
    printf("%d\n",schools.size());
    int order = 1;
    int cur_score = schools[0].total_score;
    for(int i = 0;i<schools.size();i++){
        if(schools[i].total_score<cur_score){
            order = i+1;
            cur_score = schools[i].total_score;
        }
        printf("%d ",order);
        cout<<schools[i].school_name;
        printf(" %d %d\n",schools[i].total_score,schools[i].score.size());
    }
    return 0;
}
```
离谱了，改成二分搜索也不行  
```cpp
#include<bits/stdc++.h>
#include<algorithm>
#include<vector>
using namespace std;

string lower(string s){
    for(int c = 0;c < s.size();c++)if(s[c]<'a')s[c]=s[c]+32;
    return s;
}

struct node{
    string school_name;
    vector<int> score;
    int total_score;
};

vector<node> schools;

int hash_score(string id, int score){
    if(id[0]=='A')return score;
    else if(id[0]=='B')return int((double)score/1.5);
    return int((double)score*1.5);
}

int find_school(string school){
    //O(n)
    /*
    for(int i = 0; i < schools.size();i++){
        if(schools[i].school_name>school)break;
        else if(schools[i].school_name == school)return i;
    }
    return -1;
    */
    //O(log n)
    if(!schools.size())return -1;
    int right = schools.size()-1;
    int left = 0;
    int mid = left + (right-left)/2;
    while(schools[mid].school_name!=school && right>=left){
        //要查找的学校名称更大
        if(schools[mid].school_name<school)left = mid+1;
        //中位学校名称更大
        else right = mid-1;
        mid = left + (right-left)/2;
    }
    if(schools[mid].school_name==school)return mid;
    return -1;
    
}

bool comp(node a, node b){
    return a.school_name<b.school_name;
}

int totalScore(vector<int> scores){
    int sum = 0;
    for(int s:scores)sum+=s;
    return sum;
}

bool comp_total_score(node a, node b){
    if(a.total_score!=b.total_score)return a.total_score>b.total_score;
    else if(a.score.size()!=b.score.size())return a.score.size()<b.score.size();
    return a.school_name < b.school_name;
}

int N;

int main(){
    scanf("%d\n",&N);
    while(N--){
        string id,school;
        //char数组 用于scanf
        char id_c[6], school_c[6];
        int score;
        scanf("%s %d %s\n",id_c,&score,school_c);
        id = id_c;
        //char[] 转 string
        school = school_c;
        school = lower(school);
        int s_order = find_school(school);
        //如果没有这个学校 新建一个
        if(s_order == -1){
            node new_school = node();
            new_school.school_name = school;
            new_school.score.emplace_back(hash_score(id,score));
            schools.emplace_back(new_school);
            //加入完成了要排个序
            sort(schools.begin(),schools.end(),comp);
        }
        //如果有了，加入就行
        else schools[s_order].score.emplace_back(hash_score(id,score));
    }
    //算每个学校得分
    for(int i = 0;i<schools.size();i++)schools[i].total_score=totalScore(schools[i].score);
    //按照题目要求排序
    sort(schools.begin(),schools.end(),comp_total_score);
    //输出总共多少个学校
    printf("%d\n",schools.size());
    int order = 1;
    int cur_score = schools[0].total_score;
    for(int i = 0;i<schools.size();i++){
        if(schools[i].total_score<cur_score){
            order = i+1;
            cur_score = schools[i].total_score;
        }
        printf("%d ",order);
        cout<<schools[i].school_name;
        printf(" %d %d\n",schools[i].total_score,schools[i].score.size());
    }
    return 0;
}
```
不是……怎么就运行超时了…… 
```cpp
#include<bits/stdc++.h>
#include<algorithm>
#include<vector>
using namespace std;

string lower(string s){
    for(int c = 0;c < s.size();c++)if(s[c]<'a')s[c]=s[c]+32;
    return s;
}

struct node{
    string school_name;
    vector<int> score;
    int total_score;
};

vector<node> schools;

int hash_score(string id, int score){
    if(id[0]=='A')return score;
    else if(id[0]=='B')return int((double)score/1.5);
    return int((double)score*1.5);
}

int find_school(string school){
    for(int i = 0; i < schools.size();i++){
        if(schools[i].school_name>school)break;
        else if(schools[i].school_name == school)return i;
    }
    return -1;
}

bool comp(node a, node b){
    return a.school_name<b.school_name;
}

int totalScore(vector<int> scores){
    int sum = 0;
    for(int s:scores)sum+=s;
    return sum;
}

bool comp_total_score(node a, node b){
    if(a.total_score!=b.total_score)return a.total_score>b.total_score;
    else if(a.score.size()!=b.score.size())return a.score.size()<b.score.size();
    return a.school_name < b.school_name;
}

int N;

int main(){
    scanf("%d\n",&N);
    while(N--){
        string id,school;
        //char数组 用于scanf
        char id_c[6], school_c[6];
        int score;
        scanf("%s %d %s\n",id_c,&score,school_c);
        id = id_c;
        //char[] 转 string
        school = school_c;
        school = lower(school);
        int s_order = find_school(school);
        //如果没有这个学校 新建一个
        if(s_order == -1){
            node new_school = node();
            new_school.school_name = school;
            new_school.score.emplace_back(hash_score(id,score));
            schools.emplace_back(new_school);
            //加入完成了要排个序
            sort(schools.begin(),schools.end(),comp);
        }
        //如果有了，加入就行
        else schools[s_order].score.emplace_back(hash_score(id,score));
    }
    //算每个学校得分
    for(int i = 0;i<schools.size();i++)schools[i].total_score=totalScore(schools[i].score);
    //按照题目要求排序
    sort(schools.begin(),schools.end(),comp_total_score);
    //输出总共多少个学校
    printf("%d\n",schools.size());
    int order = 1;
    int cur_score = schools[0].total_score;
    for(int i = 0;i<schools.size();i++){
        if(schools[i].total_score<cur_score){
            order = i+1;
            cur_score = schools[i].total_score;
        }
        printf("%d ",order);
        cout<<schools[i].school_name;
        printf(" %d %d\n",schools[i].total_score,schools[i].score.size());
    }
    return 0;
}
```
