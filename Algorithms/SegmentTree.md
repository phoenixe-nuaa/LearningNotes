[toc]

# [Lecture 7](https://www.youtube.com/watch?v=h0IeW9MYLfQ)

## Segment Tree 线段树

线段树是解决区间问题的利器，无论是区间修改还是区间询问，线段树可以处理其中的很多问题。经典的线段树操作包含，**单点修改**、**区间 add**、**区间 set**、**询问区间最值**、**询问区间和**，所有操作支持在线，时间为**O(logN)**。

### 核心思想

#### 建树

将区间进行（二进制）拆分，这样对于修改和询问只关注其中的的区间。 

这里我们会把所有包含了一段以上信息的点拆分为长度差不多的两段，拆分后我们把大段作为拆分后两段的父亲节点，这样我们就得到了一棵满二叉树（full binary tree，内节点度均为 2）。对于单位区间个数刚好是2的幂次的情况，我们会得到一棵完美二叉树（perfect binary tree）。 

![img](https://lh6.googleusercontent.com/Ee5px2f1FpC7sosL3n44YlHT6ZUydrKwdLdeYvaAXv6O3wY7vP-XAHiIFnzfk1sOxSM--JHX1hr1zgPosqbC0xndMT3UhkdHzTL69zUMQ-Rbh97_P4OwnwM4q6S2cTdKuyYPmQ2v)

#### 总节点数

由于线段树是一棵满二叉树，**总结点树 = 单位节点数 \* 2 - 1**。

#### 每层区间宽度

每层节点的区间长度最多只有两种，L和L+1。

#### 线段树的地基

线段树的单位区间只可能出现在最下面两行，也就是说，线段树只可能最下面的一行存在一些空缺。 

#### 层数

![img](https://lh4.googleusercontent.com/7tNwq83HwOD6dyXNjjrYrmiFFq7wk9lqPSIPlcKGvYVyL52SoEfidxzzxr3xi3a3FSJpfZqhWWfZHOQx8sSHa-LmppmdTqmPYRF5pHt4LCUvWBSzHNK05ygxEcK9kaTSQ8xeoi81)

#### 单点修改

这里我们发现一个单位区间也会属于O(logN)个线段树中的区间，而这些区间就是树上一条根到叶子的链。因此对于某个位置的修改我们只要修改这些影响到的区间即可。

#### 区间查询

线段树最关键的操作，按照我们的划分很多区间询问我们没必要非得进行到叶子。当某个父节点已经包含了所有想询问的信息，且所有包含的信息都是被访问的信息时，我们就可以忽略其所有子节点。 

#### 代码实现

##### 节点

线段树有两种代码实现流派，一种是指针流 

```java
class TreeNode {
    // Info
    int sum;
    
    // interval
    // int l,r;
    
    TreeNode lson, rson;
    TreeNode(int sum) {
        this.sum = sum;
        this.lson = this.rson = null;
    }
}
```

注意：区间信息不一定要维护在节点里，可以随着操作递归的过程推导区间信息。指针流的最大好处是在我们处理一些问题时可以**动态开点**。所谓动态开点，对于线段树而言，如果我们能在高层解决问题时，完全没有必要建立完整的树，因此我们完全可以只建立我们需要的节点。![img](https://lh4.googleusercontent.com/q7RqC0hbod8EsEDOFTvzQ4q-JGdC6gg222qoMmeFzSsNO09BhdvCURxpLgOI5u-wE-f4bp_PR-9gZ8onWXWRrFOex2zVKHVzZRx6M1ywT6rPF2B35X8WuU7xTlGHO18oKYD2pVey)

##### 建树

使用堆式存储处理完全二叉树（complete binary tree）是没有问题的，但是线段树最下方一行可能会出现城墙形状，因此显然2N个节点可能是不够用的，那么究竟要多少节点才够呢？经验上来说这个大小要开到**4倍**。

```java
class SegTree {
  int[] sum;

  public void SegTree(int[] a) {
    int n = a.length;
    sum = new int[n << 2]; // 4n
    build(1, 1, n, a);

    int lson(int o) {
      return o << 1;
    }

    int rson(int o) {
      return o << 1 | 1;
    }

    void pull(int o) {
      sum[o] = sum[o << 1] + sum[o << 1 | 1];
    }

    // o: node idx
    // interval [l, r]
    void build(int o, int l, int r, int[] a) {
      if (l == r) {
        sum[o] = a[l];
        return;
      }
      int m = (l + r) >> 1;
      build(o << 1, l, m, a);
      build(o << 1 | 1, m + 1, r, a);
      pull(o);
    }
  }
}
```

##### 单点修改

我们先在线段树上二分，找到目标单位区间，在回溯的时候记得把区间值改对即可。

```java
class SegTree {
  ...
  // a[p] = v
  public void update(int o, int l, int r, int p, int v) {
    if (l == r) {
      sum[o] = v;
      return;
    }
    int m = (l + r) >> 1;
    if (p <= m) {
      update(o << 1, l, m, p, v);
    }
    else {
      update(o << 1 | 1, m + 1, r, p, v);
    }
    pull(o);
  }
}
```

##### 区间询问

```java
class SegTree {
  ...
  // sum(ql, qr)
  public int query(int o, int l, int r, int ql, int qr) {
    // ( )
    if (ql <= l && r <= qr) {
      return sum[o];
    }
    // x x
    else if (r < ql || qr < l) {
      return 0;
    }
    // { }
    int m = (l + r) >> 1;
    return query(o << 1, l, m, ql, qr) + query(o << 1 | 1, m + 1, r, ql, qr);
  }
}
```

#### 例题

307. [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

模板题。

```java
class NumArray {
    int[] sum;
    int n;

    public NumArray(int[] nums) {
        n = nums.length;
        sum = new int[n << 2]; // 4n
        build(1, 0, n - 1, nums);
    }
    
    public void pull(int o) {
        sum[o] = sum[o << 1] + sum[o << 1 | 1];
    }
    
    // o: node idx
    // interval [l, r]
    public void build(int o, int l, int r, int a[]) {
        if (l == r) {
            sum[o] = a[l];
            return;
        }
        int m = (l + r) >> 1;
        build(o << 1, l, m, a);
        build(o << 1 | 1, m + 1, r, a);
        pull(o);
    }
    
    // a[p] = v
    public void update(int o, int l, int r, int p, int v) {
        if (l == r) {
            sum[o] = v;
            return;
        }
        int m = (l + r) >> 1;
        if (p <= m) {
            update(o << 1, l, m, p, v);
        }
        else {
            update(o << 1 | 1, m + 1, r, p, v);
        }
        pull(o);
    }
    
    public void update(int index, int val) {
        update(1, 0, n - 1, index, val);
    }
    
    public int query(int o, int l, int r, int ql, int qr) {
        if (ql <= l && r <= qr) {
            return sum[o];
        }
        else if (r < ql || qr < l) {
            return 0;
        }
        int m = (l + r) >> 1;
        return query(o << 1, l, m, ql, qr) + query(o << 1 | 1, m + 1, r, ql, qr);
    }
    
    public int sumRange(int left, int right) {
        return query(1, 0, n - 1, left, right);
    }
}
```

#### 懒标记与区间修改

##### 标记之间的影响

##### 例题

732. [My Calendar III](https://leetcode.com/problems/my-calendar-iii/)

##### 更多题目

218. [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

区间 max 修改，单点求值 / 扫描线

699. [Falling Squares](https://leetcode.com/problems/falling-squares/)

区间 set 修改，区间 max 查询

715. [Range Module](https://leetcode.com/problems/range-module/)

区间 set 区间 sum

850. [Rectangle Area II](https://leetcode.com/problems/rectangle-area-ii/)

#### 权值线段树

315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

327. [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/)

前缀和 + 离散化 + 线段树 / 前缀和 + 动态线段树（单点修改，rank 查询）

493. [Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

离散化 + 线段树（单点修改，区间询问）

1649. [Create Sorted Array through Instructions](https://leetcode.com/problems/create-sorted-array-through-instructions/)

单点修改，区间询问

#### 其他

308. [Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

二维线段树（建议写二维树状数组）

1157. [Online Majority Element In Subarray](https://leetcode.com/problems/online-majority-element-in-subarray/)

分治结构利用线段树实现区间查询。