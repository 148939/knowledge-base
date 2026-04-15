# 检索索引：本文档收录【数据结构与算法】【核心基础】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

数据结构是计算机存储、组织数据的方式，指相互之间存在一种或多种特定关系的数据元素的集合。算法是解决特定问题的步骤描述，是指令的有限序列。数据结构与算法是计算机科学的基础，是编写高效程序的核心。

## 2. 核心知识点（结构化层级）

### 2.1 线性数据结构

- 数组（Array）：连续内存存储，随机访问O(1)，插入删除O(n)
- 链表（Linked List）：非连续内存存储，访问O(n)，插入删除O(1)
- 栈（Stack）：后进先出（LIFO）
- 队列（Queue）：先进先出（FIFO）
- 哈希表（Hash Table）：键值对存储，平均O(1)访问

### 2.2 树形数据结构

- 二叉树（Binary Tree）：每个节点最多两个子节点
- 二叉搜索树（BST）：左子树<根<右子树
- 平衡树（AVL、红黑树）：自平衡二叉搜索树
- 堆（Heap）：完全二叉树，用于优先队列
- 前缀树（Trie）：字符串检索

### 2.3 图形数据结构

- 图的表示：邻接矩阵、邻接表
- 图的遍历：深度优先搜索（DFS）、广度优先搜索（BFS）
- 最短路径：Dijkstra、Floyd、Bellman-Ford
- 最小生成树：Prim、Kruskal

### 2.4 排序算法

- 冒泡排序、选择排序、插入排序：O(n²)
- 快速排序、归并排序、堆排序：O(n log n)
- 计数排序、桶排序、基数排序：线性时间

### 2.5 搜索算法

- 线性搜索：O(n)
- 二分搜索：O(log n)
- 哈希搜索：平均O(1)

## 3. 官方标准语法 / API

```python
# 数组操作
arr = [1, 2, 3, 4, 5]
print(arr[0])  # 访问第一个元素
arr.append(6)  # 添加元素
arr.insert(2, 10)  # 插入元素

# 链表节点定义
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# 栈操作
stack = []
stack.append(1)  # 入栈
stack.pop()  # 出栈

# 队列操作
from collections import deque
queue = deque()
queue.append(1)  # 入队
queue.popleft()  # 出队

# 二叉树节点定义
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# 二分搜索
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

## 4. 官方示例代码（可运行）

```python
# 示例1: 链表实现与操作
class LinkedList:
    def __init__(self):
        self.head = None
    
    def append(self, val):
        new_node = ListNode(val)
        if not self.head:
            self.head = new_node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node
    
    def delete(self, val):
        if not self.head:
            return
        if self.head.val == val:
            self.head = self.head.next
            return
        current = self.head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
                return
            current = current.next
    
    def print_list(self):
        current = self.head
        while current:
            print(current.val, end=" -> ")
            current = current.next
        print("None")

# 示例2: 二叉树遍历
class BinaryTree:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        if not self.root:
            self.root = TreeNode(val)
            return
        self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        if val < node.val:
            if not node.left:
                node.left = TreeNode(val)
            else:
                self._insert_recursive(node.left, val)
        else:
            if not node.right:
                node.right = TreeNode(val)
            else:
                self._insert_recursive(node.right, val)
    
    # 前序遍历：根-左-右
    def preorder_traversal(self, node):
        if node:
            print(node.val, end=" ")
            self.preorder_traversal(node.left)
            self.preorder_traversal(node.right)
    
    # 中序遍历：左-根-右
    def inorder_traversal(self, node):
        if node:
            self.inorder_traversal(node.left)
            print(node.val, end=" ")
            self.inorder_traversal(node.right)
    
    # 后序遍历：左-右-根
    def postorder_traversal(self, node):
        if node:
            self.postorder_traversal(node.left)
            self.postorder_traversal(node.right)
            print(node.val, end=" ")
    
    # 层序遍历：BFS
    def level_order_traversal(self):
        if not self.root:
            return
        queue = [self.root]
        while queue:
            node = queue.pop(0)
            print(node.val, end=" ")
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

# 示例3: 快速排序
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)

# 示例4: 归并排序
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# 示例5: 图的DFS和BFS
class Graph:
    def __init__(self):
        self.graph = {}
    
    def add_edge(self, u, v):
        if u not in self.graph:
            self.graph[u] = []
        self.graph[u].append(v)
    
    # DFS
    def dfs(self, start, visited=None):
        if visited is None:
            visited = set()
        visited.add(start)
        print(start, end=" ")
        if start in self.graph:
            for neighbor in self.graph[start]:
                if neighbor not in visited:
                    self.dfs(neighbor, visited)
    
    # BFS
    def bfs(self, start):
        visited = set()
        queue = [start]
        visited.add(start)
        while queue:
            vertex = queue.pop(0)
            print(vertex, end=" ")
            if vertex in self.graph:
                for neighbor in self.graph[vertex]:
                    if neighbor not in visited:
                        visited.add(neighbor)
                        queue.append(neighbor)

# 主程序
if __name__ == "__main__":
    print("=== 链表操作 ===")
    ll = LinkedList()
    ll.append(1)
    ll.append(2)
    ll.append(3)
    ll.print_list()
    ll.delete(2)
    ll.print_list()
    
    print("\n=== 二叉树操作 ===")
    bt = BinaryTree()
    for val in [5, 3, 7, 2, 4, 6, 8]:
        bt.insert(val)
    print("前序遍历:", end=" ")
    bt.preorder_traversal(bt.root)
    print("\n中序遍历:", end=" ")
    bt.inorder_traversal(bt.root)
    print("\n后序遍历:", end=" ")
    bt.postorder_traversal(bt.root)
    print("\n层序遍历:", end=" ")
    bt.level_order_traversal()
    
    print("\n\n=== 排序算法 ===")
    arr = [64, 34, 25, 12, 22, 11, 90]
    print(f"原始数组: {arr}")
    print(f"快速排序: {quick_sort(arr.copy())}")
    print(f"归并排序: {merge_sort(arr.copy())}")
    
    print("\n=== 图的遍历 ===")
    g = Graph()
    g.add_edge(0, 1)
    g.add_edge(0, 2)
    g.add_edge(1, 2)
    g.add_edge(2, 0)
    g.add_edge(2, 3)
    g.add_edge(3, 3)
    print("DFS从顶点2开始:", end=" ")
    g.dfs(2)
    print("\nBFS从顶点2开始:", end=" ")
    g.bfs(2)
```

## 5. 官方注意事项 / 最佳实践

1. **时间复杂度分析**
   - 理解大O记号，分析算法的时间和空间复杂度
   - 优先选择时间复杂度低的算法
   - 考虑数据规模，选择合适的数据结构

2. **空间优化**
   - 注意递归深度，避免栈溢出
   - 优先使用迭代替代递归
   - 合理使用缓存和哈希表

3. **代码质量**
   - 编写清晰、可读的代码
   - 添加必要的注释
   - 处理边界条件

4. **常见错误**
   - 数组越界
   - 空指针异常
   - 递归终止条件错误
   - 哈希冲突处理不当

## 6. 官方文档参考链接

- 《算法导论》：https://mitpress.mit.edu/books/introduction-algorithms-third-edition
- LeetCode：https://leetcode.cn/
- 可视化算法：https://visualgo.net/zh
- 算法可视化：https://www.cs.usfca.edu/~galles/visualization/
- Python数据结构与算法：https://runestone.academy/runestone/books/published/pythonds/index.html
