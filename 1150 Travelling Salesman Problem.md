# 哈密尔顿回路，一个NP问题
------
>NP问题 指 在多项式时间里只能判断 是 或 否 的一类问题  
P问题 指 能在多项式时间里解决的问题  
  
<font color = red>当然, 我有点忘了, 有错的地方还请各位大佬指正</font>  
  
但其实，题目跟这个……没有什么很大的关系，你只需读懂题目，知道题目想让你判断一条路是不是汉密尔顿回路即可。  
当然，路径在本题分很多类，主要有如下三种:  
1. TS simple cycle, 标准汉密尔顿回路，一条路径从一个起始点开始并回到起始点，中间经过其他任何节点一次。换句话说，其他节点必须到过一次，起始的那个点必须到过两次。  
2. TS cycle, 维持一个cycle该有的样子，即起始节点和终点必须相同，中间必须访问到所有节点，不过次数不做限制，即次数大于等于1。
3. Not a TS cycle, 这个好理解，有没到达的节点，即 不为cycle  
废话不多说，上代码！  
```cpp
#include<bits/stdc++.h>
#include<vector>
using namespace std;
int dis[201][201];
int N,M,K;
int visit[201];
int current_smallest_index = -1,current_smallest_dis = 10000000;//记录最短路径的下标及其总距离
vector<vector<int>> all;
int main(){
  for(int i = 0;i<201;i++)for(int j = 0;j<201;j++)dis[i][j]=10000000;//把每个点之间的距离都初始化到很大
  //输入 N，M部分
  scanf("%d %d\n",&N,&M);
  while(M--){
    int start,end_,dis_;
    scanf("%d %d %d\n",&start,&end_,&dis_);
    dis[start][end_] = dis[end_][start] = dis_;
  }
  //输入 K
  cin>>K;
  //输入 路径，但输入完马上判断并输出
  for(int k = 1;k<=K;k++){
    //初始化visit数组，这个数组记录自己有没有来过某节点
    for(int i = 1;i<=N;i++)visit[i]=0;
    //这条路上有total_size个节点
    int total_size;
    scanf("%d ",&total_size);
    //暂时存放路径
    vector<int> temp = vector<int>();
    while(total_size--){
      int current_node;
      //输入节点序号
      scanf("%d",&current_node);
      getchar();
      temp.emplace_back(current_node);
    }
    //把路径存入all，all记录所有路径
    all.emplace_back(temp);
    //这个变量存放路径总长度
    int calc_dis = 0;
    //这个bool用来判断两个节点之间是否有路子
    bool has_dis = true;
    for(int i = 0;i<temp.size()-1;i++){
      //如果两个节点间没有路
      if(dis[temp[i]][temp[i+1]]==10000000){
        //直接输出 路走窄了，格局小了
        printf("Path %d: NA (Not a TS cycle)\n",k);
        //并把有路子变成false
        has_dis = false;
        break;
      }
      //这个节点来了一次，加一下。
      //为什么不用把i+1节点也访问一下呢？
      //因为就回路而言，最后一个是起始点，如果+1，会导致起始点访问2次，后面的代码量增加。
      //就算不是回路，i+1节点会在下一次循环访问到，这里加的话，就加多了。
      //“只能加一点点，不能加多”  --by 二仙桥大爷
      visit[temp[i]]++;
      //距离增加
      calc_dis+=dis[temp[i]][temp[i+1]];
    }
    //没路子就continue
    if(!has_dis)continue;
    //有路子，格局打开了
    else{
      int i = 1;
      //not_fully记录是不是路走窄了，有节点漏了；redundant记录路走多了，节点访问多了
      bool not_fully = false,redundant = false;
      for(;i<=N;i++)if(visit[i]!=1){
        // == 0 就是路走窄了
        if(visit[i]==0)not_fully=true;
        // > 1 路走多了
        if(visit[i]>1)redundant = true;
      }
      //Not a cycle
      //如果路径头尾对不上或者路走窄了，它就不是个回路
      if(not_fully||temp[0]!=temp[temp.size()-1])printf("Path %d: %d (Not a TS cycle)\n",k,calc_dis);
      else{
        //路没走窄，只是走多了，那就是个普通的cycle
        if(redundant)printf("Path %d: %d (TS cycle)\n",k,calc_dis);//abnormal situation
        //路没走窄，也没走多，那就是个simple cycle
        else printf("Path %d: %d (TS simple cycle)\n",k,calc_dis);//normal situation
        //最后比较最短路径
        if(calc_dis<current_smallest_dis){
          //把当前路的下标+1给index
          current_smallest_index = all.size();
          //把当前路的距离给calc_dis
          current_smallest_dis = calc_dis;
        }
      }
    }
  }
  //打印最短路相关信息
  printf("Shortest Dist(%d) = %d\n",current_smallest_index,current_smallest_dis);
  return 0;
}
```
