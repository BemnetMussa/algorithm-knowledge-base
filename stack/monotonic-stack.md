# Monotonic Stack

## One-sentence definition
A monotonic stack is an ordinary stack that you keep sorted (always increasing or always decreasing) by popping elements that break the order *before* you push a new one, which lets you answer "nearest greater/smaller element" questions for every item in a single `O(n)` pass.

## When to use it (use cases)
- **Next/Previous Greater or Smaller Element** for every item in an array.
- "How many days until a warmer temperature?" style span problems (Daily Temperatures).
- **Largest Rectangle in Histogram** and **Maximal Rectangle**.
- **Trapping Rain Water** and other "bounded by taller bars on both sides" problems.
- Stock span, remove-k-digits to make the smallest/largest number, and other "keep a clean increasing/decreasing sequence" greedy problems.
- Any brute-force where you wrote two nested loops to look for the *nearest* element that beats the current one — that `O(n²)` usually collapses to `O(n)` with a monotonic stack.

## Step-by-step explanation (plain language, no code first)
1. It is **not a new data structure** — it is a normal stack plus one rule about *when* you are allowed to push.
2. Decide the order you want to keep: **decreasing** (top is the smallest) when you are hunting for *greater* elements, **increasing** (top is the largest) when you are hunting for *smaller* elements.
3. Walk through the array once. Before pushing the current item, **pop every item on top that breaks the order**.
4. The moment you pop an item, you have just discovered that item's answer — the current element is its "next greater/smaller". That is the whole trick: an item is popped exactly when its answer appears.
5. After popping, whatever is **left on top** is the current item's "previous greater/smaller" (useful for left-side questions).
6. Store **indices**, not values, when you also need distances or widths (so you can compute `i - j`).
7. Each index is pushed once and popped at most once, so the total work is `O(n)` even though there is a loop inside a loop.

## Visual intuition (ASCII)

### Next Greater Element on `[2, 1, 3]`, decreasing stack of indices
```text
i=0 val=2   stack empty -> push 0           stack(idx): [0]      vals:[2]
i=1 val=1   1 < 2, does not beat top -> push 1   stack: [0,1]    vals:[2,1]
i=2 val=3   3 > 1 -> pop 1, res[1]=3
            3 > 2 -> pop 0, res[0]=3
            push 2                          stack: [2]           vals:[3]
end         nothing left to answer -> res[2] = -1

result (next greater to the right): [3, 3, -1]
```
The stack values always read top-to-bottom as **increasing height of "still waiting" bars**; a tall new bar clears all the short ones waiting under it, and each one's answer is that tall bar.

### Increasing vs decreasing — which to use
```text
Looking for a GREATER neighbor  -> keep a DECREASING stack (pop the smaller-or-equal)
Looking for a SMALLER neighbor  -> keep an INCREASING stack (pop the greater-or-equal)

Want the NEXT one (to the right) -> the popped element's answer = current element
Want the PREVIOUS one (to the left) -> after popping, the new top = current's answer
```

## Templates (Python)

### 1) Next Greater Element to the right
Use when: for each item you want the first larger value that appears later.
Input: `nums` — list of numbers.
Output: `res[i]` = next greater value, or `-1` if none.
```python
def next_greater(nums):
    res = [-1] * len(nums)
    stack = []  # indices, values DECREASING from bottom to top

    for i, x in enumerate(nums):
        # x beats everything on top that is smaller -> their answer is x.
        while stack and nums[stack[-1]] < x:
            res[stack.pop()] = x
        stack.append(i)

    return res  # leftover indices keep -1 (no greater element exists)
```

### 2) Previous Smaller Element to the left
Use when: for each item you want the nearest smaller value before it (mirror of template 1).
Input: `nums` — list of numbers.
Output: `res[i]` = previous smaller value, or `-1` if none.
```python
def previous_smaller(nums):
    res = [-1] * len(nums)
    stack = []  # indices, values INCREASING from bottom to top

    for i, x in enumerate(nums):
        # Pop anything >= x; the value left on top is the previous smaller.
        while stack and nums[stack[-1]] >= x:
            stack.pop()
        res[i] = nums[stack[-1]] if stack else -1
        stack.append(i)

    return res
```

### 3) Daily Temperatures (distance variant)
Use when: you need *how far away* the next greater element is, not its value.
Input: `temps` — list of daily temperatures.
Output: `res[i]` = days to wait for a warmer day, or `0` if none.
```python
def daily_temperatures(temps):
    res = [0] * len(temps)
    stack = []  # indices, temperatures decreasing

    for i, t in enumerate(temps):
        while stack and temps[stack[-1]] < t:
            j = stack.pop()
            res[j] = i - j          # distance, because we stored indices
        stack.append(i)

    return res
```

### 4) Largest Rectangle in Histogram
Use when: find the biggest axis-aligned rectangle that fits under a bar chart.
Input: `heights` — bar heights.
Output: maximum rectangle area.
```python
def largest_rectangle(heights):
    stack = []        # indices, heights INCREASING
    best = 0
    heights = heights + [0]   # trailing 0 flushes the stack at the end

    for i, h in enumerate(heights):
        # A shorter bar closes off every taller bar still waiting:
        # that taller bar can no longer extend to the right.
        while stack and heights[stack[-1]] > h:
            top = stack.pop()
            height = heights[top]
            left = stack[-1] if stack else -1   # previous shorter bar
            width = i - left - 1                # right bound i, left bound left
            best = max(best, height * width)
        stack.append(i)

    return best
```

## Time & space complexity (Big O)
- Time: `O(n)` — each index is pushed once and popped at most once, so the inner `while` does at most `n` pops in total (amortized `O(1)` per element).
- Space: `O(n)` for the stack in the worst case (e.g., a strictly increasing or strictly decreasing input never pops early).
- This beats the naive `O(n²)` of comparing every element against every later element.

## Practice questions (concept check)
1. You want, for each element, the **next smaller** element to its right. Increasing or decreasing stack — and why?
2. Why do we push **indices** instead of values in the Daily Temperatures and Histogram templates?
3. In Largest Rectangle in Histogram, why do we append a trailing `0` to `heights`?

<details>
<summary>Answers</summary>

1. **Increasing** stack. To find a *smaller* neighbor you pop everything greater-or-equal than the current value; what gets popped just found its "next smaller" (the current element), and the stack stays increasing from bottom to top.
2. Because the answers need **distance or width** (`i - j`, `i - left - 1`), and you can only compute those from positions. Values alone lose the location information. (If you only needed the value, like template 1, storing values would also work.)
3. The trailing `0` is a **sentinel** shorter than every real bar, so it forces the `while` loop to pop and finalize every bar still left on the stack at the end. Without it, bars that never meet a shorter bar to their right would never be measured.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i)
2. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures)
3. [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii) (circular array)
4. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)
5. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)
6. [Remove K Digits](https://leetcode.com/problems/remove-k-digits)

## One thing that was confusing to me
I kept mixing up "increasing stack" with "I am looking for increasing values." It is the opposite: to find a **greater** element you keep a **decreasing** stack, because you only park elements that are still *waiting* for something bigger, and a waiting line of things-still-smaller-than-nothing-yet is naturally decreasing. The rule that finally stuck for me: *pop the elements you are searching a neighbor for; the current element is their answer.*

## See also
- [Stack](./stack.md) – the base LIFO structure; a monotonic stack is just a stack with a push rule.
- [Two Pointers](../array/two-pointers.md) – another "single pass instead of nested loops" technique; Trapping Rain Water can be solved either way.
- [Sliding Window](../sliding-window/sliding-window.md) – the monotonic **deque** variant powers sliding-window maximum.
- [Heap](../heap/heap.md) – when you need the global max/min repeatedly rather than the *nearest* one.
