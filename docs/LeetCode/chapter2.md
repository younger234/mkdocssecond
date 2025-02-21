# 11.称最多水的容器
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n=height.size();
        if(n==1) return 0;

        int left=0;
        int right=n-1;
        int m=0;
        int max=0;
        int now;

        while(right-left>0){
            m=std::min(height[left],height[right]);
            now=(right-left)*m;

            if(now>max) max=now;

            if(m==height[left]){
                while(left<right&&height[left]<=m) left++;
            }else{
                while(left<right&&height[right]<=m) right--;
            }

        }

        return max;
```

双指针法，可以将时间复杂度降低到O(1)量级。注意本题内层while判断条件不要漏了left < right,否则循环可能永远停不下来。


# 12.整数转罗马数字

```cpp
const pair<int,string> values[]={
    {1000,"M"},{900,"CM"},{500,"D"},{400,"CD"},{100,"C"},{90,"XC"},{50,"L"},{40,"XL"},{10,"X"},{9,"IX"},{5,"V"},
    {4,"IV"},{1,"I"}
};

class Solution {
public:
    string intToRoman(int num) {
        string a;
        for(auto const [v,s]:values){
            while(num>=v){
                num-=v;
                a+=s;
            }
            if(num==0) break;
        }
        return a;
    }
};

```
这是**方法一**，贪心思路，每次找到最大的数字进行减操作。关注一下c++中pair的使用。在for循环中是(auto const [v,s]:values)用中括号，而不是花括号{v,s}。

同时，关于pair的用法，可以用std::make_pair(a,b)构建一个pair

**方法二**其实更加简单，就是用标准字符串的加法。如下
```cpp
const string thousands[] = {"", "M", "MM", "MMM"};
const string hundreds[]  = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
const string tens[]      = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
const string ones[]      = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

class Solution {
public:
    string intToRoman(int num) {
        return thousands[num / 1000] + hundreds[num % 1000 / 100] + tens[num % 100 / 10] + ones[num % 10];
    }
};
```

# 13.罗马数字转成整数
枚举所有情况即可
```cpp
class Solution {
public:
    int romanToInt(string s) {
        int num=0;
        int i=0;
        int n=s.size();
        while(i<n){
            if(s[i]=='M') {num+=1000;i++;}
            if(s[i]=='D') {num+=500;i++;}
            if(s[i]=='C') {
                if(s[i+1]=='M'){num+=900;i+=2;}
                else if(s[i+1]=='D'){num+=400;i+=2;}
                else{num+=100;i++;}
            }
            if(s[i]=='X'){
                if(s[i+1]=='C'){num+=90;i+=2;}
                else if(s[i+1]=='L'){num+=40;i+=2;}
                else{num+=10;i++;}
            }
            if(s[i]=='L'){num+=50;i++;}
            if(s[i]=='V'){num+=5;i++;}
            if(s[i]=='I'){
                if(s[i+1]=='X'){num+=9;i+=2;}
                else if(s[i+1]=='V'){num+=4;i+=2;}
                else{num+=1;i++;}
            }
        }
    return num;
    }
};
```

# 最长公共前缀
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string a;
        int i=0;
        while(strs[0][i]==strs[1][i]&&strs[0][i]==strs[2][i]){
            a+=strs[0][i];
            i++;
        }
        return a;
    }
};
```
这是一个错误的做法，没有考虑到不同的字符串长度不同，会产生越界报错现象。

正确的做法如下：
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
       // if(strs.empty()) return "";
        int n=strs.size();
        string a;
        
        for(int i=0;i<strs[0].size();i++){
            for(int j=0;j<n;j++){
                if(strs[j][i]!=strs[0][i]||i>strs[j].size()){
                    return a;
                }
            }
            a+=strs[0][i];
        }
        return a;
    }
};
```

难度普通

# 15.三数之和

这道题是第一题两数之和的加强版

**方法一**仍然用哈希表的方法，这个方法时间复杂度为O(n*n)，关键在于每一步的严谨性，首先要将nums[]排序，从而跳过重复的。其次，应该在遍历第一层i循环过程中每次新建一个哈希表，不可一开始在双循环外面直接建立哈希表。并且，关于i和j的跳过重复的操作还是有点复杂的
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        
        std::sort(nums.begin(), nums.end()); // 先对数组进行排序
        for(int i=0;i<nums.size();i++){
            if(i>0&&nums[i]==nums[i-1]) continue;
            unordered_map<int,int> hash;
            for(int j=i+1;j<nums.size();j++){
                //if(j>0&&nums[j]==nums[j-1]) continue;
                auto it = hash.find(-nums[i]-nums[j]);
                if(it!=hash.end()){
                    ans.push_back({nums[i],nums[j],it->first});
                    while(j+1<nums.size()&&nums[j]==nums[j+1]) j++;
                }
                    //hash[nums[i]]=i;

                    hash[nums[j]]=j;
                
            }
        }
        return ans;
    }
};

```
**方法二**是用双指针法，遍历first然后second和third指针分别从左边和右边向对方移动以便寻找到最终的需要的答案，具体过程中的每一步的条件都很严谨，容易错误
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        int n=nums.size();
        std::sort(nums.begin(),nums.end());
        int third;
        for(int first=0;first<n;first++){
            if(first>0&&nums[first]==nums[first-1]) continue;
            third=n-1;
            for(int second=first+1;second<n;second++){
                if(second>first+1 && nums[second]==nums[second-1]) continue;
                while(second<third&&nums[first]+nums[second]+nums[third]>0) third--;
                if(second==third)break;
                
                if(nums[first]+nums[second]+nums[third]==0) ans.push_back({nums[first],nums[second],nums[third]});
            }
        }
        return ans;
    }
};
```


# 17.最接近的三数之和
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int n= nums.size();
        int best=1e5;
        int current;
        int second;
        int third;
        std::sort(nums.begin(),nums.end());

        for(int first=0;first<n;first++){
                second=first+1;
                third=n-1;
                while(second<third){
                    int sum=nums[first]+nums[second]+nums[third];
                    if(sum==target) return target;
                    if(abs(sum-target)<abs(best-target)) best = sum;
                    if(sum>target){
                        third--; 
                    }else{
                        second++;
                    }
                }  
            }
        return best;
        }
};
```
同样是双指针法。关注绝对值函数abs的运用，以及可以在表示一个很大的数时用科学计数法，比如这道题中的best=1e5;表示一乘十的五次方。

# 17.电话号码的字母组合

这是一道很经典的回溯算法题，同时也是对c++各种容器操作、语法运用的经典。本人的解答：
```cpp
void backtrack(vector<string> &ans,string &a,int index,string &digits,unordered_map<char,string> &phonemap){
    if(index==digits.length()){
        ans.push_back(a);
    }
    char k = digits[index];
    for(char c:phonemap[k]){
        a.push_back(c);
        backtrack(ans,a,index+1,digits,phonemap);
        a.pop_back();
    }
}

class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if(digits.empty()) return {};
        unordered_map<char,string> phonemap{
            {'2',"abc"},{'3',"def"},{'4',"ghi"},
            {'5',"jkl"},{'6',"mno"},{'7',"pqrs"},
            {'8',"tuv"},{'9',"wxyz"}
        };
        string a;
        vector<string> ans;
        backtrack(ans,a,0,digits,phonemap);
        return ans;
    }
};
```
几点关注一下吧：
1. unordered map哈希表的建立
2. 对于空容器vector的返回，用花括号即可，return {};
3. string digits.length(),标准字符串的长度函数
4. 向量vector操作函数，push_back和pop_back(),进栈或出栈 VS set a.insert()函数，这些各自的函数不要混淆
5. map中哈希表索引直接用phonemap[k]，官方题解也行吧，用了at函数
    ```cpp  char digit = digits[index];
            const string& letters = phoneMap.at(digit);
            for (const char& letter: letters) {
    ```

# 19.删除列表的倒数第n个结点
```cpp
**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dump= new ListNode(0,head);
        ListNode *tempt=new ListNode;
        tempt=head;
        int l=0;
        while (tempt){
            l++;
            tempt=tempt->next;
        }
        tempt=dump;
        
        if(l==n) return head->next;
        for(int i=0;i<l-n;i++){
            tempt=tempt->next;
        }
        tempt->next=tempt->next->next;
        return head;
    }
};
```

复习一下链表吧，顺便把c++中的new函数用熟练，例如本题中ListNode* dump=new ListNode(0,head);同时，链表经常建立一个空表头来避免对头节点的分类讨论。还蛮好的一道基础题。

# 20.有效的括号
```cpp
class Solution {
public:
    bool isValid(string s) {
        int n=s.size();
        if(n%2==1) return false;
        stack<char> stk;
        unordered_map<char,char> m={
            {')','('},{']','['},{'}','{'}
        };
        for(char c:s){
            if(m.count(c)){
                if(stk.empty()||stk.top()!=m[c]){
                    return false;
                }else{
                    stk.pop();
                }
            }else{
                stk.push(c);
            }
        }
        return stk.empty();
    }
};
```
注意堆栈的使用！stack类的函数，pop(),empty(),push(···)。同时，关于map，map.count()函数是用于键的计数，而不是值的计数。

或者用简单的方法，直接枚举三种情况：
```cpp
class Solution {
public:
    bool isValid(string s) {
      stack<int> st;
      for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(' || s[i] == '[' || s[i] == '{') st.push(i);
        else {
          if (st.empty()) return false;
          if (s[i] == ')' && s[st.top()] != '(') return false;
          if (s[i] == '}' && s[st.top()] != '{') return false;
          if (s[i] == ']' && s[st.top()] != '[') return false;
          st.pop();
        }
      }
      return st.empty();
    }
};
```