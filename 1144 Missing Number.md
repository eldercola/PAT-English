# 找缺失数
[题目链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805343463260160).  
题意要求我们从一串数字中找出这串序列缺少的最小**自然数**. 
> **>=1的正整数**即为自然数  

题目中先输入一个N，代表接下来会输入N个数。  
接下来的N个数就是这串序列，保证每个数的大小在**int**范围内。  
  
思路很简单:  
双指针法(其实是单指针),判断当前位和下一位的数值上的关系即可  
有以下三种情况:假设队列/数组```q```,当前指针指向```pos```处。  
1. ```q[pos]<=0 && q[pos+1]<=0```: 指针直接往下挪一位即可(即```pos+1```)  
2. ```q[pos]<=0 && q[pos+1]>0```: 如果```q[pos+1]==1```成立, 那么指针往下移一位;否则,```1```就是我们要找的答案。  
3. ```q[pos]>0 && q[pos+1]>0```: 如果```q[pos+1]==q[pos]+1或者q[pos+1]==q[pos]```成立, 那么指针往下移一位;否则,```q[pos]+1```就是我们要找的答案。  
> 为什么 3 中是```q[pos+1]==q[pos]+1或者q[pos+1]==q[pos]```成立？因为题目给的例子中,就存在两个数大小一样的情况,我们需要忽略这种大小一致的情况. 

```c++
#include<bits/stdc++.h>
#include<vector>
using namespace std;

vector<int> q;
int N;

void find_solution(int pos){
    if(q.size()==1){
        if(q[0]<=0)printf("%d\n",1);
        else printf("%d\n",q[0]+1);
        return;
    }
    else{//q.size()>=2
        //双指针法开始寻找
        //不过先判断当前位置是否合法
        //边缘条件 当前指针在最后一位
        if(pos == q.size()-1){
            if(q[pos]<=0)printf("%d\n",1);
            else printf("%d\n",q[pos]+1);
            return;
        }
        //当前指针在最后一位之前
        else{
            //第一种情况:q[pos]<=0&&q[pos+1]<=0,也就是当前位与下一位都小于0,那么向下一位探索
            if(q[pos]<=0&&q[pos+1]<=0)find_solution(pos+1);
            //第二种情况:q[pos]<=0&&q[pos+1]>0,找到一个临界点,那么先判断q[pos+1]是不是1,如果不是,可以直接输出1,如果是,向下一位探索
            else if(q[pos]<=0&&q[pos+1]>0){
                if(q[pos+1]!=1){
                    printf("%d\n",1);
                    return;
                }
                else find_solution(pos+1);
            }
            //第三种情况:q[pos]>0&&q[pos+1]>0,那就判断两个数是不是连续的
            else{
                if(q[pos]+1==q[pos+1]||q[pos]==q[pos+1])find_solution(pos+1);
                else{
                    printf("%d\n",q[pos]+1);
                    return;
                }
            }
        }
    }
}

int main(){
    scanf("%d\n",&N);
    int p = N;
    while(p--){
        int t;
        scanf("%d",&t);
        getchar();
        q.emplace_back(t);
    }
    sort(q.begin(),q.end());
    find_solution(0);
    return 0;
}
```
