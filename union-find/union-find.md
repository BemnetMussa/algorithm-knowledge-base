# Union-Find

## One-sentence definition
Union-Find is a data structure that efficiently tracks connected components by supporting fast `find` (component lookup) and `union` (merge components) operations.

## When to use it (use cases)
- Dynamic connectivity problems in undirected graphs.
- Cycle detection in undirected graphs.
- Counting connected components.
- Kruskal's Minimum Spanning Tree algorithm.
- Grouping elements by equivalence relations.

## Step-by-step explanation (plain language, no code first)
1. Start with each element as its own parent (each node is its own set).
2. `find(x)` returns the representative (root) of `x`'s set.
3. `union(a, b)` merges sets if their representatives differ.
4. Use **path compression** in `find` to flatten trees.
5. Use **union by rank/size** to keep trees shallow.

## Visual intuition (ASCII)

### Before unions
```text
0   1   2   3   4
```

### After union(0,1), union(1,2), union(3,4)
```text
0 - 1 - 2      3 - 4
```

### Parent array idea
```text
parent[i] = representative parent of i
find(i) follows parent links until root
```

## Templates (Python)
### 1) Union-Find with path compression + union by rank
Use when: default Union-Find template for most problems.
Input: number of nodes `n`, `union(a,b)`, `find(x)`.
Output: merged sets + connectivity queries.
```python
class UnionFind:
    def __init__(self, size):
        self.root = [i for i in range(size)]
        self.rank = [0] * size
        
    def find(self, x):
        if x == self.root[x]:
            return x
        self.root[x] = self.find(self.root[x])
        return self.root[x]
        
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.root[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.root[rootX] = rootY
            else:
                self.root[rootY] = rootX
                self.rank[rootX] += 1
```

### 2) Union-Find with union by size
Use when: you want quick access to component sizes.
Input: number of nodes `n`.
Output: supports `size_of(x)` queries.
```python
class UnionFind:
    def __init__(self, size):
        self.root = [i for i in range(size)]
        self.size = [1] * size
        
    def find(self, x):
        if x == self.root[x]:
            return x
        self.root[x] = self.find(self.root[x])
        return self.root[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.size[rootX] > self.size[rootY]:
                self.root[rootY] = rootX
                self.size[rootX] += self.size[rootY]
            else:
                self.root[rootX] = rootY
                self.size[rootY] += self.size[rootX]
```

### 3) Count connected components from edges
Use when: you are given `n` and edge list, need number of components.
Input: `n`, list of undirected edges.
Output: integer component count.
```python
def count_components(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        uf.union(u, v)
    
    # Count unique roots
    roots = set()
    for i in range(n):
        roots.add(uf.find(i))
    return len(roots)
```

### 4) Detect cycle in an undirected graph
Use when: detect if any edge connects already-connected vertices.
Input: `n`, list of undirected edges.
Output: boolean (`True` if cycle exists).
```python
def has_cycle_undirected(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        if uf.find(u) == uf.find(v):
            return True
        uf.union(u, v)
    return False
```

### 5) Union-Find with dictionary (for large input ranges)
Use when: input values are very large (like 10^9) or sparse, where array would waste memory.
Input: coordinates or large numbers as nodes.
Output: same connectivity operations but memory-efficient.
```python
from collections import defaultdict

class UnionFind:
    def __init__(self):
        self.root = dict()
        self.size = defaultdict(lambda: 1)
        
    def find(self, x):
        if x not in self.root:
            self.root[x] = x
            return x
        while x != self.root[x]:
            self.root[x] = self.root[self.root[x]]
            x = self.root[x]
        return x

    def union(self, x, y):
        rootX, rootY = self.find(x), self.find(y)
        if rootX != rootY:
            if self.size[rootX] > self.size[rootY]:
                self.root[rootY] = rootX
                self.size[rootX] += self.size[rootY]
            else:
                self.root[rootX] = rootY
                self.size[rootY] += self.size[rootX]
```

## Time & space complexity (Big O)
- Time: amortized near `O(1)` per operation (formally `O(alpha(n))`)
- Space: `O(n)`

## Practice questions (concept check)
1. Why do we need both path compression and union by rank/size?
2. Why does `union(a, b)` returning `False` help with cycle detection?
3. Why is DSU best for undirected connectivity but not direct shortest-path problems?

<details>
<summary>Answers</summary>

1. Both heuristics keep trees shallow, making operations very fast in practice.
2. If representatives are already equal, adding that edge closes a cycle.
3. DSU tracks component membership, not path length or path structure.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Check if There is a Valid Path in a Grid](https://leetcode.com/problems/check-if-there-is-a-valid-path-in-a-grid)
2. [Redundant Connection](https://leetcode.com/problems/redundant-connection)
3. [Regions Cut by Slashes](https://leetcode.com/problems/regions-cut-by-slashes)
4. [Accounts Merge](https://leetcode.com/problems/accounts-merge)
5. [Most Stones Removed with same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column)
6. [Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations)
7. [Checking Existence of Edge Length Limited Paths](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths)
8. [Remove Max Number of Edges to Keep Graph Fully Traversable](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable)
9. [Minimize Malware Spread](https://leetcode.com/problems/minimize-malware-spread)

## One thing that was confusing to me
At first, I thought DSU could answer path queries, but I learned it only answers whether nodes belong to the same component (and can track component metadata like size).

## See also
- [Depth-First Search (DFS)](../graph/dfs.md)
- [Heap](../heap/heap.md)
