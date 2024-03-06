# laicode19. K Closest In Sorted Array
Given a target integer T, a non-negative integer K and an integer array A sorted in ascending order, find the K closest numbers to T in A. If there is a tie, the smaller elements are always preferred.

Assumptions
A is not null
K is guranteed to be >= 0 and K is guranteed to be <= A.length

Return
A size K integer array containing the K closest numbers(not indices) in A, sorted in ascending order by the difference between the number and T. 

Examples
A = {1, 2, 3}, T = 2, K = 3, return {2, 1, 3} or {2, 3, 1}
A = {1, 4, 6, 8}, T = 3, K = 3, return {4, 1, 6}

### 注意：一定要在写代码前就分好类。三种情况。用双指针去查找p在中间时刻的情况。

***自己代码***
```java
public class Solution {
  public int[] kClosest(int[] array, int target, int k) {
    // Write your solution here
    // 先找到最接近target的那个数，然后左右两边双指针开始找最接近的数（k-1）
    int p = closest(array,target); // 最接近的数的索引

    // 开始双指针找 -- 分三种情况
    // 1.p在数组的最头 -- 直接取数组的前k个
    // 2.p在数组的最末尾 -- 直接取数组的后k个
    // 3.p在数组的中间部分 -- 双指针找
    int[] result = new int[k]; // 长度为k的数组接收结果
    if (p == 0) { // case1
      for (int i = 0;i < k;i++) {
        result[i] = array[i];
      }
    }
    else if (p == array.length - 1) { // case2
      for (int i = 0;i < k;i++) {
        result[i] = array[array.length - 1 - i];
      }
    }
    else { // case3
      int left = p - 1;
      int right = p + 1;
      result[0] = array[p]; // 先把最接近的放到第一个

      // stop condition:left或right走到尽头，出循环把另外一头按顺序接上 或者 result填满了
      // left < 0 || right > array.length - 1 || i >= k
      int i = 1; // i用来代表放入result的index，因为0提前放入，所以从1开始
      while (left >= 0 && right < array.length && i < k) {
        if (Math.abs(array[left] - target) <= Math.abs(array[right] - target)) { // 左边近取左边，取谁移谁
        // 这里条件要用 <= ，因为如果left和right一样近 视left更近一点
          result[i] = array[left];
          left--;
        }
        else {
          result[i] = array[right];
          right++;
        }
        i++;
      }
      // 出循环对应stop condition有三种情况
      while (right < array.length && i < k) { // left走到头，result还没填满,把right这一边按顺序填满
        result[i] = array[right];
        i++;
        right++;
      }
      while (left >= 0 && i < k) { // right走到头，result还没填满，把left这一边按顺序填满
        result[i] = array[left];
        i++;
        left--;
      }
    }
    return result; // 如果result填满出循环，此时的result就是结果
  }
  
  private int closest(int[] array,int target) {
    int left = 0;
    int right = array.length - 1;
    while (left < right - 1) {
      int mid = left + (right - left)/2;
      if (array[mid] == target) {
        return mid;
      }
      else if (array[mid] < target) {
        left = mid;
      }
      else {
        right = mid;
      }
    }
    return Math.abs(array[left] - target) < Math.abs(array[right] - target)?left:right;
  }
}
```

***老师代码***
```java
public class KClosest{
  public int[] kClosest(int[] array, int target, int k) {
    // Write your solution here
    if(array == null || array.length == 0){
      return array;
    }
    if(k == 0){
      return new int[0];
    }
    // left is the index of the largest smaller or equal element,
    // right = left + 1.
    // These two should be the cloest to target.
    int left = largestSmallerEqual(array, target);
    int right = left + 1;
    int[] result = new int[k];
    //this is a typical merge operation.
    for(int i = 0; i < k; i++){
      // we can advance the left pointer when:
      // 1. right pointer is already out of bound.
      // 2. right pointer is not out of bound, left pointer is not out of
      // bound, and array[left] is closer to target.
      if(right >= array.length || left >= 0 && target - array[left] <= array[right] - target){
        result[i] = array[left--];
      }else{
        result[i] = array[right++];
      }
    }
    return result;
  }

  private int largestSmallerEqual(int[] array, int target){
    // find the largest smaller or equal element's index in the array
    int left = 0;
    int right = array.length - 1;
    while(left < right - 1){
      int mid = left + (right - left) / 2;
      if (array[mid] <= target){
        left = mid;
      } else {
        right = mid;
      }
    }
    if (array[right] <= target){
      return right;
    }
    if (array[left] <= target){
      return left;
    }
    return -1;
  }
}
```

TC:O(logn + k) //ps:别忘记加k的时间复杂度～      
SC:O(1) //ps:返回的array不占***额外的***空间复杂度
