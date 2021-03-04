[toc]

# [Lecture 4 & 5](https://www.youtube.com/watch?v=jDQJPRn3aRw) 

## Monotone Queue

### 知识

- 单调队列往往解决的问题是**滑动窗口中的最值问题**。
- 以固定长度的滑动窗口中的最大值为例。问题描述：给定N长数组A求所有长度为K的子区间的最大值（一共N - K + 1个）。通过观察，我们发现，一个新进来的元素将淘汰所有比他小的正在维护的信息的意义，因为这个元素进来更晚也将更晚被滑出。也就是说，我们维护的所有信息，在下标递增时，值是递减的。
- 于是我们用一个 deque 来维护一些信息二元组（下标，值），队首到队尾下标递增，值递减，当一个新值进入时，我们淘汰掉所有不如它大的值。当窗口向后滑动时，淘汰掉队首下标过小的值。

### 例题

239. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

模板题。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        // only stores idx
        Deque<Integer> deq = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!deq.isEmpty() && nums[deq.getLast()] < nums[i]) {
                deq.removeLast();
            }
            deq.addLast(i);
            if (deq.getFirst() < i - k + 1) {
                deq.removeFirst();
            }
            if (i >= k - 1) {
                ans[i - k + 1] = nums[deq.getFirst()];
            }
        }
        return ans;
    }
}
```

1562. [Find Latest Group of Size M](https://leetcode.com/problems/find-latest-group-of-size-m/)

求按位置把全0数组变成1的过程中最后出现长度为M的全1序列的时刻。转化为某个位置变成1的时间序列，也就是求M长度的滑动窗口内的max值（对应某时刻变成窗口内全1）要比两侧时刻中的最小值还要小（左右侧为0）。返回这个max。

```java
class Solution {
    public int findLatestStep(int[] arr, int m) {
        int n = arr.length;
        int[] ti = new int[n];
        int cnt = 1;
        for (int a : arr) {
            ti[a - 1] = cnt;
            cnt++;
        }
        // sliding window maximum
        int k = m;
        int[] ans = new int[n - k + 1];
        // only stores idx
        Deque<Integer> deq = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!deq.isEmpty() && ti[deq.getLast()] < ti[i]) {
                deq.removeLast();
            }
            deq.addLast(i);
            if (deq.getFirst() < i - k + 1) {
                deq.removeFirst();
            }
            if (i >= k - 1) {
                ans[i - k + 1] = ti[deq.getFirst()];
            }
        }
        int res = -1;
        // 也可以设置arr[-1]和arr[n]都为n + 1，则不需要特判
        for (int i = 0; i < ans.length; i++) {
            int left = i - 1 >= 0 ? ti[i - 1] : Integer.MAX_VALUE;
            int right = i + m <= n - 1 ? ti[i + m] : Integer.MAX_VALUE;
            if (Math.min(left, right) > ans[i])
                res = Math.max(res, left == Integer.MAX_VALUE && right == Integer.MAX_VALUE ? ans[i] : Math.min(left, right) - 1);
        }
        return res;
    }
}
```

#### 滑动窗口 + 单调队列

1438. [Longest Contiguous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

最值的差不超过定值。维护一个滑窗最大值序列和滑窗最小值序列。超出limit时移动左边界。还需要再看看。[38:26](https://www.youtube.com/watch?v=jDQJPRn3aRw)开始。

```java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        int res = 0;
        int n = nums.length;
        Deque<Integer> min = new ArrayDeque<>();
        Deque<Integer> max = new ArrayDeque<>();
        int l = 0;
        for (int r = 0; r < n; r++) {
            int a = nums[r];
            while (!min.isEmpty() && nums[min.getLast()] >= a) {
                min.removeLast();
            }
            min.addLast(r);
            while (!max.isEmpty() && nums[max.getLast()] <= a) {
                max.removeLast();
            }
            max.addLast(r);
            while (nums[max.getFirst()] - nums[min.getFirst()] > limit) {
                if (min.getFirst() == l)
                    min.removeFirst();
                if (max.getFirst() == l)
                    max.removeFirst();
                l++;
            }
            res = Math.max(res, r - l + 1);
        }
        return res;
    }
}
```

1499. [Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

求满足特定条件的表达式的最值max(y_i + y_j + |x_i - x_j|)。因为按x排序(i < j)，可以去掉绝对值表示转化为max(y_i + y_j + x_j - x_i)，当我们固定j时，可以分离i和j，变成max(y_j + x_j) + max(y_i - x_i)。因为要满足|x_i - x_j| <= k。当x_j增大时，x_i也要随之右移。满足单调性，即可以用滑动窗口解决。

```java
class Solution {
    // return y_i - x_i
    private int getVal(int[][] p, int id) {
        return p[id][1] - p[id][0];
    }
    
    public int findMaxValueOfEquation(int[][] points, int k) {
        Deque<Integer> deq = new ArrayDeque<>();
        int res = Integer.MIN_VALUE;
        int n = points.length;
        for (int j = 0; j < n; j++) {
            while (!deq.isEmpty() && points[deq.getFirst()][0] + k < points[j][0])
                deq.removeFirst();
            if (!deq.isEmpty()) {
                res = Math.max(res, getVal(points, deq.getFirst()) + points[j][0] + points[j][1]);
            }
            while (!deq.isEmpty() && getVal(points, deq.getLast()) <= getVal(points, j)) {
                deq.removeLast();
            }
            deq.addLast(j);
        }
        return res;
    }
}
```

402. [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)

+贪心取窗口中最小。同1673。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        int n = num.length();
        k = n - k;
        Deque<Integer> deq = new ArrayDeque<>();
        StringBuilder sb = new StringBuilder();
        char[] ch = num.toCharArray();
        for (int i = 0; i < n; i++) {
            while (!deq.isEmpty() && ch[deq.getLast()] > ch[i]) {
                deq.removeLast();
            }
            deq.addLast(i);
            if (i < n - k)
                continue;
            int p = deq.getFirst();
            deq.removeFirst();
            sb.append(ch[p]);
        }
        return sb.length() == 0 ? "0" : sb.toString().replaceFirst("^0+(?!$)", "");
    }
}
```

1673. [Find the Most Competitive Subsequence](https://leetcode.com/problems/find-the-most-competitive-subsequence/)

+贪心取窗口中最小。n个数中选k个，要求字典序最小。要保证够取k个数字，第一个数字的选择窗口是[0, n - k]，第二个数字的选择窗口为[id[0] + 1, n - k + 1]，第i个数字的选择窗口为[id[i - 1] + 1, n - k + i + 1]。从n - k开始，依此pop最小值序列首的元素，即当前窗口中的最小值。

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        int n = nums.length;
        Deque<Integer> deq = new ArrayDeque<>();
        int[] ans = new int[k];
        int j = 0;
        for (int i = 0; i < n; i++) {
            while (!deq.isEmpty() && nums[deq.getLast()] > nums[i]) {
                deq.removeLast();
            }
            deq.addLast(i);
            if (i < n - k)
                continue;
            int p = deq.getFirst();
            deq.removeFirst();
            ans[j++] = nums[p];
        }
        return ans;
    }
}
```

#### 单调队列优化 DP

1425. [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/)

1696. [Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

1687. [Delivering Boxes from Storage to Port](https://leetcode.com/problems/delivering-boxes-from-storage-to-ports/submissions/)