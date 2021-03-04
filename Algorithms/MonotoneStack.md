[toc]

# [Lecture 5 & 6](https://www.youtube.com/watch?v=eqoZyUS4faw)

## Monotone Stack

### 通过不插入维护单调性的单调栈

**![img](https://lh4.googleusercontent.com/LA5PhUqCFuhrMbv2K50oS1F5DjZDRTUNR0lrLNPkamo2Ww-_XzvkL-OfbjyLsMR4n6vUXS8EoXAF0mFdWFODfMl3fGjFgJk0sbwWHMcEqpzeErxWhqqcO2mXXMFPi9JpWekBEE-f)**

#### 例题

155. [Min Stack](https://leetcode.com/problems/min-stack/)

模板题。维护一个从栈底到栈顶单调减的min栈，还需要一个val栈来记录进栈和出栈序列。进栈时，只要更小的先进栈，后面出现的更大的就不会成为min，所以只进栈比min栈栈顶等于或更小的元素。出栈时，只要val栈栈顶不等于min栈顶，就只出栈val栈，否则一起出栈。

```java
class MinStack {
    Stack<Integer> stVal;
    Stack<Integer> stMin;
    
    /** initialize your data structure here. */
    public MinStack() {
        stVal = new Stack<>();
        stMin = new Stack<>();
    }
    
    public void push(int x) {
        if (stMin.isEmpty() || stMin.peek() >= x)
            stMin.push(x);
        stVal.push(x);
    }
    
    public void pop() {
        if (!stMin.isEmpty() && stVal.peek().equals(stMin.peek()))
            stMin.pop();
        stVal.pop();
    }
    
    public int top() {
        return stVal.peek();
    }
    
    public int getMin() {
        return stMin.peek();
    }
}
```

### 通过弹出元素维护单调性的单调栈

**![img](https://lh3.googleusercontent.com/gfxM9GVDRGz7Cam4qxABa6hAlfEnIKiZLpodO7_TUBPUWVFVHlyc8qUCQRf0IChuROxsdGeG8lyRXERxD2_rRV8bJG4cwlJ6geyu-5d01dcUi4yX0bNjLfKj3D5xNxz92_zFPDoJ)**

#### 复杂度分析

每个值只会进栈一次，最多出栈一次，因此时间复杂度O(N)。

#### 例题

##### 最远延伸位置

901. [Online Stock Span](https://leetcode.com/problems/online-stock-span/)

模板。维护值单调减栈。最远延伸为栈里的前一个元素。为了处理第一天以后未来出现比栈内所有元素都大的元素排空栈的corner case，在-1位置push一个最大值。

```java
class StockSpanner {
    private Stack<Integer> stVal;
    private Stack<Integer> stIdx;
    private int idx;
    
    public StockSpanner() {
        stVal = new Stack<>();
        stIdx = new Stack<>();
        idx = 0;
        stVal.push(100005);
        stIdx.push(-1);
    }
    
    public int next(int price) {
        while (!stVal.isEmpty() && stVal.peek() <= price) {
            stVal.pop();
            stIdx.pop();
        }
        int ans = idx - stIdx.peek();
        stVal.push(price);
        stIdx.push(idx++);
        return ans;
    }
}
```

496. [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

维护一个值单调减栈。对应元素的右侧第一个最大数即，使他们出栈的元素。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        // map from x to next greater element of x
        Map<Integer, Integer> map = new HashMap<>(); 
        Stack<Integer> stack = new Stack<>();
        for (int num : nums2) {
            while (!stack.isEmpty() && stack.peek() < num)
                map.put(stack.pop(), num);
            stack.push(num);
        }   
        for (int i = 0; i < nums1.length; i++)
            nums1[i] = map.getOrDefault(nums1[i], -1);
        return nums1;
    }
}
```

503. [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)

环上，注意double array，同时保证答案的合法长度不超过一个len。

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        Stack<Integer> st = new Stack<>();
        int len = nums.length;
        int[] res = new int[len];
        Arrays.fill(res, -1);
        for (int i = 0; i < 2 * len; i++) {
            int idx = i % len;
            while (!st.isEmpty() && nums[idx] > nums[st.peek()]) {
                res[st.pop()] = nums[idx];
            }
            if (i < len)
                st.push(idx);
        }
        return res;
    }
}
```

739. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

相当于找右边的第一个更大的数字，同503。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        Stack<Integer> st = new Stack<>();
        int n = T.length;
        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && T[st.peek()] < T[i]) {
                res[st.peek()] = i - st.pop();
            }
            st.push(i);
        }
        return res;
    }
}
```

1063. [Number of Valid Subarrays](https://leetcode.com/problems/number-of-valid-subarrays/)

维护一个单调增的栈，每次加上push前栈里的个数（以栈里的元素为leftmost的子序列）。

```java
class Solution {
    public int validSubarrays(int[] nums) {
        int n = nums.length;
        int cnt = n;
        Stack<Integer> st = new Stack<>();
        int prev = 0;
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && nums[st.peek()] > nums[i]) {
                st.pop();
            }
            cnt += st.size();
            st.push(i);
        }
        return cnt;
    }
}
```

907. [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/)

维护两个栈，分别记录以某个元素为终点和起点的合法子数组。相当于找某元素向左和向右延伸，找遇到的第一个最小值。

```java
class Solution {
    public int sumSubarrayMins(int[] arr) {
        int n = arr.length;
        long res = 0l;
        long mod = (int)1e9 + 7;
        // ascending
        Stack<Integer> stR = new Stack<>();
        Stack<Integer> stL = new Stack<>();
        int[] left = new int[n];
        int[] right = new int[n];
        for (int i = 0 ; i <= n; i++) {
            int num = i == n ? 0 : arr[i];
            while (!stR.isEmpty() && arr[stR.peek()] > num) {
                right[stR.peek()] = i - stR.pop();
            }
            stR.push(i);
        }
        for (int i = n - 1 ; i >= -1; i--) {
            int num = i == -1 ? 0 : arr[i];
            while (!stL.isEmpty() && arr[stL.peek()] >= num) {
                left[stL.peek()] = stL.pop() - i;
            }
            stL.push(i);
        }
        for (int i = 0; i < n; i++) {
            res = (res + (long)arr[i] * left[i] * right[i]) % mod;
        }
        return (int)res;
    }
}
```

也可以用一个栈维护。因为序列单调递增，左边第一个最小值即栈里前一个数，右边第一个最小值为使当前元素出栈的元素。

```java
class Solution {
    public int sumSubarrayMins(int[] A) {
        Stack<Integer> s = new Stack<>();
        int n = A.length, j, k;
        long res = 0, mod = (long)1e9 + 7;
        for (int i = 0; i <= n; i++) {
            while (!s.isEmpty() && A[s.peek()] > (i == n ? 0 : A[i])) {
                j = s.pop();
                k = s.isEmpty() ? -1 : s.peek();
                res = (res + (long)A[j] * (i - j) * (j - k)) % mod;

            }
            s.push(i);
        }
        return (int)res;
    }
}
```

##### 出栈时维护信息

###### 计算器系列

用单调栈维护优先级。正常计算顺序是从左到右，即依次出栈计算。当后一个优先级高于当前的优先级，入栈，即会先被出栈计算。反之，出栈计算。

224. [Basic Calculator](https://leetcode.com/problems/basic-calculator/)

\+ - ()

227. [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

\+ - * /

772. [Basic Calculator III](https://leetcode.com/problems/basic-calculator-iii/)

\+ - * / ()

```java
class Solution {
    public int calculate(String s) {
        // operator mapping
        Map<Character, BinaryOperator<Integer>> opmap = new HashMap<>();
        opmap.put('+', (a, b) -> a + b);
        opmap.put('-', (a, b) -> a - b);
        opmap.put('*', (a, b) -> a * b);
        opmap.put('/', (a, b) -> a / b);
        Stack<Integer> stVal = new Stack<>();
        Stack<Integer> stPri = new Stack<>();
        Stack<Character> stOp = new Stack<>();
        int prio = 0;
        int num = 0;
        // make sure to calculate it all
        s += '+';
        for (char c : s.toCharArray()) {
            if (c == ' ')
                continue;
            else if (c == '(')
                prio += 2;
            else if (c == ')')
                prio -= 2;
            else if ('0' <= c && c <= '9')
                num = num * 10 + c - '0';
            // + - * /
            else {
                int curPri = prio;
                if (c == '*' || c == '/')
                    curPri += 1;
                stVal.push(num);
                num = 0;
                while (!stPri.isEmpty() && stPri.peek() >= curPri) {
                    int b = stVal.pop();
                    int a = stVal.pop();
                    char op = stOp.pop();
                    stPri.pop();
                    stVal.push(opmap.get(op).apply(a, b));
                }
                stPri.push(curPri);
                stOp.push(c);
            }
        }
        return stVal.peek();
    }
}
```

770. [Basic Calculator IV](https://leetcode.com/problems/basic-calculator-iv/)

\+ - * / () variable

150. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

逆波兰表达式不需要单调栈，普通栈即可。

```java
class Solution {
    public int evalRPN(String[] tokens) {
        int a, b;
        Stack<Integer> nums = new Stack<>();
        for (String s : tokens) {
            if (s.equals("+"))
                nums.push(nums.pop() + nums.pop());
            else if (s.equals("-")) {
                b = nums.pop();
                a = nums.pop();
                nums.push(a - b);
            }
            else if (s.equals("*"))
                nums.push(nums.pop() * nums.pop());
            else if (s.equals("/")) {
                b = nums.pop();
                a = nums.pop();
                nums.push(a / b);                
            }
            else
                nums.push(Integer.parseInt(s));
        }
        return nums.pop();
    }
}
```

###### 直方图系列

84. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

转化为以当前高度为最小值（矩形上界）的最长延伸。同907。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        // ascending
        Stack<Integer> st = new Stack<>();
        int n = heights.length;
        int max = 0;
        for (int i = 0; i <= n; i++) {
            int curH = i == n ? 0 : heights[i];
            while (!st.isEmpty() && heights[st.peek()] > curH) {
                // j k m
                int k = st.pop();
                int j = st.isEmpty() ? -1 : st.peek();
                int m = i;
                max = Math.max(max, heights[k] * (m - j - 1));
            }
            st.push(i);
        }
        return max;
    }
}
```

85. [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

dp

###### 限制可以弹出的部分

402. [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)

维护一个单调增的栈，即出现更小的则淘汰栈顶更大的，当k不够时不再排栈。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        // ascending
        StringBuilder sb = new StringBuilder();
        int n = num.length();
        if (k >= num.length()) 
            return "0";
        for (int i = 0; i < n; i++) {
            while (k > 0 && sb.length() != 0 && sb.charAt(sb.length() - 1) > num.charAt(i)) {
                sb.setLength(sb.length() - 1);
                k--;
            }
            sb.append(num.charAt(i));
        }
        while (k > 0) {
            sb.setLength(sb.length() - 1);
            k--;
        }
        return sb.toString().replaceFirst("^0+(?!$)", "");
    }
}
```

1673. [Find the Most Competitive Subsequence](https://leetcode.com/problems/find-the-most-competitive-subsequence/)

注意这两个题目也可以使用优先队列或者单调队列来做。

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        // ascending
        ArrayList<Integer> res = new ArrayList<>();
        int n = nums.length;
        k = n - k;
        if (k >= n) {
            return new int[0];
        }
        for (int i = 0; i < n; i++) {
            while (k > 0 && res.size() != 0 && res.get(res.size() - 1) > nums[i]) {
                res.remove(res.size() - 1);
                k--;
            }
            res.add(nums[i]);
        }
        while (k > 0) {
            res.remove(res.size() - 1);
            k--;
        }
        int[] arr = new int[res.size()];
        int idx = 0;
        for (int a : res) {
            arr[idx++] = a;
        }
        return arr;
    }
}
```

316. [Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/) 

维护一个单调递增栈来保证字典序最小，只有当需要出栈时的元素在后面再次出现，才能合法出栈。所以考虑用一个lastSeen数组来记录每个元素出现的最后位置。同时记录每个字符是否已经出现在栈里，因为已经是最优结果，我们不再将该字符推入栈中。

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        int n = s.length();
        int[] lastSeen = new int[26];
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            lastSeen[c - 'a'] = i;
        }
        Stack<Character> st = new Stack<>();
        // where exist in stack
        boolean[] inStack = new boolean[26];
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (inStack[c - 'a'])
                continue;
            while (!st.isEmpty() && st.peek() > c && lastSeen[st.peek() - 'a'] > i) {
                inStack[st.peek() - 'a'] = false;
                st.pop();
            }
            st.push(c);
            inStack[c - 'a'] = true;
        }
        StringBuilder sb = new StringBuilder();
        while (!st.isEmpty())
            sb.append(st.pop());
        return sb.reverse().toString();
    }
}
```

1081. [Smallest Subsequence of Distinct Characters](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

同316。

###### 其他

456. [132 Pattern](https://leetcode.com/problems/132-pattern/)

768. [Max Chunks To Make Sorted II](https://leetcode.com/problems/max-chunks-to-make-sorted-ii/)

[1:51:14](https://www.youtube.com/watch?v=eqoZyUS4faw)

1130. [Minimum Cost Tree From Leaf Values](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/)

42. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)