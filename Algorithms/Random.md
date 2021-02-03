[toc]

# [Lecture 1](https://www.youtube.com/watch?v=rfi4L0X80yY)

## 重要数量级与复杂度对照

| **数据量**            | 复杂度              | 常见算法                                                     |
| --------------------- | ------------------- | ------------------------------------------------------------ |
| 500 ~ 3000            | O(N[^2])            | Floyd，区间dp                                                |
| 5\*10[^4] ~ 5\*10[^5] | O(N<sub>log</sub>N) | 快排，线段树，Dijkstra                                       |
| 10[^7]                | O(N)                | 滑动窗口，单调栈，前缀和，扫描线，遍历图（DFS BFS），拓扑排序，字典树，KMP，Manacher |
| 10[^18]               | O(<sub>log</sub>N)  | 二分，快速幂，GCD                                            |

## 有序数组打乱/随机排列

首先想到：

```java
Collections.shuffle(list);
```

### Fisher–Yates shuffle

类似Reservoir sampling（可用于*stream data*）：

```java
public void shuffle(int[] arr) {
  for (int i = 0; i < arr.length; i++) {
    int p = rnd.nextInt(i + 1);
    int t = arr[i];
    arr[i] = arr[p];
    arr[p] = t;
  }
}
```

另一种写法（需*已知长度和序列信息*）：

```java
public void shuffle(int[] arr) {
  int n = arr.length;
  for (int i = 0; i < n; i++) {
    // i ~ n
    int p = i + rand.nextInt(n - i);
    swap(arr, p, i);
  }
}
```

### Reservoir Sampling

从n个数中随机m个作为样本(m <= n)。蓄水池抽样不一定非要用于给定的数组，*给定一个序列或者流stream长度未知的时候同样有效*。

```java
public void shuffle(int[] arr, int m) {
  int n = arr.length;
  for (int i = 0; i < arr.length; i++) {
    int p = rnd.nextInt(i + 1);
    if (p < m) {
      // int t = arr[i];
      // arr[i] = arr[p];
      // arr[p] = t;
      arr[p] = arr[i];
    }
  }
}
```

### 例题

#### Fisher–Yates shuffle & Reservoir Sampling

384. ##### [Shuffle an Array](https://leetcode.com/problems/shuffle-an-array/)

reset存一个原数组的copy，shuffle是Fisher-Yate算法，随机生成0到i的数j，交换i位置和j位置的数。

```java
class Solution {
  int[] arr;
  int[] copy;
    
  public Solution(int[] nums) {
    arr = nums.clone();
    copy = nums.clone();
  }

  // Resets the array to its original configuration and returns it.
  public int[] reset() {
    return copy;
  }

  // Returns a random shuffling of the array.
  public int[] shuffle() {
    Random rand = new Random();
    for (int i = 0; i < arr.length; i++) {
      int j = rand.nextInt(i + 1);
      int tmp = arr[i];
      arr[i] = arr[j];
      arr[j] = tmp;
    }
    return arr;
  }
}
```

382. ##### [Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)

follow-up是链表长度未知，很自然地想到蓄水池抽样，对应m=1的情况。

```java
class Solution {
  ListNode head;
  int ans;
  
  public Solution(ListNode head) {
    this.head = head;
    ans = head.val;
  }

  public int getRandom() {
    ListNode curr = head;
    Random rand = new Random();
    int cnt = 1;
    while (curr != null) {
      int p = rand.nextInt(cnt + 1);
      if (p < 1) {
        ans = curr.val;
      }
      curr = curr.next;
      cnt++;
    }
    return ans;
  }
}
```

#### Rejection Sampling

470. ##### [Implement Rand10() Using Rand7()](https://leetcode.com/problems/implement-rand10-using-rand7/)

==还要再看。==

用随机7去生成随机10，注意如果用rand7 + rand7概率并不是均等的。用7进制两位数来表征，rand7分别生成数字的两位，就可以保证49种情况（0~48）的概率均等。我们可以抛弃40 ~ 48，这样0 ~ 39的末位数字(0~9)的出现是等概率的。模10再+1就得到了等概率的1到10。

```java
class Solution extends SolBase {
  public int rand10() {
    int rand40 = 40;
    while (rand40 >= 40) {
 		rand40 = (rand7() - 1) * 7 + rand7() - 1;
    }
    return rand40 % 10 + 1;
  }
}
```

519. ##### [Random Flip Matrix](https://leetcode.com/problems/random-flip-matrix/)

==还要再看。==

随机翻转矩形中为0的点，要求call random的次数越少越好。所以考虑每次只随机还是0的点。类似[Fisher-Yates shuffle算法的modern版本](https://en.wikipedia.org/wiki/Fisher–Yates_shuffle)。因为数据范围较大，会出现memory limit exceed，因为call flip的次数不超过1000，考虑可以随机1000个结果存下来。

478. [Generate Random Point in a Circle](https://leetcode.com/problems/generate-random-point-in-a-circle)

随机生成圆中的点，因为需要保证面积上均匀分布，考虑像蒙特卡洛法求pi一样，给定d长的正方形，随机生成横纵坐标，然后判断在不在圆里，如果不在的话就reject，在的话就保留。

```java
class Solution {
  double r;
  double x0;
  double y0;
  Random rand;
  public Solution(double radius, double x_center, double y_center) {
    r = radius;
    x0 = x_center;
    y0 = y_center;
    rand = new Random();
  }

  public double[] randPoint() {
    double x = x0 - r;
    double y = y0 - r;
    while (Math.sqrt(Math.pow(x - x0, 2) + Math.pow(y - y0, 2)) > r) {
      x = x0 - r + r * 2 * rand.nextDouble();
      y = y0 - r + r * 2 * rand.nextDouble();
    }
    return new double[]{x, y};
  }
}
```

#### 带权重的随机数

前置知识前缀和，二分

528. [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight)

生成0到权重和-1的随机数，分段表示随机到哪一个数，可以在前缀和上二分，假设生成的随机数是k，找<=k的最大值（也可以用treemap的ceilingKey做）。例如，[5，7，3]。权值前缀和数组为[0，5，12，15]。随机生成0~14的值。如果是0~4就是0号元素。如果是5~11就是1号元素。如果是12~14就是2号元素。

```java
class Solution {
  int[] preSum;
  Random rand;
  int N;
  public Solution(int[] w) {
    N = w.length;
    preSum = new int[N + 1];
    preSum[0] = 0;
    for (int i = 1; i < N + 1; i++) {
 preSum[i] = preSum[i - 1] + w[i - 1];
    }
    rand = new Random();
  }

  public int pickIndex() {
    int r = rand.nextInt(preSum[N]);
    int left = 0;
    int right = N + 1;
    while (left < right) {
      int mid = (left + right) / 2;
      if (preSum[mid] == r)
        return mid;
      else if (preSum[mid] < r)
        left = mid + 1;
      else if (preSum[mid] > r)
        right = mid;
    }
    return left - 1;
  }
}
```

497. ##### [Random Point in Non-overlapping Rectangles](https://leetcode.com/problems/random-point-in-non-overlapping-rectangles)

一堆不重叠的矩形，随机选取矩形里的点。可以用rejection sampling做，但是效率比较低，因为如果矩形很稀疏，则会产生很多无效random call。考虑可以先以面积为权重随机矩形，然后再随机矩形里的点。注意：x的范围0~2有三个点0，1，2，所以计算面积的时候是(x2 - x1 + 1) * (y2 - y1 + 1)。

```java
class Solution {
  int[] preSum;
  Random rand;
  int N;
  int[] w;
  int[][] rects;
    
  public Solution(int[][] rects) {
    this.rects = rects;
    N = rects.length;
    w = new int[N];
    for (int i = 0; i < rects.length; i++) {
      int area = (rects[i][2] - rects[i][0] + 1) * (rects[i][3] - rects[i][1] + 1);
      w[i] = area;
    }
    preSum = new int[N + 1];
    preSum[0] = 0;
    for (int i = 1; i < N + 1; i++) {
      preSum[i] = preSum[i - 1] + w[i - 1];
    }
    rand = new Random();
  }

  public int[] pick() {
    int r = rand.nextInt(preSum[N]);
    int left = 0;
    int right = N + 1;
    int id = -1;
    while (left < right) {
      int mid = (left + right) / 2;
      if (preSum[mid] == r) {
        id = mid;
        break;
      }
      else if (preSum[mid] < r)
        left = mid + 1;
      else if (preSum[mid] > r)
        right = mid;
    }
    if (id == -1)
      id = left - 1;
    // x1 ~ x2
    int x = rand.nextInt(rects[id][2] - rects[id][0] + 1) + rects[id][0];
    // y1 ~ y2
    int y = rand.nextInt(rects[id][3] - rects[id][1] + 1) + rects[id][1];
    return new int[]{x, y};
  }
}
```

710. ##### [Random Pick with Blacklist](https://leetcode.com/problems/random-pick-with-blacklist/)

随机0~N之间非黑名单里的点。因为需要optimize random call的次数，所以rejection sampling并不适用。换一种思路，黑名单里的点把0~N分割成了很多小段，可以先按小段长度作为权重来随机，然后再随机选中小段里的点。遍历N会TLE，考虑遍历blacklist来建立interval。

```java
class Solution {
  class Interval {
    int start;
    int end;
    int len;
    
    public Interval(int start, int end, int len) {
      this.start = start;
      this.end = end;
      this.len = len;
    }
  }

  int[] preSum;
  int M;
  List<Interval> list;
  Random rand;

  public Solution(int N, int[] blacklist) {
    Arrays.sort(blacklist);
    int start = 0;
    list = new ArrayList<>();
    
      for (int b : blacklist) {
      if (b != start) {
        int len = b - start;
        list.add(new Interval(start, b - 1, len)); 
      }
      start = b + 1;
    }
    if (start < N)
      list.add(new Interval(start, N - 1, N - start));
    // Set<Integer> set = new HashSet<>();
    // for (int b : blacklist)
    //   set.add(b);
    // list = new ArrayList<>();
    // int start = 0;
    // for (int i = 0; i < N; i++) {
    //   if (set.contains(i)) {
    //     if (i != start) {
    //       int len = i - start;
    //       list.add(new Interval(start, i - 1, len));
    //     }
    //     start = i + 1;
    //   }
    // }
    // if (start < N)
    //   list.add(new Interval(start, N - 1, N - start));
    M = list.size();
    preSum = new int[M + 1];
    preSum[0] = 0;
    for (int i = 1; i < M + 1; i++) {
      preSum[i] = preSum[i - 1] + list.get(i - 1).len;
      // System.out.println("start: " + list.get(i - 1).start + " end: " + list.get(i - 1).end + " len: " + list.get(i - 1).len);
    }
    // System.out.println();
    rand = new Random();
  }

  public int pick() {
    // 0 ~ preSum[M] - 1
    int r = rand.nextInt(preSum[M]);
    int left = 0;
    int right = M + 1;
    int id = -1;
    while (left < right) {
      int mid = (left + right) / 2;
      if (preSum[mid] == r) {
        id = mid;
        break;
      }
      else if (preSum[mid] < r)
        left = mid + 1;
      else if (preSum[mid] > r)
        right = mid;
    }
    if (id == -1)
      id = left - 1;
    Interval ival = list.get(id);
    // start ~ end
    return rand.nextInt(ival.end - ival.start + 1) + ival.start;
  }
}
```

