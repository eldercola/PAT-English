# 哈希表的插入、查找
[题目链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805343236767744).  
题目描述的比较冗长,主要分为以下三个部分:  
1. 对于本题中的哈希表, 哈希算法为```H(key) = key%Table_Size```.  
2. 解决冲突的办法为二次探索(Quadratic probing (with positive increments only) 只向正方向增加,这个下文会详细展开).  
3. 给定一个```Table_Size```,这个```Size```最好是一个**质数(prime)**, 如果没给一个质数,那么需要找到最小的比给定```size```的质数。  
  
输入分为三行:  
1. 题目第一行给三个数据: ```表大小 Table_Size, 插入的数字个数 N, 要查询的数字个数 M```三个数均**小于10000**.  
2. 接下来一行就是要插入的数据逐个输入, 输入完成后换行。  
3. 最后一行是逐个输入要查询的数据,输入完成后换行.  
  
输出分为两个部分:
1. 无法插入的数据，假如15无法插入，输出```15 cannot be inserted.```当然 有换行。(为什么会无法插入,下文会解释)  
2. 平均查询次数(```精确到一位小数```).  
  
思路在代码注释中体现, **请从```main()```开始阅读**。    
```c++
#include<bits/stdc++.h>
using namespace std;
int HashTable[10000];
int Msize, N, M;

int resize(int size_M){
    //1,2,3全是质数,所以只考虑大于3的情况就可以了
    if(size_M>3){
        bool flag = true;//假设输入的size_M是质数
        for(int i = 2;i<=size_M/2;i++){//查找的因数范围从2到size_M/2,复杂度 O(n^(1/2))
            if(size_M%i==0){//找到因数了,这个数不是质数
                flag = false;//标记一下
                break;//直接退出
            }
        }
        if(!flag){//不是质数
            for(int i=size_M+1;i<11000;i++){//从size_M+1开始一直找，找到下一个质数为止, 复杂度 O(n^(1/2))
                if(i==resize(i)){//每个i都试探一下,如果i是个质数,那么返回值不会变
                    size_M=i;//这个i就是我们要的size
                    break;//直接退出
                }
            }
        }
    }
    return size_M;
}

bool insert_to_table(int num, int t){//第一个参数是待插入的数，第二个是偏移量；这是一个递归函数
    if(t>=Msize)return false;//失败停止条件是 二次探索的停止条件 即偏移量绝对值(但在这里都是正数，所以跟绝对值也没啥关系)大于哈希表的size
    else if(HashTable[(num%Msize+t*t)%Msize] == 0){//取余加上偏移量平方后，需要再散列/再哈希，因为有可能越界。如果找到的位置上是0(还没有数)，那么就填进去
        HashTable[(num%Msize+t*t)%Msize] = num;
        return true;//成功的停止条件就是 把数填进去了
    }
    return insert_to_table(num, t+1);//还没找到，向偏移量+1去探索
}

int find_in_table(int num){
    int i = 0;//从偏移量为0开始找
    for(;i<Msize;i++){//偏移量要小于Msize，也就是表长度，因为偏移量不可能大于等于表长度
        if(HashTable[(num%Msize+i*i)%Msize]==num)break;//找到了，退出
        else if(HashTable[(num%Msize+i*i)%Msize]==0)break;//这个位置上是0说明这个数不存在，也退出
    }
    return 1+i;//查找次数就是偏移量+1
}

int main(){
    double result = 0.00;//总查询次数
    for(int i = 0;i<10000;i++)HashTable[i]=0;//数组初始化
    scanf("%d %d %d\n",&Msize,&N,&M);
    int p = M;//后面还要用到 要查询的数的个数，所以拿p存一下
    Msize = resize(Msize);//用resize()调整table_size
    for(int i = 0;i<N;i++){
        int temp;
        scanf("%d",&temp);
        getchar();
        bool flag = insert_to_table(temp, 0);//0偏移插入temp这个数
        if(flag == false)printf("%d cannot be inserted.\n", temp);//如果没插入成功，那就报错
    }
    while(M--){
        int temp_find;
        scanf("%d",&temp_find);
        getchar();
        result = result+1.00*find_in_table(temp_find);//查找数
    }
    printf("%.1f\n",result/(double)p);//输出
    return 0;
}
```
