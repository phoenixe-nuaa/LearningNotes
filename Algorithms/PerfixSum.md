[toc]

# [Lecture 2](https://www.youtube.com/watch?v=hGJtySsGIwM)

## 一维前缀和

### 知识

一个序列A的前缀和序列B可以看作序列A数据的一种组织方式。

![img](https://lh6.googleusercontent.com/Yz9qu2ufZ1w8c8zsJ6e05hL-Jp1HGBKBeuu9MmqARus-iFRK4If90CVjesovkKjNDMxTJvcF2C5CnpaIWODoS-oPocO5kI8pdDp4-BSfwHpdBRwkv84Hp30Br4s_gcqbt3lfVIkd)

因此我们得到了这样一个数据结构：

可以用一次O(N)的时间完成序列的初始化（得到前缀和序列）。

可以每次O(1)的时间快速的计算原序列 某个区间的和。

也适用于写入很少读入频繁的情况。

**注意**

如果不启用b<sub>0</sub>，求[0, r]的区间和需要特判。连续子数组subarray求**个数**考虑前缀和，求**最长最短**length考虑滑动窗口。

### 例题

303. #### [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

前缀和裸题。

```java
class NumArray {
    int[] preSum;
    
    public NumArray(int[] nums) {
        int N = nums.length;
        preSum = new int[N + 1];
        for (int i = 1; i < N + 1; i++) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
    }
    
    public int sumRange(int i, int j) {
        return preSum[j + 1] - preSum[i];
    }
}
```

560. #### [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

找连续子数组和为K的个数。看数据范围之后确定O(N[^2])的算法行不通，考虑O(N)和O(N<sub>log</sub>N)的算法。因为有负数，否则因为单调性可以用双指针。考虑存在两个前缀和的差值等于K。用map去维护出现过的前缀和--个数对，可以将单次复杂度降到O(1)。总复杂度是O(N)。注意要记得check某个前缀和本身是否等于K。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int N = nums.length;
        int[] preSum = new int[N + 1];
        for (int i = 1; i < N + 1; i++) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
        Map<Integer, Integer> map = new HashMap<>();
        int res = 0;
        // map.put(0, 1);
        for (int i = 1; i < N + 1; i++) {
            if (preSum[i] == k)
                res++;
            res += map.getOrDefault(preSum[i] - k, 0);
            map.put(preSum[i], map.getOrDefault(preSum[i], 0) + 1);
        }
        return res;
    }
}
```

1248. #### [Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/)

因为是找子数组里奇数个数等于k的个数。首先因为只看寄偶性，所以先转化成01串，这样找奇数个数等于k就可以转化成找两个前缀和的差等于k。依旧是用前缀和 + Map来做。

```java
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int N = nums.length;
        for (int i = 0 ; i < N; i++) {
            if (nums[i] % 2 == 1)
                nums[i] = 1;
            else
                nums[i] = 0;
        }
        int[] preSum = new int[N + 1];
        for (int i = 1; i < N + 1; i++) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int res = 0;
        for (int i = 1; i < N + 1; i++) {
            res += map.getOrDefault(preSum[i] - k, 0);
            map.put(preSum[i], map.getOrDefault(preSum[i], 0) + 1);
        }
        return res;
    }
}
```

974. #### [Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

求被k整除的subarray个数。大思路还是用前缀和+map。细节升级了一些，因为不可能对所有的子数组和去枚举可能的被k整除的数字，所以考虑对数组进行预处理（把所有的值都转化为[0, K - 1]内）。类似前面奇偶性转化，因为有(a + b) % k = (a % k + b % k) % k的特性，对所有数字进行模K的操作（范围变成[-K + 1, K - 1]）。负数的范围可以利用a % k = (a + k) % k的特性，加K转成正数。例如，(a<sub>i</sub> + a<sub>i</sub>+1 + … + a<sub>j</sub>) % k = 0，等价于(a<sub>i</sub> % k + a<sub>i</sub>+1 % k + … + a<sub>j</sub> % k) % k = 0 → b’<sub>j</sub> = b’<sub>i</sub>（b‘的范围都在[0, k-1]之间）。因为k不是很大，可以考虑数组来存。

```java
class Solution {
    public int subarraysDivByK(int[] A, int K) {
        int N = A.length;
        int[] map = new int[K];
        map[0]++;
        int sum = 0;
        int res = 0;
        for (int a : A) {
            a %= K;
            if (a < 0)
                a += K;
            // sum = (sum + a) % K; 小技巧:sum+a的范围是[0, 2k-2], 其中只有[k, 2k-2]的需要修正, 减k即可
            sum += a;
            if (sum >= K)
                sum -= K;
            res += map[sum];
            map[sum]++;
        }
        return res;
    }
}
```

1177. #### [Can Make Palindrome from Substring](https://leetcode.com/problems/can-make-palindrome-from-substring/)

==还要再看。==

子串是否能够通过交换位置和替换字母k次形成palindrome串。先写几个case感受一下，”abcdef” -> k = 3 (n/2)。“abcde” -> k = 2。“aabb” -> k = 0。“aabc” -> k = 1。“abc” -> k = 1。“aaabc” -> “abaca” -> k = 1。

总结：if cnt[char] is even pass; if cnt[char] is odd put one in the center and then exchange the left half to another to make symmetric pairs.

```java
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
        List<Boolean> ans = new ArrayList<>(); 
        int[][] cnt = new int[s.length() + 1][26];
        for (int i = 0; i < s.length(); ++i) {
            cnt[i + 1] = cnt[i].clone();
            cnt[i + 1][s.charAt(i) - 'a']++;
        }
        for (int[] q : queries) {
            int sum = 0; 
            for (int i = 0; i < 26; i++) {
                sum += (cnt[q[1] + 1][i] - cnt[q[0]][i]) % 2;
            }
            ans.add(sum / 2 <= q[2]);
        }
        return ans;
    }
}
```

437. #### [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

树上前缀和。dfs维护包含当前点到root路径的前缀和map。注意检查 b<sub>i</sub> - b<sub>i</sub>的特殊情况。

```java
class Solution {
    int res = 0;
    Map<Integer, Integer> map = new HashMap<>();
    int k;
    
    public int pathSum(TreeNode root, int sum) {
        k = sum;
        dfs(root, 0);
        return res;
    }
    
    public void dfs(TreeNode root, int preSum) {
        if (root == null)
            return;
        preSum += root.val;
        // check b_i - b_0
        if (preSum == k)
            res++;
        res += map.getOrDefault(preSum - k, 0);
        map.put(preSum, map.getOrDefault(preSum, 0) + 1);
        dfs(root.left, preSum);
        dfs(root.right, preSum);
        map.put(preSum, map.getOrDefault(preSum, 0) - 1);
        preSum -= root.val;
    }
}
```

#### （新）1658. [Minimum Operations to Reduce X to Zero](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/)

==还要再看。==

## 二维前缀和

### 知识

![img](https://lh3.googleusercontent.com/yeeG_eERNMGkWIh4Ios00CUvtxWrgenBTNrsWYt9F8zIutiWoQoLJFSSdSMten2inYKTx4C7hyH3MznFDLBusYnkiiQ2wtU9h9saz4JMzGGhjdX7KE1ZUrW0aqApKcmce0yZSlaW)

求(r<sub>1</sub>, c<sub>1</sub>)到(r<sub>2</sub>, c<sub>2</sub>)的矩形和：

**res = sum(r<sub>2</sub> + 1, c<sub>2</sub> + 1) - sum(r<sub>1</sub>, c<sub>2</sub> + 1) - sum(r<sub>2</sub> + 1, c<sub>1</sub>) + sum(r<sub>1</sub>, c<sub>1</sub>)**

### 例题

304. #### [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

```java
class NumMatrix {
    int[][] sum;
    int m;
    int n;
    
    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return;
        m = matrix.length;
        n = matrix[0].length;
        sum = new int[m + 1][n + 1];
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        int imax = Math.max(row1, row2);
        int imin = Math.min(row1, row2);
        int jmax = Math.max(col1, col2);
        int jmin = Math.min(col1, col2);
        return sum[imax + 1][jmax + 1] - sum[imax + 1][jmin] - sum[imin][jmax + 1] + sum[imin][jmin];
    }
}
```

1314. #### [Matrix Block Sum](https://leetcode.com/problems/matrix-block-sum/)

```java
class Solution {
    public int[][] matrixBlockSum(int[][] mat, int K) {
        int m = mat.length;
        int n = mat[0].length;
        int[][] sum = new int[m + 1][n + 1];
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + mat[i - 1][j - 1];
            }
        }
        int[][] answer = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int r1 = Math.max(0, i - K);
                int r2 = Math.min(i + K, m - 1);
                int c1 = Math.max(0, j - K);
                int c2 = Math.min(j + K, n - 1);
                answer[i][j] = sum[r2 + 1][c2 + 1] - sum[r1][c2 + 1] - sum[r2 + 1][c1] + sum[r1][c1];
            }
        }
        return answer;
    }
}
```

1292. #### [Maximum Side Length of a Square with Sum Less than or Equal to Threshold](https://leetcode.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/)

前缀和/滑动窗口

==还要再看。==

## 一维差分

### 知识

![img](https://lh5.googleusercontent.com/kXYJ9z2AQ6kdfH2-a7yRYgyLido_QkaDpoZchOiY9FuplZUt4XYyhrkc47TDj5bBeugZfdrv6fdoKZSbDzHLw43rCi3ax24ack4nx3f9I0Y92Ha0Yandf0G-PehU7cl7_WyVmakg)

### 例题

370. #### [Range Addition](https://leetcode.com/problems/range-addition/)

模板。初始化diff数组，对diff数组对应位置做修改，算前缀和返回原数组。

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] diff = new int[length + 1];
        for (int[] u : updates) {
            diff[u[0]] += u[2];
            diff[u[1] + 1] -= u[2];
        }
        int[] res = new int[length];
        for (int i = 1; i < length + 1; i++) {
            diff[i] += diff[i - 1];
            res[i - 1] = diff[i - 1];
        }
        return res;
    }
}
```

1094. #### [Car Pooling](https://leetcode.com/problems/car-pooling/)

==还要再看。==

1109. #### [Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings/)

几乎是模板，同370。

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] diff = new int[n + 1];
        for (int[] u : bookings) {
            diff[u[0] - 1] += u[2];
            diff[u[1]] -= u[2];
        }
        int[] res = new int[n];
        for (int i = 1; i < n + 1; i++) {
            diff[i] += diff[i - 1];
            res[i - 1] = diff[i - 1];
        }
        return res;
    }
}
```

56. #### [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

区间覆盖

837. #### [New 21 Game](https://leetcode.com/problems/new-21-game/)

差分优化 dp 方程

==还要再看。==