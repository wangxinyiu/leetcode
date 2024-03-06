# laicode20. Search In Unknown Sized Sorted Array

Given a integer dictionary A of unknown size, where the numbers in the dictionary are sorted in ascending order, determine if a given target integer T is in the dictionary. Return the index of T in A, return -1 if T is not in A.

Assumptions
dictionary A is not null
dictionary.get(i) will return null(Java)/INT_MIN(C++)/None(Python) if index i is out of bounds

Examples
A = {1, 2, 5, 9, ......}, T = 5, return 2
A = {1, 2, 5, 9, 12, ......}, T = 7, return -1

题目给一个接口
```java
interface Dictionary {
  public Integer get(int index);
}
```
### Version one: 我的思路：把左边界的index当成0，把右边界的index当成Integer.MAX_VALUE. then, use binary search to find the target.
```java
public class Solution {
  public int search(Dictionary dict, int target) {
    // Write your solution here
    int left = 0;
    int right = Integer.MAX_VALUE;
    while(left <= right){
      int mid = left + (right - left) / 2;
      if(dict.get(mid) == target){
        return mid;
      }else if(dict.get(mid) < target){
        left = mid + 1;
      }else{
        right = mid - 1;
      }
    }
    return 0;
  }
}
```
Error: not consider when this index not exist in dictionary. -> NullPointerException. 

### 答案：   
需要先去把左右边界固定住。在去binarySearch。防止NPE
```java
public class Solution {
    public int search(Dictionary dict, int target) {
        if (dict == null) {
            return -1;
        }
        //缩小dict的左右边界
        int left = 0;
        int right = 1;
        //find the right boundary for binary search.
        //extends until we are sure the target is within the [left, right) range.
        while (dict.get(right) != null && dict.get(right) < target) {
            left = right;
            right = right * 2;
        }
        return binarySearch(dict, left, right, target);
    }

    private int binarySearch(Dictionary dict, int left, int right, int target) {
        //只需要找到target就行，所以用经典的binarySearch。
        while (left <= right) {
            int mid = left + (right - left) / 2;
            //需要考虑是否mid为null的情况！！！ mid为null的时候我们就需要更改右边界往左找
            if(dict.get(mid) == null || dict.get(mid) > target){
              right = mid - 1;
            }else if(dict.get(mid) < target){
              left = mid + 1;
            }else{
              return mid;
            }
        }
        return -1;
    }
}
```
TC: O(logT), T is the index of target value.
SC: O(1)

### Version two: 根据答案的思路，改了自己的代码，居然过了！！！ 成就感！！！ 其实就是要考虑一下NPE的情况！！！但时间复杂度需要在思考思考。
```java
public class Solution {
  public int search(Dictionary dict, int target) {
    // Write your solution here
    int left = 0;
    int right = Integer.MAX_VALUE;
    while(left <= right){
      int mid = left + (right - left) / 2;
      //mid有可能NPE, 和mid > target在一个分类下都要向左找.
      if(dict.get(mid) == null || dict.get(mid) > target){
        right = mid - 1;
      }else if(dict.get(mid) < target){
        left = mid + 1;
      }else{
        return mid;
      }
    }
    return -1;
  }
}
```
Integer的取值范围：-2^31 至 2^31 - 1 -> -2147483648 至 2147483647     
TC: 我认为时间复杂度也就是O(31)?因为log2147483647最多也就是31？？？？有待考证？？？？       
SC：没有产生extra space, so O(1).      


