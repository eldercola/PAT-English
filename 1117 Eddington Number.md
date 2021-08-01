# 搜索
这题要从一串无序数字中，找出```E```个数字，然后这```E```个数字恰好每个都大于```E```.  
所以第一件事情就是排序。  
然后就是搜索。   
  
## 线性搜索
考虑到排序之后的序列，大的数字在后，小的在前，那么其实应该从后向前搜索。  
搜索的停止条件是，找到了一个数字，下标为```i```, 它恰好小于等于序列```(i,N-1)```的长度, 那么最后的返回值就是```(i+1,N-1)```的长度.   
由后向前的线性搜索```AC```了  
## 二分搜索
设置```left, right, mid```变量, 即区间最左端与最右端还有中间。 
搜索的停止条件是```left>=right```.  
如果```mid```指向的值大于```(mid,N-1)```长度, 那么把```right```设置为```mid```,然后重新计算```mid```并进行下一轮循环.  
如果```mid```指向的值小于等于```(mid,N-1)```长度, 那么把```left```设置为```mid+1```,然后重新计算```mid```并进行下一轮循环.  
思路没错, 就是```错了一个点```.  
  
以下是代码:
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;

int N;
vector<int> dis;

/*
//用二分法来确定从后向前的最大天数
//错一个1分的点
int find_max(){
    int left = 0;
    int right = N-1;
    int tail = N-1;
    int mid = (left + right)/2;
    while(left < right){
        if(dis[mid] > tail - mid + 1){
            right = mid;
            mid = (left + right)/2;
        }
        else{
            left = mid + 1;
            mid = (left + right)/2;
        }
    }
    return tail+1-mid;
}
*/
//用从后向前的线性搜索确定
//AC
int find_max(){
    int i;
    for(i = N-1;i>=0;i--){
        if(dis[i]<=N-i)break;
    }
    int head = i+1;
    return N-head;
}

int main(){
    scanf("%d",&N);
    for(int i = 0;i<N;i++){
        int d;
        scanf("%d",&d);
        dis.emplace_back(d);
    }
    sort(dis.begin(),dis.end());
    int result = find_max();
    printf("%d\n",result);
    return 0;
}
```
