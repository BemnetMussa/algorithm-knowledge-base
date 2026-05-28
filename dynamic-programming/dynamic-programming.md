# Dynamic Programming (DP)

## One-sentence definition
Dynamic Programming solves problems by breaking them into overlapping subproblems and reusing solutions (memoization or tabulation).

## Why it's foundational
- Many advanced algorithms (graph shortest paths, knapsack variants, string algorithms, sequence problems) rely on DP patterns.
- Teaches decomposition, state design, and complexity trade-offs used across algorithmic problem solving.

## When to use it (use cases)
- Optimization problems (maximize/minimize with constraints).
- Counting / combinatorics with overlapping subproblems.
- Sequence and string problems (LCS, edit distance, LIS).
- Knapsack and resource allocation variants.

## Step-by-step approach (practical recipe)
1. Identify the subproblem and what parameters define a state.
2. Define the DP state meaning precisely (e.g., `dp[i]` = best answer for prefix ending at `i`).
3. Derive the recurrence relation from smaller states.
4. Choose memoization (top-down) or tabulation (bottom-up).
5. Decide iteration order if using tabulation so dependencies are computed first.
6. Initialize base cases correctly.
7. (Optional) Optimize space by keeping only required previous states.

## Common DP patterns
- 1D DP (prefix/suffix): Fibonacci, climbing stairs, maximum subarray variants.
- 2D DP (grid / string): edit distance, LCS, unique paths.
- DP on trees: post-order traversals combining child results.
- DP with bitmasking: Held–Karp for TSP-like state (small N up to ~20).
- Knapsack variations: 0/1, unbounded, bounded – use weight or value as state.
- LIS / patience sorting: O(n log n) optimization for increasing subsequences.

## Templates (Python)

### 1) Top-down memoization (recursive)
```python
from functools import lru_cache

def fib(n):
    @lru_cache(None)
    def dfs(k):
        if k <= 1:
            return k
        return dfs(k-1) + dfs(k-2)
    return dfs(n)
```

When to use: clear recursion and easy state definition; avoids manual table management.

### 2) Bottom-up tabulation (iterative)
```python
def fib_bottom(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[0], dp[1] = 0, 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

When to use: better control of iteration order and often faster due to iterative loops.

### 3) 0/1 Knapsack (classic DP)
```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    for i in range(1, n+1):
        w, v = weights[i-1], values[i-1]
        for cap in range(capacity + 1):
            dp[i][cap] = dp[i-1][cap]
            if cap >= w:
                dp[i][cap] = max(dp[i][cap], dp[i-1][cap-w] + v)
    return dp[n][capacity]
```

Space-optimized version keeps a single 1D array iterating capacity backwards.

### 4) Longest Increasing Subsequence (O(n log n) hint)
Use patience sorting / binary-search method for O(n log n). But DP `O(n^2)` is:
```python
def lis_n2(arr):
    n = len(arr)
    dp = [1] * n
    for i in range(n):
        for j in range(i):
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp) if dp else 0
```

## Tips & pitfalls
- Spend time designing state; many solutions fail due to wrong state meaning.
- Prefer memoization while prototyping; tabulation often faster in final code.
- Watch base cases and off-by-one errors on indices.
- When optimizing, consider whether values or weights should be the DP dimension.
- For large constraints, check if greedy, mathematical formulas, or monotonic optimizations apply.

## Time & space complexity
- Depends on number of states × transitions per state. Always reason about state count.
- Example: 0/1 knapsack `O(n * capacity)` time and `O(capacity)` space with optimization.

## Example problems (practice)
1. Climbing Stairs / Fibonacci variants
2. 0/1 Knapsack
3. Longest Increasing Subsequence (LIS)
4. Longest Common Subsequence (LCS)
5. Edit Distance (Levenshtein)
6. Partition Equal Subset Sum
7. Coin Change (counting / min coins)

## Small worked example — Edit Distance (bottom-up)
```python
def edit_distance(a, b):
    m, n = len(a), len(b)
    dp = [[0] * (n+1) for _ in range(m+1)]
    for i in range(m+1):
        dp[i][0] = i
    for j in range(n+1):
        dp[0][j] = j
    for i in range(1, m+1):
        for j in range(1, n+1):
            if a[i-1] == b[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    return dp[m][n]
```

## Further reading
- CLRS / Intro to Algorithms — dynamic programming chapters
- Competitive programming resources: CP-algorithms DP section

## See also
- [Recursion](../recursion/recursion.md)
- [Searching / Binary Search](../searching/binary-search.md)

---
*Contributed: concise DP reference for the knowledge base.*
