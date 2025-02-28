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