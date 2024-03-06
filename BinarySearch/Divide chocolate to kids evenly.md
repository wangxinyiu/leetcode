# Divide chocolate to kids evenly
## 1. 切巧克力问题 - 二分

K个小朋友。N 块巧克力。第 i 块是 Hi * Wi 的方格组成的长方形。小明需要从这 N 块巧克力中切出K块巧克力分给小朋友们。切出的巧克力需要满足：

1. 形状是正方形，边长是整数
2. 大小相同

例如一块 6 * 5 的巧克力可以切出 6 块 2 * 2的巧克力或者 2块 3 * 3 的巧克力。

当然小朋友们都希望得到的巧克力尽可能打，你能帮小明计算出最大的边长是多少吗？

### *思路*：
二分查找最终的块数的边长，看这个边长的巧克力最多可以分几个小朋友，如果大于等于小朋友的总数，则证明该边长进入可能备选答案中。然后去二分查找比这个边长小的边长。
- step one: find the left and right boundary.
- step two: the mid of the left and right boundary as the edge of the square.
- step three: check the total count under the edge.
- step four: if the total is bigger than the number of kids, its could be used. otherwise, we need to decrease the length of the edge. repeat the operations again.

```java
 public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int N = scan.nextInt();
        int K = scan.nextInt();

        int[] h = new int[100000];
        int[] w = new int[100000];

        for (int i = 0; i < N; i++) {
            h[i] = scan.nextInt();
            w[i] = scan.nextInt();
        }
        scan.close();

        int left = 1;
        int right = 100000;
        int res = 0;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            int count = 0;
            // 以mid为边长，一共可以分count块
            for (int i = 0; i < N; i++) {
                count += (h[i] / mid) * (w[i] / mid);
            }
            if (count >= K) {
                left = mid + 1;
                res = mid;
            }
            else {
                right = mid - 1;
            }
        }
        System.out.println(res);
    }
```
two questions need to discuss:
1. left = mid + 1 rather than left = left + 1;
2. if the count equals to K why need to continue the loop.
