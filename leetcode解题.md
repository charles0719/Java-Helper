# LeetCode算法练习

# 字符串

## [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

### 核心思路

- 获取每个位数的数值
- 用long存储反转数值，和Integer的最大值和最小值进行比较

### 代码

```java
 public int reverse(int x) {
        int term;
        long b = 0;
        while (x != 0) {
            term = x % 10;
            x = x / 10;
            b = b*10 + term;
        }
        if(b > Integer.MAX_VALUE || b < Integer.MIN_VALUE) {
            return 0;
        }
        return (int) b;
    }
```

# 数组

## [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

### 核心思路

- 使用int k =0 记录所有不是0的数据
- 可以使用直接赋值的方法或者交换数组的方法

### 代码

```java
    public void moveZeroes(int[] arr) {
        int k = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] != 0) {
                if (k != i) {
                    swap(arr, k++, i);
                } else {
                    k++;
                }
            }
        }
    }

    private void swap(int[] nums, int i, int j) 	{
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
```

## [912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)

给你一个整数数组 `nums`，请你将该数组升序排列。

### 核心思路

- 桶排序
- 初始化的数组长度
- 索引即值

### 代码

```java
     void bucketSort(int[] arr){
        int[] bk = new int[50000 * 2 + 1];
        for(int i = 0; i < arr.length; i++){
            bk[arr[i] + 50000] += 1;
        }
        int ar = 0;
        for(int i = 0; i < bk.length; i++){
            for(int j = bk[i]; j > 0; j--){
                arr[ar++] = i - 50000;
            }
        }
    }
```

# 栈，队列

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

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

## [703. 数据流中的第K大元素](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)

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

## [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

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

## [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

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

## [面试题40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

### 核心思路

- 优先队列

### 代码

```java
    public int[] getLeastNumbers(int[] arr, int k) {
        PriorityQueue<Integer> p = new PriorityQueue();
        for (int num : arr) {
            p.add(num);
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = p.poll();
        }
        return res;
    }
```

# Map和Set

## [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

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

## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

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

# 链表

## [面试题24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

### 核心思路

- 链表的操作

### 代码1--非递归

```java
ListNode cur = head;
        ListNode pre = null;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
```

### 代码2--递归

```java
private ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
```

## [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

### 核心思路

- 使用dummyHead
- 对于移除head节点的数据，要进行特殊处理
- 对于不等于val的值，cur=cur.next

### 代码

```java
		ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode cur = dummyHead;
        while (cur.next != null) {
            if (cur.next.val == val) {
                cur.next = cur.next.next;
            } else {
                cur = cur.next;
            }
        }
        return dummyHead.next;
```

## [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

### 核心思路

- 使用dummyHead，避免对头指针进行特殊操作
- 指针的方向换了之后，对下一个节点的操作进行切换

### 代码

```java
private ListNode swapPairs(ListNode head) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode pre = dummyHead;
        while (pre.next != null && pre.next.next != null) {
            ListNode node1 = pre.next;
            ListNode node2 = node1.next;
            ListNode next = node2.next;

            node1.next = next;
            pre.next = node2;
            node2.next = node1;
            
            pre = node1;
        }
        return dummyHead.next;
    }
```

## [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

### 核心思路

- 对于尾指针不适用

### 代码

```java
    private void deleteNode1(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
```

# 递归

## [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

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

## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

### 核心思路

- 递归函数里面的index代表着遍历到了字符串的第几个字符，str代表着进入递归函数的进行拼接的字符串

### 代码

```java
    private static HashMap<Character, String> map;

    static {
        map = new HashMap<>();
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");
        map.put('0', " ");
        map.put('1', "");
    }

    private ArrayList<String> res;

    public List<String> letterCombinations(String digits) {
        res = new ArrayList<>();
        if(digits.equals(""))
            return res;
        findStr(digits, 0, "");
        return res;
    }

    private void findStr(String digits, int index, String str) {
        if (index == digits.length()) {
            res.add(str);
            return;
        }
        Character c = digits.charAt(index);
        String letter = map.get(c);
        for (int i = 0; i < letter.length(); i++) {
            findStr(digits, index + 1,  str+letter.charAt(i));
        }
    }
```

## [46. 全排列](https://leetcode-cn.com/problems/permutations/)

### 核心思路

- 使用used记录元素是否被使用过
- 递归函数的参数：list是被存储的单元数据，index是存放第n个元素

### 代码

```java
    private ArrayList<List<Integer>> res;
    private boolean[] used;
    public List<List<Integer>> permute(int[] nums) {
        LinkedList<Integer> list = new LinkedList<>();
        res = new ArrayList<List<Integer>>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        used = new boolean[nums.length];
        genPerm(nums, 0, list);
        return res;
    }
     private void genPerm(int[] nums, int index, LinkedList<Integer> list) {
        if (index == nums.length) {
            res.add((LinkedList<Integer>) list.clone());
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (!used[i]) {
                used[i] = true;
                list.addLast(nums[i]);
                genPerm(nums, index + 1, list);
                used[i] = false;
                list.removeLast();
            }
        }
    }
```

## [77. 组合](https://leetcode-cn.com/problems/combinations/)

给定两个整数 *n* 和 *k*，返回 1 ... *n* 中所有可能的 *k* 个数的组合。

### 核心思路

- 边界判定

- 递归参数
- start参数比较特别，当前插入数组的元素。

### 代码

```java
    private ArrayList<List<Integer>> res;
    public List<List<Integer>> combine(int n, int k) {
        res = new ArrayList<>();
        LinkedList<Integer> list = new LinkedList<>();
        if (n <= 0 || k <= 0 || n < k) {
            return res;
        }
        doCombine(n, k, 1, list);
        return res;
    }
    private void doCombine(int n, int k, int start, LinkedList<Integer> list) {
        if (list.size() == k) {
            res.add((List<Integer>) list.clone());
            return;
        }
        for (int i = start; i <= n; i++) {
            list.addLast(i);
            doCombine(n, k, i + 1, list);
            list.removeLast();
        }
    }
```

## [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

示例:

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false

### 核心思路

- 二维数组
- 记录访问过的图标
- index用于记录word的第n的字符
- inArea用于判断坐标是否在board之间

### 代码

```java
 private int[][] d = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    private int m, n;
    private boolean[][] visited;

    public boolean exist(char[][] board, String word) {
        if (board == null || word == null) {
            return false;
        }
        m = board.length;
        if (m == 0) {
            return false;
        }
        n = board[0].length;
        if (n == 0) {
            return false;
        }
        visited = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (findWord(board, word, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean findWord(char[][] board, String word, int index, int startx, int starty) {
        if (index == word.length() - 1) {
            return board[startx][starty] == word.charAt(index);
        }
        if (board[startx][starty] == word.charAt(index)) {
            visited[startx][starty] = true;
            for (int i = 0; i < d.length; i++) {
                int x = startx + d[i][0];
                int y = starty + d[i][1];
                if (inArea(x, y) && !visited[x][y] && findWord(board, word, index + 1, x, y)) {
                    return true;
                }
            }
            visited[startx][starty] = false;
        }
        return false;
    }

    private boolean inArea(int x, int y) {
        return x >= 0 && x < m && y >= 0 && y < n;
    }
```



# 贪心算法

## [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

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

# 二叉树

## [102. 二叉树的层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

### 核心思路

- 队列记录
- 上一层的数据用完就扔

### 代码

```java
List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) {
            return res;
        }
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(root);
        while (!q.isEmpty()) {
            int count = q.size();
            List<Integer> list = new ArrayList<Integer>();
            while (count > 0) {
                TreeNode temp = q.poll();
                list.add(temp.val);
                if (temp.left != null) {
                    q.add(temp.left);
                }
                if (temp.right != null) {
                    q.add(temp.right);
                }
                count--;

            }
            res.add(list);

        }
        return res;
    }
```

## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

### 核心思路

- 递归

### 代码

```java
 public int maxDepth(TreeNode root) {
        if (root == null)
            return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
```

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

### 核心思路

- 递归

### 代码

```java
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
```

## [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

### 核心思路

- 叶子结点
- 空节点
- 非叶子和非空节点

### 代码

```java
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        int val = root.val;
        if (root.left == null && root.right == null) {
            return val == sum;
        }
        return hasPathSum(root.left, sum - val) || hasPathSum(root.right, sum - val);
    }
```

## [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

给定一个二叉树，返回所有从根节点到叶子节点的路径。

**说明:** 叶子节点是指没有子节点的节点。

### 核心思路

- 分析叶子结点和空节点的情况

### 代码

```java
public List<String> binaryTreePaths(TreeNode root) {
        ArrayList<String> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        if (root.left == null && root.right == null) {
            res.add(root.val + "");
            return res;
        }
        List<String> leftRes = binaryTreePaths(root.left);
        for (String str : leftRes) {
            res.add(root.val + "->" + str);
        }
        List<String> rightRes = binaryTreePaths(root.right);
        for (String str : rightRes) {
            res.add(root.val + "->" + str);
        }
        return res;

    }
```

## [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

### 核心思路

- findPath找到根是root节点的所有路径

### 代码

```java
    public int pathSum(TreeNode root, int sum) {
        if (root == null) {
            return 0;
        }
        return findPath1(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
    }

    private int findPath(TreeNode root, int sum) {
        if (root == null) {
            return 0;
        }
        int res = 0;

        if (root.val == sum) {
            res += 1;
        }

        return res + findPath(root.left, sum - root.val) + findPath(root.right, sum - root.val);
    }
```

## [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

### 核心思路

- p的值和q的值在root的两边，返回root（包含root是p或者q节点）
- p的值和q的值在root的一边，返回root的left或者right

### 代码

```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
         if (root == null) {
            return null;
        }
        if (root.val > p.val && root.val > q.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        if (root.val < p.val && root.val < q.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        return root;
    }
```

# 动态规划

## [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

- 自上向下解决
- 自下向上解决
- 使用数组记录，由于使用memo[n]所以创建数组使用memo[n+1]


### 代码1
```java
    public int fib(int N) {
        if (N == 0 || N == 1) {
            return N;
        }
        int[] memos = new int[N + 1];
        memos[1] = 1;
        memos[0] = 0;
        for (int i = 2; i <= N; i++) {
            memos[i] = memos[i - 1] + memos[i - 2];
        }
        return memos[N];
    }
```

### 代码2

```java
    public int fib(int n) {
        int[] memos = new int[n + 1];
        int res = fib(n, memos);
        return res;
    }

    public int fib(int n ,int[] memos){
        if (n == 1 || n == 0) {
            return n;
        }
        if (memos[n] == 0) {
            memos[n] = fib(n - 1, memos) + fib(n - 2, memos);
        }
        return memos[n];
    }
```

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

### 核心思路

上同

### 代码

```java
    public int climbStairs(int n) {
        int[] res = new int[n + 1];
        res[0] = 1;
        res[1] = 1;
        for (int i = 2; i <= n; i++) {
            res[i] = res[i - 1] + res[i - 2];
        }
        System.out.println(Arrays.toString(res));
        return res[n];
    }
```



## [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

### 核心思路

- 使用数组记录数据
- 记录初始数据，不断计算memo[n]的数值
- 不断使用高阶的integerBreak去调用低阶，最低阶的直接return

### 代码

```java
    public int integerBreak(int n) {
        int[] memo = new int[n + 1];
        memo[1] = 1;
        for (int j = 2; j <= n; j++) {
            for (int i = 1; i <= j - 1; i++) {
                memo[j] = max3(memo[j - i] * i, memo[j], (j - i) * i);
            }
        }
        return memo[n];
    }
    
    private int max3(int a, int b, int c) {
        return Math.max(a, Math.max(b, c));
    }
```

