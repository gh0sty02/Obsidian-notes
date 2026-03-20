---
base: "[[Study Plan/DSA/Resources/List of Resources/List of Resources.base]]"
Priority: High
Status: Done
---
A **Linked List** is a linear data structure where elements (nodes) are stored in non-contiguous memory locations. Each node contains:

1. **Data**
2. **Pointer (Reference) to the next node** (except in the case of Doubly Linked Lists, where there is an additional pointer to the previous node)

### **Types of Linked Lists**:

3. **Singly Linked List** – Each node points to the next node.
4. **Doubly Linked List** – Each node has pointers to both the next and previous nodes.
5. **Circular Linked List** – The last node points back to the first node.

### **Time Complexity Analysis**

| Operation | Singly Linked List | Doubly Linked List |
| --- | --- | --- |
| Access/Search | O(n) | O(n) |
| Insert (at head) | O(1) | O(1) |
| Insert (at tail) | O(n) | O(1) |
| Delete (from head) | O(1) | O(1) |
| Delete (from tail) | O(n) | O(1) |

---

## **💪 FAANG-Level Linked List Problems Categorized by Difficulty**

### **🟢 Easy Problems**

These problems focus on basic operations, traversal, and insertion/deletion in a linked list.

6. **Reverse a Linked List** – Iterative & Recursive
7. **Find Middle of a Linked List** – Fast & Slow Pointers
8. **Delete a Node Without Head Reference** – In-Place Deletion
9. **Detect Cycle in a Linked List** – Floyd’s Cycle Detection Algorithm
10. **Merge Two Sorted Linked Lists** – Two Pointers Merge
11. **Remove Duplicates from Sorted Linked List** – In-Place Deduplication
12. **Check if Linked List is Palindrome** – Reverse & Compare
13. **Intersection of Two Linked Lists** – Finding the merge point
14. **Nth Node from the End** – Two Pointers Approach

---

### **🟡 Medium Problems**

These problems introduce advanced techniques like fast and slow pointers, stack usage, and recursion.

15. **Flatten a Multilevel Doubly Linked List** – DFS/BFS Approach
16. **Add Two Numbers Represented as Linked List** – Reverse & Add
17. **Rotate Linked List** – Break & Attach
18. **Reverse Nodes in k-Group** – Recursive + Iterative
19. **Remove Duplicates from Unsorted Linked List** – HashSet or Sorting
20. **Partition List (Less than X on Left, Greater on Right)** – Two Pointers
21. **Sort Linked List (Merge Sort on LL)** – Merge Sort O(n log n)
22. **Delete Node in a Doubly Linked List** – Handling Previous & Next Pointers
23. **Swapping Nodes in a Linked List** – Find & Swap
24. **Split Linked List in Parts** – Dividing Linked List
25. **Reorder List (1st, last, 2nd, second-last, …)** – Reverse Half & Merge

---

### **🔴 Hard Problems**

These problems require deep understanding and optimization techniques.

26. **Copy List with Random Pointer** – HashMap or O(1) Space Trick
27. **Merge k Sorted Linked Lists** – Heap (Priority Queue) or Divide & Conquer
28. **LRU Cache using Linked List & HashMap** – Doubly Linked List + HashMap
29. **Reverse Nodes Between M and N** – Pointer Manipulation
30. **Find the Sum of Two Numbers in Circular Linked List** – [Tricky Modifications]
31. **Flatten a Linked List with Next and Child Pointers** – Recursive & Iterative Approaches
32. **Maximum Twin Sum in a Linked List** – Reverse Half & Sum
33. **Find Median of a Linked List in O(1) Space** – [Two Pointers]
34. **Design a Cache System using Linked List** – [FAANG System Design Challenge]
35. **Convert Binary Tree to Doubly Linked List** – Inorder Traversal

---

## **🛠 Advanced Techniques to Master**

36. **Two Pointers Approach**
    - Detect cycle (Floyd’s Algorithm)
    - Find middle node
    - Find nth node from the end
37. **Stack Usage**
    - Check if linked list is palindrome
    - Reverse in k-groups
38. **HashMap Optimization**
    - Detect loop (HashSet)
    - LRU Cache
39. **Recursion for Complex Reversals**
    - Reverse between M and N
    - Reverse k-groups