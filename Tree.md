# Vertical Order Traversal

[leetcode314. Binary Tree Vertical Order Traversal](https://leetcode.com/problems/binary-tree-vertical-order-traversal/description/)       
[leetcode987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)      

1. 遍历 二叉树 同时收集坐标 (build a new class)
2. 把坐标收集起来，依据题目要求进行排序，组装成题目要求的返回数据格式即可

**区别**
314 -> If two nodes are in the same row and column, the order should be from left to right. -> 可以用层序遍历解决       
987 -> 先记录 row 靠上的 element 如果同层的话 按照 treenode 的 val 大小排序

```java
    class Node {
        TreeNode node;
        int row; // 如果是 314 题，不需要记录收集 row 坐标了，用层序遍历，自然是自上而下，子左向右
        int col;
        Node(TreeNode node, int row, int col) {
            this.node = node;
            this.row = row;
            this.col = col;
        }
    }
```

# sada 