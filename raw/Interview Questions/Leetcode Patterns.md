---

---

### 1. Sliding Window

**The Big Idea:** This pattern is about efficiently processing a **contiguous** part of a data structure. Imagine you have a long train, and you're looking through a "viewfinder" (the window) that can only see a few cars at a time. You slide this viewfinder along the train to find what you're looking for. The viewfinder can be a fixed size (e.g., "find the max sum of any 3 consecutive cars") or a dynamic size (e.g., "find the longest sequence of cars without any duplicates").

**When to Look for This Pattern (The Signals):**

- The input is a linear data structure like an **Array, String, or Linked List**.
- The problem asks for something related to a **contiguous subarray or substring**.
- You see keywords like **"longest"**, **"shortest"**, **"minimum"**, **"maximum"**, or **"contains"** applied to a subarray/substring.

**Classic LeetCode Example: Longest Substring with K Distinct Characters (LeetCode #340)Problem:** Given a string `s` and an integer `k`, find the length of the longest substring of `s` that contains at most `k` distinct characters.

**Code in Action:**

```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    """
    Finds the longest substring with at most K distinct characters using a dynamic sliding window.
    """
    if k == 0:
        return 0

    left = 0
    max_len = 0
    char_frequency = {} # Our window's state: counts of distinct characters

    # The right pointer of the window is 'right' from the loop
    for right, char in enumerate(s):
        # EXPAND the window by adding the rightmost character
        char_frequency[char] = char_frequency.get(char, 0) + 1

        # CHECK VALIDITY: Is our window still valid?
        # A window is invalid if we have more than k distinct characters.
        while len(char_frequency) > k:
            # If invalid, SHRINK the window from the left
            left_char = s[left]
            char_frequency[left_char] -= 1
            if char_frequency[left_char] == 0:
                del char_frequency[left_char] # Remove character if its count is zero

            left += 1 # Move the left pointer to shrink the window

        # UPDATE RESULT: The window is now valid.
        # Calculate its length and see if it's the new maximum.
        max_len = max(max_len, right - left + 1)

    return max_len

# Example:
print(longest_substring_with_k_distinct("araaci", 2)) # Output: 4 (for "araa")
print(longest_substring_with_k_distinct("cbbebi", 3)) # Output: 5 (for "cbbeb")

```

---

### 2. Two Pointers

**The Big Idea:** This is a broad but powerful pattern for processing data from two different points simultaneously. Think of it as having two fingers on a list. You might start them at opposite ends and move them toward each other, or start them both at the beginning and move them at different speeds. The key is that you're using the relationship between the two pointers to make intelligent decisions and avoid the brute-force O(n²) approach of comparing every element to every other element.

**When to Look for This Pattern (The Signals):**

- The problem involves a **sorted array** (or a linked list) and asks you to find a pair, triplet, or subarray that satisfies a condition.
- Keywords: **"pair with a sum"**, **"triplets that sum to zero"**, **"squaring a sorted array"**, **"remove duplicates"**.
- The problem can be solved by processing the data from both ends inward, or with one pointer "leading" another.

**Classic LeetCode Example: Squaring a Sorted Array (LeetCode #977)Problem:** Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

**Code in Action:**```python
def sortedSquares(nums: list[int]) -> list[int]:
"""
Squares a sorted array and returns a sorted result using the Two Pointers pattern.
The key insight is that the largest squares will come from the edges of the original array.
"""
n = len(nums)
result = [0] * n # Create a result array of the same size

```plain text
left, right = 0, n - 1
# 'write_pointer' will place the largest square at the end of the result array
write_pointer = n - 1

# Move pointers from the outside in
while left <= right:
    left_square = nums[left] ** 2
    right_square = nums[right] ** 2

    # Compare the squares at the two ends
    if left_square > right_square:
        # The left square is larger, so it belongs at the end of the result
        result[write_pointer] = left_square
        left += 1 # Move the left pointer in
    else:
        # The right square is larger or equal, so it belongs at the end
        result[write_pointer] = right_square
        right -= 1 # Move the right pointer in

    write_pointer -= 1 # Move the write pointer to the left

return result

```

# Example:

print(sortedSquares([-4, -1, 0, 3, 10])) # Output: [0, 1, 9, 16, 100]

```plain text

---

### 3. Fast & Slow Pointers (Hare & Tortoise)

**The Big Idea:** A specific, brilliant application of the Two Pointers pattern, almost exclusively for **linked lists**. You have a "slow" pointer (the tortoise) that moves one step at a time, and a "fast" pointer (the hare) that moves two steps at a time. Their interaction reveals properties of the list you couldn't otherwise see. If there's a cycle, the hare is guaranteed to eventually lap the tortoise.

**When to Look for This Pattern (The Signals):**
*   The problem involves a **Linked List**.
*   You need to detect a **cycle**.
*   You need to find the **middle** element.
*   You need to find the Nth element from the end.

**Classic LeetCode Example: Linked List Cycle (LeetCode #141)**
**Problem:** Given `head`, the head of a linked list, determine if the linked list has a cycle.

**Code in Action:**
```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head: ListNode) -> bool:
    """
    Detects if a linked list has a cycle using the Fast & Slow Pointers pattern.
    """
    if not head:
        return False

    slow = head
    fast = head

    # The loop condition is crucial: we can only proceed if 'fast' and 'fast.next' exist,
    # because 'fast' needs to jump two steps. If either is None, we've reached the end.
    while fast and fast.next:
        slow = slow.next # The tortoise moves one step
        fast = fast.next.next # The hare moves two steps

        # The moment they point to the same node, we've found a cycle.
        if slow == fast:
            return True

    # If the loop completes, it means the fast pointer reached the end of the list.
    return False

```

---

### 4. Merge Intervals

**The Big Idea:** This pattern deals with problems where you have a collection of intervals (`[start, end]`) and you need to combine the ones that overlap. The absolute, non-negotiable first step is to **sort the intervals by their start time**. Once sorted, you can process them linearly, making simple decisions about merging.

**When to Look for This Pattern (The Signals):**

- The problem statement explicitly mentions **"intervals"**.
- You are asked to find **"overlapping"** events, schedules, or ranges.
- Keywords: **"merge"**, **"insert interval"**, **"conflicting appointments"**.

**Classic LeetCode Example: Merge Intervals (LeetCode #56)Problem:** Given an array of `intervals`, merge all overlapping intervals.

**Code in Action:**

```python
def merge(intervals: list[list[int]]) -> list[list[int]]:
    """
    Merges overlapping intervals. The core logic relies on sorting first.
    """
    if not intervals:
        return []

    # 1. THE MOST IMPORTANT STEP: Sort by the start of each interval.
    intervals.sort(key=lambda x: x[0])

    merged = [intervals[0]] # Start with the first interval

    for current_interval in intervals[1:]:
        last_merged_interval = merged[-1]

        # Check for an overlap: Does the current interval start before the last one ends?
        if current_interval[0] <= last_merged_interval[1]:
            # If they overlap, we merge them by extending the end of the last interval.
            # We take the maximum of the two ends.
            last_merged_interval[1] = max(last_merged_interval[1], current_interval[1])
        else:
            # No overlap, so we can just add the current interval to our list.
            merged.append(current_interval)

    return merged

# Example:
print(merge([[1,4],[2,6],[8,10]])) # Output: [[1,6],[8,10]]

```

---

### 5. Cyclic Sort

**The Big Idea:** This is a clever pattern for a specific scenario: you have an array of `n` numbers, and you know the numbers are supposed to be in a specific range (e.g., `1` to `n`). The goal is to place each number in its "correct" index. For example, the number `1` should be at index `0`, `2` at index `1`, and so on. You iterate through the array, and if a number is not in its correct spot, you swap it with the number that *is* in its correct spot. You keep swapping until the number at your current position is correct, then you move on.

**When to Look for This Pattern (The Signals):**

- The input is an **array of numbers**.
- The numbers are in a given **consecutive range**, like `1` to `n`.
- The problem asks you to find a **missing number**, a **duplicate number**, or the **smallest missing positive**.

**Classic LeetCode Example: Find the Missing Number (LeetCode #268)Problem:** Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

**Code in Action:**

```python
def findMissingNumber(nums: list[int]) -> int:
    """
    Finds the missing number using the Cyclic Sort pattern.
    The goal is to place each number at its corresponding index.
    e.g., the number '3' should be at index 3.
    """
    i = 0
    n = len(nums)
    while i < n:
        correct_index = nums[i]
        # If the number is not at its correct index AND its correct index is valid
        if correct_index < n and nums[i] != nums[correct_index]:
            # Swap it into place
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            # The number is either in the correct spot, or it's the 'n' value
            # which has no valid spot. Move on.
            i += 1

    # After sorting, iterate through the array one last time.
    # The first index where the number doesn't match the index is the missing one.
    for i in range(n):
        if nums[i] != i:
            return i

    # If all numbers match their indices, the missing number is 'n'.
    return n

# Example:
print(findMissingNumber([3, 0, 1])) # Output: 2

```

---

*This covers the first 5 patterns in detail. I will continue with the remaining patterns in the next message to keep the response clear and readable.*