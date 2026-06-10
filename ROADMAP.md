# DSA Learning Roadmap

A structured, beginner-to-advanced study path based on industry-standard roadmaps (NeetCode, roadmap.sh, Scaler).
Topics are ordered so every concept builds on the one before it.
Notes marked **✅ In this repo** have a file you can read right now. Everything else is a topic to contribute or study externally.

---

## How to use this roadmap

1. Work through stages in order — do not skip ahead.
2. For each topic: read the note (if available), understand the pattern, then solve the linked practice problems.
3. Aim for at least 2–3 problems per topic before moving on.
4. Come back and revisit earlier stages after finishing later ones — patterns repeat.

---

## Stage 1 — Foundations
> Goal: get comfortable with basic data structures before touching any algorithm.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 1 | Pick a language (Python / Java / C++) | — | Commit to one. Python recommended for beginners. |
| 2 | Arrays & Strings basics | — | Indexing, slicing, iteration, built-in methods. |
| 3 | Big O Notation | — | Learn to analyse time and space before writing any solution. |
| 4 | Recursion | ✅ [recursion/recursion.md](./recursion/recursion.md) | Master this before trees, graphs, or backtracking. |
| 5 | Linked List (Singly) | ✅ [linked-list/singly-linked-list.md](./linked-list/singly-linked-list.md) | Pointer manipulation is tested everywhere. |
| 6 | Linked List (Doubly) | ✅ [linked-list/doubly-linked-list.md](./linked-list/doubly-linked-list.md) | Needed for LRU cache, deque problems. |
| 7 | Stack | ✅ [stack/stack.md](./stack/stack.md) | LIFO. Used in expression parsing, undo/redo, monotonic problems. |
| 8 | Queue & Deque | ✅ [queue/queue.md](./queue/queue.md) | FIFO. Foundation for BFS. |
| 9 | Hash Map & Hash Set | ✅ [hash-map/hash-map.md](./hash-map/hash-map.md) | Most frequent interview data structure. O(1) lookup. |

---

## Stage 2 — Core Patterns
> Goal: learn the universal problem-solving patterns that cover ~60% of interview questions.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 10 | Two Pointers | ✅ [array/two-pointers.md](./array/two-pointers.md) | Opposing, fast/slow, same-direction. |
| 11 | Sliding Window | ✅ [sliding-window/sliding-window.md](./sliding-window/sliding-window.md) | Fixed and variable size windows. |
| 12 | Prefix Sum | ✅ [prefix-sum/prefix-sum.md](./prefix-sum/prefix-sum.md) | Range queries, subarray sum counting. |
| 13 | Binary Search | ✅ [searching/binary-search.md](./searching/binary-search.md) | Learn the template cold. Apply to non-obvious problems. |
| 14 | Bitwise Operations | ✅ [Bitwise/bitwise.md](./Bitwise/bitwise.md) | AND, OR, XOR, shifts, bitmask tricks. |

---

## Stage 3 — Sorting & Searching
> Goal: understand how data gets ordered and how search is optimised.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 15 | Bubble Sort | — | Simple but slow. Good for understanding stability. |
| 16 | Insertion Sort | — | Fast on nearly-sorted data. |
| 17 | Selection Sort | — | Simple in-place sort. |
| 18 | Merge Sort | ✅ [sorting/merge-sort.md](./sorting/merge-sort.md) | Stable, guaranteed O(n log n). |
| 19 | Quick Sort | ✅ [sorting/quick-sort.md](./sorting/quick-sort.md) | Fast in practice. Understand pivot strategies. |
| 20 | Counting / Radix Sort | — | O(n) sorts for restricted input ranges. |
| 21 | Jump Search | ✅ [searching/jump-search.md](./searching/jump-search.md) | Good for sorted arrays without random access. |
| 22 | Exponential Search | ✅ [searching/exponential-search.md](./searching/exponential-search.md) | For unbounded or very large sorted inputs. |

---

## Stage 4 — Trees
> Goal: trees appear in ~30% of interviews. Understand traversal inside out.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 23 | Binary Tree basics | ✅ [tree/binary-tree.md](./tree/binary-tree.md) | Node structure, height, depth, leaf. |
| 24 | Tree Traversal (Pre / In / Post-order) | ✅ [tree/binary-tree.md](./tree/binary-tree.md) | Recursive and iterative versions. |
| 25 | Level-order Traversal (BFS on trees) | ✅ [tree/binary-tree.md](./tree/binary-tree.md) | Foundation for many tree problems. |
| 26 | Binary Search Tree (BST) | — | Insert, delete, search. Inorder = sorted. |
| 27 | Heap / Priority Queue | ✅ [heap/heap.md](./heap/heap.md) | Min-heap, max-heap, top-k problems. |
| 28 | Trie (Prefix Tree) | ✅ [trie/trie.md](./trie/trie.md) | Autocomplete, word search, prefix matching. |
| 29 | AVL Tree (self-balancing BST) | — | Concept-level understanding sufficient for most interviews. |
| 30 | Segment Tree | — | Range queries with updates. Advanced. |
| 31 | Fenwick Tree (Binary Indexed Tree) | — | Efficient prefix sums with point updates. Advanced. |

---

## Stage 5 — Graphs
> Goal: graphs are the hardest and most rewarding stage. Build up from traversal to advanced algorithms.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 32 | Graph representations | — | Adjacency list vs matrix. Directed vs undirected. |
| 33 | Depth First Search (DFS) | ✅ [graph/dfs.md](./graph/dfs.md) | Recursive and iterative. Cycle detection. |
| 34 | Breadth First Search (BFS) | ✅ [graph/bfs.md](./graph/bfs.md) | Shortest path on unweighted graphs. |
| 35 | Topological Sort | ✅ [graph/topological-sort.md](./graph/topological-sort.md) | Kahn's (BFS) and DFS postorder. |
| 36 | Union-Find (Disjoint Set) | ✅ [union-find/union-find.md](./union-find/union-find.md) | Connected components, cycle detection. |
| 37 | Dijkstra's Algorithm | ✅ [graph/dijkstra.md](./graph/dijkstra.md) | Shortest path on weighted graphs (non-negative). |
| 38 | Bellman-Ford Algorithm | — | Shortest path with negative edges. |
| 39 | Floyd-Warshall Algorithm | — | All-pairs shortest paths. |
| 40 | Kruskal's Algorithm (MST) | ✅ [graph/kruskal.md](./graph/kruskal.md) | Minimum spanning tree using Union-Find. |
| 41 | Prim's Algorithm (MST) | — | Minimum spanning tree using a priority queue. |

---

## Stage 6 — Recursion Patterns
> Goal: learn the structured recursive patterns that cover backtracking, divide-and-conquer, and DP.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 42 | Backtracking | ✅ [backtracking/backtracking.md](./backtracking/backtracking.md) | Subsets, permutations, combinations, N-Queens. |
| 43 | Divide and Conquer | — | Merge sort, quick sort, binary search are all examples. |
| 44 | Greedy Algorithms | — | Activity selection, interval scheduling, Huffman coding. |

---

## Stage 7 — Dynamic Programming
> Goal: the most feared stage. Work through patterns in order — do not try to memorise solutions.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 45 | DP fundamentals (memo vs tabulation) | ✅ [dynamic-programming/dynamic-programming.md](./dynamic-programming/dynamic-programming.md) | Start here. Understand state design. |
| 46 | 1D DP — Fibonacci / Climbing Stairs | — | Simplest DP problems. |
| 47 | 1D DP — House Robber pattern | — | Max sum with skip constraint. |
| 48 | 2D DP — Unique Paths / Grid DP | — | DP on a grid. |
| 49 | 0/1 Knapsack | — | Item selection with capacity constraint. |
| 50 | Unbounded Knapsack / Coin Change | — | Items can be reused. |
| 51 | Longest Common Subsequence (LCS) | — | String DP template. |
| 52 | Longest Increasing Subsequence (LIS) | — | O(n²) DP and O(n log n) binary search. |
| 53 | Edit Distance | — | Classic string transformation DP. |
| 54 | Partition / Subset Sum | — | DP on boolean states. |
| 55 | Bitmask DP | — | Encode state as an integer bitmask. Small N ≤ 20. |
| 56 | DP on Trees | — | Post-order traversal combining child results. |
| 57 | DP on Intervals | — | Matrix chain, burst balloons, stone merge. |

---

## Stage 8 — Advanced Topics
> Goal: for competitive programming, senior interviews, or going beyond LC Medium.

| # | Topic | In this repo | Notes |
|---|-------|-------------|-------|
| 58 | Monotonic Stack / Queue | — | Next greater element, largest rectangle, sliding window max. |
| 59 | Interval problems | — | Merge intervals, meeting rooms, sweep line. |
| 60 | String algorithms (KMP, Rabin-Karp) | — | Pattern matching in O(n). |
| 61 | Suffix Arrays & Suffix Trees | — | Advanced string problems. |
| 62 | Skip List | — | Probabilistic alternative to balanced BST. |
| 63 | B-Trees | — | Disk-based storage, database indexes. |
| 64 | Network Flow (Ford-Fulkerson) | — | Max flow, min cut problems. |
| 65 | Geometry (Convex Hull, line intersection) | — | Rare but appears in some company interviews. |

---

## Recommended study schedule

| Week | Focus |
|------|-------|
| 1–2 | Stage 1 — Foundations (pick language, arrays, recursion, linked list, stack, queue, hash map) |
| 3–4 | Stage 2 — Core Patterns (two pointers, sliding window, prefix sum, binary search, bitwise) |
| 5–6 | Stage 3 — Sorting & Searching |
| 7–9 | Stage 4 — Trees (binary trees, BST, heap, trie) |
| 10–13 | Stage 5 — Graphs (DFS, BFS, topo sort, union-find, Dijkstra, MST) |
| 14–15 | Stage 6 — Recursion Patterns (backtracking, divide and conquer, greedy) |
| 16–20 | Stage 7 — Dynamic Programming (work through sub-topics in order) |
| 21+ | Stage 8 — Advanced Topics (as needed for target companies) |

---

## Practice platforms

| Platform | Best for |
|----------|---------|
| [LeetCode](https://leetcode.com) | Company-specific prep, most widely used |
| [NeetCode](https://neetcode.io/roadmap) | Curated problem lists with video explanations |
| [roadmap.sh DSA](https://roadmap.sh/datastructures-and-algorithms) | Visual topic map |
| [Codeforces](https://codeforces.com) | Competitive programming and harder problems |
| [AlgoMap](https://algomap.io/roadmap) | Free structured roadmap with problems |

---

## Contribution guide

If a topic above says **—** in the "In this repo" column, that is an open contribution opportunity.
Follow the template in [README.md](./README.md) and claim the topic before you start.
