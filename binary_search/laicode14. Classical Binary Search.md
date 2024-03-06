经典的 Binary Search Summary

在一个数组里如果找到目标target就返回，找不到返回 -1.

```java
public class Solution {
  public int binarySearch(int[] array, int target) {
    // Write your solution here
    int left = 0;
    int right = array.length - 1;
    //left <= right 是为了当数组长度为1时，也能进循环去check.
    while(left <= right){
      int mid = left + (right - left) / 2;
      if(array[mid] == target){
        return mid;
      }else if(array[mid] < target){
      //left = mid + 1 是因为如果此时这个mid不是我要的解 我不需要在把它留在我的范围内了。所以可以left = mid + 1
      //eg. {1, 2} target 找 2 -> left = 0, right = 1; mid = 0. If left = mid, time out.
        left = mid + 1;
      }else{
      //但是right 可以不用 mid - 1。 因为编程语言都是向下取整。所以就算我在把这个不合适的mid包括在范围里，它最后也会被踢出。
        right = mid - 1;
      }
    }
    return -1; 
  }
}
```

Summary: We don't need to fill the while condition at the beginning. We need to consider how to reduce the range first. Then, according to the if-else condition, we could set the while condition. Finally, don't forget to check the element which comes out from the loop.
