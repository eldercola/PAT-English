# 快排找Pivot
  
给你一串互不相同的数字序列，要从中找出备选的Pivot, 条件如下:  
1. 该数字左边的数都小于它本身  
2. 该数字右边的数都大于它本身  
  
#### 我的思路如下:  

维护两个数组: ```left_max数组, 存储左边数字中的最大值(不包括自己)```和```right_min数组, 存储右边数字中的最小值(不包括自己)```  
  
遍历这N个数一次, 每个循环中做如下检查:  
1. (从前往后) 检查前一个数与前一个位置上的left_max, 把更大者记录到自己的left_max数组上  
2. (从后往前) 检查后一个数与后一个位置上的right_min, 把更小者记录到自己的right_min数组上  
  
接着再遍历N一次, 做如下检查:  
```left_max[i]<nums[i]&&right_min[i]>nums[i]```, 如果满足这个条件就是一个备选Pivot, 记录一下即可。  
  
最后输出即可。
  
#### 源代码
```c++
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int N;
int nums[100001];
int left_max[100001], right_min[100001];
vector<int>legal_nums;

int main(){
    scanf("%d",&N);
    for(int i = 0;i<N;i++)scanf("%d",&nums[i]);
    left_max[0] = nums[0];
    right_min[N-1] = nums[N-1];
    for(int i = 1;i<N;i++){
        left_max[i] = nums[i-1]>left_max[i-1]?nums[i-1]:left_max[i-1];
        right_min[N-1-i] = nums[N-1-i+1]<right_min[N-1-i+1]?nums[N-1-i+1]:right_min[N-1-i+1];
    }
    for(int i = 0;i<N;i++)if(left_max[i]<=nums[i]&&right_min[i]>=nums[i])legal_nums.push_back(nums[i]);
    //for(int i = 0;i<N;i++)cout<<nums[i]<<" "<<left_max[i]<<" "<<right_min[i]<<endl;//调试用
    printf("%d\n",legal_nums.size());
    for(int i = 0;i<legal_nums.size();i++){
        if(i)printf(" ");
        printf("%d",legal_nums[i]);
    }
    printf("\n");
    return 0;
}
```
