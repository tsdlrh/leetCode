# leetCode
leetcode 
 * [一、有关数组的题目](#一有关数组的题目)
    * [1\. 二分查找法的应用](#1-二分查找法的应用)
      * [（1）704\. 二分查找](#1704-二分查找)
      * [其他语言版本](#其他语言版本)
      * [（2）35\. 搜索插入位置](#235-搜索插入位置)
      * [（3）34\.在排序数组中查找元素的第一个和最后一个位置](#334在排序数组中查找元素的第一个和最后一个位置)
      * [（4）69\.x的平方根](#469x的平方根)
      * [（5）367\.有效的完全平方数](#5367有效的完全平方数)
    * [2\. 移除元素](#2-移除元素)
      * [（1）27\.移除元素](#127移除元素)
      * [（2）26\.删除有序数组中的重复项](#226删除有序数组中的重复项)
      * [（3）283\.移动零](#3283移动零)
      * [（4）844\.比较含退格的字符串](#4844比较含退格的字符串)
      * [（5）977\.有序数组的平方](#5977有序数组的平方)
    * [3\.滑动窗口](#3滑动窗口)
      * [（1）209\.长度最小的数组](#1209长度最小的数组)
      * [（2）904\.水果成篮](#2904水果成篮)
      * [（3）76\.最小覆盖子串](#376最小覆盖子串)
    * [4、螺旋矩阵](#4螺旋矩阵)
      * [（1）59、螺旋矩阵II](#159螺旋矩阵ii)
      * [（2）54、螺旋矩阵](#254螺旋矩阵)
      * [（3）29、顺时针打印矩阵](#329顺时针打印矩阵)
  * [二、链表](#二链表)
    * [（1）203\.移除俩表](#1203移除俩表)
    * [（2）707、设计链表](#2707设计链表)
    * [(3) 206、反转链表](#3-206反转链表)
    * [(4) 24、两两交换链表中的节点](#4-24两两交换链表中的节点)
    * [(5)19、删除链表中倒数第N个节点](#519删除链表中倒数第n个节点)
    * [(6)142\.环形链表](#6142环形链表)
  * [三、哈希表](#三哈希表)
    * [1、异位词](#1异位词)
      * [（1）242、有效的字母异位词](#1242有效的字母异位词)
      * [（2）383、赎金信](#2383赎金信)
      * [（3）242、字母异位词分组](#3242字母异位词分组)
      * [(4)438、找到字符串中所有字母异位词](#4438找到字符串中所有字母异位词)
    * [2、数组交集](#2数组交集)
      * [（1）349、两个数组的交集](#1349两个数组的交集)
      
### 一、有关数组的题目 

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

**Python版本**
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        n = len(nums)
        left,right,ans=0,n-1,n

        while left<=right:
            mid = (right+left)//2
            if nums[mid]>=target:
                ans=mid
                right=mid-1
            else:
                left=mid+1
        return ans                       
             
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
**Python版本**
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        n=len(nums)
        def binarySearch(nums,target,flag):
            left,right,ans=0,n-1,n
            while left<=right:
                mid=(left+right)//2
                if (nums[mid]>target)or(flag and nums[mid]>=target):
                    ans=mid
                    right=mid-1
                else:
                    left=mid+1
            return ans

        leftindex=binarySearch(nums,target,flag=True)
        rightindex=binarySearch(nums,target,flag=False)-1
        if leftindex<=rightindex and nums[leftindex]==target and nums[rightindex]==target:
            return [leftindex,rightindex]
        else:
            return [-1,-1]  
```


#### （4）69.x的平方根

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
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        left,right,ans=0,x,x
        while left<=right:
            mid=(left+right)//2
            if mid*mid<=x:
                ans=mid
                left=mid+1
            else:
                right=mid-1
        return ans           
···
        

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
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x==0:
            return x
        C,x0=x,x
        while(True):
            x1 = 0.5*(x0+C/x0)
            if abs(x0-x1)<1e-7:
                break
            x0=x1
        return int(x0)        
```        
        
       

#### （5）367.有效的完全平方数

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
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        left,right=0,num
        while left<=right:
            mid=(left+right)//2
            if mid*mid>num:
                right=mid-1
            elif mid*mid<num:
                left=mid+1
            else:
                return True
        return False                
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
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num<2:
            return True
        C,x0=num,num//2
        while x0*x0>num:
            x1=(x0+C/x0)//2
            x0=x1
        return x0*x0==num    
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

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        if len(nums)==0:
            return 0
        slow,fast=0,0
        while(fast<len(nums)):
            if nums[fast]!=val:
                nums[slow]=nums[fast]
                slow +=1
            fast+=1
        return slow
 ```

#### （2）26.删除有序数组中的重复项

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

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums)==0:
            return 0
        slow,fast=1,1
        while fast<len(nums):
            if nums[fast]!=nums[fast-1]:
                nums[slow]=nums[fast]
                slow+=1
            fast+=1
        return slow      
           
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
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n=len(nums)
        slow,fast=0,0
        while fast<n:
            if nums[fast]!=0:
                nums[slow]=nums[fast]
                slow+=1
            fast+=1
        for i in range(slow,n):
            nums[i]=0
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

```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        def build(s:str)->str:
            res=list()
            for ch in s:
                if ch!='#':
                    res.append(ch)
                elif res:
                    res.pop()
            return "".join(res)
        return build(s)==build(t)                
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
若 skip 不为 0，则说明当前字符需要删去，我们让 skip 减 1。
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

```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        i,j=len(s)-1,len(t)-1
        skipS=skipT=0
        while i>=0 or j>=0:
            while i>=0:
                if s[i]=="#":
                    skipS+=1
                    i-=1
                elif skipS>0:
                    skipS-=1
                    i-=1
                else:
                    break
            while j>=0:
                if t[j]=="#":
                    skipT+=1
                    j-=1
                elif skipT>0:
                    skipT-=1
                    j-=1
                else:
                    break
            if i>=0 and j>=0:
                if s[i]!=t[j]:
                    return False
            elif i>=0 or j>=0:
                return False
            i-=1
            j-=1
        return True

```

#### （5）977.有序数组的平方

题目链接：https://leetcode-cn.com/problems/squares-of-a-sorted-array/submissions/

**方法一：使用归并排序法**
```Java
class Solution {
    public int[] sortedSquares(int[] nums) {
     //数组中有负数和正数
     //负数的平方，排序为降序，正数的平方排序为升序
     //两个有序数组，使用归并排序即可
    int n = nums.length;
     //第一步，先找到正数和负数的分界线nums[neg]  
     //nums[0]-nums[neg]<0 nums[neg+1]-nums[n-1]>0
     int neg = -1;//初始化为-1
     for(int i=0;i<n;i++){
         if(nums[i]<0){
             neg = i;//找到分界点
         }else{
             break;
         }
     }
     //第二步，对两个有序数组进行归并排序
     //负数的平方组成的有序数组从nums[0]-nums[neg]，这是降序排列的 nums[neg]-nums[0]是负数平方的升序排列
     //正数的平方组成的有序数组从nums[neg+1]-nums[n-1]
     //指针i指向负数升序排列的第一个，指针j指针正数升序排列的第一个，比较两者的大小，较小的放入ans[]数组中
     int[] ans = new int[n];
     int index =0,i=neg,j=neg+1;
      while(i>=0||j<n){//i降序，每次循环减一，下限为0，j升序，每次循环加一，上限为n
        if(i<0){//i<0表示原数组没有负数，全部是正数，此时直接讲nums[j]平方即可
            ans[index] = nums[j]*nums[j];
            j++;
        }else if(j==n){//j=n表示原数组没有正数，全市负数，i从最小的负数开始遍历，此时nums[i]的平方是最小的，i--，nums[i]的平方增加
            ans[index] = nums[i]*nums[i];
            i--;
        }else if(nums[i]*nums[i]<nums[j]*nums[j]){//此时表示原数组，正负数都存在，取两个有序数组中最小的放入新数组中
           ans[index] = nums[i]*nums[i];
           i--;
        }else{
            ans[index] = nums[j]*nums[j];
            j++;
        }

        index++;//每次循环，新数组中增加一个平方数

      }
      return ans;
    }
}
```

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        n=len(nums)
        neg=-1
        #计算得到正负数的分界点
        for i in range(0,n):
            if nums[i]<0:
                neg=i
            else:
                break
        ans=[0]*n
        index,i,j=0,neg,neg+1
        while i>=0 or j<n:
            if i<0:#说明没有负数
               ans[index]=nums[j]**2
               j+=1
            elif j==n:#说明没有正数
               ans[index]=nums[i]**2
               i-=1
            elif nums[i]**2<nums[j]**2:
               ans[index]=nums[i]**2
               i-=1
            else:
               ans[index]=nums[j]**2
               j+=1
            index+=1
        return ans      
```        
**方法二：使用左右指针法**

```Java
class Solution {
    public int[] sortedSquares(int[] nums) {
     //双指针法
     //平方后的数组最大值，或者在最右端或者在最左端
     //考虑使用双指针，一个指向起始位置，一个指向终止位置
     int right = nums.length-1;
     int left = 0;
     int[] res = new int[nums.length];
     int index = res.length-1;

     while(left<=right){
         if(nums[left]*nums[left]>nums[right]*nums[right]){
             res[index--] = nums[left]*nums[left];
             left++;
         }else{
             res[index--] = nums[right]*nums[right];
             right--;
         }
     }
     return res;
    }
}
```

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        #左右指针
        n=len(nums)
        left,right,index=0,n-1,n-1
        res=[0]*n

        while left<=right:
            if nums[left]**2>nums[right]**2:
                res[index]=nums[left]**2
                index-=1
                left+=1
            else:
                res[index]=nums[right]**2
                index-=1
                right-=1
        return res            
```


### 3.滑动窗口

#### （1）209.长度最小的数组

题目链接：https://leetcode-cn.com/problems/minimum-size-subarray-sum/submissions/

**使用滑动窗口法**

C++版本
```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
    int result = INT32_MAX;
    int sum = 0;//滑动窗口的数值之和
    int i = 0;//活动窗口的起始位置
    int sublength = 0;//滑动窗口的长度
    for(int j=0;j<nums.size();j++){
        sum += nums[j];
        //这里使用while，每次更新i起始位置，并不断比较子序列是否符合条件
        while(sum>=target){
            sublength = (j-i+1);//取子序列的长度
            result = result<sublength ? result : sublength;
            sum -= nums[i++];//这是体现滑动窗口的精髓，不断变更i的位置
        }
        
    }
    //如果result没有被赋值的话，返回0，否则返回符合条件的最小子序列
    return result == INT32_MAX ? 0: result;
    }
};
```

Java版本

```Java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
      int n = nums.length;
      int res = Integer.MAX_VALUE;
      int i = 0,sum =0,subLength = 0;
      for(int j = 0;j<n;j++){
        sum += nums[j];
        while(sum>=target){
          subLength = (j-i+1);//获取子序列的长度
          res = res<subLength ? res:subLength;
          sum -= nums[i++];//体现了滑动窗口的精髓，每次更新i的位置
        }
      }
      return res == Integer.MAX_VALUE ? 0 :res;
    }
}
```

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        n=len(nums)
        res,i,Sum,sublen=n+1,0,0,0
        for j in range(0,n):
            Sum+=nums[j]
            while Sum>=target:
                sublen=j-i+1
                res= res if res<sublen else sublen
                Sum-=nums[i]
                i+=1
        return 0 if res==n+1 else res    
```
#### （2）904.水果成篮

题目链接 ：https://leetcode-cn.com/problems/fruit-into-baskets/submissions/

**滑动窗口+哈希表**

```Java
class Solution {
    public int totalFruit(int[] tree) {
     int n = tree.length;//树的长度
     int res =0,i = 0;//res是窗口的长度，i是起始位置
     Counter count = new Counter();//新创建了衣蛾count类，重写了add和get方法
     for(int j=0;j<n;j++){//j代表窗口的终止位置
         count.add(tree[j],1);//将第一个树的类型，对应的value=1放入mao中
         while(count.size()>=3){//当时counte中的种类超过2时，跳出循环，比较大小
             count.add(tree[i],-1);//将起始位置i的类型，对应的value=-1，加入counter中
             if(count.get(tree[i])==0){//如果value等于0，有重复的类型，删去上一个类型
               count.remove(tree[i]);
            }
            i++;//起始位置加一
         }
         res = res> j-i+1 ? res : j-i+1; //返回最大致
     }
     return res;
    }


//Counter类继承了HashMap，key值不可重复，如果重复覆盖前一个值
class Counter extends HashMap<Integer,Integer>{
    //重写了get方法，如果Map存在k，则返回k对应的value值，,否则返回0，也就是不存在
    public int get(int k){
        return containsKey(k)?super.get(k):0;
    }

    //重写add方法，将key 和 key对应的value + v之和得值放入新的key,value中
    public void add(int k,int v){
        put(k,get(k)+v);
    }
}
}

```

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        ans=i=0
        count=collections.Counter()
        for j,x in enumerate(fruits):
            count[x]+=1
            while len(count)>=3:
                count[fruits[i]]-=1
                if count[fruits[i]]==0:
                    del count[fruits[i]]
                i+=1
            ans=max(ans,j-i+1)
        return ans            
  ```      

#### （3）76.最小覆盖子串

题目链接：https://leetcode-cn.com/problems/minimum-window-substring/

```Java
class Solution {
     //使用滑动窗口
     //借助哈希表
     Map<Character,Integer> ori = new HashMap<Character,Integer>();
     Map<Character,Integer> cnt = new HashMap<Character,Integer>();

     public String minWindow(String s, String t){
         int tLen = t.length();
         //将t字符创存入哈希表中，key对应字符，value对应字符出现的次数，返回ori哈希表
         for(int i=0;i<tLen;i++){
             char c = t.charAt(i);
             //getOrDefault() 方法获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值。

             ori.put(c,ori.getOrDefault(c,0)+1);
         }   
    //双指针，l用于收缩窗口，r用于延伸窗口 窗口的位置[l,r]
     int l = 0,r=-1;
     //初始化最小窗口的长度len,左边窗口的位置ansL,右边窗口的位置ansR
     int len = Integer.MAX_VALUE,ansL = -1,ansR = -1;
     int slen = s.length();
     //延伸指针循环遍历s字符创
     while(r<slen){
         //每次循环r增加一
         ++r;
         //如果延伸指针在字符创长度内，并且s字符串该位置对应的字符，在t字符哈希表中
         if(r<slen && ori.containsKey(s.charAt(r))){
            //cnt用于存储所有包含t字符的字符串
            //将s字符串对应的该位置r的对应字符，以及字符出现的次数放入cnt哈希表中
             cnt.put(s.charAt(r),cnt.getOrDefault(s.charAt(r),0)+1);
         }
         //check用于判断cnt中字符出现的次数是不是小于ori中对应字符出现的次数
         while(check() && l<=r){
             //r-l+1为滑动窗口的长度
             if(r-l+1<len){
                 len = r-l+1;//子字符串的最小长度为len
                 ansL = l;//子字符串窗口的左边位置
                 ansR = l+len;//子字符串窗口的右边位置
             }
             //如果ori总共包含子字符串最左边的字符
             if(ori.containsKey(s.charAt(l))){
                 //收缩窗口
                 //cnt哈希表中最左边的字符所对应的次数减一，表示窗口向右滑动
                 cnt.put(s.charAt(l),cnt.getOrDefault(s.charAt(l),0)-1);
             }
             //窗口的起始位置每次循环加一
             ++l;
         }
     }
     //如果ansl没有变化，说明s字符串不包含涵盖t字符串的子串，返回空串，否则返回子串
     return ansL == -1?"":s.substring(ansL,ansR);

     }
 
    //判断cnt中字符出现的次数是不是小于ori中对应字符出现的次数
    public boolean check(){
        //entrySet()返回set视图，诸如[1=Google, 2=Runoob, 3=Taobao]
        //iterator()获取Set视图的迭代器，用于每次循环使用
        Iterator iter = ori.entrySet().iterator();
        //循环HashMap,如果map存在元素
        while(iter.hasNext()){
            //Map.Entry是Map的一个内部接口,具有getKey(),getValue()方法
            Map.Entry entry = (Map.Entry) iter.next();
            //返回key值
            Character key = (Character) entry.getKey();
            //返回val值
            Integer val = (Integer)entry.getValue();
            //如果滑动窗口哈希表中对应的字符出现的次数小于t字符串哈希表中的出现的次数，返回false,否则为true
            if(cnt.getOrDefault(key,0)<val){
                return false;
            }
        }
        return true;
    }

 }
 ```
 
 ```python
 def minWindow(self, s: str, t: str) -> str:
        need=collections.defaultdict(int)
        for c in t:
            need[c]+=1
        needCnt=len(t)
        i=0
        res=(0,float('inf'))
        for j,c in enumerate(s):
            if need[c]>0:
                needCnt-=1
            need[c]-=1
            if needCnt==0:       #步骤一：滑动窗口包含了所有T元素
                while True:      #步骤二：增加i，排除多余元素
                    c=s[i] 
                    if need[c]==0:
                        break
                    need[c]+=1
                    i+=1
                if j-i<res[1]-res[0]:   #记录结果
                    res=(i,j)
                need[s[i]]+=1  #步骤三：i增加一个位置，寻找新的满足条件滑动窗口
                needCnt+=1
                i+=1
        return '' if res[1]>len(s) else s[res[0]:res[1]+1]    #如果res始终没被更新过，代表无满足条件的结果
```

### 4、螺旋矩阵

#### （1）59、螺旋矩阵II

题目链接：https://leetcode-cn.com/problems/spiral-matrix-ii/submissions/


```Java
class Solution {
    public int[][] generateMatrix(int n) {
     int[][] res = new int[n][n];
     int offset = 1;//控制每一圈循环的长度
     int startX = 0;//初始化行
     int startY = 0;//初始化列
     int num = 1;//初始化填充数字
     int mid = n/2;//中间需要填充数字的位置
     int loop = n/2;//需要循环的圈数
     while(loop>0){

         int i = startX;
         int j = startY;

         //上，从左到右
         for(;j<startY+n-offset;j++){
             res[startX][j] = num++;
         }
         //右侧，从上到下
         for(;i<startX+n-offset;i++){
             res[i][j] = num++;
         }
         //下侧，从右到左
         for(;j>startY;j--){
             res[i][j] = num++;
         }
         //左侧，从下到上
         for(;i>startX;i--){
             res[i][j] = num++;
         }

         startX++;
         startY++;

         offset += 2;
         loop--;

     }
    //如果n为奇数，中间的值需要另外填充
     if(n%2==1){
         res[mid][mid] = num;
     }
     return res;
    }
}
```

#### （2）54、螺旋矩阵

题目连接：https://leetcode-cn.com/problems/spiral-matrix/submissions/

```Java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
     //按层模拟
     //首先输出最外层，其次输出次外层，最后输出最内层
     //对于每层，从左上方开始顺时针遍历
     //上：
     List<Integer> order = new ArrayList<Integer>();
     if(matrix==null || matrix.length==0 || matrix[0].length==0){
         return order;//判断非空条件
     }

     int rows = matrix.length;
     int cols = matrix[0].length;

     int left = 0,right = cols-1,top = 0,bottom = rows-1;
     while(left<=right && top<=bottom){
         //最上侧，从左往右  
         for(int col=left;col<=right;col++){
          order.add(matrix[top][col]);
         }
         //最右侧，从上到下
         for(int row = top+1;row<=bottom;row++){
             order.add(matrix[row][right]);
         }
         //如果left<right且top<bottom,则从右到左，遍历下面元素
         if(left<right && top<bottom){
             //则从右到左，遍历下面元素
             for(int col = right-1;col>left;col--){
              order.add(matrix[bottom][col]);
             }
             //从下到上，遍历左侧元素
             for(int row=bottom;row>top;row--){
                 order.add(matrix[row][left]);
             }

         }

         left++;
         right--;
         top++;
         bottom--;
     }
     return order;

    }
}
```

```python
class Solution:
    def spiralOrder(matrix):
        if not matrix or not matrix[0]:
            return list()
        rows,colums=len(matrix),len(matrix[0])
        visited=[[False]*colums for _ in range(rows)]#辅助矩阵，标记该位置是否已经被访问过
        total = rows*colums
        order=[0]*total

        dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # 控制顺时针旋转
        r,c,dirIdx=0,0,0
        for i in range(total):
            order[i]=matrix[r][c]
            visited[r][c]=True
            dx,dy=dirs[dirIdx]
            nextR,nextC=r+dx,c+dy
            if nextR<0 or nextR>=rows or nextC<0 or nextC>=colums or visited[nextR][nextC]:
                dirIdx=(dirIdx+1)%4
                dx, dy = dirs[dirIdx]
            r,c=r+dx,c+dy
        return order
```        

#### （3）29、顺时针打印矩阵

题目连接：https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/submissions/

```Java

class Solution {
    public int[] spiralOrder(int[][] matrix) {
        //判断非空条件
        if(matrix==null || matrix.length==0 || matrix[0].length ==0){
            return new int[0];
        }

        int rows  = matrix.length;//矩阵的行数
        int cols = matrix[0].length;//矩阵的列数

        int [] res = new int[rows*cols];//存储遍历值的矩阵

        int n = 0;//需要填充的位置
        int left=0,right=cols-1,top=0,bottom=rows-1;
        
        while(left<=right && top<=bottom){
            //上侧，从左到右
            for(int col = left;col<=right;col++){
                res[n++] = matrix[top][col];
            }
            //右侧，从上到下
            for(int row = top+1;row<=bottom;row++){
                res[n++] = matrix[row][right];
            }

            //判断left<right和top<bottom
            if(left<right && top<bottom){
                //下侧，从右到左
                for(int col = right-1;col>left;col--){
                    res[n++] = matrix[bottom][col];
                }
                //左侧，从下到上
                for(int row = bottom;row>top;row--){
                    res[n++] = matrix[row][left];
                }
            }

            left++;
            top++;
            right--;
            bottom--;
        }
        return res;
    }
}
```

```python
class Solution:
    def spiralOrder(matrix):
        if not matrix or not matrix[0]:
            return list()
        rows,colums=len(matrix),len(matrix[0])
        visited=[[False]*colums for _ in range(rows)]#辅助矩阵，标记该位置是否已经被访问过
        total = rows*colums
        order=[0]*total

        dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # 控制顺时针旋转
        r,c,dirIdx=0,0,0
        for i in range(total):
            order[i]=matrix[r][c]
            visited[r][c]=True
            dx,dy=dirs[dirIdx]
            nextR,nextC=r+dx,c+dy
            if nextR<0 or nextR>=rows or nextC<0 or nextC>=colums or visited[nextR][nextC]:
                dirIdx=(dirIdx+1)%4
                dx, dy = dirs[dirIdx]
            r,c=r+dx,c+dy
        return order
```        
        
        


## 二、链表

### （1）203.移除俩表
题目链接：https://leetcode-cn.com/problems/remove-linked-list-elements/submissions/

 ``` Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        //判断链表非空
        if(head==null){
            return head;
        }

        //借助虚拟节点
        ListNode dummy = new ListNode(-1,head);
        ListNode pre = dummy;
        ListNode cur = head;
        while(cur!=null){
            if(cur.val==val){
                pre.next = cur.next;
            }else{
                pre = cur;
            }
            cur = cur.next;
        }
        return dummy.next;
    }
}
```

```python
class Solution:
    def removeElements(head:ListNode,val:int):
        dummy=ListNode(0)
        dummy.next=head
        p=dummy
        while p is not None:
            if p.next and p.next.value == val:
                p.next=p.next.next
            else:
                p=p.next
        return dummy.next
```        

### （2）707、设计链表

题目链接:https://leetcode-cn.com/problems/design-linked-list/submissions/

```Java
class MyLinkedList {

    int size;//初始化链表的元素
    ListNode head;//虚拟化头结点
    /**初始化链表 */
    public MyLinkedList() {
      size = 0;
      head = new ListNode(0);

    }
    
    /** 获取链表中第 index 个节点的值。如果索引无效，则返回-1 */
    public int get(int index) {
      if(index>=size || index<0){
          return -1;
      }
      ListNode cur = head;
      for(int i=0; i<=index; i++){
          cur = cur.next;
      }
      return cur.val;
    }
    
    /** 在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点 */
    public void addAtHead(int val) {
      addAtIndex(0,val);
    }
    
    /**将值为 val 的节点追加到链表的最后一个元素 */
    public void addAtTail(int val) {
      addAtIndex(size,val);
    }
    
    /** 在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点 */
    public void addAtIndex(int index, int val) {
     if(index>size){
         return;
     }
     if(index<0){
         index = 0;
     }
     size++;
     ListNode cur = head;
     for(int i=0;i<index;i++){
         cur = cur.next;
     }
     ListNode add = new ListNode(val);
     add.next = cur.next;
     cur.next = add;
    }
    
    /** 如果索引 index 有效，则删除链表中的第 index 个节点 */
    public void deleteAtIndex(int index) {
    if(index>=size ||index<0){
        return;
    }
    size--;
    ListNode cur = head;
    for(int i=0;i<index;i++){
        cur = cur.next;
    }
     cur.next = cur.next.next;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
 ```
 
 ```python
 class MyLinkedList:

    def __init__(self):
        """
        初始化链表
        """
        self.size=0
        self.head=ListNode(0)


    def get(self, index: int) -> int:
        """
       得到第index位置的值
        """
        if index<0 or index>=self.size:
            return -1
        curr=self.head
        for _ in range(index+1):
            curr=curr.next
        return curr.val



    def addAtHead(self, val: int) -> None:
        """
        Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
        """
        self.addAtIndex(0,val)


    def addAtTail(self, val: int) -> None:
        """
        Append a node of value val to the last element of the linked list.
        """
        self.addAtIndex(self.size,val)


    def addAtIndex(self, index: int, val: int) -> None:
        """
        Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
        """
        if index>self.size:
            return

        if index<0:
            index=0

        self.size+=1
        pred=self.head
        for _ in range(index):
            pred=pred.next

        to_add=ListNode(val)
        to_add.next=pred.next
        pred.next=to_add


    def deleteAtIndex(self, index: int) -> None:
        """
        Delete the index-th node in the linked list, if the index is valid.
        """
        if index<0 or index>self.size:
            return
        self.size-=1
        pred=self.head
        for _ in range(index):
            pred=pred.next

        pred.next=pred.next.next
```        
 
 ### (3) 206、反转链表
 题目链接：https://leetcode-cn.com/problems/reverse-linked-list/
 
 
 ```Java
 //双指针法
 class Solution {
    public ListNode reverseList(ListNode head) {
    ListNode tmp = null;
    ListNode pre = null;
    ListNode cur = head;
    while(cur!=null){
        tmp = cur.next;//保存下一个节点
        cur.next = pre;
        pre = cur;
        cur = tmp;
    }
    return pre;

    }
}
```

```python
class Solution:
    def reverseList(head):
        pre=None
        cur=head
        while cur:
            tmp=cur.next
            cur.next=pre
            pre=cur
            cur=tmp
        return pre
```




### (4) 24、两两交换链表中的节点
题目链接：https://leetcode-cn.com/problems/swap-nodes-in-pairs/

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;

        while(pre.next!=null && pre.next.next!=null){
            ListNode tmp1 = pre.next.next.next;
            ListNode tmp2 = pre.next;

            //步骤一
            pre.next = pre.next.next;
            //步骤二
            pre.next.next = tmp2;
            //步骤三
            pre.next.next.next = tmp1;

            //移动两位，进行下一步循环
            pre = pre.next.next;
        }

        return dummy.next;

    }
}
 
```

```python
class Solution:
    def swapPairs(head:ListNode)->ListNode:
        dummy=ListNode(0)
        dummy.next=head
        pre=dummy

        while pre.next and pre.next.next:
            tmp1=pre.next.next.next
            tmp2=pre.next

            #步骤一
            pre.next=pre.next.next
            #步骤二
            pre.next.next=tmp2
            #步骤三
            pre.next.next.next=tmp1

            #移动两位,进行下一步循环
            pre=pre.next.next

        return dummy.next
```


### (5)19、删除链表中倒数第N个节点
题目链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/submissions/

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
     //定义虚拟化头结点
     ListNode dummy = new ListNode(0);
     dummy.next = head;
     ListNode slow = dummy;//初始化慢指针
     ListNode fast = dummy;//初始化快指针

     //先将快指针移动n+1步
     while(n-->0){
         fast = fast.next;
     }
     //之后，快慢指针一起移动，直到快指针指向Null,此时慢指针指向的节点是要删除节点的上一个节点
     ListNode pre = null;
     while(fast!=null){
       pre = slow;
       slow = slow.next;
       fast = fast.next;
     }

     pre.next = slow.next;//删除节点

     return dummy.next;
    }
 ```
 
 ```python
 class Solution:
    def removeNthFromEnd(head:ListNode,n:int)->ListNode:
        #定义虚拟头节点
        dummy=ListNode(0)
        dummy.next=head
        slow=dummy#定义慢指针
        fast=dummy#定义快指针

        #快指针先移动n+1步
        while n>0:
            fast=fast.next
            n-=1

        #之后快慢指针一起移动
        #直到快指针指向NULL,此时 慢指针指向的节点是要删除节点的上一个节点
        pre=ListNode(None)
        while fast:
            pre=slow
            slow=slow.next
            fast=fast.next

        pre.next=slow.next#删除节点
        return dummy.next
```
 
 
 ### (6)142.环形链表
 
 题目链接：https://leetcode-cn.com/problems/linked-list-cycle-ii/submissions/
 
 ```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;

        //快指针走两步，慢指针走一步，两者相遇，即是有环
        while(fast!=null && fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow==fast){//如果有环
             ListNode index1 = fast;
             ListNode index2 = head;
             //两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
             while(index1!=index2){
                 index1 = index1.next;
                 index2 = index2.next;
             }
             return index1;//或者 return index2
            }
        }
        return null;
    }
}
```

```python
class Solution:
    def detectCycle(head:ListNode)->ListNode:
        slow,fast=head,head#定义快慢指针
        #快指针走两步，慢指针走一步，两者相遇即是有环
        while fast and fast.next:
            slow=slow.next
            fast=fast.next.next
            if slow==fast:#此时表示有环的存在
                index1=fast
                index2=head
                #两个指针，从头结点和相遇节点，各走一步，直到相遇，相遇点即为环入口
                while index1!=index2:
                    index1=index1.next
                    index2=index2.next
                return index1
        return None
```

## 三、哈希表

### 1、异位词

#### （1）242、有效的字母异位词
题目链接:https://leetcode-cn.com/problems/valid-anagram/submissions/

```Java
class Solution {
    public boolean isAnagram(String s, String t) {
         //使用哈希表的方法
         int[] record = new int[26];

        //将s字符串中的每一个字符出现的次数，添加+1到数组中
         for(char c:s.toCharArray()){
             record[c-'a'] += 1;
         }
         //将t字符串中每一个字符出现的次数，添加-1到数组中
         for(char c:t.toCharArray()){
             record[c-'a'] -=1;
         }

         for(int i:record){
             //如果数组中次数都是0，说明两个字符串为异位词
             if(i!=0){
                 return false;
             }
         }
         return true;
    }
}
```

```python
class Solution:
    def isAnagram(s:str,t:str)->bool:
        record=[0]*26
        for i in range(len(s)):
            record[ord(s[i])-ord("a")]+=1
        for i in range(len(t)):
            record[ord(t[i])-ord("a")]-=1
        for i in range(26):
            if record[i]!=0:
                return False
        return True
```



#### （2）383、赎金信
题目链接：https://leetcode-cn.com/problems/ransom-note/

``` Java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
     //使用哈希表
     int[] record = new int[26];

     //添加ransdomNote
     for(char c:ransomNote.toCharArray()){
         record[c-'a'] += 1;
     }

     //添加magazine
     for(char c:magazine.toCharArray()){
         record[c-'a'] -= 1;
     }

     for(int i:record){
         //i>=1表示赎金信中有字符没有被杂志覆盖
         if(i>=1){
            return false;
         }
     }
     return true;
    }
}
```

```python
class Solution:
    def canConstruct(self,ransomNote:str,managine:str)->bool:
        n,m=len(ransomNote),len(managine)
        if n>m or n==0:
            return False
        rec=[0]*26
        for i in range(n):
            rec[ord(ransomNote[i])-ord("a")]+=1
        for i in range(m):
            rec[ord(managine[i])-ord("a")]-=1
        for i in range(26):
            if rec[i]>=1:
                return False
        return True
```


#### （3）49、字母异位词分组

题目链接：https://leetcode-cn.com/problems/group-anagrams/submissions/

```Java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
    Map<String,List<String>> map = new HashMap<String,List<String>>();
    for(String str:strs){
        int[] counts = new int[26];
        int length =str.length();
        for(int i=0;i<length;i++){
            counts[str.charAt(i)-'a']++;
        }
        //将每个出现次数大于0的字母和出现的次数按顺序拼接成字符串，作为哈希表的键值
        //比如“eat”的键值为“a1e1t1”    "tea"的键值为“a1e1t1”
        StringBuffer sb = new StringBuffer();
        for(int i=0;i<26;i++){
            if(counts[i]!=0){
                sb.append((char)('a'+i));
                sb.append(counts[i]);
            }
        }

        String key = sb.toString();
        //如果两者的键值相等，调用list.add叠加字符串1+字符串2，否则null+字符串3
        List<String> list = map.getOrDefault(key,new ArrayList<String>());
        list.add(str);
        map.put(key,list);
    }
    return new ArrayList<List<String>>(map.values());
     
    }
}
```

```python
class Solution:
    def groupAnagram(strs:List[str])->List[List[str]]:
        mp=collections.defaultdict(list)

        for st in strs:
            counts = [0]*26
            for ch in st:
                counts[ord(ch)-ord("a")]+=1
            mp[tuple(counts)].append(st)
        return list(mp.values())
```


#### (4)438、找到字符串中所有字母异位词
题目链接：https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/submissions/

```Java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
      int n = s.length(),m=p.length();
      List<Integer> res = new ArrayList<>();
      if(n<m) return res;
      //记录字母出现的次数
      int[] pCnt = new int[26];
      int[] sCnt = new int[26];
      for(int i=0;i<m;i++){
          pCnt[p.charAt(i)-'a']++;
          sCnt[s.charAt(i)-'a']++;
      }
      //如果两者相等，找到第一个异位词索引0
      if(Arrays.equals(sCnt,pCnt)){
          res.add(0);
      }
      //继续遍历s字符串索引为[m,n)的字母，在sCnt中每增加一个新字母，去除一个旧字母
      for(int i=m;i<n;i++){
          sCnt[s.charAt(i-m)-'a']--;
          sCnt[s.charAt(i)-'a']++;
          //如果相等，返回res中的索引值i-m+1
          if(Arrays.equals(sCnt,pCnt)){
              res.add(i-m+1);
          }
      }
      return res;
    }
}
```
```python
class Solution:
    def findAnagrams(s: str, p: str) -> List[int]:
        n,m,res=len(s),len(p),[]
        if n<m: return res
        pcnt=[0]*26
        scnt=[0]*26
        for i in range(m):
            pcnt[ord(p[i])-ord("a")]+=1
            scnt[ord(s[i])-ord("a")]+=1

        if pcnt==scnt:
            res.append(0)

        for i in range(m,n):
            scnt[ord(s[i-m])-ord("a")]-=1
            scnt[ord(s[i])-ord("a")]+=1
            if scnt==pcnt:
                res.append(i-m+1)

        return res
```
### 2、数组交集

#### （1）349、两个数组的交集
题目链接：https://leetcode-cn.com/problems/intersection-of-two-arrays/

```Java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
    if(nums1.length==0 || nums1==null || nums2==null || nums2.length==0){
        return new int[0];
    }

    //创建哈希Set
    Set<Integer> set1 = new HashSet<>();
    Set<Integer> res = new HashSet<>();

    //将Nums1数组放入set1中
    for(int i:nums1){
        set1.add(i);
    }

    //遍历Nums2数组，判断是否有相同的元素
    for(int n:nums2){
        if(set1.contains(n)){
            res.add(n);
        }
    }

    //把set转成数组
    int[] ans = new int[res.size()];
    int index = 0;
    for(int j:res){
        ans[index++] = j;
    }
    return ans;
    }
}
```

```python
class Solution:
    def intersection(nums1:List[int],nums2:List[int])->List[int]:
        result_set=set()
        set1=set(nums1)
        for num in nums2:
            if num in set1:
                result_set.add(num)
        return list(result_set)
```
  
  
  
### 3、快乐数

### （1）202、快乐数
题目链接：https://leetcode-cn.com/problems/happy-number/

```Java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }
        return n == 1;
    }

    private int getNextNumber(int n) {
        int res = 0;
        while (n > 0) {
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }
}
```

```python
class Solution:
    def isHappy(n:int)->bool:
        def get_next(n):
            total_sum=0
            while n>0:
                n,digit=divmod(n,10)
                total_sum+=digit**2
            return total_sum

        #方法一：哈希表
        # seen=set()
        # while n!=1 and n not in seen:
        #     seen.add(n)
        #     n=get_next(n)
        # return n==1

        #方法二：快慢指针
        slow=n
        fast=get_next(n)
        while fast!=1 and slow!=fast:
            slow=get_next(slow)
            fast=get_next(get_next(fast))
        return fast==1

```

### 4、数的和

### （1）1、两数之和
题目链接：https://leetcode-cn.com/problems/two-sum/
```Java
public int[] twoSum(int[] nums, int target) {
    int[] res = new int[2];
    if(nums == null || nums.length == 0){
        return res;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++){
        int temp = target - nums[i];
        if(map.containsKey(temp)){
            res[1] = i;
            res[0] = map.get(temp);
        }
        map.put(nums[i], i);
    }
    return res;
}
```

```python
class Solution:
    def twoSum(nums:List[int],target:int)->List[int]:
        res=dict()
        for i,num in enumerate(nums):
            print("i=",i)
            print("num=",num)
            print("res=",res)
            if target-num in res:
                return [res[target-num],i]
            res[nums[i]]=i
        return []
```

### （2）454、四数相加II
题目链接：https://leetcode-cn.com/problems/4sum-ii/

```Java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int temp;
        int res = 0;
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                temp = i + j;
                if (map.containsKey(temp)) {
                    map.put(temp, map.get(temp) + 1);
                } else {
                    map.put(temp, 1);
                }
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                temp = i + j;
                if (map.containsKey(0 - temp)) {
                    res += map.get(0 - temp);
                }
            }
        }
        return res;
    }
}
```

```python
class Solution:
    def fourSumCount(nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        countAB=collections.Counter(u+v for u in nums1 for v in nums2)
        ans=0
        for u in nums3:
            for v in nums4:
                if -u-v in countAB:
                    ans+=countAB[-u-v]
        return ans
```

### (3)15、三数之和
题目链接：https://leetcode-cn.com/problems/3sum/

```Java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                return result;
            }

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;
            while (right > left) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    
                    right--; 
                    left++;
                }
            }
        }
        return result;
    }
}
```

```python
class Solution:
    def threeSum(nums:List[int])->List[List[int]]:
        ans=[]
        n=len(nums)
        nums.sort()
        for i in range(n):
            left=i+1
            right=n-1
            if nums[i]>0:
                break
            if i>=1 and nums[i]==nums[i-1]:
                continue
            while left<right:
                total=nums[i]+nums[left]+nums[right]
                if total>0:
                    right-=1
                elif total<0:
                    left+=1
                else:
                    ans.append([nums[i],nums[left],nums[right]])
                    while left!=right and nums[left]==nums[left+1]:left+=1
                    while left!=right and nums[right]==nums[right-1]:right-=1
                    left+=1
                    right-=1
        return ans
```


### (4)18、四数之和
题目链接：https://leetcode-cn.com/problems/4sum/

```Java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
       
        for (int i = 0; i < nums.length; i++) {

            if (i > 0 && nums[i - 1] == nums[i]) {
                continue;
            }
            
            for (int j = i + 1; j < nums.length; j++) {

                if (j > i + 1 && nums[j - 1] == nums[j]) {
                    continue;
                }

                int left = j + 1;
                int right = nums.length - 1;
                while (right > left) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum > target) {
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```

```python
class Solution:
    def fourSum(nums:List[int],target:int)->List[List[int]]:
        hashmap=dict()
        for n in nums:
            if n in hashmap:
                hashmap[n]+=1
            else:
                hashmap[n]=1

        ans=set()
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                for k in range(j+1,len(nums)):
                    val =target-(nums[i]+nums[j]+nums[k])
                    if val in hashmap:
                        count=(nums[i]==val)+(nums[j]==val)+(nums[k]==val)
                        if hashmap[val]>count:
                            ans.add(tuple(sorted([nums[i],nums[j],nums[k],val])))
                    else:
                        continue
        return ans
```


### 四、字符串的题目

### （1）344、反转字符串
题目链接：https://leetcode-cn.com/problems/reverse-string/

```Java
class Solution {
    public void reverseString(char[] s) {

       int left = 0;
       int right = s.length-1;
       while(left<right){
           char tmp1 = s[left];
           char tmp2 = s[right];
           s[right] = tmp1;
           s[left] = tmp2;

           left++;
           right--;

       }
    }
}
```
```python
class Solution:
    def reverseString(s:List[str])->None:
        left,right=0,len(s)-1
        while left<right:
            tmp1=s[left]
            tmp2=s[right]
            s[right]=tmp1
            s[left]=tmp2

            left+=1
            right-=1
```


### (2)541、反转字符串II
题目链接：https://leetcode-cn.com/problems/reverse-string-ii/

```Java
class Solution {
    public String reverseStr(String s, int k) {
     StringBuffer res = new StringBuffer();
     int length = s.length();
     int start = 0;
     while(start<length){
         StringBuffer temp = new StringBuffer();
         //找到k和2k处
         int firstK = (start + k >length)?length:start+k;
         int secondK = (start + (2*k) >length )? length : start+(2*k);

         //start开始的位置，肯定套反转
         temp.append(s.substring(start,firstK));
         res.append(temp.reverse());

         //如果fisrtK 到 secondK 之间有元素，这些元素放入res之间即可
         if(firstK<secondK){
             res.append(s.substring(firstK,secondK));
         }
         start += (2*k);
     }
     return res.toString();
    }
}
```

```python
class Solution:
    def reverseStr(s:str,k:int)->str:

        def reverseString(s1):
            left, right = 0, len(s1) - 1
            while left < right:
                s1[left],s1[right]=s1[right],s1[left]
                left += 1
                right -= 1
            return s1

        t=list(s)
        print(t)
        for i in range(0,len(t),2*k):
            t[i:i+k]=reverseString(t[i:i+k])
        return "".join(t)
```

### (3)05、替换空格
题目链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/submissions/

```Java
class Solution {
    public String replaceSpace(String s) {
     if(s==null){
         return null;
     }

     StringBuffer sb = new StringBuffer();
     for(int i= 0;i<s.length();i++){
        if(" ".equals(String.valueOf(s.charAt(i)))){
            sb.append("%20");
        }else{
            sb.append(s.charAt(i));
        }

     }
     return sb.toString();
}
}

```

```python
class Solution:
    def replaceSpace(s:str)->str:
        #方法一：
        # t=list(s)
        # for i in range(0,len(t)):
        #     if t[i]==' ':
        #         t[i]="%20"
        # return "".join(t)
        #
        #方法二：
        t=s.split(' ')
        return "%20".join(t)
```


### (4)151、翻转字符串里的单词
题目链接：https://leetcode-cn.com/problems/reverse-words-in-a-string/submissions/

```Java
class Solution {
   /**
     * 不使用Java内置方法实现
     * <p>
     * 1.去除首尾以及中间多余空格
     * 2.反转整个字符串
     * 3.反转各个单词
     */
    public String reverseWords(String s) {
        // System.out.println("ReverseWords.reverseWords2() called with: s = [" + s + "]");
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        // System.out.println("ReverseWords.removeSpace() called with: s = [" + s + "]");
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        // System.out.println("ReverseWords.removeSpace returned: sb = [" + sb + "]");
        return sb;
    }

    /**
     * 反转字符串指定区间[start, end]的字符
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        // System.out.println("ReverseWords.reverseString() called with: sb = [" + sb + "], start = [" + start + "], end = [" + end + "]");
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        // System.out.println("ReverseWords.reverseString returned: sb = [" + sb + "]");
    }

    private void reverseEachWord(StringBuilder sb) {
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseString(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
```

```python
class Solution:
    def reverseWords(s:str)->str:
        # return " ".join(reversed(s.split()))  #调用API实现单词翻转
        #使用双端队列
        left,right=0,len(s)-1
        #去掉字符串开头的空白字符
        while left<=right and s[left]==' ':
            left+=1
        #去掉字符串末尾的空白字符
        while left<=right and s[right]==' ':
            right-=1

        d,word= collections.deque(),[]
        #将单词push到队列的头部
        while left<=right:
            print("left=",left)
            print("s[left]=",s[left])
            print("word=",word)
            print("d=",d)
            if s[left]==' 'and word:
                d.appendleft("".join(word))
                word=[]
            elif s[left]!=" ":
                word.append(s[left])
            left+=1
        d.appendleft("".join(word))

        return " ".join(d)
```

### (5)58、左旋字符串
题目链接：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/submissions/

```Java
class Solution {
    public String reverseLeftWords(String s, int n) {
     int len = s.length();
     StringBuilder sb = new StringBuilder(s);
     //1、反转区间为前n的子串
     reverString(sb,0,n-1);
     //2、反转区间为n到末尾的子串
     reverString(sb,n,len-1);
     //3、反转整个字符串
     return sb.reverse().toString();
    }

    private void reverString(StringBuilder sb,int left,int right){
        while(left<right){
           char temp = sb.charAt(left);
           sb.setCharAt(left,sb.charAt(right));
           sb.setCharAt(right,temp);
           left++;
           right--; 
        }
    }
}
```

```python
class Solution:
    def reversedLeftWords(s:str,n:int):
        #方法一
        # leng= len(s)
        # t=list(s)
        #
        # def reverString(s,left,right):
        #     while left<=right:
        #         s[left],s[right]=s[right],s[left]
        #         left+=1
        #         right-=1
        #     return list("".join(s))
        #
        # #翻转区间为前n的子串
        # r1=reverString(t,0,n-1)
        # #翻转区间为n到末尾的子串
        # r2=reverString(r1,n,leng-1)
        # #翻转整个字符串
        # return "".join(reverString(r2,0,leng-1))

       #方法二：字符串切片
       # return s[n:]+s[:n]

       #方法三：列表遍历
       res=[]
       for i in range(n,len(s)):
           res.append(s[i])
       for i in range(n):
           res.append(s[i])
       return "".join(res)
```

### (6)28、实现strStr()
题目链接：https://leetcode-cn.com/problems/implement-strstr/submissions/

```Java
class Solution {

    //kmp算法
    //得到前缀表
    public void getNext(int[] next,String s){
       int j =-1;
       next[0]= j;
       for(int i=1;i<s.length();i++){
           while(j>=0 && s.charAt(i)!=s.charAt(j+1)){
               j = next[j];
           }
           if(s.charAt(i)==s.charAt(j+1)){
               j++;
           }
           next[i] = j;
       }
    }
    public int strStr(String haystack, String needle) {
     if(needle.length()==0){
       return 0;
     }

     int[] next = new int[needle.length()];
     getNext(next,needle);
     int j = -1;
     for(int i=0;i<haystack.length();i++){
         while(j>=0 && haystack.charAt(i)!=needle.charAt(j+1)){
             j = next[j];
         }
         if(haystack.charAt(i)==needle.charAt(j+1)){
             j++;
         }
         if(j==needle.length()-1){
             return (i-needle.length()+1);
         }
     }
     return -1;
    }
}
```

```python
class Solution:
    #KMP算法
    def strStr(self, haystack: str, needle: str) -> int:
        a=len(needle)
        b=len(haystack)
        if a==0:
            return 0
        next=self.getnext(a,needle)
        p=-1
        for j in range(b):
            while p>=0 and needle[p+1]!=haystack[j]:
                p=next[p]
            if needle[p+1]==haystack[j]:
                p+=1
            if p==a-1:
                return j-a+1
        return -1

    def getnext(self,a,needle):
        next=['' for i in range(a)]
        k=-1
        next[0]=k
        for i in range(1,len(needle)):
            while (k>-1 and needle[k+1]!=needle[i]):
                k=next[k]
            if needle[k+1]==needle[i]:
                k+=1
            next[i]=k
        return next
```



### (6)459、重复的字符串
题目链接：https://leetcode-cn.com/problems/repeated-substring-pattern/

```Java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
      if(s.equals("")) return false;

      int len = s.length();
      s = " "+s;
      char[] c = s.toCharArray();
      int[] next = new int[len+1];

      for(int i=2,j=0;i<=len;i++){
          while(j>0 && c[i]!=c[j+1]){
              j = next[j];
          }
          if(c[i]==c[j+1]){
              j++;
          }
          next[i] = j;
      }

      if(next[len]>0 && len%(len-next[len])==0){
          return true;
      } 
      return false;   
   }
}
```

```python
class Solution:
    def repeatedSubstringPattern(self,s:str)->bool:
        n=len(s)
        for i in range(1,n//2+1):
            if n%i==0:
                if all(s[j]==s[j-i] for j in range(i,n)):
                    return True
        return False
```


### 五、双指针法的题目

### (1) 27、移除元素
题目链接：https://leetcode-cn.com/problems/remove-element/
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i,n = 0,len(nums)
        for j in range(n):
            if nums[j] != val:
                nums[i] = nums[j]
                i += 1
        return i
```

### (2) 344、反转字符串
题目链接：https://leetcode-cn.com/problems/reverse-string/
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1
        while(left < right):
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```
### (3) 剑指05、替换空格
题目链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/
```c++
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0; // 统计空格的个数
        int sOldSize = s.size();
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是每个空格替换成"%20"之后的大小
        s.resize(s.size() + count * 2);
        int sNewSize = s.size();
        // 从后先前将空格替换为"%20"
        for (int i = sNewSize - 1, j = sOldSize - 1; j < i; i--, j--) {
            if (s[j] != ' ') {
                s[i] = s[j];
            } else {
                s[i] = '0';
                s[i - 1] = '2';
                s[i - 2] = '%';
                i -= 2;
            }
        }
        return s;
    }
};
```

### (4) 151、翻转字符串里的单词
题目链接：https://leetcode-cn.com/problems/reverse-words-in-a-string/
```c++
// 版本一
class Solution {
public:
    // 反转字符串s中左闭又闭的区间[start, end]
    void reverse(string& s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    // 移除冗余空格：使用双指针（快慢指针法）O(n)的算法
    void removeExtraSpaces(string& s) {
        int slowIndex = 0, fastIndex = 0; // 定义快指针，慢指针
        // 去掉字符串前面的空格
        while (s.size() > 0 && fastIndex < s.size() && s[fastIndex] == ' ') {
            fastIndex++;
        }
        for (; fastIndex < s.size(); fastIndex++) {
            // 去掉字符串中间部分的冗余空格
            if (fastIndex - 1 > 0
                    && s[fastIndex - 1] == s[fastIndex]
                    && s[fastIndex] == ' ') {
                continue;
            } else {
                s[slowIndex++] = s[fastIndex];
            }
        }
        if (slowIndex - 1 > 0 && s[slowIndex - 1] == ' ') { // 去掉字符串末尾的空格
            s.resize(slowIndex - 1);
        } else {
            s.resize(slowIndex); // 重新设置字符串大小
        }
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); // 去掉冗余空格
        reverse(s, 0, s.size() - 1); // 将字符串全部反转
        int start = 0; // 反转的单词在字符串里起始位置
        int end = 0; // 反转的单词在字符串里终止位置
        bool entry = false; // 标记枚举字符串的过程中是否已经进入了单词区间
        for (int i = 0; i < s.size(); i++) { // 开始反转单词
            if ((!entry))) {
                start = i; // 确定单词起始位置
                entry = true; // 进入单词区间
            }
            // 单词后面有空格的情况，空格就是分词符
            if (entry && s[i] == ' ' && s[i - 1] != ' ') {
                end = i - 1; // 确定单词终止位置
                entry = false; // 结束单词区间
                reverse(s, start, end);
            }
            // 最后一个结尾单词之后没有空格的情况
            if (entry && (i == (s.size() - 1)) && s[i] != ' ' ) {
                end = i;// 确定单词终止位置
                entry = false; // 结束单词区间
                reverse(s, start, end);
            }
        }
        return s;
    }
    
    /* 主函数简单写法
    string reverseWords(string s) {
        removeExtraSpaces(s);
        reverse(s, 0, s.size() - 1);
        for(int i = 0; i < s.size(); i++) {
            int j = i;
            // 查找单词间的空格，翻转单词
            while(j < s.size() && s[j] != ' ') j++;
            reverse(s, i, j - 1);
            i = j;
        }
        return s;
    }
    */
};
```

### (5) 206、翻转链表
题目链接：https://leetcode-cn.com/problems/reverse-linked-list/
```python
#双指针
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while(cur!=None):
            temp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = temp
        return pre
```
 
### (6) 19、删除链表的倒数第N个节点
题目链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        head_dummy = ListNode()
        head_dummy.next = head

        slow, fast = head_dummy, head_dummy
        while(n!=0): #fast先往前走n步
            fast = fast.next
            n -= 1
        while(fast.next!=None):
            slow = slow.next
            fast = fast.next
        #fast 走到结尾后，slow的下一个节点为倒数第N个节点
        slow.next = slow.next.next #删除
        return head_dummy.next
```

### （7）0207、链表相交
题目链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lengthA,lengthB = 0,0
        curA,curB = headA,headB
        while(curA!=None): #求链表A的长度
            curA = curA.next
            lengthA +=1
        
        while(curB!=None): #求链表B的长度
            curB = curB.next
            lengthB +=1
        
        curA, curB = headA, headB

        if lengthB>lengthA: #让curA为最长链表的头，lenA为其长度
            lengthA, lengthB = lengthB, lengthA
            curA, curB = curB, curA

        gap = lengthA - lengthB #求长度差
        while(gap!=0): 
            curA = curA.next #让curA和curB在同一起点上
            gap -= 1
        
        while(curA!=None):
            if curA == curB:
                return curA
            else:
                curA = curA.next
                curB = curB.next
        return None
```

### (8) 142、环形链表II
题目链接：https://leetcode-cn.com/problems/linked-list-cycle-ii/
```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            # 如果相遇
            if slow == fast:
                p = head
                q = slow
                while p!=q:
                    p = p.next
                    q = q.next
                #你也可以return q
                return p

        return None
 ```       
    
### (9) 15、三数之和
题目链接：https://leetcode-cn.com/problems/3sum/

        
        
        
  


