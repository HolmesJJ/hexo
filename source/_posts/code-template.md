---
title: 代码模板
---

对 [LeetCode](https://leetcode.com/problemset/algorithms/) 常用算法的整理，更多的内容可以参考 [LeetCode整理](https://github.com/HolmesJJ/LeetCode)。

## 二分查找

### 使用条件

{% iframe "https://holmesjj.github.io/code-training/#/mrq/binarySearch" "100%" "270" %}

<details>
<summary>显示答案</summary>

* 排序数组(30-40%是二分)
* 当面试官要求你找一个比O(n)更小的时间复杂度算法的时候(99%)
* 找到数组中的一个分割位置，使得左半部分满足某个条件，右半部分不满足(100%)
* 找到一个最大/最小的值使得某个条件被满足(90%)

</details>

### 典型例题

* [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
* 矩阵二分查找 [74. Search a 2D Matrix](74.%20Search%20a%202D%20Matrix.md)
* 二叉树与二分查找的结合 [222. Count Complete Tree Nodes](222.%20Count%20Complete%20Tree%20Nodes.md)

### 非递归版代码

{% iframe "https://holmesjj.github.io/code-training/#/code/binarySearchIteration" "100%" "430" %}

<details>
<summary>显示答案</summary>

```java
// Corner Case 处理
if (nums == null || nums.length == 0) {
    return -1;
}

// 若找不到target，则返回比target大一点的值的位置
// 若target值重复，则始终返回第一个target的位置；若要返回最后一个target的位置，则需要修改为nums[mid] <= target
public int binarySearch(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        // | ---------- | --target-- |
        if (nums[mid] < target) {
            low = mid + 1;
        }
        // | --target-- | ---------- |
        else {
            high = mid - 1;
        }
    }
    return low;
}

// 找不到target
int pos = binarySearch(nums, target);
if (pos == nums.length || nums[pos] > target) {
    return -1;
}
```

</details>

### 递归版代码

{% iframe "https://holmesjj.github.io/code-training/#/code/BinarySearchRecursion" "100%" "430" %}

<details>
<summary>显示答案</summary>

```java
// Corner Case 处理
if (nums == null || nums.length == 0) {
    return -1;
}

// 若找不到target，则返回比target大一点的值的位置
// 若target值重复，则始终返回第一个target的位置；若要返回最后一个target的位置，则需要修改为nums[mid] <= target
public int binarySearch(int[] nums, int target, int low, int high) {
    if (low > high) {
        return low;
    }
    int mid = low + (high - low) / 2;
    // | ---------- | --target-- |
    if (nums[mid] < target) {
        return binarySearch(nums, target, mid + 1, high);
    }
    // | --target-- | ---------- |
    else {
        return binarySearch(nums, target, low, mid - 1);
    }
}

// 找不到target
int pos = binarySearch(nums, target, 0, nums.length - 1);
if (pos == nums.length || nums[pos] > target) {
    return -1;
}
```

</details>

### 复杂度

<details>
<summary>显示答案</summary>

* 时间复杂度：O(logn)
* 空间复杂度：O(1)

</details>

## 二叉树

* 深度Depth从上到下，root的深度为0
* 高度Height从下到上，leaf的高度为0
* 二叉树每层结点数 = 2^Height = 2^Depth
* 二叉树总结点 = 2^(Height + 1) - 1 = 2^(Depth + 1) - 1

## 广度优先遍历(BFS)

### 典型例题
* 灵活运用层次遍历 [662. Maximum Width of Binary Tree](662.%20Maximum%20Width%20of%20Binary%20Tree.md)
* 深入理解BFS的特性，图与层次遍历的结合 [934. Shortest Bridge](934.%20Shortest%20Bridge.md)

### 使用条件

{% iframe "https://holmesjj.github.io/code-training/#/mrq/bfs" "100%" "270" %}

<details>
<summary>显示答案</summary>

* 拓扑排序(100%)
* 出现连通块的关键词(100%)
* 分层遍历(100%)
* 简单图最短路径(100%)
* 给定一个变换规则，从初始状态变到终止状态最少几步(100%)

</details>

### 代码

{% iframe "https://holmesjj.github.io/code-training/#/code/BFS" "100%" "380" %}

<details>
<summary>显示答案</summary>

```java
// 返回类型根据题目要求决定
public void bfs(char[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    if (m == 0 || n == 0) {
        return;
    }

    // 8个方向
    int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}, {-1, -1}, {-1, 1}, {1, -1}, {1, 1}};

    // 已访问结点
    boolean[][] visited = new boolean[m][n];
    // 源点被访问
    visited[0][0] = true;

    // 队列记录结点的坐标(i, j)
    Queue<int[]> qn = new LinkedList<>();
    // 从源点开始
    qn.add(new int[]{0, 0});

    // BFS遍历
    while (!qn.isEmpty()) {
        int[] node = qn.poll();
        int i = node[0];
        int j = node[1];
        // 遍历与当前结点相邻的结点
        // 8个方向的相邻结点
        for (int dir = 0; dir < dirs.length; dir++) {
            int ni = i + dirs[dir][0];
            int nj = j + dirs[dir][1];
            // 在grid中
            if (ni >= 0 && ni < m && nj >= 0 && nj < n /* && 是否相邻 */) {
                // 排除已经访问过的结点
                // 把符合条件的结点入队
                if (visited[ni][nj] == false /* && 其它条件 */) {
                    qn.add(new int[]{ni, nj});
                    // 结点被访问
                    visited[ni][nj] = true;
                }
            }
        }
    }
}
```

</details>

### 树的层次遍历代码

{% iframe "https://holmesjj.github.io/code-training/#/code/levelOrder" "100%" "380" %}

<details>
<summary>显示答案</summary>

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> levels = new ArrayList<>();
    if (root == null) {
        return levels;
    }

    Queue<TreeNode> qn = new LinkedList<>();
    qn.add(root);

    // BFS遍历
    while (!qn.isEmpty()) {
        int size = qn.size();
        TreeNode node = null;
        // 一层一层遍历，一个for循环代表一层
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            node = qn.poll();
            level.add(node.val);
            if (node.left != null) {
                qn.add(node.left);
            }
            if (node.right != null) {
                qn.add(node.right);
            }
        }
        levels.add(level);
    }
    return levels;
}
```

</details>

### 复杂度

<details>
<summary>显示答案</summary>

* 时间复杂度：O(E + V)
* 空间复杂度：O(V)

</details>

## 深度优先遍历(DFS)

### 使用条件

{% iframe "https://holmesjj.github.io/code-training/#/mrq/dfs" "100%" "270" %}

<details>
<summary>显示答案</summary>

* BFS不能解决的问题，否则很容易超时
* 找满足某个条件的所有方案(99%)
* 二叉树Binary Tree的问题(90%)
* 组合问题(95%)
    * 问题模型：求出所有满足条件的"组合"
    * 判断条件：组合中的元素是顺序"无关"的
* 排列问题(95%)
    * 问题模型：求出所有满足条件的"排列"
    * 判断条件：组合中的元素是顺序"相关"的

</details>

### 代码

{% iframe "https://holmesjj.github.io/code-training/#/code/dfs" "100%" "380" %}

<details>
<summary>显示答案</summary>

```java
// 返回类型根据题目要求决定
public void dfs(char[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    if (m == 0 || n == 0) {
        return;
    }

    // 8个方向
    int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}, {-1, -1}, {-1, 1}, {1, -1}, {1, 1}};

    // 已访问结点
    boolean[][] visited = new boolean[m][n];
    // 源点被访问
    visited[0][0] = true;

    // 栈记录结点的坐标(i, j)
    Stack<int[]> sn = new Stack<>();
    // 从源点开始
    sn.add(new int[]{0, 0});

    // DFS遍历
    while (!sn.isEmpty()) {
        // 从栈中拿出来第一个结点，但没有弹出
        int[] node = sn.peek();
        int i = node[0];
        int j = node[1];
        // 记录当前与结点的相邻的结点是否都已经被访问过
        boolean isAllVisited = true;
        // 遍历与当前结点相邻的结点
        // 8个方向的相邻结点
        for (int dir = 0; dir < dirs.length; dir++) {
            int ni = i + dirs[dir][0];
            int nj = j + dirs[dir][1];
            // 在grid中
            if (ni >= 0 && ni < m && nj >= 0 && nj < n /* && 是否相邻 */) {
                // 排除已经访问过的结点
                // 把符合条件的结点入队
                if (visited[ni][nj] == false /* && 其它条件 */) {
                    isAllVisited = false;
                    sn.add(new int[]{ni, nj});
                    // 结点被访问
                    visited[ni][nj] = true;
                    // 有这个break才是正确的DFS，但是没有这个break可以加快速度
                    break;
                }
            }
        }
        // 当前与结点的相邻的结点都已经被访问过，当前结点可以弹出
        if (isAllVisited) {
            sn.pop();
        }
    }
}
```

</details>

### 复杂度

<details>
<summary>显示答案</summary>

* 时间复杂度：O(E + V)
* 空间复杂度：O(V)

</details>

## 迪杰斯特拉(Dijkstra's)

### 使用条件

{% iframe "https://holmesjj.github.io/code-training/#/mrq/dijkstra" "100%" "270" %}

<details>
<summary>显示答案</summary>

* 最短路径问题(100%)
* 不存在负边权

</details>

### 代码

{% iframe "https://holmesjj.github.io/code-training/#/code/dijkstra" "100%" "380" %}

<details>
<summary>显示答案</summary>

```java
// 返回类型根据题目要求决定
public void dijkstra(char[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    if (m == 0 || n == 0) {
        return;
    }

    // 8个方向
    int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}, {-1, -1}, {-1, 1}, {1, -1}, {1, 1}};

    // 初始化结点到源点的最短距离为无穷大
    int[][] dists = new int[m][n];
    for (int i = 0; i < dists.length; i++) {
        Arrays.fill(dists[i], Integer.MAX_VALUE);
    }
    // Dijkstra找到最短距离
    // 源点到源点自己的距离为1
    dists[0][0] = 1;

    // 初始化结点的父结点
    int[][][] parents = new int[m][n][2];

    // 已访问结点
    boolean[][] visited = new boolean[m][n];
    // 源点被访问
    visited[0][0] = true;

    // 由于所有边权都是一样，不需要使用优先队列PriorityQueue
    // 队列记录结点的坐标(i, j)
    Queue<int[]> qn = new LinkedList<>();
    // 从源点开始
    qn.add(new int[]{0, 0});

    // BFS遍历
    while (!qn.isEmpty()) {
        // 获取当前优先队列中结点到源点距离最短的结点
        int[] node = qn.poll();
        int i = node[0];
        int j = node[1];
        // 从源点到当前结点的距离最小的结点被弹出时，才标记当前结点为被访问
        visited[i][j] = true;
        // 遍历与当前结点相邻的结点
        // 8个方向的相邻结点
        for (int dir = 0; dir < dirs.length; dir++) {
            int ni = i + dirs[dir][0];
            int nj = j + dirs[dir][1];
            // 在grid中
            if (ni >= 0 && ni < m && nj >= 0 && nj < n /* && 是否相邻 */) {
                // 排除已经访问过的结点
                // 更新结点到源点距离的最短距离 或 直接加入到优先队列
                // 更新父结点
                // 权重为1
                if (visited[ni][nj] == false && dists[ni][nj] > dists[i][j] + 1 /* && 其它条件 */) {
                    qn.add(new int[]{ni, nj});
                    parents[ni][nj] = new int[]{i, j};
                    // 更新最短距离
                    dists[ni][nj] = dists[i][j] + 1;
                }
            }
        }
    }

    // 栈记录父结点的坐标
    Stack<int[]> sn = new Stack<>();
    // 最后一个结点的坐标(m - 1, n - 1)
    sn.add(new int[]{m - 1, n - 1});
    // 最后一个结点的父结点坐标
    int[] parent = parents[m - 1][n - 1];
    sn.add(parent);
    while (parent[0] != 0 || parent[1] != 0) {
        parent = parents[parent[0]][parent[1]];
        sn.add(parent);
    }
    System.out.println("The shortest distance is " + dists[m - 1][n - 1]);
    while (!sn.isEmpty()) {
        int[] node = sn.pop();
        int i = node[0];
        int j = node[1];
        // 打印最后一个结点
        if (sn.isEmpty()) {
            System.out.print("(" + i + ", " + j + ")");
        } else {
            System.out.print("(" + i + ", " + j + "), ");
        }
    }
}
```

</details>

### 复杂度

<details>
<summary>显示答案</summary>

* 时间复杂度：O(E + VlogV)
* 空间复杂度：O(V)

</details>

## 回溯

### 使用条件

{% iframe "https://holmesjj.github.io/code-training/#/mrq/backtracking" "100%" "270" %}

<details>
<summary>显示答案</summary>

* 排列(100%)
* 组合(100%)
* 切割(100%)
* 子集(100%)
* 棋盘(100%)

</details>

### 代码

{% iframe "https://holmesjj.github.io/code-training/#/code/backtracking" "100%" "450" %}

<details>
<summary>显示答案</summary>

```java
// results是最终结果
List<List<Integer>> results = new ArrayList<>();
// result是其中一个结果
List<Integer> result = new ArrayList<>();
backtracking(results, result);

public void backtracking(List<List<Integer>> results, List<Integer> result /* 其它条件和参数 */) {
    if (/* 终止条件 */) {
        // 收集结果，需要创建一个新的对象
        results.add(new ArrayList<>(result));
        return;
    }
    // 遍历集合元素
    for (/* 集合元素 */) {
        // 处理result
        result.add();
        backtracking(results, result /* 其它条件和参数 */);
        // 回溯操作
        result.remove(result.size() - 1);
    }
}
```

</details>

### 复杂度

<details>
<summary>显示答案</summary>

* 时间复杂度：
    * Hamiltonian Cycle：O(n!)
    * WordBreak and StringSegment：O(2^n)
    * NQueens：O(n!)
* 空间复杂度：O(n)

</details>

## `List` 和 `Array`

{% iframe "https://holmesjj.github.io/code-training/#/code/listArray" "100%" "380" %}

<details>
<summary>显示答案</summary>

### `List<Integer>` to `int[]`
```java
int[] array = list.stream().mapToInt(i -> i).toArray();
// OR
int[] array = list.stream().mapToInt(Integer::intValue).toArray();
```

### 反转
```java
// List
Collections.reverse(list);
// Array
Collections.reverse(Arrays.asList(arr));
```

</details>

## `HashMap`

{% iframe "https://holmesjj.github.io/code-training/#/code/hashMap" "100%" "380" %}

<details>
<summary>显示答案</summary>

### 遍历
```java
for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
    int key = entry.getKey();
    int value = entry.getValue();
}
```

### 记录元素出现的次数
```java
for (int i = 0; i < nums.length; i++) {
    map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
}
```

</details>

## `HashMap` 和 `List`

{% iframe "https://holmesjj.github.io/code-training/#/code/hashMapList" "100%" "380" %}

<details>
<summary>显示答案</summary>

### `HashMap<String, String>` to `List<String>`
```java
List<String> list = new ArrayList<>(map.values());
```

</details>

## `Math`

{% iframe "https://holmesjj.github.io/code-training/#/code/math" "100%" "380" %}

<details>
<summary>显示答案</summary>

```java
Math.abs(x)     // 绝对值
Math.pow(x, y)  // x^y，返回double
Math.max(x, y)  // 返回较大值
Math.min(x, y)  // 返回较小值
```

</details>

## 其它

{% iframe "https://holmesjj.github.io/code-training/#/code/lengthOfN" "100%" "380" %}

<details>
<summary>显示答案</summary>

### n的长度
```java
int l = (int) (Math.log10(n) + 1);
```

</details>

### 多层循环break的使用

{% iframe "https://holmesjj.github.io/code-training/#/code/breakInNestedLoops" "100%" "380" %}

<details>
<summary>显示答案</summary>

```java
stop:
for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
        if (grid[i][j] == 1) {
            // 直接跳出两层循环
            break stop;
        }
    }
}
```

</details>

## 贪心算法
局部最优 => 全局最优

## ToDo
* `TreeMap` 和 `HashMap` 的遍历和排序
* 非递归Tree遍历
