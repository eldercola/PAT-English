URL: https://pintia.cn/problem-sets/994805342720868352/problems/994805460652113920  
In this question, we are ordered to find the start address of the character which is the start of the common suffix.  
The input contains one test case. For each case, the first line contains two addresses of nodes and a positive N (≤10^5 ), where the two addresses are the addresses of the first nodes of the two words, and N is the total number of nodes. The address of a node is a 5-digit positive integer, and NULL is represented by −1.  

Then N lines follow, each describes a node in the format:  
  
Address Data Next  
where Address is the position of the node, Data is the letter contained by this node which is an English letter chosen from { a-z, A-Z }, and Next is the position of the next node.  


My resolution:(C++ clang++)  
create a struct node, which contains address-string, character-char, next-string(the next address of next character).  
I store the address as string because the address may be out of the range of int. It's sophisticated and difficult for me to consider about this issue.  
Use map<string, node> to store every node in a map.  
According to the first address, we start to traverse the first word.  
Use set<string> to store the address we arrived.  
Making a while loop whose end condition is next address is "-1" to traverse the first word. In this loop, we keep track of every address we arrived and stored in set.  
Making another while loop for second word. In this loop, we are in order to find whether current address had been in the set. If so, we could break and output this address. Else we continue to traverse until the next is "-1".  
Solved!  
  
中文版：  
创建 struct，取名node，包含address-string, character-char, next-string(下一个字符的地址).  
用string来存放“地址”。因为地址可能会超过int可以表示的范围，我并不想被这个问题卡住，所以使用string来偷懒。  
利用map<string,node> 来把输入的node存入map  
遍历第一个单词。利用set<string>来存放遍历过的地址。  
使用一个while循环来实现。结束条件为next地址为"-1"。  
再使用第二个while循环遍历第二个单词。只要有一个字符的地址出现在了set中，我们就break然后输出这个地址。否则循环继续，直到next为"-1"。  
  
参考资料：  
http://www.cplusplus.com/reference/set/set/find/  
--主要查阅了set中find()方法。  
Searches the container for an element equivalent to val and returns an iterator to it if found, otherwise it returns an iterator to set::end.  
  
代码：Code:  
```cpp
#include<bits/stdc++.h>
#include<map>
#include<set>
using namespace std;

struct node{
    string address, next="-1";
    char word;
};

map<string,node> words;
set<string> traverse;

int main(){
    string first,second;
    int N,i;
    cin>>first>>second>>N;
    for(i=0;i<N;i++){
        string add,next;
        char word;
        node newChar;
        cin>>add>>word>>next;
        newChar.address=add;
        newChar.next=next;
        newChar.word=word;
        words[add]=newChar;
    }
    string result = "-1";
    node p = words[first];
    string next = p.next;
    while(next!="-1"){
        traverse.insert(p.address);
        p=words[next];
        next = p.next;
    }
    node q = words[second];
    next = q.next;
    while(next!="-1"){
        if(traverse.find(q.address)!=traverse.end()){
            //set的find返回一个迭代器，如果set中有这个元素，返回对应元素的迭代器，否则返回set的end()
            //参考见http://www.cplusplus.com/reference/set/set/find/
            result = q.address;
            break;
        }
        else{
            q=words[next];
            next = q.next;
        }
    }
    cout<<result<<endl;
    return 0;
}
```
