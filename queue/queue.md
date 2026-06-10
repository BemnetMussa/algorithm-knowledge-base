# Queue & Deque

## One-sentence definition
A queue is a First-In-First-Out (FIFO) collection where you add at the back and remove from the front, and a deque ("double-ended queue") lets you add or remove from both ends.

## When to use it (use cases)
- Processing items in arrival order (print jobs, task scheduling, message buffers).
- Breadth-First Search (BFS) on trees and graphs.
- Level-order traversal of a tree.
- Sliding-window problems where you need the max/min of the current window (monotonic deque).
- Implementing a stack, a queue, or both at once (a deque can do everything).

## Step-by-step explanation (plain language, no code first)
1. Think of a line of people at a shop: the first person to arrive is the first to be served — that is FIFO.
2. **Enqueue** means add an item to the back of the line.
3. **Dequeue** means remove and return the item at the front of the line.
4. **Peek front** means look at who is next without removing them.
5. A **deque** removes the "only one end" restriction: you can push/pop at the front *and* the back. That makes it a superset of both a stack and a queue.
6. Always check the queue is not empty before removing, or you will get an error.

## Visual intuition (ASCII)

### FIFO order (queue)
```text
enqueue(1) enqueue(2) enqueue(3)        dequeue() -> 1     dequeue() -> 2

front -> [1, 2, 3] <- back               [2, 3]             [3]

The item added FIRST comes out FIRST (opposite of a stack).
```

### Deque (both ends are open)
```text
        push_front <--- [ a, b, c ] ---> push_back
        pop_front  ---> [ a, b, c ] <--- pop_back
```

## Templates (Python)

### 1) Queue with `collections.deque`
Use when: you need an efficient FIFO queue.
Input: items to enqueue.
Output: values dequeued in arrival order.
```python
from collections import deque

def queue_demo():
    q = deque()

    q.append(1)        # enqueue at back,  TC: O(1)
    q.append(2)
    front = q[0]       # peek front,       TC: O(1)  -> 1

    first = q.popleft()  # dequeue front,  TC: O(1)  -> 1
    is_empty = not q
    return front, first, is_empty
```

> Note: do not use a plain Python list as a queue. `list.pop(0)` is `O(n)`
> because every remaining element shifts left. `deque.popleft()` is `O(1)`.

### 2) BFS with a queue
Use when: finding shortest path (in edges) on an unweighted graph, or level-order traversal.
Input: `graph` — adjacency dict; `start` — starting node.
Output: list of nodes in BFS visit order.
```python
from collections import deque

def bfs(graph, start):
    visited = {start}
    order = []
    q = deque([start])

    while q:
        node = q.popleft()
        order.append(node)
        for nxt in graph[node]:
            if nxt not in visited:
                visited.add(nxt)   # mark on enqueue to avoid duplicates
                q.append(nxt)

    return order
```

### 3) Deque used from both ends
Use when: you need stack-like and queue-like access at once.
Input: none (demo).
Output: the deque after mixed operations.
```python
from collections import deque

def deque_demo():
    dq = deque()

    dq.append(2)        # back  -> [2]
    dq.appendleft(1)    # front -> [1, 2]
    dq.append(3)        # back  -> [1, 2, 3]

    dq.pop()            # remove back  -> [1, 2]
    dq.popleft()        # remove front -> [2]
    return list(dq)
```

### 4) Sliding window maximum (monotonic deque)
Use when: you need the maximum of every window of size `k` in one pass.
Input: `nums` — list of numbers; `k` — window size.
Output: list of window maximums.
```python
from collections import deque

def max_sliding_window(nums, k):
    dq = deque()   # holds indices, values decreasing front -> back
    res = []

    for i, x in enumerate(nums):
        # Drop indices whose values can never be the max again.
        while dq and nums[dq[-1]] < x:
            dq.pop()
        dq.append(i)

        # Drop the front index if it slid out of the window.
        if dq[0] <= i - k:
            dq.popleft()

        if i >= k - 1:
            res.append(nums[dq[0]])  # front index = current window max

    return res
```

## Time & space complexity (Big O)
- Enqueue / `append`: `O(1)`
- Dequeue / `popleft`: `O(1)` (with `deque`; `O(n)` with a plain list)
- Peek front / back: `O(1)`
- Search for a value: `O(n)`
- Space: `O(n)` for `n` stored items
- Sliding window maximum: `O(n)` total — each index is pushed and popped at most once.

## Practice questions (concept check)
1. What is the difference between FIFO and LIFO?
2. Why is `collections.deque` preferred over a plain list for a queue?
3. In the sliding-window-maximum template, why do we store indices in the deque instead of the values themselves?

<details>
<summary>Answers</summary>

1. FIFO (queue) removes the oldest item first; LIFO (stack) removes the newest item first.
2. `deque.popleft()` is `O(1)`, while removing the front of a list (`list.pop(0)`) is `O(n)` because all later elements shift left.
3. With indices we can check whether the front element has slid out of the window (`dq[0] <= i - k`); a raw value would not tell us its position.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks)
2. [Number of Islands](https://leetcode.com/problems/number-of-islands)
3. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges)
4. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum)
5. [Design Circular Queue](https://leetcode.com/problems/design-circular-queue)

## One thing that was confusing to me
At first I used a normal list and did `q.pop(0)` for the queue, and my BFS was strangely slow on large graphs. The list *looked* correct, but every `pop(0)` quietly shifted the whole list, turning an `O(n)` BFS into `O(n^2)`. Switching to `deque` fixed it — the data structure you choose changes the complexity, not just the syntax.

## See also
- [Stack](../stack/stack.md) – the LIFO counterpart; a deque can act as either a stack or a queue.
- [Breadth First Search](../graph/bfs.md) – BFS is built directly on a queue.
- [Sliding Window](../sliding-window/sliding-window.md) – the monotonic-deque trick powers window max/min problems.
