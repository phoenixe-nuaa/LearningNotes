[toc]

# [Lecture 3](https://www.youtube.com/watch?v=_vAgCkov9bw)

### 知识

#### 双指针适合什么问题

问题具有单调性![img](https://lh3.googleusercontent.com/KMnJ_rfj-KxXp8XIxNeLbE5op-gD_bLXMzH8Bpy4QduJGusCPBz33iPiddPLGscqRJdS2a7RruhO4qiE_mjPnr6-EuPNQ_xHdNNGNsSbaeyeCF_F4KGNWAFP96aHEde2RKIZ3B5S)三类问题![img](https://lh6.googleusercontent.com/hA6nMC9r2I4xZLFNukJgb1Yre-4cHuyHv39LmYKmlPmL6WJU7rnTx7ZGqSn2iJLqzAkwOaHh3X3wYhfNGEx-H8mVPNDFXhLrPRLSHUvzWlf2gFgVms8WPyrriq2l5zzg67SLhO4d)

#### 复杂度

![img](https://lh5.googleusercontent.com/07m_s9sB1CbdHRAEMPlHtaAsG7rzLa8g8MAxlN3kb9Fdz8Id4SN6ZEl3bU-9siR8yTUfHgNIWimYm5FUepfgSFZHdesZZqJvEgSjPNn75arRX1yrAIaFPSWvekPzPIOwl1H0BtR3)

### 例题

487. #### [Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii/)

求连续1的最长区间，最多可以flip一个0。转化求包含最多一个0的最长区间。时间复杂度O(N)，最差情况下每个字符被访问两次。也可以用Prefix Sum来做。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        // maximum window with at most one zero
        int l = 0; 
        int r = 0;
        int res = 1;
        int cnt0 = 0;
        while (r < nums.length) {
            if (nums[r] == 0)
                cnt0++;
            r++;
            while (cnt0 > 1) {
                if (nums[l] == 0)
                    cnt0--;
                l++;
            }
            res = Math.max(res, r - l);
        }
        return res;
    }
}
```

1004. #### [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

求最多可以flip K个0的最长连续1区间。只需要注意res初始变为0，因为如果K=0且位全0数组则答案为0。以及控制cnt0不超过K。

```java
class Solution {
    public int longestOnes(int[] A, int K) {
        // maximum window with at most k zero
        int l = 0; 
        int r = 0;
        int res = 0;
        int cnt0 = 0;
        while (r < A.length) {
            if (A[r] == 0)
                cnt0++;
            r++;
            while (cnt0 > K) {
                if (A[l] == 0)
                    cnt0--;
                l++;
            }
            res = Math.max(res, r - l);
        }
        return res;
    }
}
```

1248. #### [Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/)

计数包含k个奇数数组的个数。因为只和奇偶性相关，考虑转化为01数组。维护两个了，分别是L<sub>max</sub>和L<sub>min</sub>。L<sub>min</sub>是不得不推才推，L<sub>max</sub>是可以推就推。注意，统计答案的时候需要检查cnt<sub>min</sub>是否等于k，也就是r符合k个odd序列的右边界要求再去统计。

```java
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int res = 0;
        int r = 0;
        int lmin = 0;
        int cntmin = 0;
        int lmax = 0;
        int cntmax = 0;
        while (r < nums.length) {
            if (nums[r] % 2 == 1) {
                cntmin++;
                cntmax++;
            }
            r++;
            while (cntmin > k) {
                if (nums[lmin] % 2 == 1)
                    cntmin--;
                lmin++;
            }
            while (cntmax > k || (cntmax == k && nums[lmax] % 2 == 0)) {
                if (nums[lmax] % 2 == 1)
                    cntmax--;
                lmax++;
            }
            // System.out.println(r + " " + lmin + " " + cntmin + " " + lmax + " " + cntmax);
            if (cntmin == k)
                res += lmax - lmin + 1;
        }
        return res;
    }
}
```

930. #### [Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)

求子串和等于sum的个数。**对于二进制序列，和就是个数！**和1248类似，维护一个l<sub>min</sub>和一个l<sub>max</sub>。需要注意的是，这一次S可以取到0，所以对于全0序列，需要考虑l<sub>max</sub>不能无限移动，即超过r。

```java
class Solution {
    public int numSubarraysWithSum(int[] A, int S) {
        int res = 0;
        int r = 0;
        int lmin = 0;
        int cntmin = 0;
        int lmax = 0;
        int cntmax = 0;
        while (r < A.length) {
            if (A[r] == 1) {
                cntmin++;
                cntmax++;
            }
            r++;
            while (cntmin > S) {
                if (A[lmin] == 1)
                    cntmin--;
                lmin++;
            }
            while ((cntmax > S || (cntmax == S && A[lmax] == 0)) && lmax < r - 1) {
                if (A[lmax] == 1)
                    cntmax--;
                lmax++;
            }
            // System.out.println(r + " " + lmin + " " + cntmin + " " + lmax + " " + cntmax);
            if (cntmin == S)
                res += lmax - lmin + 1;
        }
        return res;
    }
}
```

424. #### [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

求可以替换k个字符的最长重复字符串。**枚举不变的字母，变成** **01** **串！**

```java
class Solution {
    public int characterReplacement(String s, int k) {
        char[] ch = s.toCharArray();
        int res = 0;
        for (int i = 'A'; i <= 'Z'; i += 1) {
            int l = 0;
            int r = 0;
            int cnt = 0;
            int ans = 0;
            while (r < ch.length) {
                if (ch[r] != i)
                    cnt++;
                r++;
                while (cnt > k) {
                    if (ch[l] != i)
                        cnt--;
                    l++;
                }
                ans = Math.max(ans, r - l);
            }
            res = Math.max(res, ans);
        }
        return res;
    }
}
```

713. #### [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)

求乘积小于K的子数组个数。确认数组都是正数，才能保证单调性。虽然求数量，但是是不用维护的。而且观察数据范围，不会溢出int，不需要开long。

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int l = 0; 
        int r = 0;
        int res = 0;
        int pd = 1;
        while (r < nums.length) {
            pd *= nums[r];
            r++;
            while (pd >= k && l < r) {
                pd /= nums[l];
                l++;
            }
            res += r - l;
        }
        return res;

    }
}
```

209. #### [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

注意元素都是正的或者在前缀和数组上二分

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int l = 0; 
        int r = 0;
        int res = Integer.MAX_VALUE;
        int sum = 0;
        while (r < nums.length) {
            sum += nums[r];
            r++;
            while (sum >= s) {
                res = Math.min(res, r - l);
                sum -= nums[l];
                l++;
            }
        }
        if (res == Integer.MAX_VALUE)
            return 0;
        return res;
    }
}
```

3. #### [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

统计出现的字母，每次出现重复把左边界推到重复字符的后一位。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<>();
        int l = 0;
        int r = 0;
        int max = 0;
        while (r < s.length()) {
            char c = s.charAt(r);
            r++;
            if (map.containsKey(c)) {
                int last = map.get(c);
                l = Math.max(last, l);
            }
            max = Math.max(max, r - l);
            map.put(c, r);
        }
        return max;
    }
}
```

因为前面的都是合法串，只需要check当前引入即r位置的字符是否出现过。如果重复，不断移动左界直到重复字符消失。另一种写法：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int l = 0;
        int r = 0;
        int res = 0;
        int[] cnt = new int[128];
        boolean rep = false;
        for (; r < n; r++) {
            char c = s.charAt(r);
            cnt[c]++;
            if (cnt[c] > 1)
                rep = true;
            while (rep) {
                c = s.charAt(l);
                cnt[c]--;
                if (cnt[c] == 1)
                    rep = false;
                l++;
            }
            res = Math.max(res, r - l + 1);
        }
        return res;
    }
}
```

904. #### [Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)

求最多两个相异元素的最长区间。同上，用一个map来统计当前连续区间的数字个数，当map长度大于2时，移动左边界，直到使得某个数字全部消失。

```java
class Solution {
    public int totalFruit(int[] tree) {
        Map<Integer, Integer> map = new HashMap<>();
        int l = 0;
        int r = 0;
        int max = 0;
        while (r < tree.length) {
            int a = tree[r];
            r++;
            map.put(a, map.getOrDefault(a, 0) + 1);
            while (map.size() > 2 && l < r) {
                int b = tree[l];
                l++;
                map.put(b, map.get(b) - 1);
                if (map.get(b) == 0)
                    map.remove(b);
            }
            max = Math.max(max, r - l);
        }
        return max;
    }
}
```

159. #### [Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)

同上，改成String而已。

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        Map<Character, Integer> map = new HashMap<>();
        int l = 0;
        int r = 0;
        int max = 0;
        while (r < s.length()) {
            char c = s.charAt(r);
            r++;
            map.put(c, map.getOrDefault(c, 0) + 1);
            while (map.size() > 2 && l < r) {
                char d = s.charAt(l);
                l++;
                map.put(d, map.get(d) - 1);
                if (map.get(d) == 0)
                    map.remove(d);
            }
            max = Math.max(max, r - l);
        }
        return max;
    }
}
```

992. #### [Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)

统计数字频率和出现的种类数。求子串里**正好**K个不同字母的子串个数，转化为**最多K个-最多K-1个**。

```java
class Solution {
    public int subarraysWithKDistinct(int[] A, int K) {
        return atMost(A, K) - atMost(A, K - 1);
    }
    
    public int atMost(int[] A, int K) {
        Map<Integer, Integer> map = new HashMap<>();
        int l = 0;
        int r = 0;
        int max = 0;
        while (r < A.length) {
            int a = A[r];
            r++;
            map.put(a, map.getOrDefault(a, 0) + 1);
            while (map.size() > K && l < r) {
                int b = A[l];
                l++;
                map.put(b, map.get(b) - 1);
                if (map.get(b) == 0)
                    map.remove(b);
            }
            max += r - l;
        }
        return max;
    }
}
```

76. #### [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

==还要再看。==

统计目标串的频率滑动窗口，边滑动边统计所有需要字母的频率，和已经满足需求的种类数

```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            need.put(t.charAt(i), need.getOrDefault(t.charAt(i), 0) + 1);
        }
        HashMap<Character, Integer> window = new HashMap<>();
        int l = 0, r = 0, count = 0, ans = Integer.MAX_VALUE, start = 0;
        while (r < s.length()) {
            char c = s.charAt(r);
            r++;
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (need.get(c).equals(window.get(c)))
                    count++;
            }
            while (count == need.size()) {
                if (r - l < ans) {
                    start = l;
                    ans = r - l;
                }
                char d = s.charAt(l);
                l++;
                if (need.containsKey(d)) {
                    if (window.get(d).equals(need.get(d)))
                        count--;
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return ans == Integer.MAX_VALUE ? "" : s.substring(start, start + ans);
    }
}
```

### 更多例题

1610. #### [Maximum Number of Visible Points](https://leetcode.com/problems/maximum-number-of-visible-points/)

==还要再看==

计算几何 + 环上双指针。将所有的点转化为和x轴的角度，排序，然后找序列中的最长子区间（满足差值<= 视角）。**注意两点**：环上因为某点断开拉成区间，而跨某点的区间只能通过接一倍区间长度L来实现。除此之外，要限制合法最长区间长度为L。

```java
class Solution {
    private double PI = Math.acos(-1.0);
    // convert to angle
    public double ang(double x, double y) {
        return Math.atan2(y, x) * 180.0 / PI;
    }
    // convert the subtraction to valid angles
    public double sub(double x, double y) {
        if (x - y >= 0)
            return x - y;
        return x - y + 360;
    }
    
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {
        int n = points.size();
        int cnt0 = 0;
        int x0 = location.get(0);
        int y0 = location.get(1);
        List<Double> p = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int x = points.get(i).get(0);
            int y = points.get(i).get(1);
            x -= x0;
            y -= y0;
            if (x == 0 && y == 0)
                cnt0++;
            else
                p.add(ang(x, y));
        }
        // sliding window
        Collections.sort(p);
        n = p.size();
        int l = 0;
        int res = 0;
        for (int r = 0; r < n * 2; r++) {
            while ((l + n) <= r || (sub(p.get(r % n), p.get(l % n)) > angle)) {
                l++;
            }
            res = Math.max(res, r - l + 1);
        }
        return res + cnt0;
    }
}
```

1234. #### [Replace the Substring for Balanced String](https://leetcode.com/problems/replace-the-substring-for-balanced-string/)

转化为求最短的，包含所有需要替换的字符的区间。同76。

```java
class Solution {
    public int balancedString(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        int n = s.length();
        StringBuilder sb = new StringBuilder();
        for (Character c : map.keySet()) {
            int k = map.get(c);
            if (k > n / 4) {
                while (k > n / 4) {
                    sb.append(c);
                    k--;
                }
            }
        }
        if (sb.length() == 0)
            return 0;
        return minWindow(s, sb.toString());
    }
    
    public int minWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            need.put(t.charAt(i), need.getOrDefault(t.charAt(i), 0) + 1);
        }
        HashMap<Character, Integer> window = new HashMap<>();
        int l = 0;
        int r = 0;
        int count = 0;
        int ans = Integer.MAX_VALUE;
        int start = 0;
        while (r < s.length()) {
            char c = s.charAt(r);
            r++;
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (need.get(c).equals(window.get(c)))
                    count++;
            }
            while (count == need.size() && l < r) {
                ans = Math.min(ans, r - l);
                char d = s.charAt(l);
                l++;
                if (need.containsKey(d)) {
                    if (window.get(d).equals(need.get(d)))
                        count--;
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```

719. #### [Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/)

**二分** + 2 pointers。==还要再看。==

395. #### [Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)

不枚举种类数，发现不具备单调性！枚举最终串中字母的种类数，维护各个字母的频率及频率到达 K 个的字母种类。

```java
class Solution {
    public int longestSubstring(String s, int k) {
        char[] ch = s.toCharArray();
        int n = s.length();
        int res = 0;
        // enumerate #kind of characters in the result string
        for (int i = 1; i <= 26; i++) {
            int l = 0;
            int r = 0;
            int noLessThanK = 0;
            Map<Character, Integer> map = new HashMap<>();
            while (r < n) {
                char c = ch[r];
                r++;
                map.put(c, map.getOrDefault(c, 0) + 1);
                if (map.get(c) == k)
                    noLessThanK++;
                while (map.size() > i && l < r) {
                    char d = ch[l];
                    l++;
                    if (map.get(d) == k)
                        noLessThanK--;
                    map.put(d, map.getOrDefault(d, 0) - 1);
                    if (map.get(d) == 0)
                        map.remove(d);
                }
                if (noLessThanK == i)
                    res = Math.max(res, r - l);
            }
        }
        return res;
    }
}
```

### 不符合单调性的题目

1371. #### [Find the Longest Substring Containing Vowels in Even Counts](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/)

无法同时满足多个条件

862. #### [Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)

存在负数