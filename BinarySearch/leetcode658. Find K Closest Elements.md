# leetcode658. Find K Closest Elements
Given a sorted integer array arr, two integers k and x, return the k closest integers to x in the array. The result should also be sorted in ascending order.

An integer a is closer to x than an integer b if:

```
  |a - x| < |b - x|, or
  |a - x| == |b - x| and a < b
```

Example 1:
```java
Input: arr = [1,2,3,4,5], k = 4, x = 3
Output: [1,2,3,4]
```

Example 2:
```java
Input: arr = [1,2,3,4,5], k = 4, x = -1
Output: [1,2,3,4]
``` 

Constraints:      
```
1 <= k <= arr.length  
1 <= arr.length <= 104  
arr is sorted in ascending order         
-104 <= arr[i], x <= 104  
```

## 思路
跟laicode19题一样，但是返回值不一样。laicode19题是需要返回一个数组的要求是（按照离target的差值从小到大排序     
本题是需要返回k个。（按照原数组的顺序排序 -> 所以先找到离target最近的数在双指针就不知道索引 -> 很麻烦    
因为返回的是一个与原数组相同的顺序 -> 滑动窗口   
我们可以找到窗口嘴左边left索引的范围 -> binarySearch的去找最合适的条件的窗口的最左边 -> return 循环的去放k个值     

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        // Initialize binary search bounds
        int left = 0;
        int right = arr.length - k;
        
        // Binary search against the criteria described
        while (left < right) {
            int mid = (left + right) / 2;
            if (x - arr[mid] > arr[mid + k] - x) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        // Create output in correct format
        List<Integer> result = new ArrayList<Integer>();
        for (int i = left; i < left + k; i++) {
            result.add(arr[i]);
        }
        
        return result;
    }
}
```
