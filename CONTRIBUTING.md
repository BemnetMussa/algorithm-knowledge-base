# Contributing Guide

Thanks for contributing to the Algorithm Knowledge Base.

## Quick checklist
1. Claim your topic first (GitHub Issue or claim table in `README.md`).
2. Create your note file in the correct folder (see folder structure in `README.md`).
3. Follow the required section order from the template in `README.md`.
4. Add at least one reusable template with `Use when:`, `Input:`, and `Output:` before each code block.
5. Add concept-check questions with answers inside a `<details>` block.
6. Add LeetCode/Codeforces practice links (no answers needed for those).
7. Open a pull request with the title format: `Add note: <Algorithm Name> by @your-username`.

## Writing expectations
- Prefer clear, practical wording over formal textbook style.
- Include small ASCII diagrams when they improve understanding.
- Keep content beginner-friendly but not oversimplified.
- Write in your own words — no copy-paste from Wikipedia or textbooks.

## Template conventions

Every code block must be preceded by three lines:

```
Use when: <when this exact template applies>
Input: <what the function takes>
Output: <what it returns>
```

This makes templates easy to scan and reuse quickly. See any existing file (e.g., `heap/heap.md`) for examples.

## Section order (must match this order exactly)

1. `## One-sentence definition`
2. `## When to use it (use cases)`
3. `## Step-by-step explanation (plain language, no code first)`
4. `## Templates (<language>)`
5. `## Time & space complexity (Big O)`
6. `## Practice questions (concept check)` — with a `<details>` answers block
7. `## Practice questions (LeetCode/Codeforces)`
8. `## One thing that was confusing to me`
9. `## See also`

## Naming and paths
- Use lowercase and hyphens for filenames: `merge-sort.md`, `binary-search.md`.
- Place your file in the matching topic folder. Current folders:

```text
array/              ← two pointers and array techniques
backtracking/
Bitwise/
dynamic-programming/
graph/              ← BFS, DFS, Dijkstra, Kruskal, Topological Sort
heap/
linked-list/
prefix-sum/
recursion/
searching/          ← Binary Search, Jump Search, Exponential Search
sliding-window/
sorting/            ← Merge Sort, Quick Sort
trie/
union-find/
```

## Example paths
- `heap/heap.md`
- `recursion/recursion.md`
- `linked-list/singly-linked-list.md`
- `sorting/merge-sort.md`
- `backtracking/backtracking.md`
