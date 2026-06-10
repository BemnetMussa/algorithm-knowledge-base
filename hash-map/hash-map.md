# Hash Map & Hash Set

## One-sentence definition
A hash map stores key→value pairs and a hash set stores unique keys, both using a hash function to give average `O(1)` insert, lookup, and delete.

## When to use it (use cases)
- Fast membership checks ("have I seen this before?").
- Counting frequencies (characters, words, numbers).
- Grouping items by a key (anagrams, categories).
- Caching / memoization (store computed results by input).
- Mapping one thing to another (value → index, node → clone).
- De-duplicating a collection (a set drops repeats automatically).

## Step-by-step explanation (plain language, no code first)
1. A hash map keeps an internal array of "buckets."
2. When you insert a key, a **hash function** turns the key into a number, and that number picks a bucket.
3. The pair is stored in that bucket; looking it up later runs the same hash and jumps straight to the bucket — no scanning the whole structure.
4. Two different keys can hash to the same bucket. That is a **collision**, and the map handles it (e.g., by chaining several pairs in one bucket).
5. As long as collisions are rare, insert/lookup/delete are `O(1)` on average. In a bad worst case (everything collides) they degrade to `O(n)`.
6. A hash **set** is the same idea but stores only keys — it answers "is this present?" not "what value maps to it?"
7. Keys must be **hashable** (immutable): strings, numbers, tuples are fine; lists and dicts are not.

## Visual intuition (ASCII)

### Buckets and collisions (chaining)
```text
hash("cat") -> 1
hash("dog") -> 3
hash("ant") -> 1   (collides with "cat")

bucket 0: ()
bucket 1: ("cat", 5) -> ("ant", 2)   <- two keys share a bucket
bucket 2: ()
bucket 3: ("dog", 9)

Lookup "ant": hash -> bucket 1, then walk the short chain to find "ant".
```

## Templates (Python)

### 1) Hash map and hash set basics
Use when: you need O(1) key→value storage or O(1) membership.
Input: keys/values to store.
Output: looked-up value and membership result.
```python
def basics():
    m = {}                 # hash map (dict)
    m["a"] = 1             # insert,  TC: O(1) avg
    m["b"] = 2
    val = m.get("a", 0)   # lookup with default, TC: O(1) avg -> 1
    has_b = "b" in m      # membership, TC: O(1) avg -> True

    s = set()             # hash set
    s.add(10)
    s.add(10)             # duplicate ignored -> set stays {10}
    return val, has_b, 10 in s
```

### 2) Frequency counting
Use when: you need how many times each item appears.
Input: `items` — any iterable of hashable values.
Output: dict of item → count.
```python
def count_frequency(items):
    freq = {}
    for x in items:
        freq[x] = freq.get(x, 0) + 1   # default 0, then increment
    return freq

# Or simply: from collections import Counter; Counter(items)
```

### 3) Two Sum (value → index map)
Use when: find two numbers that add up to a target in one pass.
Input: `nums` — list of ints; `target` — desired sum.
Output: indices of the two numbers, or `None`.
```python
def two_sum(nums, target):
    seen = {}  # value -> index
    for i, x in enumerate(nums):
        need = target - x
        if need in seen:           # complement already seen? done.
            return [seen[need], i]
        seen[x] = i
    return None
```

### 4) Group anagrams (key derived from data)
Use when: cluster items that share a computed key.
Input: `words` — list of strings.
Output: list of groups, each group a list of anagrams.
```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)
    for w in words:
        key = "".join(sorted(w))   # same letters -> same key
        groups[key].append(w)
    return list(groups.values())
```

## Time & space complexity (Big O)
- Insert: `O(1)` average, `O(n)` worst case (many collisions)
- Lookup / membership: `O(1)` average, `O(n)` worst case
- Delete: `O(1)` average, `O(n)` worst case
- Iterating all entries: `O(n)`
- Space: `O(n)` for `n` stored keys

## Practice questions (concept check)
1. What is a hash collision, and why does a hash map still work when one happens?
2. Why must dictionary keys be immutable (hashable)?
3. When would average `O(1)` lookups degrade to `O(n)`?

<details>
<summary>Answers</summary>

1. A collision is when two different keys hash to the same bucket. The map stores both in that bucket (e.g., a small chain) and walks the short chain to find the exact key, so correctness is preserved.
2. The hash is computed from the key's contents. If a key could change after insertion, its hash would change and the map could no longer find it in the right bucket.
3. When almost every key lands in the same bucket (a pathological or adversarial hash), every lookup must scan a long chain — that is the `O(n)` worst case.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Two Sum](https://leetcode.com/problems/two-sum)
2. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate)
3. [Valid Anagram](https://leetcode.com/problems/valid-anagram)
4. [Group Anagrams](https://leetcode.com/problems/group-anagrams)
5. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence)

## One thing that was confusing to me
At first I treated "O(1) average" as if it were always instant, then got a slow solution when keys clustered. I learned the `O(1)` is an *average* that relies on a good hash spreading keys across buckets; the worst case is genuinely `O(n)`, which is why interviewers sometimes ask about collisions.

## See also
- [Two Pointers](../array/two-pointers.md) – an alternative to a hash map for some pair-sum problems when the array is sorted.
- [Prefix Sum](../prefix-sum/prefix-sum.md) – often paired with a hash map to count subarrays with a target sum.
- [Trie](../trie/trie.md) – a different way to store and look up keys, specialized for string prefixes.
