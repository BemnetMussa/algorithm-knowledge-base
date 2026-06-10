# Binary Search Tree (BST)

## One-sentence definition
A Binary Search Tree is a binary tree where every node's left subtree holds only smaller values and its right subtree only larger values, giving ordered `O(h)` search, insert, and delete.

## When to use it (use cases)
- Keeping data sorted while still allowing fast inserts and deletes.
- Looking up, inserting, or removing a key in `O(log n)` when balanced.
- Range queries ("all values between a and b") and finding the floor/ceiling of a value.
- Getting sorted output for free via in-order traversal.
- Order statistics (kth smallest/largest) and predecessor/successor queries.

## Step-by-step explanation (plain language, no code first)
1. Every node obeys the **BST property**: everything in its left subtree is smaller, everything in its right subtree is larger.
2. To **search**, start at the root and compare. If the target is smaller, go left; if larger, go right; if equal, you found it. Each step throws away half the tree.
3. To **insert**, search for the value as if looking it up; when you fall off the tree (hit an empty spot), that is where the new node goes.
4. To **delete** a node, there are three cases:
   - No children: just remove it.
   - One child: replace the node with that child.
   - Two children: replace its value with its **in-order successor** (smallest value in the right subtree), then delete that successor.
5. An **in-order traversal** of a BST always visits values in sorted ascending order — this is the easiest way to validate a BST.
6. Speed depends on **height** `h`. A balanced BST has `h ≈ log n`; a degenerate (sorted-insert) BST becomes a line with `h = n`, which is why self-balancing trees (AVL, Red-Black) exist.

## Visual intuition (ASCII)

### BST property
```text
            8
          /   \
         3     10
        / \      \
       1   6      14
          / \    /
         4   7  13

Left subtree of 8 = {1,3,4,6,7}  (all < 8)
Right subtree of 8 = {10,13,14}  (all > 8)
In-order traversal: 1 3 4 6 7 8 10 13 14   (sorted!)
```

### Balanced vs degenerate
```text
balanced (h ~ log n)        degenerate (h = n) from inserting 1,2,3,4
        2                    1
       / \                    \
      1   3                    2
           \                    \
            4                    3
                                  \
                                   4
```

## Templates (Python)
Uses the `TreeNode` from [binary-tree.md](./binary-tree.md) (`val`, `left`, `right`).

### 1) Search
Use when: check whether a key exists / return its node.
Input: `root` — BST root; `target` — value to find.
Output: the node holding `target`, or `None`.
```python
def search_bst(root, target):
    node = root
    while node:
        if target == node.val:
            return node
        node = node.left if target < node.val else node.right
    return None
```

### 2) Insert
Use when: add a value while keeping the BST property.
Input: `root` — BST root (may be None); `val` — value to insert.
Output: the (possibly new) root.
```python
def insert_bst(root, val):
    if not root:
        return TreeNode(val)
    if val < root.val:
        root.left = insert_bst(root.left, val)
    elif val > root.val:
        root.right = insert_bst(root.right, val)
    # equal -> ignore duplicate (or handle as you prefer)
    return root
```

### 3) Delete
Use when: remove a value and keep the BST valid.
Input: `root` — BST root; `key` — value to delete.
Output: the new root.
```python
def delete_bst(root, key):
    if not root:
        return None
    if key < root.val:
        root.left = delete_bst(root.left, key)
    elif key > root.val:
        root.right = delete_bst(root.right, key)
    else:
        # Found it. Handle 0/1/2 children.
        if not root.left:
            return root.right
        if not root.right:
            return root.left
        # Two children: grab in-order successor (min of right subtree).
        succ = root.right
        while succ.left:
            succ = succ.left
        root.val = succ.val
        root.right = delete_bst(root.right, succ.val)
    return root
```

### 4) Validate a BST
Use when: confirm a tree obeys the BST property (common interview trap).
Input: `root` — tree root.
Output: `True` if it is a valid BST, else `False`.
```python
def is_valid_bst(root, low=float("-inf"), high=float("inf")):
    if not root:
        return True
    if not (low < root.val < high):
        return False
    # Each node must fall inside an allowed (low, high) range.
    return (is_valid_bst(root.left, low, root.val) and
            is_valid_bst(root.right, root.val, high))
```

## Time & space complexity (Big O)
- Search / Insert / Delete: `O(h)` — `O(log n)` if balanced, `O(n)` if degenerate.
- In-order traversal: `O(n)` time.
- Space: `O(h)` recursion stack — `O(log n)` balanced, `O(n)` worst case.
- Self-balancing variants (AVL, Red-Black) guarantee `O(log n)` for all operations.

## Practice questions (concept check)
1. Why does an in-order traversal of a BST come out sorted?
2. Why can BST operations degrade to `O(n)`, and how is that fixed?
3. When deleting a node with two children, why do we replace it with the in-order successor specifically?

<details>
<summary>Answers</summary>

1. In-order visits left → node → right, and the BST property keeps smaller values left and larger values right, so values are emitted in ascending order.
2. If keys are inserted in sorted order the tree becomes a straight line of height `n`, so each operation walks all nodes. Self-balancing trees (AVL, Red-Black) rotate to keep height `O(log n)`.
3. The in-order successor is the smallest value larger than the node, so putting it in the node's place keeps everything in the left subtree smaller and everything remaining in the right subtree larger — the BST property still holds.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree)
2. [Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree)
3. [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst)
4. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)
5. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst)

## One thing that was confusing to me
I first validated a BST by only checking each node against its direct children — and it passed trees that were actually invalid. The fix was realizing every node must fit a *range* inherited from all its ancestors, not just beat its immediate parent. That is why the validate template threads `low`/`high` bounds down the tree.

## See also
- [Binary Tree (Basics & Traversal)](./binary-tree.md) – the structure and traversals a BST builds on.
- [Binary Search](../searching/binary-search.md) – the same halve-the-search-space idea, on an array.
- [Heap](../heap/heap.md) – another ordered binary tree, but ordered by priority, not full sort.
- [Trie](../trie/trie.md) – a search tree specialized for string keys.
