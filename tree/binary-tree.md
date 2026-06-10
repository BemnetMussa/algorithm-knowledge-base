# Binary Tree (Basics & Traversal)

## One-sentence definition
A binary tree is a hierarchy of nodes where each node has at most two children (left and right), and "traversal" means visiting every node in a defined order.

## When to use it (use cases)
- Modeling hierarchy (file systems, org charts, the DOM, expression trees).
- Fast ordered operations when it is a Binary Search Tree (search/insert/delete).
- Any problem phrased as "for each node, combine results from its subtrees" (height, sum, diameter, lowest common ancestor).
- Level-by-level processing (BFS / level-order) such as "right side view" or "minimum depth."
- A stepping stone to heaps, tries, segment trees, and BST.

## Step-by-step explanation (plain language, no code first)
1. A tree is made of **nodes**. Each node holds a value and pointers to up to two children: `left` and `right`.
2. The top node is the **root**. A node with no children is a **leaf**.
3. There is exactly one path from the root to any node — no cycles.
4. To **traverse** is to visit every node once. Depth-first traversals differ only by *when* you visit the current node relative to its children:
   - **Pre-order**: node → left → right (visit before descending).
   - **In-order**: left → node → right (for a BST this yields sorted order).
   - **Post-order**: left → right → node (visit after both subtrees — good for deleting/freeing or combining child results).
5. **Level-order** (BFS) visits the tree top-to-bottom, one level at a time, using a queue instead of recursion.
6. Most tree problems are solved by recursion: solve it for the children, then combine.

## Visual intuition (ASCII)

### A sample tree and its traversals
```text
          1
        /   \
       2     3
      / \     \
     4   5     6

Pre-order  (node,left,right): 1 2 4 5 3 6
In-order   (left,node,right): 4 2 5 1 3 6
Post-order (left,right,node): 4 5 2 6 3 1
Level-order (BFS):            1 2 3 4 5 6
```

## Templates (Python)

### 0) Node definition
Use when: you need a binary tree node.
Input: a value, optional left/right children.
Output: a node object.
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### 1) Recursive DFS traversals
Use when: you need pre / in / post-order and recursion depth is safe.
Input: `root` — a TreeNode (or None).
Output: list of values in the chosen order.
```python
def preorder(root):
    if not root:
        return []
    return [root.val] + preorder(root.left) + preorder(root.right)

def inorder(root):
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

def postorder(root):
    if not root:
        return []
    return postorder(root.left) + postorder(root.right) + [root.val]
```

### 2) Iterative in-order (explicit stack)
Use when: recursion depth might overflow, or an interviewer asks for the iterative version.
Input: `root` — a TreeNode.
Output: list of values in in-order.
```python
def inorder_iterative(root):
    res, stack = [], []
    node = root
    while node or stack:
        while node:          # go as far left as possible
            stack.append(node)
            node = node.left
        node = stack.pop()   # leftmost unvisited node
        res.append(node.val)
        node = node.right    # then explore the right subtree
    return res
```

### 3) Level-order traversal (BFS with a queue)
Use when: you need nodes grouped by depth.
Input: `root` — a TreeNode.
Output: list of levels, each a list of values.
```python
from collections import deque

def level_order(root):
    if not root:
        return []
    res, q = [], deque([root])
    while q:
        level = []
        for _ in range(len(q)):     # process exactly one level
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(level)
    return res
```

### 4) Common recursive pattern — max depth (height)
Use when: any "combine results from subtrees" problem.
Input: `root` — a TreeNode.
Output: integer height (number of nodes on the longest root-to-leaf path).
```python
def max_depth(root):
    if not root:
        return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

## Time & space complexity (Big O)
- Every traversal visits each node once: `O(n)` time.
- Recursive DFS space: `O(h)` for the call stack, where `h` is the height (`O(log n)` if balanced, `O(n)` if skewed).
- Iterative DFS space: `O(h)` for the explicit stack.
- Level-order space: `O(w)` where `w` is the widest level (up to `O(n)`).

## Practice questions (concept check)
1. For which kind of tree does in-order traversal produce sorted output, and why?
2. Why does level-order use a queue while DFS uses a stack (or recursion)?
3. What is the worst-case recursion depth for a binary tree with `n` nodes, and when does it happen?

<details>
<summary>Answers</summary>

1. A Binary Search Tree. In-order visits left → node → right, and a BST keeps smaller values on the left and larger on the right, so values come out ascending.
2. Level-order must finish a whole level before the next, which is FIFO — a queue. DFS dives deep first and backtracks, which is LIFO — a stack (recursion uses the implicit call stack).
3. `O(n)`, when the tree is completely skewed (each node has only one child), forming a line.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)
2. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)
3. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree)
4. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree)
5. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree)

## One thing that was confusing to me
I kept mixing up pre/in/post-order. What finally clicked: the prefix only tells you *when the current node is visited* relative to its children — "pre" = before both children, "in" = between them, "post" = after both. The left-before-right order never changes.

## See also
- [Recursion](../recursion/recursion.md) – every DFS traversal is a recursion exercise.
- [Breadth First Search](../graph/bfs.md) – level-order is BFS applied to a tree.
- [Depth First Search](../graph/dfs.md) – pre/in/post-order are DFS applied to a tree.
- [Heap](../heap/heap.md) – a complete binary tree specialized for min/max access.
