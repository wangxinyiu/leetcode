# laicode17. Closest In Sorted Array
Given a target integer T and an integer array A sorted in ascending order, find the index i in A such that A[i] is closest to T.

Assumptions
There can be duplicate elements in the array, and we can return any of the indices with same value.
Examples
A = {1, 2, 3}, T = 2, return 1
A = {1, 4, 6}, T = 3, return 1
A = {1, 4, 6}, T = 5, return 1 or 2
A = {1, 3, 3, 4}, T = 2, return 0 or 1 or 2

Corner Cases
What if A is null or A is of zero length? We should return -1 in this case.

写里面的if else时，因为要找closet 所以我们也不能判定我们array[mid] < target时这个mid是不是我们最终的结果，所以要把它继续包括在循环里继续check。所以 left = mid. 同理，right = mid. 写完if else后我们再看while里怎么填写。为了防止死循环需要写成 left < right - 1. eg. input{1,2} target = 3. 如果while里填写的是 left <= right 或者 left < right 都会发生死循环。死循环的问题解决后，left < right - 1 意味着剩俩个数出来，我们在进行比较。看看哪个数更为合适。

```java
public class Solution {
    public int closest(int[] array, int target) {
        // Write your solution here
        // corner case
        if (array == null || array.length == 0) {
            return -1;
        }
        int left = 0;
        int right = array.length - 1;
        while (left < right - 1) {
            int mid = left + (right - left) / 2;
            if (array[mid] == target) {
                return mid;
            } else if (array[mid] < target) {
                left = mid;
            } else {
                right = mid;
            }
        }
        if (Math.abs(array[left] - target) > Math.abs(array[right] - target)) {
            return right;
        }
        return left;
    }
}
```
//TC: O(log n).   
//SC: O(1).   
