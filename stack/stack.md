# Stack

## One-sentence definition
A stack is a Last-In-First-Out (LIFO) collection where you only ever add to or remove from the same end (the "top").

## When to use it (use cases)
- Reversing things (characters, lists, traversal order).
- Matching pairs (balanced parentheses, valid HTML/XML tags).
- Undo/redo history in editors.
- Tracking function calls (the call stack itself is a stack).
- Iterative DFS on trees and graphs (replacing recursion).
- "Most recent unresolved item" problems — Next Greater Element, daily temperatures, largest rectangle (monotonic stack).

## Step-by-step explanation (plain language, no code first)
1. Think of a stack of plates: you can only take the top plate, and you can only place a new plate on top.
2. **Push** means add an item to the top.
3. **Pop** means remove and return the top item.
4. **Peek** (or **top**) means look at the top item without removing it.
5. The last item you pushed is always the first one you pop back out — that is the LIFO rule.
6. Before popping or peeking, always check the stack is not empty, or you will get an error.

## Visual intuition (ASCII)

### LIFO order
```text
push(1) push(2) push(3)        pop() -> 3     pop() -> 2

  | 3 |  <- top                  | 2 | <- top    | 1 | <- top
  | 2 |                          | 1 |           |   |
  | 1 |                          |   |           |   |
  +---+                          +---+           +---+

The item pushed LAST comes out FIRST.
```

## Templates (Python)

### 1) Stack basics with a list
Use when: you need a simple LIFO stack.
Input: items to push.
Output: the values popped back in reverse order.
```python
def stack_demo():
    stack = []

    stack.append(1)   # push,  TC: O(1) amortized
    stack.append(2)   # push
    top = stack[-1]   # peek,  TC: O(1)  -> 2

    last = stack.pop()  # pop,  TC: O(1)  -> 2
    is_empty = not stack
    return top, last, is_empty
```

### 2) Balanced parentheses
Use when: checking that every opening bracket has a matching closing bracket in the right order.
Input: `s` — a string of brackets `()[]{}`.
Output: `True` if balanced, else `False`.
```python
def is_balanced(s):
    pairs = {')': '(', ']': '[', '}': '{'}
    stack = []

    for ch in s:
        if ch in '([{':
            stack.append(ch)
        elif ch in pairs:
            # Closing bracket must match the most recent opening one.
            if not stack or stack.pop() != pairs[ch]:
                return False

    return not stack  # leftover openings means unbalanced
```

### 3) Iterative DFS with an explicit stack
Use when: traversing a graph/tree without recursion (avoids deep-recursion limits).
Input: `graph` — adjacency dict; `start` — starting node.
Output: list of nodes in DFS visit order.
```python
def dfs_iterative(graph, start):
    visited = set()
    order = []
    stack = [start]

    while stack:
        node = stack.pop()
        if node in visited:
            continue
        visited.add(node)
        order.append(node)
        # Push neighbors; they will be processed top-down (LIFO).
        for nxt in graph[node]:
            if nxt not in visited:
                stack.append(nxt)

    return order
```

### 4) Monotonic stack — Next Greater Element
Use when: for each item, you need the next item to its right that is larger.
Input: `nums` — list of numbers.
Output: list where `res[i]` is the next greater value, or `-1` if none.
```python
def next_greater(nums):
    res = [-1] * len(nums)
    stack = []  # holds indices whose answer is not found yet

    for i, x in enumerate(nums):
        # While current value beats the value at the stack's top index,
        # we just found that index's next greater element.
        while stack and nums[stack[-1]] < x:
            res[stack.pop()] = x
        stack.append(i)

    return res
```

## Time & space complexity (Big O)
- Push: `O(1)` amortized
- Pop: `O(1)`
- Peek / top: `O(1)`
- Search for a value: `O(n)`
- Space: `O(n)` for `n` stored items
- Monotonic stack pass: `O(n)` total — each element is pushed and popped at most once.

## Practice questions (concept check)
1. What does LIFO mean, and how is it different from a queue's FIFO?
2. Why must you check `not stack` before calling `pop()`?
3. In the monotonic stack template, why is the total time `O(n)` even though there is a `while` loop inside the `for` loop?

<details>
<summary>Answers</summary>

1. LIFO = Last-In-First-Out: the most recently added item is removed first. A queue is FIFO (First-In-First-Out): the oldest item is removed first.
2. Popping an empty stack raises an error (e.g., `IndexError` in Python); the check prevents crashing on unmatched input.
3. Each index is pushed exactly once and popped at most once, so the inner `while` does at most `n` pops across the whole run — amortized `O(n)`.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses)
2. [Min Stack](https://leetcode.com/problems/min-stack)
3. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures)
4. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)
5. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)

## One thing that was confusing to me
At first I thought a "monotonic stack" was a special data structure, but it is just a normal stack plus a rule: you pop elements before pushing so the stack stays sorted (increasing or decreasing). The data structure never changes — only how you decide when to push and pop.

## See also
- [Recursion](../recursion/recursion.md) – recursion uses the implicit call stack; the iterative DFS template makes it explicit.
- [Depth First Search](../graph/dfs.md) – DFS is naturally a stack-based traversal.
- [Heap](../heap/heap.md) – another way to manage "which element comes out next," ordered by priority instead of arrival time.
