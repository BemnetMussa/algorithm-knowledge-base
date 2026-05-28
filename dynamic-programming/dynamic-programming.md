# Dynamic Programming (DP)

## One-sentence definition
Dynamic Programming solves problems by breaking them into overlapping subproblems and reusing previously computed solutions instead of recomputing them (via memoization or tabulation).

## When to use it (use cases)
- Optimization problems (maximize/minimize with constraints).
- Counting or combinatorics problems with overlapping subproblems.
- Sequence and string problems (LCS, edit distance, LIS).
- Knapsack and resource allocation variants.

## Step-by-step explanation (plain language, no code first)
1. Identify that the problem has **overlapping subproblems** — the same smaller problem is solved more than once in a naive recursion.
2. Define the **DP state**: what parameters fully describe a subproblem? (e.g., `dp[i]` = best answer for prefix ending at index `i`).
3. Write the **recurrence relation**: how does `dp[i]` depend on smaller states?
4. Choose **top-down** (memoized recursion) or **bottom-up** (iterative table).
5. Set **base cases** before filling the table.
6. If using bottom-up, decide **iteration order** so dependencies are ready before they're needed.
7. (Optional) Reduce space by keeping only the states you actually need.

**Common patterns:**
- 1D DP (prefix/suffix): Fibonacci, climbing stairs, max subarray.
- 2D DP (grid/string): edit distance, LCS, unique paths.
- Knapsack: 0/1, unbounded, bounded.
- LIS: O(n²) DP or O(n log n) with binary search.
- Bitmask DP: encode subsets as integers (useful for small N ≤ 20).

## Templates (Python)

### 1) Top-down memoization (recursive + cache)
Use when: the recursion structure is clear and you want to avoid manual table management.
Input: `n` — integer index.
Output: nth Fibonacci number.
```python
from functools import lru_cache

def fib(n):
    @lru_cache(None)
    def dfs(k):
        if k <= 1:
            return k
        return dfs(k - 1) + dfs(k - 2)
    return dfs(n)
```

### 2) Bottom-up tabulation (iterative)
Use when: you want better control over iteration order and faster constant factors.
Input: `n` — integer index.
Output: nth Fibonacci number.
```python
def fib_bottom_up(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[0], dp[1] = 0, 1
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```

### 3) 0/1 Knapsack
Use when: each item can be taken at most once; maximize value within a weight capacity.
Input: `weights`, `values` — lists of equal length; `capacity` — max weight.
Output: maximum total value achievable.
```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        w, v = weights[i - 1], values[i - 1]
        for cap in range(capacity + 1):
            dp[i][cap] = dp[i - 1][cap]
            if cap >= w:
                dp[i][cap] = max(dp[i][cap], dp[i - 1][cap - w] + v)
    return dp[n][capacity]
```

### 4) Longest Increasing Subsequence (O(n²) DP)
Use when: finding the length of the longest strictly increasing subsequence.
Input: `arr` — list of integers.
Output: length of the longest increasing subsequence.
```python
def lis(arr):
    n = len(arr)
    dp = [1] * n
    for i in range(n):
        for j in range(i):
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp) if dp else 0
```

### 5) Edit Distance (bottom-up 2D DP)
Use when: finding the minimum number of insertions, deletions, and substitutions to transform one string into another.
Input: `a`, `b` — two strings.
Output: minimum edit distance (integer).
```python
def edit_distance(a, b):
    m, n = len(a), len(b)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[m][n]
```

## Time & space complexity (Big O)
- Complexity = **number of states × work per state**.
- Top-down and bottom-up have the same asymptotic complexity.
- 0/1 Knapsack: `O(n × capacity)` time, `O(capacity)` space with 1D optimization.
- Edit Distance: `O(m × n)` time, `O(n)` space with row-only optimization.
- LIS: `O(n²)` with DP; `O(n log n)` with binary search (patience sorting).

## Practice questions (concept check)
1. What is the difference between top-down memoization and bottom-up tabulation?
2. How do you know a problem has "overlapping subproblems"?
3. In the 0/1 knapsack, why do we iterate capacity backwards when using a 1D array?

<details>
<summary>Answers</summary>

1. Top-down starts from the original problem and recurses, caching results as it goes. Bottom-up starts from base cases and builds up to the answer iteratively. Both produce the same result; top-down is often easier to write first, bottom-up is usually faster in practice.
2. Draw the recursion tree. If you see the same `(function, arguments)` call appearing more than once at different branches, the subproblems overlap.
3. Iterating backwards prevents using the same item twice. If you went left to right, `dp[cap - w]` would already reflect the current item being added, which would allow it to be selected multiple times (turning it into unbounded knapsack).

</details>

## Practice questions (LeetCode/Codeforces)
1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) – classic 1D DP warm-up.
2. [Coin Change](https://leetcode.com/problems/coin-change/) – unbounded knapsack variant.
3. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
4. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
5. [Edit Distance](https://leetcode.com/problems/edit-distance/)
6. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) – 0/1 knapsack.
7. [Unique Paths](https://leetcode.com/problems/unique-paths/) – 2D grid DP.

## One thing that was confusing to me
I used to jump straight to coding without defining the state precisely. Many wrong solutions came from a vague state definition. Writing out "dp[i] means…" in plain English before writing any code made a huge difference — it forces you to be exact about what you're storing and why.

## See also
- [Recursion](../recursion/recursion.md) – memoized recursion is top-down DP.
- [Backtracking](../backtracking/backtracking.md) – DP can sometimes replace exponential backtracking.
- [Searching / Binary Search](../searching/binary-search.md) – LIS uses binary search for the O(n log n) solution.
