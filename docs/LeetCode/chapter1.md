# 1.两数之和
哈希法，时间复杂度为O(n)
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        int i;
        for(i=0;i<nums.size();i++){
            auto it = hash.find(target-nums[i]);
            if(it != hash.end()){
                return {it->second,i};
            }
            hash[nums[i]]=i;
        }
        return {};
    }
};
```
map第一个是键值，第二个是值，我们的哈希表将具体数字作为键值，索引作为值，hash.find函数找键值的复杂度为O(1)，非常好用。若没找到，就把这组键值和值塞入哈希表中，以便下次查找。

关于unordered_map的内容。

示例：使用 `unordered_map` 存储和检索学生分数

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
int main() {
    // 创建一个unordered_map，键是学生的姓名（string类型），值是学生的分数（int类型）
    std::unordered_map<std::string, int> studentScores;
    // 插入一些学生的分数
    studentScores["Alice"] = 85;
    studentScores["Bob"] = 90;
    studentScores["Charlie"] = 78;
    // 查找并输出Alice的分数
    std::string studentName = "Alice";
    auto search = studentScores.find(studentName);
    if (search != studentScores.end()) {
        std::cout << studentName << "'s score is " << search->second << std::endl;
    } else {
        std::cout << studentName << " not found" << std::endl;
    }
    // 遍历unordered_map并输出所有学生的分数
    std::cout << "All students' scores:" << std::endl;
    for (const auto& pair : studentScores) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    // 尝试删除一个不存在的键
    studentScores.erase("David");
    // 删除Bob的记录
    if (studentScores.erase("Bob") > 0) {
        std::cout << "Bob's record has been removed." << std::endl;
    }
    // 再次遍历unordered_map
    std::cout << "Students' scores after removal:" << std::endl;
    for (const auto& pair : studentScores) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    return 0;
}
```

详解

1. **创建 `unordered_map`**：
   ```cpp
   std::unordered_map<std::string, int> studentScores;
   ```
   这里创建了一个 `unordered_map` 容器，键是 `std::string` 类型，值是 `int` 类型。
2. **插入元素**：
   ```cpp
   studentScores["Alice"] = 85;
   studentScores["Bob"] = 90;
   studentScores["Charlie"] = 78;
   ```
   通过键值对的方式插入数据。每个学生的姓名作为键，分数作为值。
3. **查找元素**：
   ```cpp
   auto search = studentScores.find(studentName);
   ```
   使用 `find` 方法来查找键为 "Alice" 的元素。如果找到了，`find` 方法返回一个迭代器指向这个元素；如果没有找到，返回一个迭代器指向 `unordered_map` 的 `end()`。
4. **访问值**：
   ```cpp
   if (search != studentScores.end()) {
       std::cout << studentName << "'s score is " << search->second << std::endl;
   }
   ```
   如果找到了元素，可以通过迭代器访问键值对中的值（`search->second`）。
5. **遍历 `unordered_map`**：
   ```cpp
   for (const auto& pair : studentScores) {
       std::cout << pair.first << ": " << pair.second << std::endl;
   }
   ```
   使用范围 `for` 循环遍历 `unordered_map`。`pair` 是一个键值对，`pair.first` 是键，`pair.second` 是值。
6. **删除元素**：
   ```cpp
   studentScores.erase("David");
   ```
   尝试删除键为 "David" 的元素，如果键不存在，则不进行任何操作。
   ```cpp
   if (studentScores.erase("Bob") > 0) {
       std::cout << "Bob's record has been removed." << std::endl;
   }
   ```
   删除键为 "Bob" 的元素，`erase` 方法返回删除的元素数量。在这个例子中，因为 "Bob" 存在，所以返回 1，并输出删除信息。
7. **再次遍历 `unordered_map`**：
   ```cpp
   for (const auto& pair : studentScores) {
       std::cout << pair.first << ": " << pair.second << std::endl;
   }
   ```
   再次遍历 `unordered_map`，此时 "Bob" 的记录已经被删除。
这个例子展示了 `unordered_map` 的基本用法，包括插入、查找、遍历和删除操作。由于 `unordered_map` 不保证元素的顺序，所以输出的顺序可能与插入的顺序不同。


# 无重复数字的最长子串

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> a;
        int right=-1;
        int i;
        int ans=0;
        int n=s.size();

        for(i=0;i<n;i++){           
            if(i!=0){
                a.erase(s[i-1]);
            }
           
            while(right<n-1&&!a.count(s[right+1])){
                a.insert(s[right+1]);
                right++;
            }

            
            ans=max(ans,right-i+1);
        }
        return ans;
    }
};
```

这题中unordered_set是一串无序串，插入查找等复杂度都是O(1)，最好一开始就定义int n=s.size()，后面都用n代替，如果不是这样可能会出错，因为s.size()返回size_t。同时还有s.erase(),s.insert(),s.count()等函数好用，搞清楚用熟练即可。


# 最长回文子串
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n=s.size();
        if(n<=1) return s;

        //vector<vector<int>> dp(n, vector<int>(n));
        int dp[n][n];
        int begin=0;
        int maxlen =1;

        for(int i=0;i<n;i++) dp[i][i]=1;

        for(int l=2;l<=n;l++){
            for(int i = 0;i < n;i++){
                int j= l+i-1;
                if(j>=n) break;
                if(s[i]!=s[j]) {dp[i][j]=0;}
                else{
                    if(j-i<3)dp[i][j]=1;
                    else dp[i][j]=dp[i+1][j-1];
                }
            

                if(dp[i][j]&&j-i+1>maxlen){
                    begin=i;
                    maxlen=j-i+1;
                }
            }
        }
        
        return s.substr(begin,maxlen);
    }
};
```
动态规划部分挺经典的，可以重新做。同时，关注字符串子串的表示为s.substr(begin,len).


# 7.整数反转

```cpp
class Solution {
public:
    int reverse(int x) {
        int ans = 0;
        int n;
        while (x!=0){
            if (ans < INT_MIN / 10 || ans > INT_MAX / 10) {
                return 0;
            }

            n = x % 10;
            x/=10;
            ans = ans*10+n;
        }
        return ans;
    }
};
```

在您提供的代码中，`if (ans < INT_MIN / 10 || ans > INT_MAX / 10)` 这一行是用来检查整数溢出的。在反转整数的过程中，如果 `ans` 的值超出了 `int` 类型的范围，那么乘以 10 并加上新的数字 `n` 将会导致溢出。
这里是详细解释：
- `INT_MIN` 和 `INT_MAX` 是定义在 `<climits>` 头文件中的宏，分别表示 `int` 类型能表示的最小值和最大值。
- 当你尝试反转一个整数时，例如将 `123` 反转为 `321`，每次循环中你都会将当前的 `ans` 乘以 10，然后加上新的数字。如果 `ans` 已经接近 `INT_MAX` 或 `INT_MIN`，再乘以 10 就可能会导致溢出。
为了避免这种情况，代码在每次循环之前检查 `ans` 是否在安全范围内。具体来说：
- `ans < INT_MIN / 10` 检查 `ans` 是否小于 `int` 类型最小值的十分之一。如果是这样，那么在乘以 10 后加上任何负数（因为 `n` 是 `x` 的最后一位数字，`x` 是负数时 `n` 也是负数）都会导致溢出。
- `ans > INT_MAX / 10` 检查 `ans` 是否大于 `int` 类型最大值的十分之一。如果是这样，那么在乘以 10 后加上任何正数都会导致溢出。
如果 `ans` 超出了这个范围，函数就会返回 0，表示发生了溢出，反转操作无法进行。这是一种常见的溢出检查方法，可以确保在反转整数时不会超出 `int` 类型的限制。



# 9.回文数

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        std::string a=to_string(x);
        std::string b=a;
        std::reverse(b.begin(),b.end());
        //if(!std::strcmp(a,b)) return true;
        //else return false;
        return a==b;
    }
};
```

关注**c风格字符串**和**c++标准字符串**的区别！

C风格字符串和C++标准字符串（`std::string`）在多个方面存在差异，以下是一些主要的区别：
1. **类型和封装**:
   - C风格字符串是一个以 null 字符 (`'\0'`) 结尾的字符数组（`char[]`）。
   - C++标准字符串是一个类，它封装了字符数组并提供了一系列操作字符串的成员函数。
2. **内存管理**:
   - C风格字符串需要手动管理内存，包括分配、释放和调整大小。
   - C++标准字符串自动管理内存，它会根据需要分配和释放内存。
3. **功能**:
   - C风格字符串的功能有限，通常只能通过标准库函数（如 `strcpy`, `strlen`, `strcmp` 等）进行操作。
   - C++标准字符串提供了丰富的成员函数，包括但不限于 `append`, `find`, `replace`, `substr` 等，以及运算符重载，使得字符串操作更加方便和直观。
4. **安全性**:
   - C风格字符串容易出错，例如缓冲区溢出、忘记添加 null 终止字符等。
   - C++标准字符串更加安全，因为它会检查边界并在许多情况下防止错误。
5. **长度和容量**:
   - 对于C风格字符串，要获取长度需要使用 `strlen` 函数，而且没有直接的方式来获取容量。
   - C++标准字符串提供了 `size()` 成员函数来获取当前字符串的长度，以及 `capacity()` 来获取当前分配的内存容量。
6. **迭代器**:
   - C风格字符串没有提供迭代器概念。
   - C++标准字符串提供了迭代器，可以用于标准库算法，如 `std::sort` 或 `std::find`。
7. **性能**:
   - C风格字符串在某些情况下可能更高效，尤其是在处理简单的字符串操作时，因为它没有类封装带来的额外开销。
   - C++标准字符串在大多数情况下提供了更好的性能，尤其是在复杂操作和内存管理方面。
8. **兼容性**:
   - C风格字符串可以在C和C++中使用。
   - C++标准字符串只能在C++中使用。
9. **构造和销毁**:
   - C风格字符串通常不需要显式的构造和销毁过程。
   - C++标准字符串有构造函数和析构函数，它们在创建和销毁对象时被调用。
这些区别使得C++标准字符串在大多数情况下更方便、更安全、更灵活，而C风格字符串在某些特定的应用中可能更轻量级和高效。

## 关于c++标准字符串函数
csdn上讲的好，[click here](https://blog.csdn.net/m0_73494049/article/details/142172139?ops_request_misc=&request_id=&biz_id=102&utm_term=c++%E6%A0%87%E5%87%86%E5%AD%97%E7%AC%A6%E4%B8%B2&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-142172139.142^v101^pc_search_result_base9&spm=1018.2226.3001.4187)

