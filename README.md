# leetCode
leetcode 

## 一、有关数组的题目 

### 1. 二分查找法的应用

#### （1）704. 二分查找

题目链接：https://leetcode-cn.com/problems/binary-search/

**C++：**
```C++
// 版本一
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1; // 定义target在左闭右闭的区间里，[left, right]
        while (left <= right) { // 当left==right，区间[left, right]依然有效，所以用 <=
            int middle = left + ((right - left) / 2);// 防止溢出 等同于(left + right)/2
            if (nums[middle] > target) {
                right = middle - 1; // target 在左区间，所以[left, middle - 1]
            } else if (nums[middle] < target) {
                left = middle + 1; // target 在右区间，所以[middle + 1, right]
            } else { // nums[middle] == target
                return middle; // 数组中找到目标值，直接返回下标
            }
        }
        // 未找到目标值
        return -1;
    }
};

```


```C++
// 版本二
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size(); // 定义target在左闭右开的区间里，即：[left, right)
        while (left < right) { // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            int middle = left + ((right - left) >> 1);
            if (nums[middle] > target) {
                right = middle; // target 在左区间，在[left, middle)中
            } else if (nums[middle] < target) {
                left = middle + 1; // target 在右区间，在[middle + 1, right)中
            } else { // nums[middle] == target
                return middle; // 数组中找到目标值，直接返回下标
            }
        }
        // 未找到目标值
        return -1;
    }
};
```

#### 其他语言版本

**Java：**

（版本一）左闭右闭区间

```java
class Solution {
    public int search(int[] nums, int target) {
        // 避免当 target 小于nums[0] nums[nums.length - 1]时多次循环运算
        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else if (nums[mid] > target)
                right = mid - 1;
        }
        return -1;
    }
}
```

（版本二）左闭右开区间

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else if (nums[mid] > target)
                right = mid;
        }
        return -1;
    }
}
```

**Python：**

（版本一）左闭右闭区间

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left <= right:
            middle = (left + right) // 2

            if nums[middle] < target:
                left = middle + 1
            elif nums[middle] > target:
                right = middle - 1
            else:
                return middle
        return -1
```

（版本二）左闭右开区间

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left,right  =0, len(nums)
        while left < right:
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid+1
            elif nums[mid] > target:
                right = mid
            else:
                return mid
        return -1
```



#### （2）35. 搜索插入位置

题目链接：https://leetcode-cn.com/problems/search-insert-position/

**Java版本：**
（版本一）左闭右闭区间
```Java
class Solution {
    public int searchInsert(int[] nums, int target) {
     int n = nums.length;
     int left = 0,right=n-1;
     int ans = n;
     //如果不存在数组中，将返回按顺序插入的位置pos
     //则有 nums[pos-1]<target<=nums[pos]
     //也就是遍历有序数组，找到第一个大于等于target的pos值即可，使用二分法
     //此时为左区间闭，右区间闭的情况
     while(left<=right){
         int mid = ((right-left)>>1)+left;//防止溢出
         if(nums[mid]>=target){//每次循环，逐次逼近第一个大于等于target的mid值,此时的mid值便是pos值，也就是target将要插入的位置
             ans = mid;
             right = mid -1;//[left,right]->[left,mid-1],说明
         }
         else{
             left = mid+1;//[left,right]->[mid+1,right]
         }
    
     }
     return ans;
    }
}

```
（版本二）左闭右开区间
```Java
class Solution {
    public int searchInsert(int[] nums, int target) {
     int n = nums.length;
     int left = 0,right=n;
     int ans = n;
     //如果不存在数组中，将返回按顺序插入的位置pos
     //择优 nums[pos-1]<target<=nums[pos]
     //也就是遍历有序数组，找到第一个大于等于target的pos值即可，使用二分法
     //此时为左区间闭，右区间开的情况
     while(left<right){
         int mid = ((right-left)>>1)+left;//防止溢出
         if(nums[mid]>=target){//每次循环，逐次逼近第一个大于等于target的mid值,此时的mid值便是pos值，也就是target将要插入的位置
             ans = mid;
             right = mid;//[left,right)>[left,mid)
         }
         else{
             left = mid+1;//[left,right)->[mid+1,right)
         }
    
     }
     return ans;
    }
}
```


#### （3）34.在排序数组中查找元素的第一个和最后一个位置

题目链接：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/


**Java：**
```Java
class Solution {
    //分析题目，可以得到，
    //Leftidx满足:nums[leftIdx-1]<target<=nums[leftIdx],代表第一个等于target的位置
    //rightIdx满足：nums[rightIdx]<=target<nums[rightIdx+1],代表第一个大于target的位置-1
    public int[] searchRange(int[] nums, int target) {
        int leftIdx = binarySearch(nums, target, true);//输出第一个等于target的位置
        int rightIdx = binarySearch(nums, target, false) - 1;//输出第一个大于target的位置-1
        if (leftIdx <= rightIdx && rightIdx < nums.length && nums[leftIdx] == target && nums[rightIdx] == target) {
            return new int[]{leftIdx, rightIdx};
        } 
        return new int[]{-1, -1};
    }

    //二分查找法，输入，Nums数组，target,
    //lower=true表示nums[mid] >= target,此时循环判断输出为第一个等于target的位置
    //lower=false表示nums[mid] > target，此时循环判断输出为第一个大于target的位置
    public int binarySearch(int[] nums, int target, boolean lower) {
        int left = 0, right = nums.length - 1, ans = nums.length;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
}
```

#### (4)69.x的平方根

题目链接：https://leetcode-cn.com/problems/sqrtx/submissions/

**f方法一：二分查找法 **
```Java
class Solution {
    public int mySqrt(int x) {
     //在0-x的整数中寻找一个数，它是的平方是最大的小于等于x的值
     int left = 0, right = x;
     int res = x;
    while(left<=right){
        int mid = left+((right-left)>>2);
        if(（long）mid*mid<=x){//long防止x的值大于Integer.MAX_VALUE
           res = mid;
           left = mid +1;
        }else{
           right = mid -1;
        }
    }
    return res;
    }
}
```

**方法二：牛顿迭代法 **

```Java
class Solution {
    public int mySqrt(int x) {
        if(x==0)
         return 0;
         double C=x,x0=x;
         while(true){
             double xi = 0.5*(x0+C/x0);
             if(Math.abs(x0-xi)<1e-7){
                 break;
             } 
             x0 = xi;
         }
        
        return (int)x0;
    }
}
```

#### (5)367.有效的完全平方数

题目链接：https://leetcode-cn.com/problems/valid-perfect-square/submissions/


**方法一：二分查找法**
```Java
class Solution {
    public boolean isPerfectSquare(int num) {
      //在0-num中，使用二分查找法，查找一个数的平方等于Num
      int left =0,right=num,res=-1;
      while(left<=right){
          int mid = left+((right-left)>>2);
          if((long)mid*mid>num){
              right = mid-1;
          }else if(mid*mid<num){
              left = mid+1;
          }else{
             return true;
          }
      }
      return false;
    }
}
```

**方法二：牛顿迭代法**
```Java
class Solution {
    public boolean isPerfectSquare(int num) {
      //使用牛顿迭代法，快速逼近平方根数，如果这个数存在，则返回true，否则false
    
      if(num<2){//num=0,1都是完全平方数
          return true;
      }
      long C = num,x0 = num/2;
      while(x0*x0>num){//循环的跳出条件是x0*x0<=num
         long xi = (x0+C/x0)/2;
          x0 = xi;
      }
      return (x0*x0==num);
    }
}
```
