这道题吃瘪了……还是第一题……想法是有，无奈自己放在了最后一题去做，因为刚开始遇到了瓶颈。
最后一小时想这题，想着用回溯法去做，就像一个八皇后。
无奈在写递归函数的时候，脑子已经扛不住了，千想万想想不全，只好匆匆地把质数数组的末尾输出，捡漏几分。

题目是这样：
给你一个n,一个maxp,从1-MAXP(不含)的所有质数中，找出一个等差数列，长度为n。
如果找不到，输出MAXP内最大的质数。如果答案不唯一，输出公差最大的那个。如果还不唯一，输出数列开头数字最大的那个。

基本思路是这样：
输入->筛选质数->逐一回溯->输出

筛选质数的方法isPrime就不介绍了……这个都不会写的话PAT也用不着考……
这个回溯的函数写了有一会，主要是自己踩了点坑。
先介绍一下用到的全局变量 vector<int> ans. 这是一个动态数组，用于存放当前的等差数列。
思路如下:
举个例子 输入 3 30, 答案应为5 17 29
质数总共有：1 2 3 5 7 11 13 17 19 21 23 29
我们要做的，是从1开始遍历到29（虽然其实不用到29，但偷懒几行代码不香吗），逐一检测把他们的下标作为参数传入到下一行的这个函数，看看能不能返回true。
//逐一回溯 写在main()函数中，all是容纳所有可能的答案的数组
	for(int i = 0;i<primes.size();i++){
		ans = vector<int>();
		if(dfs(i))all.push_back(ans);
	}
下面介绍dfs()函数
```cpp
bool dfs(int cur)// 返回 bool 是因为我们要判断，如果从当前下标开始，往后遍历能否找到满足要求的数列（等差，长度为n），如果可以，返回true，不行返回false.
如果当前ans中没有数字或只有一个数字，那么还找不到公差，尝试把当前下标的数放进去便是了。
if(ans.size()==0||ans.size()==1){
	ans.push_back(primes[cur]);
	//从后往前找，而外层dfs（main那一层）从前往后，所以公差更大，就是双指针，一个指向头部，一个指向尾部，往中间靠拢。
	for(int i = primes.size()-1;i>cur;i--){
		if(dfs(i))return true;
	}
	//如果没成功，就把刚加入的primes[cur] pop掉，说明它在当前数列不合适
	ans.pop_back();
	return false;
}
如果当前数列已经可以找出差值（长度>=2）,那么就看当前数加入之后，公差还是不是一致，一致的话，从当前数下一个开始继续找，直到数列长度变为n
else{//1<ans.size()<n
	if(primes[cur]-ans[ans.size()-1]==ans[ans.size()-1]-ans[ans.size()-2]){//与末尾数的差值跟公差一致，加入ans，如果size没到，向后继续寻找
		ans.push_back(primes[cur]);
		if(ans.size()==n)return true;
		else{//继续找 
			for(int i = cur+1;i<primes.size();i++)if(dfs(i))return true;
			ans.pop_back();
			return false;
		}
	}
}
```
```cpp
代码长这样:(github里还不会用高亮)
#include<bits/stdc++.h>
using namespace std;  
int n,MAXP;
vector<int> primes;
vector<int>ans;
vector<vector<int> >all;
int tag = 0;
bool isPrime(int s){
	if(s == 1||s==2)return true;
	for(int i = 2;i*i<=s;i++)if(s%i==0)return false;
	return true;
}
bool dfs(int cur){//cur是下标
	if(ans.size()==0||ans.size()==1){
		ans.push_back(primes[cur]);
		for(int i = primes.size()-1;i>cur;i--){
			if(dfs(i))return true;
		}
		ans.pop_back();
		return false;
	}
	else{//1<ans.size()<n
		if(primes[cur]-ans[ans.size()-1]==ans[ans.size()-1]-ans[ans.size()-2]){
			ans.push_back(primes[cur]);
			if(ans.size()==n)return true;
			else{//继续找 
				for(int i = cur+1;i<primes.size();i++)if(dfs(i))return true;
				ans.pop_back();
				return false;
			}
		}
	}
}
bool comp(vector<int> x,vector<int> y){
	if(x[x.size()-1]-x[x.size()-2]==y[y.size()-1]-y[y.size()-2])return x[0]>y[0];
	else return x[x.size()-1]-x[x.size()-2]>y[y.size()-1]-y[y.size()-2];
}
int main(){
  //输入
	cin>>n>>MAXP;
  //质数筛选
	for(int i = 1;i<MAXP;i++)if(isPrime(i))primes.push_back(i);
  //逐一回溯
	for(int i = 0;i<primes.size();i++){
		ans = vector<int>();
		if(dfs(i))all.push_back(ans);
	}
  //输出
	if(all.size()==0)cout<<primes[primes.size()-1];
	else{
		sort(all.begin(),all.end(),comp);
		for(int s = 0;s<all[0].size();s++){
			if(s)cout<<" ";
			cout<<all[0][s];
		}
		cout<<endl;
	}
	return 0;
}
```
