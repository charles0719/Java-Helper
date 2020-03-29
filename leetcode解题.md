# leetcode解题

## 20. 有效的括号

### 核心思路

- 栈
- 字符比较 ==
### 代码

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack();
        for (int i = 0; i < s.length(); i++) {
            Character c = s.charAt(i);
            if (c == '{' || c == '(' || c == '[') {
                stack.push(c);
            } else {
                if (stack.size() == 0) {
                    return false;
                }
                Character pop = stack.pop();
                if (c == '}' && pop == '{' || c == ')' && pop == '(' || c == ']' && pop == '[') {

                } else {
                    return false;
                }
            }
        }
        if (stack.size() != 0) {
            return false;
        }
        return true;
    }
}
```

## 703 数据流中的第K大元素

设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。

你的 `KthLargest` 类需要一个同时接收整数 `k` 和整数数组`nums` 的构造器，它包含数据流中的初始元素。每次调用 `KthLargest.add`，返回当前数据流中第K大的元素。

### 核心思路

- 优先队列
- offer/poll
### 代码

```java
class KthLargest {
    int n;
    PriorityQueue<Integer> p;

    public KthLargest(int k, int[] nums) {
        n = k;
        p = new PriorityQueue<>(k);
        for (int num : nums) {
            add(num);
        }
    }

    public int add(int val) {
        if (p.size() < n) {
            p.offer(val);
        } else if (p.peek() < val) {
            p.poll();
            p.offer(val);
        }
        System.out.println(p.peek());
        return p.peek();
    }
}
```

## 232 用栈实现队列

使用栈实现队列的下列操作：

- push(x) -- 将一个元素放入队列的尾部。
- pop() -- 从队列首部移除元素。
- peek() -- 返回队列首部的元素。
- empty() -- 返回队列是否为空。

### 核心思路

- 使用堆栈
- 当出栈完全空的时候，才将入栈压进来。

### 代码

```java
class MyQueue {
    private Stack<Integer> inStack;

    private Stack<Integer> outStack;

    /**
     * Initialize your data structure here.
     */
    public MyQueue() {
        inStack = new Stack();
        outStack = new Stack();
    }

    /**
     * Push element x to the back of queue.
     */
    public void push(int x) {
        inStack.push(x);
    }

    /**
     * Removes the element from in front of queue and returns that element.
     */
    public int pop() {
        if (outStack.isEmpty()) {
            while (!inStack.isEmpty()) {
                outStack.push(inStack.pop());
            }
        }
        return outStack.pop();
    }

    /**
     * Get the front element.
     */
    public int peek() {
        if (outStack.isEmpty()) {
            while (!inStack.isEmpty()) {
                outStack.push(inStack.pop());
            }
        }
        return outStack.peek();
    }

    /**
     * Returns whether the queue is empty.
     */
    public boolean empty() {
        return outStack.isEmpty() && inStack.isEmpty();
    }

    public static void main(String[] args) {
        MyQueue queue = new MyQueue();
        queue.push(1);
        queue.push(2);
        System.out.println(queue.pop());
        queue.push(3);
        System.out.println(queue.pop());
        System.out.println(queue.pop());
    }
}
```

## 239 滑动窗口最大值

给定一个数组 *nums*，有一个大小为 *k* 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 *k* 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

### 核心思路

- 使用优先队列并维护
- 对小于k和不小于k的分别处理
- 最大堆的优先队列
- remove方法
- 对num的大小进行判定[]

### 代码

```java
    public int[] maxSlidingWindow1(int[] nums, int k) {
        if (nums == null || nums.length == 0) return new int[0];
        Comparator<Integer> cmp = new Comparator<Integer>() {
            public int compare(Integer e1, Integer e2) {
                return e2 - e1;
            }
        };
        PriorityQueue<Integer> p = new PriorityQueue(k, cmp);
        int[] res = new int[nums.length - k + 1];
        for (int i = 0; i < nums.length; i++) {
            if (p.size() < k) {
                p.offer(nums[i]);
            } else {
                p.remove(nums[i - k]);
                p.offer(nums[i]);
            }
            if (i >= k - 1) {
                res[i - k + 1] = p.peek();
            }
        }
        return res;
    }
```

## 242 有效的字母异位词

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。

### 核心思路

- 使用map或者使用数组（c-'a'）
- 判断字符是否清零，清零代表s和t里面的字符数相等

### 代码

```java
public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        HashMap<Character, Integer> map = new HashMap();
        for (int i = 0; i < s.length(); i++) {
            if (map.get(s.charAt(i)) == null) {
                map.put(s.charAt(i), 1);
            } else {
                Integer count = map.get(s.charAt(i));
                count++;
                map.put(s.charAt(i), count);
            }
        }
        for (int i = 0; i < t.length(); i++) {
            if (map.get(t.charAt(i)) == null) {
                map.put(t.charAt(i), -1);
            } else {
                Integer count = map.get(t.charAt(i));
                count--;
                map.put(t.charAt(i), count);
            }
        }

        for (Character c : map.keySet()) {
            if (map.get(c) != 0) {
                return false;
            }
        }
        return true;
    }
```

## 169 多数元素

### 核心思路

- 使用map更简单
- 使用count不断计数，记录最大出现的元素个数和value，前提是必须有那个多数元素

### 代码

```java
public int majorityElement(int[] nums) {
        int count = 1;
        int value = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (value == nums[i]) {
                count++;
            } else {
                count--;
            }
            if (count < 1) {
                value = nums[i];
                count = 1;
            }
        }
        return value;
    }
```

### 小故事

摩尔投票法：

核心就是对拼消耗。

玩一个诸侯争霸的游戏，假设你方人口超过总人口一半以上，并且能保证每个人口出去干仗都能一对一同归于尽。最后还有人活下来的国家就是胜利。

那就大混战呗，最差所有人都联合起来对付你（对应你每次选择作为计数器的数都是众数），或者其他国家也会相互攻击（会选择其他数作为计数器的数），但是只要你们不要内斗，最后肯定你赢。

最后能剩下的必定是自己人。

## 面试题24 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

### 核心思路



### 代码

## 50. Pow(x, n)

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数。

### 核心思路

- 对n=0,n<0,n为奇数的条件判断处理
- 对n为-2147483648要进行特殊处理，因为-n=n

### 代码

```java
		if (n == 0) {
            return 1.0;
        }
        if (n < 0) {
            return 1.0 / myPow(x, -n);
        }
        if ((n & 1) != 1) {
            return myPow( x * x, n / 2);
        }
        return myPow(x, n - 1) * x;
```

### 相关

```java
int a = -2147483648;
int b = a*-1;//-2147483648
int c = a-1;//2147483647
```

## 122. 买卖股票的最佳时机 II

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 核心思路

- 每天如果有收益，那么就收下收益。

### 代码

```java
public int maxProfit(int[] prices) {
        if (prices.length == 1) {
            return 0;
        }
        int sum = 0;
        for (int i = 1; i < prices.length; i++) {
            int sub = prices[i] - prices[i - 1];
            if (sub > 0) {
                sum += sub;
            }
        }
        return sum;
    }
```

## [102. 二叉树的层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

### 核心思路



### 代码

