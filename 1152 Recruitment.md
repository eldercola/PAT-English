# 其实是个找质数题
废话不多说  
用一个固定窗口从前往后遍历字符串即可  
上代码  
```cpp
#include<bits/stdc++.h>
using namespace std;
int N,K;
string s;
//string转int
int string_to_int(string temp){
    int total = 0;
    for(int i = temp.size()-1,unit = 1;i>=0;i--,unit*=10){
        total += (temp[i]-'0')*unit;
    }
    return total;
}
//判断质数
bool is_Prime(string t){
    int num = string_to_int(t);
    if(num == 1||num==2||num==3)return true;//1 2 3 质数，因为下面的i从2开始，所以直接把3拎出来
    if(num == 0||num%2==0)return false;//偶数 非质数
    for(int i = 2;i*i<=num;i++)if(num%i==0)return false;//其余的奇数，用i循环判断，时间复杂度O(n^(1/2))
    return true;//均不能被整除，是质数
}

int main(){
    cin>>N>>K;
    cin>>s;
    int i=0,j=K-1;
    for(;j<s.size();i++,j++){
        string sub = s.substr(i,K);
        if(is_Prime(sub)){
            cout<<sub<<endl;
            return 0;
        }
    }
    cout<<"404"<<endl;
    return 0;
}
```
