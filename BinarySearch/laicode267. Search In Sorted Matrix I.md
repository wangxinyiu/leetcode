# laicode267. Search In Sorted Matrix I - two-dimensional array binary search question
## 在一个sorted好的matrix里找element，返回其index。
Given a 2D matrix that contains integers only, which each row is sorted in an ascending order. The first element of next row is larger than (or equal to) the last element of previous row.

Given a target number, returning the position that the target locates within the matrix. If the target number does not exist in the matrix, return {-1, -1}.

Assumptions:
The given matrix is not null, and has size of N * M, where N >= 0 and M >= 0.

Examples:
matrix = { {1, 2, 3}, {4, 5, 7}, {8, 9, 10} }
target = 7, return {1, 2}
target = 6, return {-1, -1} to represent the target number does not exist in the matrix.

## For two-dimensional array's binary search:
we need changing two-dimensional to one-dimensional array to sloving the problem 
1. Calculate the length of the row and the column.
2. Think of it as a one-dimensional array. Set the left boundary as 0. Set the right boundary as row * column.
3. According to the left and right boundary, we get a mid-value. ***calculate this mid value's row and column***.

## 重点：本题重点是一个数学问题，如何把 one-dimensional index convert to two-dimensional index.     
row = mid / conlumns      
column = mid % conlumns     
         
```java
public class Solution {
  public int[] search(int[][] matrix, int target) {
    // Write your solution here
    int rows = matrix.length;
    int columns = matrix[0].length;
    int left = 0;
    int right = rows * columns - 1;
    while(left <= right){
      int mid = left + (right - left) / 2;
      //calculate which row it is.
      int row = mid / columns;
      //calculate which column it is.
      int column = mid % columns;
      if(matrix[row][column] == target){
        return new int[]{row, column};
      }else if (matrix[row][column] < target){
        left = mid + 1;
      }else{
        right = mid - 1;
      }
    }
    return new int[]{-1, -1};
  }
}
```
TC：o(log(m * n))
SC: O(1)

Errors:
1. left == 0, right == row * column - 1;
2. Transform mid to index only use columns; 
