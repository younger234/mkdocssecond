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