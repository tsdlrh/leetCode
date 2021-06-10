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

**f方法一：二分查找法**
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

**方法二：牛顿迭代法**

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




### 2. 移除元素

#### （1）27.移除元素

题目链接：https://leetcode-cn.com/problems/remove-element/submissions/

```Java
class Solution {
    public int removeElement(int[] nums, int val) {
     //双指针法，遍历数组，如果快指针指向的数字==val，此时快指针加一，否则慢指针加一
     int n = nums.length;
     if(n==0)
       return 0;
     int slow = 0,fast = 0;
     while(fast<n){
         if(nums[fast]!=val){
             nums[slow]=nums[fast];
             slow++;
         }
         fast++;
     } 
     return slow; 
    }
}
```

#### (2)26.删除有序数组中的重复项

题目链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/submissions/

```Java
class Solution {
    public int removeDuplicates(int[] nums) {
      //双指针
      int n = nums.length;
      int slow =1,fast=1;
      while(fast<n){
          if(nums[fast]!=nums[fast-1]){
              nums[slow] = nums[fast];
              slow++;
          }
          fast++;
      }
      return slow;
    }
}
```


#### （3）283.移动零

题目链接：https://leetcode-cn.com/problems/move-zeroes/

```Java
class Solution {
    public void moveZeroes(int[] nums) {
     int n = nums.length;
     //双指针，去除0，之后再加上0
     int slow = 0,fast = 0;
     while(fast<n){
         if(nums[fast]!=0){
             nums[slow] = nums[fast];
             slow++;
         }
         fast++;
     }
    for(int i=slow;i<n;i++){
        nums[i] = 0;
    }
  
    }
}
```


#### （4）844.比较含退格的字符串

题目链接：https://leetcode-cn.com/problems/backspace-string-compare/submissions/

**方法一：重构字符串**
```Java
class Solution {
    public boolean backspaceCompare(String s, String t) {
    //双指针，去除空格后比较
    return build(s).equals(build(t));
    }

    //输入字符串，输出删除空格后的字符串
    public String build(String str){
        StringBuffer ret = new StringBuffer();
        int length = str.length();
        for(int i=0;i<length;i++){
            char ch = str.charAt(i);
            if(ch!='#'){
                ret.append(ch);
            }else{
                if(ret.length()>0){
                    ret.deleteCharAt(ret.length()-1);
                }
            }
        }
        return ret.toString();
    }
}
```
**方法二：双指针**

```Java
class Solution {
    public boolean backspaceCompare(String s, String t) {
      //使用双指针
/*一个字符是否会被删掉，只取决于该字符后面的退格符，而与该字符前面的退格符无关。因此当我们逆序地遍历字符串，就可以立即确定当前字符是否会被删掉。
具体地，我们定义skip 表示当前待删除的字符的数量。每次我们遍历到一个字符：
若该字符为退格符，则我们需要多删除一个普通字符，我们让skip 加 1；
若该字符为普通字符：
若 skip 为 0，则说明当前字符不需要删去；
若 skip 不为 0，则说明当前字符需要删去，我们让 skip 减 11。
这样，我们定义两个指针，分别指向两字符串的末尾。每次我们让两指针逆序地遍历两字符串，直到两字符串能够各自确定一个字符，
然后将这两个字符进行比较。重复这一过程直到找到的两个字符不相等，或遍历完字符串为止。
 */

    int i=s.length()-1,j=t.length()-1;
    int skipS = 0,skipT = 0;//skip代表当前待删除的字符串的数量，逆序遍历，如果遇到退格符，则加1
     while(i>=0||j>=0){
         //如果s字符串遍历结束
         while(i>=0){
             if(s.charAt(i)=='#'){
                 skipS++;//如果遇到退格，加一
                 i--;//向前遍历
             }else if(skipS>0){
                 //如果遇到字符串，且skipS大于0，说明字符串前面有退格，表示该字符串将被删去
                 //此时删去字符，skipS--
                 skipS--;
                 i--;
             }else{
                 break;
             }
         }
 
         //同理，如果t字符串遍历结束
         while(j>=0){
             if(t.charAt(j)=='#'){
                 skipT++;
                 j--;
             }else if(skipT>0){
                 skipT--;
                 j--;
             }else{
                 break;
             }
         }

         //如果s和t字符串同时遍历结束
         if(i>=0 && j>=0){
             //比较这两个字符串所对应位置的字符
             //如果有不同的字符，则需要返回false
             if(s.charAt(i)!=t.charAt(j))
             return false;
         }else{
             //如果其中有字符串遍历结束，返回false;
             if(i>=0||j>=0){
                 return false;
             }
         }
         i--;
         j--;

     }
     return true;//如果在上面循环中没有返回false,则说明两个字符串相等

    }
}
```

