# Backtracking

## One-sentence definition
Backtracking is a recursive technique that builds candidates for a solution one step at a time and abandons ("backtracks") a path as soon as it determines the path cannot lead to a valid solution.

## When to use it (use cases)
- Finding all permutations or combinations of a set.
- Solving constraint-satisfaction problems (Sudoku, N-Queens).
- Generating all valid subsets or power sets.
- Maze/path-finding problems where you need to explore all routes.
- Word search on a 2D grid.

## Step-by-step explanation (plain language, no code first)
1. Start with an empty candidate solution.
2. At each step, choose one option from the remaining valid choices and add it to the candidate.
3. Check if the candidate violates any constraint — if it does, discard this option immediately and try the next one (this is the "backtrack" step).
4. If the candidate is complete and valid, record it as a solution.
5. After exploring all options from the current position, undo the last choice and return to the previous step to try a different option.
6. Repeat until all possibilities have been explored.

Think of it like navigating a maze: you walk forward until you hit a dead end, then retrace your steps to the last junction and try a different path.

```
              []
           /      \
         [1]      [2]
        /   \       \
     [1,2] [1,3]   [2,3]
      /
  [1,2,3]  ← valid solution
```

## Templates (Python)

### Template 1 — General backtracking skeleton
Use when: You need to explore all valid combinations/arrangements.  
Input: `candidates` — list of elements to choose from; `target` — the constraint or goal.  
Output: All valid solutions (list of lists).

```python
def backtrack(candidates, target):
    results = []

    def dfs(start, current):
        if <base_case>:          # solution is complete
            results.append(list(current))
            return
        for i in range(start, len(candidates)):
            if <constraint_violated>:   # prune early
                continue
            current.append(candidates[i])
            dfs(i + 1, current)         # or dfs(i, current) to allow reuse
            current.pop()               # undo choice (backtrack)

    dfs(0, [])
    return results
```

---

### Template 2 — Subsets (power set)
Use when: You need all possible subsets of a list with no duplicates.  
Input: `nums` — list of distinct integers.  
Output: All subsets as a list of lists.

```python
def subsets(nums):
    results = []

    def dfs(start, current):
        results.append(list(current))   # every state is a valid subset
        for i in range(start, len(nums)):
            current.append(nums[i])
            dfs(i + 1, current)
            current.pop()

    dfs(0, [])
    return results
```

---

### Template 3 — Permutations
Use when: You need all orderings of a list.  
Input: `nums` — list of distinct integers.  
Output: All permutations as a list of lists.

```python
def permutations(nums):
    results = []

    def dfs(current, remaining):
        if not remaining:
            results.append(list(current))
            return
        for i in range(len(remaining)):
            current.append(remaining[i])
            dfs(current, remaining[:i] + remaining[i+1:])
            current.pop()

    dfs([], nums)
    return results
```

---

### Template 4 — N-Queens
Use when: You need to place N queens on an N×N board so none attack each other.  
Input: `n` — board size.  
Output: Count of valid placements (or the boards themselves).

```python
def n_queens(n):
    count = 0
    cols = set()
    diag1 = set()   # row - col
    diag2 = set()   # row + col

    def dfs(row):
        nonlocal count
        if row == n:
            count += 1
            return
        for col in range(n):
            if col in cols or (row - col) in diag1 or (row + col) in diag2:
                continue            # prune: queen would be attacked
            cols.add(col)
            diag1.add(row - col)
            diag2.add(row + col)
            dfs(row + 1)
            cols.remove(col)
            diag1.remove(row - col)
            diag2.remove(row + col)

    dfs(0)
    return count
```

## Time & space complexity (Big O)

| Problem | Time | Space |
|---|---|---|
| Subsets | O(2^n) | O(n) call stack |
| Permutations | O(n!) | O(n) call stack |
| N-Queens | O(n!) worst case | O(n) |

- **Time** is typically exponential or factorial because we explore many paths.
- **Space** is usually O(n) for the recursion stack depth, plus O(output size) for storing results.
- Pruning reduces the constant factor dramatically in practice.

## Practice questions (concept check)
1. What is the difference between backtracking and brute force?
2. Why do we call `current.pop()` after the recursive call?
3. How does pruning improve backtracking performance?

<details>
<summary>Answers</summary>

1. Brute force generates every candidate completely before checking validity. Backtracking checks constraints at each step and abandons invalid paths early, so it avoids exploring dead-end branches entirely.
2. `current.pop()` undoes the last choice, restoring the state to what it was before the recursive call. Without it, `current` would keep growing and carry stale data into the next iteration.
3. Pruning cuts off entire subtrees of the search space the moment a constraint is violated, rather than exploring all combinations within that subtree. This can reduce exponential time to much less in practice.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Subsets](https://leetcode.com/problems/subsets/)
2. [Permutations](https://leetcode.com/problems/permutations/)
3. [Combination Sum](https://leetcode.com/problems/combination-sum/)
4. [N-Queens](https://leetcode.com/problems/n-queens/)
5. [Word Search](https://leetcode.com/problems/word-search/)

## One thing that was confusing to me
The `current.pop()` line felt wrong to me at first — it looked like it was throwing away work. The key insight is that backtracking intentionally undoes work: after fully exploring a branch, you need to restore the state so the next branch starts from the same clean slate. The recursion does the exploring; the pop does the cleanup.

## See also
- [Recursion](../recursion/recursion.md)
- [Dynamic Programming](../dynamic-programming/dynamic-programming.md)
- [DFS](../graph/dfs.md)
