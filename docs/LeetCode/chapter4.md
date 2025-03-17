# 31.下一个排列

!!! note "方法一"
    有点难想，从最后遍历到非降序的元素，再从最后遍历到比它大的元素，两者交换，之后的元素们再统统交换。时间复杂度为O(n).

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i= nums.size()-2;
        while(i>=0&&nums[i]>=nums[i+1]) i--;
        if(i>=0){
            int j=nums.size()-1;
            while(j>=0&&nums[i]>=nums[j]) j--;
            swap(nums[i],nums[j]);
        }
        reverse(nums.begin()+1+i,nums.end());
    }
};
```

!!! tip
    关注一下，c++的本身就有的函数swap(),和reverse函数，reverse函数里面是两个迭代器

# 32.最长有效括号

!!! danger 
    即使看了答案也没很明白，好难的动态规划啊呜呜呜![alt text](81405884.jpg)

# 33.搜索旋转排序数组

!!! note  "二分查找吧，难评"

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = (left + right) >> 1;
            if (nums[mid] == target) return mid;
            if (nums[left] <= nums[mid]) {
                // left 到 mid 是顺序区间
                (target >= nums[left] && target < nums[mid]) ? right = mid - 1 : left = mid + 1;
            }
            else {
                // mid 到 right 是顺序区间
                (target > nums[mid] && target <= nums[right]) ? left = mid + 1 : right = mid - 1;
            }
        }
        return -1;
    }
};
```

!!! note  "神奇法"
    
    ```cpp
    const search = function(nums, target) {
        return nums.indexOf(target);
    };
    ```


# 34.在排序数组中查找元素的第一个和最后一个位置
简单无脑

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l=0;
        int r=nums.size()-1;
        int mid;
        int i,j;
        while(l<=r){
            mid=(l+r)>>1;
            if(nums[mid]==target){
                for(i=mid;i>=0;i--){
                    if(nums[i]<nums[mid]) break;
                }
                for(j=mid;j<=r;j++){
                    if(nums[j]>nums[mid]) break;
                }
                return{i+1,j-1};
            }
            else if(nums[mid]<target) l=mid+1;
            else r=mid-1;
        }
        return {-1,-1};
    }
};
```

# 35.搜索插入位置
简单经典的二分查找，注意最后特殊情况的边界问题，多试几次即可
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l=0;
        int r= nums.size()-1;
        int mid;
        while(l<=r){
            mid=(l+r)>>1;
            if(nums[mid]==target) return mid;
            if(nums[mid]>target) r=mid-1;
            if(nums[mid]<target) l=mid+1;
        }
        if(nums[mid]>target){
            if(mid==0) return 0;
            else return mid;
        }
        else{ 
            return mid+1;
        }
    }
};
```

# 36.有效的数独

!!! note "方法一"
    有点难的一道题，重要的是要想到用哈希表的数据结构，然后两重循环遍历，其实对于行和列来说还是比较好处理的，但是对于小九宫格来说，就需要比较敏锐的数学头脑来进行构造了，难想出已逝。最后判断条件是检测到除了'.'元素外大于一的数字就寄了。

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i=0;i<9;i++){
            unordered_map <char,int> row,column,map9;
            for(int j=0;j<9;j++){
                row[board[j][i]]++;
                column[board[i][j]]++;
                int x=j/3+i/3*3;
                int y=j%3+i%3*3;
                map9[board[x][y]]++;
                if((row[board[j][i]]>1&&board[j][i]!='.')||
                (column[board[i][j]]>1&&board[i][j]!='.')||
                (map9[board[x][y]]>1&&board[x][y]!='.')) return false;
            }
        }
        return true;
    }
};
```

!!! note "方法二"
    题目官方解答的方法,内容差不多,用多维数组代替了哈希表，也可以的，挺好用的

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        int rows[9][9];
        int columns[9][9];
        int subboxes[3][3][9];
        
        memset(rows,0,sizeof(rows));
        memset(columns,0,sizeof(columns));
        memset(subboxes,0,sizeof(subboxes));
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char c = board[i][j];
                if (c != '.') {
                    int index = c - '0' - 1;
                    rows[i][index]++;
                    columns[j][index]++;
                    subboxes[i / 3][j / 3][index]++;
                    if (rows[i][index] > 1 || columns[j][index] > 1 || subboxes[i / 3][j / 3][index] > 1) {
                        return false;
                    }
                }
            }
        }
        return true;
    }
};

```

!!! tip
    memset 是 C/C++ 标准库中的一个函数，用于将一段内存区域的内容设置为指定的值。它的函数原型如下：

    ```c
    void* memset(void* ptr, int value, size_t num);
    //ptr：指向要填充的内存区域的指针。
    ```

    `value：`要设置的值，通常是一个 int 类型的值，但实际上只会使用该值的低 8 位（即一个字节）。

    `num：`要填充的字节数。

# 37.解数独

```cpp
class Solution {
private:
    bool line[9][9];
    bool column[9][9];
    bool block[3][3][9];
    bool valid;
    vector<pair<int,int>> spaces;



public:

    void dfs(vector<vector<char>> & b,int idx){
        if(idx==spaces.size()){
            valid=true;
            return;
        }

        auto [i,j]= spaces[idx];

        for(int num=0;num<9&&!valid;num++){
            if(!line[i][num]&&!column[j][num]&&!block[i/3][j/3][num]){
                line[i][num]=column[j][num]=block[i/3][j/3][num]=true;
                b[i][j]=num+'0'+1;
                dfs(b,idx+1);
                line[i][num]=column[j][num]=block[i/3][j/3][num]=false;
                //b[i][j]='.';
            }
        }
    }

    void solveSudoku(vector<vector<char>>& board) {
        memset(line,false,sizeof(line));
        memset(column,false,sizeof(column));
        memset(block,false,sizeof(block));
        valid = false;


        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j]=='.'){
                    spaces.emplace_back(i,j);
                }
                else{
                    int digit = board[i][j] - '0' - 1;
                    line[i][digit] = column[j][digit] = block[i/3][j/3][digit] = true;
                    
                }
            }
        }

        dfs(board,0);

    }
};
```

- 回溯法经典题，回溯递归
- 关注pair的使用，emplace_back(i,j)为添加元素，新建pair元素是用 auto [i,j]=···
- 注意memset函数的用法
- 本题中一定不要忘记valid的重要性，一经检查到正确结果，就停止继续递归
- 回溯法中，递归之后不能加board[i][j]='.'
- 自己再好好理解一下这道题，比较难，比较经典

# 38.外观序列
```cpp
class Solution {
public:
    string countAndSay(int n) {
        if(n==1) return "1";
        string a=countAndSay(n-1);
        string curr="";
        int start=0;
        int pos = 0;

        while(pos<a.size()){
            while(pos<a.size()&&a[pos]==a[start]) pos++;
            curr += to_string(pos-start)+a[start];
            start=pos;
        }

        return curr;

    }
};
```

!!! note
    主要是要搞清楚c++标准字符串库中to_string函数的作用，可以把整型、浮点型等变成字符串直接加



# 39.组合总和
```cpp
class Solution {
public:

    void dfs(vector<vector<int>> &ans,vector<int>&one,int target,vector<int>&c,int idx){
        if(idx==c.size()) return;
        if(target==0){
            ans.push_back(one);
            return;
        }


        //dfs(ans,one,target,c,idx+1);

        if(target-c[idx]>=0){
            one.push_back(c[idx]);
            dfs(ans,one,target-c[idx],c,idx);
            one.pop_back();
        }

        dfs(ans,one,target,c,idx+1);
    }


    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> one;
        vector<vector<int>> ans;

        dfs(ans,one,target,candidates,0);

        return ans;
        
    }
};
```

!!! note
    又是一道经典的回溯法问题啊啊，可我还是看了解析才做出来呜呜┭┮﹏┭┮



# 40.组合求数2

要求和为target但是不能重复使用，这题感觉应该算是困难题啊，看了答案才明白。

```cpp
class Solution {

private:
    vector<pair<int,int>> freq;
    vector<vector<int>>ans;
    vector<int> sequence;

public:
    void dfs(int pos,int rest){
        if(rest==0){
            ans.push_back(sequence);
            return;
        }
        if(pos == freq.size() || rest < freq[pos].first) {
            return;
        }

        dfs(pos+1,rest);

        int most = min(rest / freq[pos].first,freq[pos].second);

        for(int i = 1;i<=most; ++i){
            sequence.push_back(freq[pos].first);
            dfs(pos+1, rest-i*freq[pos].first);
        }
        for( int i =1;i<= most; ++i){
            sequence.pop_back();
        }

    }
    

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        
        for(int num: candidates){
            if(freq.empty()||num != freq.back().first){
                freq.emplace_back(num,1);
            }else{
                ++freq.back().second;
            } 
        }
        dfs(0,target);
        return ans;
    }
};
```

