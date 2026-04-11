---
title: DSA Patterns
tags: [concept, dsa, algorithms, leetcode, interviews]
sources: [sources/leetcode-patterns.md, sources/dsa-cheat-sheet.md]
last-updated: 2026-04-10
---

# DSA Patterns

Common algorithmic patterns that appear repeatedly in coding interviews. Learn the pattern, not individual problems.

---

## Quick Pattern Recognition Cheat Sheet

| Situation | Pattern |
|-----------|---------|
| Max subarray sum, increase current or start new | **Kadane's Algorithm** |
| Range queries, max/count of target subarray sum, 2D sum | **Prefix Sum** |
| Partition array, Dutch flag style placement | **Dutch National Flag + Cyclic Sort** |
| Pair/length in sorted array, trapping rain water | **Two Pointers** |
| Subarray that can grow or shrink based on condition | **Sliding Window** |
| Increasing or decreasing sequence | **Monotonic Stack** |
| Search in sorted array, find max/min over answer space | **Binary Search** |
| Max K elements, first/last K, greedy with ordering | **Heap (Priority Queue)** |
| Find all possible solutions | **Backtracking** |
| Search prefix in string | **Trie** |

---

## Pattern Deep Dives

### 1. Sliding Window

**Use when:** contiguous subarray/substring, keywords "longest", "shortest", "minimum", "maximum", "contains"

**Template:**
```python
left = 0
for right in range(len(s)):
    # expand window: add s[right]
    while window_invalid():
        # shrink: remove s[left]; left += 1
    # update result
```

**Classic:** Longest substring with K distinct characters (LC #340)

---

### 2. Two Pointers

**Use when:** sorted array, pair/triplet sum, "remove duplicates", working from both ends

**Variants:**
- Opposite ends → move toward center
- Same direction at different speeds → fast/slow
- Three pointers → sort first, fix one, two-pointer the rest

**Classic:** Squares of sorted array (LC #977), 3Sum (LC #15)

---

### 3. Fast & Slow Pointers (Floyd's)

**Use when:** linked list, detect cycle, find middle, Nth from end

```python
slow, fast = head, head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast:  # cycle detected
```

**Classic:** Linked List Cycle (LC #141), Happy Number (LC #202)

---

### 4. Merge Intervals

**Use when:** interval arrays, "overlapping", "merge", "insert interval"

**Always sort by start time first.**

**Classic:** Merge Intervals (LC #56), Meeting Rooms (LC #252)

---

### 5. Cyclic Sort

**Use when:** array of n numbers in range [0,n] or [1,n], find missing/duplicate number

**Idea:** Place each number at its correct index (number `k` belongs at index `k`).

**Classic:** Find Missing Number (LC #268), Find All Duplicates (LC #442)

---

### 6. Binary Search

**Use when:** sorted input, or "find min/max X such that condition holds"

**Template for answer-space search:**
```python
lo, hi = min_possible, max_possible
while lo < hi:
    mid = (lo + hi) // 2
    if feasible(mid):
        hi = mid
    else:
        lo = mid + 1
return lo
```

---

### 7. Monotonic Stack

**Use when:** next greater/smaller element, "span", "histogram" problems

Maintain a stack where elements are always in monotonic order. Pop when you find an element that breaks the order.

**Classic:** Daily Temperatures (LC #739), Largest Rectangle in Histogram (LC #84)

---

### 8. Tree / Graph BFS & DFS

- **BFS:** level-order, shortest path in unweighted graph
- **DFS:** explore all paths, backtracking, topological sort

---

### 9. Dynamic Programming

**Signals:** "maximum/minimum", "count ways", "is it possible", optimal substructure

**Framework:** define state → recurrence → base case → memoize or tabulate

**Patterns within DP:** 1D DP, 2D DP, interval DP, knapsack, LCS, LIS

---

### 10. Heap (Priority Queue)

**Use when:** "top K", "K closest", streaming median, merge K sorted lists

Python: `heapq` (min-heap by default; negate values for max-heap)

---

## Resources
- Swati's Patterns Sheet (Google Sheets) — DSA interview problems organized by pattern
- LeetCode Company-wise Problems — sorted by company frequency
- labuladong.online — pattern-first explanations

---

## Related
- [[sources/leetcode-patterns.md]]
- [[concepts/system-design-fundamentals]]
