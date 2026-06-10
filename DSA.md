# DSA Interview Mastery — NeetCode 150 + Bonus Problems

> **Your complete DSA roadmap.** All 150 NeetCode problems with approach guides, C++ hints, and solution links — plus bonus frequently-asked problems not covered by NeetCode 150.

---

## How to Use This Guide

- **Approach first, code second.** Read the approach, try the problem yourself for 20–30 min, then look at the solution.
- **Difficulty tags:** 🟢 Easy | 🟡 Medium | 🔴 Hard
- **Pattern matters more than the problem.** Once you recognize the pattern, 80% of the problem is solved.
- **C++ tip:** Always prefer `unordered_map` over `map` (O(1) vs O(log n)), `vector` over arrays, and know when to use `auto`.

---

## The 18 Core Patterns (Master These)

```
1. Array / Hash Map tricks
2. Two Pointers
3. Sliding Window
4. Stack (Monotonic Stack)
5. Binary Search (on answer)
6. Linked List (fast/slow pointers)
7. Trees (DFS / BFS)
8. Tries (Prefix Trees)
9. Heap / Priority Queue
10. Backtracking (explore & undo)
11. Graphs (BFS / DFS / Union-Find)
12. Advanced Graphs (Dijkstra, Bellman-Ford, Topological Sort)
13. 1D Dynamic Programming
14. 2D Dynamic Programming (Grid / String)
15. Greedy
16. Intervals
17. Math & Geometry
18. Bit Manipulation
```

---

## Section 1 — Arrays & Hashing

> **Core idea:** Use a hash map (`unordered_map` / `unordered_set`) to reduce O(n²) brute force to O(n). Think: "Can I trade space for time?"

---

### 1. Contains Duplicate 🟢
**Problem:** Given an array, return true if any value appears at least twice.
**Approach:** Insert elements into an `unordered_set`. If insertion fails (element already exists), return true.
**C++ hint:** `unordered_set<int> seen; if (!seen.insert(nums[i]).second) return true;`
**Solution:** https://neetcode.io/solutions/contains-duplicate

---

### 2. Valid Anagram 🟢
**Problem:** Check if two strings are anagrams of each other.
**Approach:** Count character frequencies using `unordered_map<char,int>` or a `int freq[26]` array. Increment for s, decrement for t. If all zeros → anagram.
**C++ hint:** `sort(s.begin(), s.end()) == sort(t.begin(), t.end())` works too but O(n log n).
**Solution:** https://neetcode.io/solutions/valid-anagram

---

### 3. Two Sum 🟢
**Problem:** Find two indices whose values sum to target.
**Approach:** One-pass hash map. For each element, check if `target - nums[i]` already exists in the map. If yes, return indices. If no, store `nums[i] → i`.
**C++ hint:** `unordered_map<int,int> seen; if (seen.count(target - nums[i])) return {seen[target-nums[i]], i};`
**Solution:** https://neetcode.io/solutions/two-sum

---

### 4. Group Anagrams 🟡
**Problem:** Group strings that are anagrams of each other.
**Approach:** For each string, sort it → use sorted string as the key in an `unordered_map<string, vector<string>>`. Group all original strings under the same key.
**C++ hint:** `string key = word; sort(key.begin(), key.end());`
**Solution:** https://neetcode.io/solutions/group-anagrams

---

### 5. Top K Frequent Elements 🟡
**Problem:** Return the k most frequent elements.
**Approach 1 (Bucket Sort):** Count frequencies, then use a bucket array where `bucket[freq]` holds all numbers with that frequency. Scan from high freq to low.
**Approach 2 (Heap):** Min-heap of size k — O(n log k).
**C++ hint:** Bucket sort gives O(n) — preferred.
**Solution:** https://neetcode.io/solutions/top-k-frequent-elements

---

### 6. Encode and Decode Strings 🟡
**Problem:** Design encode/decode functions to serialize a list of strings.
**Approach:** Prefix each string with its length + a delimiter: `"4#neet"`. Decode by reading the number before `#`, then reading exactly that many characters.
**C++ hint:** `encoded += to_string(s.size()) + "#" + s;`
**Solution:** https://neetcode.io/solutions/encode-and-decode-strings

---

### 7. Product of Array Except Self 🟡
**Problem:** Return array where each element is the product of all others — no division.
**Approach:** Two passes. Left pass: `res[i]` = product of all elements to the left. Right pass: multiply by running right product. O(n) time, O(1) extra space.
**C++ hint:** Use the output array itself as the left-product array, then do a single right pass with a variable `right = 1`.
**Solution:** https://neetcode.io/solutions/product-of-array-except-self

---

### 8. Valid Sudoku 🟡
**Problem:** Determine if a 9×9 Sudoku board is valid.
**Approach:** Use three sets — one per row, one per col, one per 3×3 box. For each cell, check if the digit already exists in its row/col/box. Box index = `(row/3)*3 + col/3`.
**C++ hint:** Use `unordered_set<string>` with encoded keys like `"r0:5"`, `"c3:5"`, `"b1:5"`.
**Solution:** https://neetcode.io/solutions/valid-sudoku

---

### 9. Longest Consecutive Sequence 🟡
**Problem:** Find length of the longest consecutive number sequence in O(n).
**Approach:** Put all numbers in an `unordered_set`. For each number, only start counting if `num - 1` is NOT in the set (i.e., it's the start of a sequence). Then count up.
**C++ hint:** `if (!s.count(num - 1)) { int len = 0; while (s.count(num++)) len++; }`
**Solution:** https://neetcode.io/solutions/longest-consecutive-sequence

---

## Section 2 — Two Pointers

> **Core idea:** Use two indices moving toward each other (or in the same direction) to avoid nested loops. Works when the array is sorted or when you need pairs/triplets.

---

### 10. Valid Palindrome 🟢
**Problem:** Check if a string is a palindrome (ignoring non-alphanumeric chars).
**Approach:** Two pointers from both ends. Skip non-alphanumeric chars. Compare `tolower(s[l]) == tolower(s[r])`. Move pointers inward.
**C++ hint:** `while (l < r) { while (!isalnum(s[l])) l++; ... }`
**Solution:** https://neetcode.io/solutions/valid-palindrome

---

### 11. Two Sum II — Input Array Is Sorted 🟡
**Problem:** Find two numbers in a sorted array that sum to target.
**Approach:** Left pointer at 0, right pointer at end. If sum < target → move left right. If sum > target → move right left.
**Solution:** https://neetcode.io/solutions/two-sum-ii-input-array-is-sorted

---

### 12. 3Sum 🟡
**Problem:** Find all unique triplets that sum to zero.
**Approach:** Sort the array. Fix one element, use two pointers for the remaining pair. Skip duplicates for both the fixed element and the pointers.
**C++ hint:** `sort(nums.begin(), nums.end()); for (int i = 0; i < n; i++) { if (i > 0 && nums[i] == nums[i-1]) continue; ... two pointer ... }`
**Solution:** https://neetcode.io/solutions/3sum

---

### 13. Container With Most Water 🟡
**Problem:** Find two lines that form a container holding the most water.
**Approach:** Two pointers. Area = `min(h[l], h[r]) * (r - l)`. Move the pointer pointing to the shorter line inward (moving the taller one can only decrease area).
**Solution:** https://neetcode.io/solutions/container-with-most-water

---

### 14. Trapping Rain Water 🔴
**Problem:** How much water can be trapped between bars?
**Approach:** Two pointers. Maintain `maxLeft` and `maxRight`. Water at position i = `min(maxLeft, maxRight) - height[i]`. Move the side with the smaller max inward.
**C++ hint:** No extra arrays needed — O(1) space.
**Solution:** https://neetcode.io/solutions/trapping-rain-water

---

## Section 3 — Sliding Window

> **Core idea:** Maintain a window [l, r] and expand/shrink it based on a condition. Use a hash map or frequency array to track window contents. Avoid recomputing from scratch.

---

### 15. Best Time to Buy and Sell Stock 🟢
**Problem:** Find max profit from one buy and one sell.
**Approach:** Track `minPrice` so far. For each price, compute `price - minPrice`. Update `maxProfit`. This is technically a sliding window / one-pass greedy.
**Solution:** https://neetcode.io/solutions/best-time-to-buy-and-sell-stock

---

### 16. Longest Substring Without Repeating Characters 🟡
**Problem:** Find length of longest substring with all unique characters.
**Approach:** Sliding window + `unordered_map<char, int>` storing last seen index. If `s[r]` was seen inside the window, move `l` to `lastSeen[s[r]] + 1`. Update max length.
**Solution:** https://neetcode.io/solutions/longest-substring-without-repeating-characters

---

### 17. Longest Repeating Character Replacement 🟡
**Problem:** You can replace at most k characters — find the longest substring with all same characters.
**Approach:** Sliding window. Track frequency of each char in window. Window is valid if `(windowSize - maxFreq) <= k`. If invalid, shrink from left.
**C++ hint:** `int maxFreq = *max_element(count, count+26);` — or maintain it as you go.
**Solution:** https://neetcode.io/solutions/longest-repeating-character-replacement

---

### 18. Permutation in String 🟡
**Problem:** Check if s1's permutation is a substring of s2.
**Approach:** Fixed-size sliding window of length `s1.size()`. Compare frequency arrays of the window vs s1. Use a match counter to avoid full array comparison each step.
**Solution:** https://neetcode.io/solutions/permutation-in-string

---

### 19. Minimum Window Substring 🔴
**Problem:** Find the smallest window in s containing all chars of t.
**Approach:** Sliding window with two freq maps — one for t, one for current window. Track `formed` (how many chars meet required freq). Expand right, shrink left when valid.
**C++ hint:** This is the hardest sliding window — master it fully.
**Solution:** https://neetcode.io/solutions/minimum-window-substring

---

### 20. Sliding Window Maximum 🔴
**Problem:** Return the max in every window of size k.
**Approach:** Monotonic decreasing deque (`deque<int>` storing indices). Remove elements outside the window from front. Remove smaller elements from back before adding new element. Front always holds the window max.
**Solution:** https://neetcode.io/solutions/sliding-window-maximum

---

## Section 4 — Stack

> **Core idea:** Stack for "last in, first out" problems — matching brackets, next greater element, monotonic sequences. Think: "Does the current element affect something I saw earlier?"

---

### 21. Valid Parentheses 🟢
**Problem:** Check if brackets are properly matched and nested.
**Approach:** Push opening brackets onto stack. For closing bracket, check if top of stack is the matching opener.
**C++ hint:** Use a `map` for pairs: `{')','('}, {']','['}, {'}','{'}`
**Solution:** https://neetcode.io/solutions/valid-parentheses

---

### 22. Min Stack 🟡
**Problem:** Stack that supports push, pop, top, and getMin in O(1).
**Approach:** Maintain two stacks — main stack and min stack. Push to min stack only if the new value ≤ current min.
**Solution:** https://neetcode.io/solutions/min-stack

---

### 23. Evaluate Reverse Polish Notation 🟡
**Problem:** Evaluate an expression in postfix notation.
**Approach:** Stack. For each token: if number → push. If operator → pop two operands, apply operator, push result.
**C++ hint:** `stoi(token)` to parse numbers. Handle negative numbers carefully.
**Solution:** https://neetcode.io/solutions/evaluate-reverse-polish-notation

---

### 24. Generate Parentheses 🟡
**Problem:** Generate all valid combinations of n pairs of parentheses.
**Approach:** Backtracking. At each step you can add `(` if `open < n`, or `)` if `close < open`. Base case: `open == close == n`.
**Solution:** https://neetcode.io/solutions/generate-parentheses

---

### 25. Daily Temperatures 🟡
**Problem:** For each day, how many days until a warmer temperature?
**Approach:** Monotonic decreasing stack storing indices. For each temperature, pop all days from stack that are cooler. Answer for each popped day = `i - poppedIndex`.
**Solution:** https://neetcode.io/solutions/daily-temperatures

---

### 26. Car Fleet 🟡
**Problem:** How many car fleets reach the destination?
**Approach:** Sort cars by position (descending). Compute time to reach for each car. Maintain a stack — if a car's time ≤ top of stack, it joins that fleet (same time, dominated). Otherwise it's a new fleet.
**Solution:** https://neetcode.io/solutions/car-fleet

---

### 27. Largest Rectangle in Histogram 🔴
**Problem:** Find the largest rectangle in a histogram.
**Approach:** Monotonic increasing stack (storing indices). When a shorter bar is found, pop taller bars and calculate their maximum rectangle width. Add sentinel `0` at the end.
**C++ hint:** This is a classic — the stack stores indices, not heights.
**Solution:** https://neetcode.io/solutions/largest-rectangle-in-histogram

---

## Section 5 — Binary Search

> **Core idea:** Binary search isn't just for sorted arrays — it works any time you have a monotonic condition (if X is possible, then X-1 is also possible). "Binary search on the answer" is a powerful technique.

---

### 28. Binary Search 🟢
**Problem:** Classic binary search on a sorted array.
**Approach:** `lo = 0, hi = n-1`. While `lo <= hi`: compute `mid = lo + (hi-lo)/2`. Compare `nums[mid]` to target. Adjust `lo` or `hi`.
**C++ hint:** Always use `lo + (hi - lo)/2` to avoid integer overflow.
**Solution:** https://neetcode.io/solutions/binary-search

---

### 29. Search a 2D Matrix 🟡
**Problem:** Search a target in a matrix where each row is sorted and first element of each row > last element of previous row.
**Approach:** Treat the matrix as a flat sorted array. `mid` maps to `matrix[mid/cols][mid%cols]`.
**Solution:** https://neetcode.io/solutions/search-a-2d-matrix

---

### 30. Koko Eating Bananas 🟡
**Problem:** Find minimum eating speed to finish all bananas in h hours.
**Approach:** Binary search on the answer (speed from 1 to max pile). For a given speed k, check if total hours ≤ h. Find the minimum valid k.
**C++ hint:** `hours = ceil(pile / k)` → use `(pile + k - 1) / k` in integer arithmetic.
**Solution:** https://neetcode.io/solutions/koko-eating-bananas

---

### 31. Find Minimum in Rotated Sorted Array 🟡
**Problem:** Find the minimum element in a rotated sorted array.
**Approach:** Binary search. If `nums[mid] > nums[right]`, minimum is in the right half. Otherwise it's in the left half (including mid).
**Solution:** https://neetcode.io/solutions/find-minimum-in-rotated-sorted-array

---

### 32. Search in Rotated Sorted Array 🟡
**Problem:** Search a target in a rotated sorted array.
**Approach:** Binary search. First determine which half is sorted (compare mid to left). Then check if target lies in the sorted half. Adjust pointers accordingly.
**Solution:** https://neetcode.io/solutions/search-in-rotated-sorted-array

---

### 33. Time Based Key-Value Store 🟡
**Problem:** Design a key-value store that retrieves the value at or before a given timestamp.
**Approach:** `unordered_map<string, vector<pair<int,string>>>`. Binary search on the timestamp vector for each key using `upper_bound`.
**Solution:** https://neetcode.io/solutions/time-based-key-value-store

---

### 34. Median of Two Sorted Arrays 🔴
**Problem:** Find the median of two sorted arrays in O(log(m+n)).
**Approach:** Binary search on the smaller array. Partition both arrays such that left half ≤ right half across both. Adjust based on boundary conditions.
**C++ hint:** This is the hardest binary search — study the partitioning logic carefully.
**Solution:** https://neetcode.io/solutions/median-of-two-sorted-arrays

---

## Section 6 — Linked List

> **Core idea:** Most linked list problems use the fast/slow pointer technique or require careful pointer manipulation. Always draw out pointer reassignments before coding.

---

### 35. Reverse Linked List 🟢
**Problem:** Reverse a singly linked list.
**Approach:** Iterative: three pointers — `prev = null, curr = head, next`. Loop: save next, point curr back, advance both. Recursive: reverse(head->next), then head->next->next = head.
**Solution:** https://neetcode.io/solutions/reverse-linked-list

---

### 36. Merge Two Sorted Lists 🟢
**Problem:** Merge two sorted linked lists into one sorted list.
**Approach:** Dummy head. Compare l1 and l2 values, attach smaller node, advance that pointer. After loop, attach the remaining list.
**Solution:** https://neetcode.io/solutions/merge-two-sorted-lists

---

### 37. Reorder List 🟡
**Problem:** Reorder list: 1→2→3→4→5 becomes 1→5→2→4→3.
**Approach:** Three steps: (1) Find middle with slow/fast pointers. (2) Reverse the second half. (3) Merge the two halves by interleaving.
**Solution:** https://neetcode.io/solutions/reorder-list

---

### 38. Remove Nth Node From End of List 🟡
**Problem:** Remove the nth node from the end of a linked list.
**Approach:** Two pointers — advance `fast` by n steps first. Then move both until `fast` hits the end. `slow` is now at the node before the target.
**C++ hint:** Use a dummy head to handle edge case where the head itself needs to be removed.
**Solution:** https://neetcode.io/solutions/remove-nth-node-from-end-of-list

---

### 39. Copy List with Random Pointer 🟡
**Problem:** Deep copy a linked list where each node has a random pointer.
**Approach:** Two passes using `unordered_map<Node*, Node*>`. Pass 1: create all copies (map old → new). Pass 2: assign next and random pointers using the map.
**Solution:** https://neetcode.io/solutions/copy-list-with-random-pointer

---

### 40. Add Two Numbers 🟡
**Problem:** Add two numbers represented as reversed linked lists.
**Approach:** Simulate addition digit by digit with a carry variable. Create a new node for each digit. After the loop, if carry remains, add an extra node.
**Solution:** https://neetcode.io/solutions/add-two-numbers

---

### 41. Linked List Cycle 🟢
**Problem:** Detect if a linked list has a cycle.
**Approach:** Floyd's cycle detection — slow moves 1 step, fast moves 2 steps. If they meet, cycle exists.
**Solution:** https://neetcode.io/solutions/linked-list-cycle

---

### 42. Find the Duplicate Number 🟡
**Problem:** Find the duplicate in an array of n+1 integers (values 1 to n) — no extra space.
**Approach:** Treat the array as a linked list (index → value as next pointer). Use Floyd's cycle detection. The cycle entrance = the duplicate.
**Solution:** https://neetcode.io/solutions/find-the-duplicate-number

---

### 43. LRU Cache 🟡
**Problem:** Design a data structure for a Least Recently Used cache.
**Approach:** `unordered_map<int, list<pair<int,int>>::iterator>` + doubly linked list (`std::list`). The list maintains order (front = most recent). On access/update, move node to front. On eviction, remove from back.
**Solution:** https://neetcode.io/solutions/lru-cache

---

### 44. Merge K Sorted Lists 🔴
**Problem:** Merge k sorted linked lists.
**Approach:** Min-heap (`priority_queue`) of size k, storing (value, list index). Pop min node, add to result, push next node from that list.
**C++ hint:** `priority_queue<pair<int,ListNode*>, vector<...>, greater<...>> pq;`
**Solution:** https://neetcode.io/solutions/merge-k-sorted-lists

---

### 45. Reverse Nodes in k-Group 🔴
**Problem:** Reverse every k nodes of a linked list.
**Approach:** Recursively reverse k nodes at a time. First check if k nodes exist. Reverse k nodes, connect to recursive result.
**Solution:** https://neetcode.io/solutions/reverse-nodes-in-k-group

---

## Section 7 — Trees

> **Core idea:** Most tree problems are solved with DFS (recursion) or BFS (level-order with a queue). Think: "What do I need to know from my children to answer at the parent?"

---

### 46. Invert Binary Tree 🟢
**Problem:** Invert a binary tree (mirror it).
**Approach:** Recursively swap left and right children for every node. One line: `swap(root->left, root->right)`, then recurse.
**Solution:** https://neetcode.io/solutions/invert-binary-tree

---

### 47. Maximum Depth of Binary Tree 🟢
**Problem:** Find the maximum depth of a binary tree.
**Approach:** DFS: `return 1 + max(maxDepth(root->left), maxDepth(root->right))`. Base case: null node returns 0.
**Solution:** https://neetcode.io/solutions/maximum-depth-of-binary-tree

---

### 48. Diameter of Binary Tree 🟢
**Problem:** Find the length of the longest path between any two nodes.
**Approach:** DFS. At each node, diameter through it = `leftDepth + rightDepth`. Track global max. Return `1 + max(left, right)` to parent.
**Solution:** https://neetcode.io/solutions/diameter-of-binary-tree

---

### 49. Balanced Binary Tree 🟢
**Problem:** Check if a binary tree is height-balanced.
**Approach:** DFS returning height. If at any node `|leftHeight - rightHeight| > 1`, return -1 (unbalanced). Parent checks for -1 and propagates.
**Solution:** https://neetcode.io/solutions/balanced-binary-tree

---

### 50. Same Tree 🟢
**Problem:** Check if two binary trees are the same.
**Approach:** Recursively check: both null → true. One null → false. Values differ → false. Recurse on both children.
**Solution:** https://neetcode.io/solutions/same-tree

---

### 51. Subtree of Another Tree 🟢
**Problem:** Check if a tree is a subtree of another.
**Approach:** For every node in the main tree, check `isSameTree(node, subRoot)`. This is O(m*n) — acceptable.
**Solution:** https://neetcode.io/solutions/subtree-of-another-tree

---

### 52. Lowest Common Ancestor of BST 🟡
**Problem:** Find the LCA of two nodes in a BST.
**Approach:** Use BST property. If both p and q < root, go left. If both > root, go right. Otherwise, root is LCA.
**Solution:** https://neetcode.io/solutions/lowest-common-ancestor-of-a-binary-search-tree

---

### 53. Binary Tree Level Order Traversal 🟡
**Problem:** Return nodes level by level.
**Approach:** BFS with a queue. At each level, process all nodes in the queue, collect their values, add their children.
**C++ hint:** `queue<TreeNode*> q; q.push(root); while (!q.empty()) { int size = q.size(); for (int i = 0; i < size; i++) { ... } }`
**Solution:** https://neetcode.io/solutions/binary-tree-level-order-traversal

---

### 54. Binary Tree Right Side View 🟡
**Problem:** Return the values visible from the right side.
**Approach:** BFS level order — add the last node at each level to the result.
**Solution:** https://neetcode.io/solutions/binary-tree-right-side-view

---

### 55. Count Good Nodes in Binary Tree 🟡
**Problem:** Count nodes where no node on the path from root is greater.
**Approach:** DFS passing `maxSoFar`. A node is "good" if `node->val >= maxSoFar`.
**Solution:** https://neetcode.io/solutions/count-good-nodes-in-binary-tree

---

### 56. Validate Binary Search Tree 🟡
**Problem:** Check if a binary tree is a valid BST.
**Approach:** DFS with `(long min, long max)` bounds. Each node must satisfy `min < node->val < max`. Left child: max = node->val. Right child: min = node->val.
**C++ hint:** Use `LONG_MIN` and `LONG_MAX` as initial bounds.
**Solution:** https://neetcode.io/solutions/validate-binary-search-tree

---

### 57. Kth Smallest Element in BST 🟡
**Problem:** Find the kth smallest element in a BST.
**Approach:** Inorder traversal (left → root → right) gives sorted order. Count nodes as you go; return when count == k.
**Solution:** https://neetcode.io/solutions/kth-smallest-element-in-a-bst

---

### 58. Construct Binary Tree from Preorder and Inorder 🟡
**Problem:** Rebuild a binary tree from preorder and inorder traversals.
**Approach:** First element of preorder = root. Find root in inorder — elements to left are left subtree, elements to right are right subtree. Recurse.
**C++ hint:** Use `unordered_map<int,int>` to store inorder indices for O(1) lookup.
**Solution:** https://neetcode.io/solutions/construct-binary-tree-from-preorder-and-inorder-traversal

---

### 59. Binary Tree Maximum Path Sum 🔴
**Problem:** Find the maximum path sum (path can start and end at any node).
**Approach:** DFS. At each node, compute max gain from left and right (use 0 if negative). Update global max with `node->val + leftGain + rightGain`. Return `node->val + max(leftGain, rightGain)` to parent.
**Solution:** https://neetcode.io/solutions/binary-tree-maximum-path-sum

---

### 60. Serialize and Deserialize Binary Tree 🔴
**Problem:** Design encode/decode functions for binary trees.
**Approach:** BFS or preorder DFS. Serialize: store values with null markers (e.g., `"1,2,null,null,3,null,null"`). Deserialize: reconstruct with a queue/index.
**Solution:** https://neetcode.io/solutions/serialize-and-deserialize-binary-tree

---

## Section 8 — Tries

> **Core idea:** A Trie (prefix tree) is a tree where each path from root to a node represents a string prefix. Each node has up to 26 children. Use for prefix/word search problems.

```cpp
struct TrieNode {
    TrieNode* children[26] = {};
    bool isEnd = false;
};
```

---

### 61. Implement Trie (Prefix Tree) 🟡
**Problem:** Implement insert, search, and startsWith.
**Approach:** Each character maps to a child node. `isEnd` marks end of a word. For `search`, traverse and check `isEnd`. For `startsWith`, just traverse.
**Solution:** https://neetcode.io/solutions/implement-trie-prefix-tree

---

### 62. Design Add and Search Words Data Structure 🟡
**Problem:** Like a Trie but with wildcard `.` that matches any character.
**Approach:** Normal Trie insert. For search, DFS recursively — when you see `.`, try all 26 children.
**Solution:** https://neetcode.io/solutions/design-add-and-search-words-data-structure

---

### 63. Word Search II 🔴
**Problem:** Given a board and list of words, find all words in the board.
**Approach:** Build a Trie from all words. DFS on the board — traverse the Trie simultaneously. When `isEnd` is reached, add word to result. Prune by removing found words from Trie.
**Solution:** https://neetcode.io/solutions/word-search-ii

---

## Section 9 — Heap / Priority Queue

> **Core idea:** Use a heap when you need repeated access to the min or max element efficiently. In C++ use `priority_queue<int>` (max-heap) or `priority_queue<int, vector<int>, greater<int>>` (min-heap).

---

### 64. Kth Largest Element in an Array 🟡
**Problem:** Find the kth largest element.
**Approach 1 (Min-Heap):** Maintain a min-heap of size k. If size exceeds k, pop. Top is the kth largest. O(n log k).
**Approach 2 (QuickSelect):** Average O(n) — partition like QuickSort, recurse only on the relevant side.
**Solution:** https://neetcode.io/solutions/kth-largest-element-in-an-array

---

### 65. IPO 🔴
**Problem:** Maximize capital by selecting at most k projects.
**Approach:** Sort projects by capital. Use a max-heap for available profits. Greedily add all affordable projects to heap, pick the most profitable, repeat.
**Solution:** https://neetcode.io/solutions/ipo

---

### 66. Find Median from Data Stream 🔴
**Problem:** Add numbers one by one, find median at any point.
**Approach:** Two heaps — max-heap for lower half, min-heap for upper half. Keep them balanced (size difference ≤ 1). Median = top of larger heap or average of both tops.
**Solution:** https://neetcode.io/solutions/find-median-from-data-stream

---

### 67. Task Scheduler 🟡
**Problem:** Schedule tasks with cooldown n — find minimum time to finish.
**Approach:** Greedy + max-heap. Always process the most frequent task available. During cooldown, keep a queue of cooling tasks. Time advances each cycle.
**Solution:** https://neetcode.io/solutions/task-scheduler

---

### 68. Design Twitter 🟡
**Problem:** Design a simplified Twitter with follow, post, and getNewsFeed.
**Approach:** `unordered_map<int, set<int>> follows` and `unordered_map<int, vector<pair<int,int>>> tweets`. For feed, merge up to 10 most recent tweets from followed users using a max-heap.
**Solution:** https://neetcode.io/solutions/design-twitter

---

### 69. Last Stone Weight 🟢
**Problem:** Smash the two heaviest stones repeatedly.
**Approach:** Max-heap. Pop two, push back the difference if nonzero. Return top or 0.
**Solution:** https://neetcode.io/solutions/last-stone-weight

---

### 70. K Closest Points to Origin 🟡
**Problem:** Find k closest points to the origin.
**Approach:** Max-heap of size k. If size > k, pop. Or use QuickSelect for O(n) average.
**Solution:** https://neetcode.io/solutions/k-closest-points-to-origin

---

## Section 10 — Backtracking

> **Core idea:** Explore all possibilities by making a choice, recursing, then undoing the choice (backtrack). Use when you need all combinations/permutations or when brute force is unavoidable but must be pruned.

```cpp
void backtrack(state) {
    if (base_case) { save result; return; }
    for (each choice) {
        make choice;
        backtrack(new state);
        undo choice;   // ← this is the backtrack
    }
}
```

---

### 71. Subsets 🟡
**Problem:** Generate all subsets of a set of distinct integers.
**Approach:** At each index, decide to include or exclude the element. Recurse for both choices.
**Solution:** https://neetcode.io/solutions/subsets

---

### 72. Combination Sum 🟡
**Problem:** Find all combinations that sum to target (elements can be reused).
**Approach:** Backtracking. At each step, try each element from current index onwards. Reuse by passing same index. Stop when sum exceeds target.
**Solution:** https://neetcode.io/solutions/combination-sum

---

### 73. Permutations 🟡
**Problem:** Generate all permutations of distinct integers.
**Approach:** Backtracking with a `visited` array. At each step, try all unvisited elements.
**C++ hint:** Alternative: use `next_permutation` (not for interviews — use backtracking).
**Solution:** https://neetcode.io/solutions/permutations

---

### 74. Subsets II 🟡
**Problem:** Generate all subsets (input may have duplicates).
**Approach:** Sort first. In the backtracking loop, skip duplicate elements at the same depth level: `if (i > start && nums[i] == nums[i-1]) continue;`
**Solution:** https://neetcode.io/solutions/subsets-ii

---

### 75. Combination Sum II 🟡
**Problem:** Find all combinations (each element used once, no duplicate combos).
**Approach:** Sort + backtrack with same duplicate-skip trick as Subsets II.
**Solution:** https://neetcode.io/solutions/combination-sum-ii

---

### 76. Word Search 🟡
**Problem:** Find if a word exists in a 2D grid of characters.
**Approach:** DFS from every cell. Mark cells as visited before recursing (e.g., temp replace with `#`), restore after.
**C++ hint:** Modifying the grid in-place avoids a visited array.
**Solution:** https://neetcode.io/solutions/word-search

---

### 77. Palindrome Partitioning 🟡
**Problem:** Partition a string so every substring is a palindrome — return all ways.
**Approach:** Backtracking. At each position, try all substrings starting here. If it's a palindrome, add it and recurse from the end of that substring.
**Solution:** https://neetcode.io/solutions/palindrome-partitioning

---

### 78. Letter Combinations of a Phone Number 🟡
**Problem:** Return all possible letter combinations from phone digits.
**Approach:** Backtracking. Map each digit to its letters. At each step, try all letters for current digit.
**Solution:** https://neetcode.io/solutions/letter-combinations-of-a-phone-number

---

### 79. N-Queens 🔴
**Problem:** Place n queens on an n×n chessboard so no two attack each other.
**Approach:** Backtracking row by row. Track occupied columns, diagonals (`col - row`), and anti-diagonals (`col + row`) using sets.
**Solution:** https://neetcode.io/solutions/n-queens

---

### 80. N-Queens II 🔴
**Problem:** Count the number of distinct N-Queens solutions.
**Approach:** Same as N-Queens but count instead of record solutions.
**Solution:** https://leetcode.com/problems/n-queens-ii/

---

## Section 11 — Graphs

> **Core idea:** Represent as adjacency list. DFS for exploration/connectivity. BFS for shortest paths in unweighted graphs. Union-Find for connectivity queries. Topological Sort for dependency ordering.

---

### 81. Number of Islands 🟡
**Problem:** Count islands in a 2D binary grid.
**Approach:** DFS/BFS from each unvisited `'1'`. Mark all connected land as visited. Each DFS call = one island.
**Solution:** https://neetcode.io/solutions/number-of-islands

---

### 82. Max Area of Island 🟡
**Problem:** Find the largest island.
**Approach:** DFS returning the size of each island. Track global max.
**Solution:** https://neetcode.io/solutions/max-area-of-island

---

### 83. Clone Graph 🟡
**Problem:** Deep copy a graph.
**Approach:** DFS/BFS with `unordered_map<Node*, Node*>` mapping old to new nodes. Before recursing on neighbors, create the clone first (to handle cycles).
**Solution:** https://neetcode.io/solutions/clone-graph

---

### 84. Walls and Gates 🟡
**Problem:** Fill each empty room with distance to nearest gate.
**Approach:** Multi-source BFS starting from all gates simultaneously. BFS guarantees minimum distance.
**Solution:** https://neetcode.io/solutions/walls-and-gates

---

### 85. Rotting Oranges 🟡
**Problem:** How many minutes until no fresh orange remains?
**Approach:** Multi-source BFS from all initially rotten oranges. Track time (BFS level = minute). Count remaining fresh oranges.
**Solution:** https://neetcode.io/solutions/rotting-oranges

---

### 86. Pacific Atlantic Water Flow 🟡
**Problem:** Find cells from which water can flow to both oceans.
**Approach:** Reverse thinking — do BFS/DFS backwards from both oceans (uphill flow). Find cells reachable from both.
**Solution:** https://neetcode.io/solutions/pacific-atlantic-water-flow

---

### 87. Surrounded Regions 🟡
**Problem:** Capture all regions not connected to the border.
**Approach:** DFS/BFS from all border `O` cells, mark as safe. Then flip unmarked `O` to `X`.
**Solution:** https://neetcode.io/solutions/surrounded-regions

---

### 88. Course Schedule 🟡
**Problem:** Detect if a cycle exists in a directed graph (can finish all courses?).
**Approach:** DFS with 3 states per node: 0 = unvisited, 1 = visiting (in current path), 2 = visited. If you reach a node with state 1 → cycle.
**Solution:** https://neetcode.io/solutions/course-schedule

---

### 89. Course Schedule II 🟡
**Problem:** Return a valid course order (topological sort).
**Approach:** Kahn's Algorithm (BFS) — compute in-degrees, start from 0 in-degree nodes, reduce neighbors' in-degrees. Or DFS postorder.
**Solution:** https://neetcode.io/solutions/course-schedule-ii

---

### 90. Graph Valid Tree 🟡
**Problem:** Check if a graph is a valid tree (connected + no cycles).
**Approach:** A valid tree has exactly `n-1` edges and is connected. Use Union-Find or DFS. If DFS visits all nodes with no back edges → valid tree.
**Solution:** https://neetcode.io/solutions/graph-valid-tree

---

### 91. Number of Connected Components 🟡
**Problem:** Count connected components in undirected graph.
**Approach:** Union-Find or DFS/BFS. Each unvisited node starts a new component.
**Solution:** https://neetcode.io/solutions/number-of-connected-components-in-an-undirected-graph

---

### 92. Redundant Connection 🟡
**Problem:** Find the edge that creates a cycle.
**Approach:** Union-Find. Process edges one by one. If both endpoints already in the same component → that edge is redundant.
**Solution:** https://neetcode.io/solutions/redundant-connection

---

### 93. Word Ladder 🔴
**Problem:** Minimum transformations from beginWord to endWord (changing one letter at a time).
**Approach:** BFS. From each word, try changing each character to a-z. If the new word is in the word set, add to queue. Remove words from set as visited.
**C++ hint:** Use `unordered_set<string>` for O(1) lookup. Generate neighbors by swapping each char.
**Solution:** https://neetcode.io/solutions/word-ladder

---

## Section 12 — Advanced Graphs

> **Core idea:** Dijkstra for weighted shortest paths, Bellman-Ford for negative weights, Prim's/Kruskal's for MST, Floyd-Warshall for all-pairs shortest paths.

---

### 94. Reconstruct Itinerary 🔴
**Problem:** Reconstruct flight itinerary using all tickets exactly once.
**Approach:** Hierholzer's algorithm for Eulerian path. DFS with sorted adjacency list (use `multiset` or sorted vector). Append to result in postorder (reverse at end).
**Solution:** https://neetcode.io/solutions/reconstruct-itinerary

---

### 95. Min Cost to Connect All Points 🟡
**Problem:** Find MST — connect all points with minimum total Manhattan distance.
**Approach:** Prim's algorithm with a min-heap. Start from any node, greedily add the cheapest edge to an unvisited node.
**Solution:** https://neetcode.io/solutions/min-cost-to-connect-all-points

---

### 96. Network Delay Time 🟡
**Problem:** Find how long for signal to reach all nodes (weighted directed graph).
**Approach:** Dijkstra's algorithm from source node. Return max of all shortest distances. If any node unreachable, return -1.
**C++ hint:** `priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;`
**Solution:** https://neetcode.io/solutions/network-delay-time

---

### 97. Swim in Rising Water 🔴
**Problem:** Find minimum time to travel from (0,0) to (n-1,n-1).
**Approach:** Dijkstra-like with min-heap — minimize the maximum elevation on the path. Or binary search + BFS/DFS.
**Solution:** https://neetcode.io/solutions/swim-in-rising-water

---

### 98. Alien Dictionary 🔴
**Problem:** Determine character order from sorted alien words.
**Approach:** Build a directed graph from adjacent word pairs (where they first differ). Topological sort. If cycle exists → invalid.
**Solution:** https://neetcode.io/solutions/foreign-dictionary

---

### 99. Cheapest Flights Within K Stops 🟡
**Problem:** Shortest path with at most k stops.
**Approach:** Bellman-Ford with k+1 iterations. Can't use standard Dijkstra (stop constraint). Make a copy of distances at start of each iteration.
**Solution:** https://neetcode.io/solutions/cheapest-flights-within-k-stops

---

## Section 13 — 1D Dynamic Programming

> **Core idea:** Break problem into subproblems. Use a `dp` array where `dp[i]` represents the answer for input of size i. Find the recurrence relation. Usually: `dp[i]` depends on `dp[i-1]`, `dp[i-2]`, etc.

---

### 100. Climbing Stairs 🟢
**Problem:** Count ways to climb n stairs (1 or 2 steps at a time).
**Approach:** Fibonacci — `dp[i] = dp[i-1] + dp[i-2]`. Just use two variables.
**Solution:** https://neetcode.io/solutions/climbing-stairs

---

### 101. Min Cost Climbing Stairs 🟢
**Problem:** Minimum cost to reach the top (can step 1 or 2).
**Approach:** `dp[i] = cost[i] + min(dp[i-1], dp[i-2])`. Answer = `min(dp[n-1], dp[n-2])`.
**Solution:** https://neetcode.io/solutions/min-cost-climbing-stairs

---

### 102. House Robber 🟡
**Problem:** Max money you can rob from non-adjacent houses.
**Approach:** `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`. Use two variables to save space.
**Solution:** https://neetcode.io/solutions/house-robber

---

### 103. House Robber II 🟡
**Problem:** Houses in a circle — can't rob first and last together.
**Approach:** Run House Robber twice — once on `nums[0..n-2]` and once on `nums[1..n-1]`. Take the max.
**Solution:** https://neetcode.io/solutions/house-robber-ii

---

### 104. Longest Palindromic Substring 🟡
**Problem:** Find the longest palindromic substring.
**Approach:** Expand around center for every possible center (2n-1 centers). O(n²) time, O(1) space.
**Solution:** https://neetcode.io/solutions/longest-palindromic-substring

---

### 105. Palindromic Substrings 🟡
**Problem:** Count all palindromic substrings.
**Approach:** Same expand-around-center technique. Count while expanding.
**Solution:** https://neetcode.io/solutions/palindromic-substrings

---

### 106. Decode Ways 🟡
**Problem:** Count ways to decode a digit string to letters (1=A, 26=Z).
**Approach:** DP. `dp[i]` = ways to decode `s[0..i-1]`. If `s[i-1]` is valid single digit, add `dp[i-1]`. If `s[i-2..i-1]` forms a valid two-digit (10-26), add `dp[i-2]`.
**Solution:** https://neetcode.io/solutions/decode-ways

---

### 107. Coin Change 🟡
**Problem:** Minimum coins needed to make amount.
**Approach:** Unbounded knapsack DP. `dp[i] = min over all coins c: dp[i-c] + 1`. Init `dp[0]=0`, rest as infinity.
**Solution:** https://neetcode.io/solutions/coin-change

---

### 108. Maximum Product Subarray 🟡
**Problem:** Find the subarray with the maximum product.
**Approach:** Track both `maxProd` and `minProd` (because a negative × negative = large positive). At each step: `maxProd = max(nums[i], maxProd*nums[i], minProd*nums[i])`.
**Solution:** https://neetcode.io/solutions/maximum-product-subarray

---

### 109. Word Break 🟡
**Problem:** Check if a string can be segmented using words from a dictionary.
**Approach:** `dp[i]` = can we form `s[0..i-1]`. For each position j < i, if `dp[j]` is true and `s[j..i-1]` is in the dict, set `dp[i] = true`.
**Solution:** https://neetcode.io/solutions/word-break

---

### 110. Longest Increasing Subsequence 🟡
**Problem:** Find the length of the longest strictly increasing subsequence.
**Approach 1 (O(n²) DP):** `dp[i] = max(dp[j]+1)` for all j < i where `nums[j] < nums[i]`.
**Approach 2 (O(n log n) Patience Sort):** Maintain a `tails` array. Binary search for each element's position.
**Solution:** https://neetcode.io/solutions/longest-increasing-subsequence

---

### 111. Partition Equal Subset Sum 🟡
**Problem:** Can the array be partitioned into two equal-sum subsets?
**Approach:** 0/1 Knapsack. Target = total/2. `dp[j]` = can we reach sum j. For each number, update dp backwards: `dp[j] |= dp[j-num]`.
**Solution:** https://neetcode.io/solutions/partition-equal-subset-sum

---

## Section 14 — 2D Dynamic Programming

> **Core idea:** `dp[i][j]` represents the answer for a subproblem involving the first i elements of one sequence and first j elements of another (or a grid position). Fill the table bottom-up.

---

### 112. Unique Paths 🟡
**Problem:** Count paths from top-left to bottom-right moving only right/down.
**Approach:** `dp[i][j] = dp[i-1][j] + dp[i][j-1]`. Can be compressed to 1D.
**Solution:** https://neetcode.io/solutions/unique-paths

---

### 113. Longest Common Subsequence 🟡
**Problem:** Find LCS length of two strings.
**Approach:** `dp[i][j]` = LCS of `s1[0..i-1]` and `s2[0..j-1]`. If chars match: `dp[i][j] = dp[i-1][j-1] + 1`. Else: `max(dp[i-1][j], dp[i][j-1])`.
**Solution:** https://neetcode.io/solutions/longest-common-subsequence

---

### 114. Best Time to Buy and Sell Stock with Cooldown 🟡
**Problem:** Max profit with cooldown (can't buy day after sell).
**Approach:** States: `held`, `sold`, `rest`. Transitions: `held = max(held, rest - price)`, `sold = held + price`, `rest = max(rest, sold)`.
**Solution:** https://neetcode.io/solutions/best-time-to-buy-and-sell-stock-with-cooldown

---

### 115. Coin Change II 🟡
**Problem:** Count number of ways to make amount using coins.
**Approach:** Unbounded knapsack counting. `dp[j] += dp[j - coin]` for each coin. Order: coins outer, amounts inner (for combinations, not permutations).
**Solution:** https://neetcode.io/solutions/coin-change-ii

---

### 116. Target Sum 🟡
**Problem:** Assign + or - to each number; count ways to reach target.
**Approach:** Reduce to subset sum. Knapsack DP with a hash map counting ways.
**Solution:** https://neetcode.io/solutions/target-sum

---

### 117. Interleaving String 🟡
**Problem:** Check if s3 is formed by interleaving s1 and s2.
**Approach:** `dp[i][j]` = can we form `s3[0..i+j-1]` using `s1[0..i-1]` and `s2[0..j-1]`.
**Solution:** https://neetcode.io/solutions/interleaving-string

---

### 118. Longest Increasing Path in a Matrix 🔴
**Problem:** Find the longest strictly increasing path in a matrix.
**Approach:** DFS + memoization. `dp[i][j]` = longest path starting at (i,j). No cycle issues since path is strictly increasing.
**Solution:** https://neetcode.io/solutions/longest-increasing-path-in-a-matrix

---

### 119. Distinct Subsequences 🔴
**Problem:** Count distinct subsequences of s that equal t.
**Approach:** `dp[i][j]` = ways to form `t[0..j-1]` from `s[0..i-1]`. If chars match: `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`. Else: `dp[i-1][j]`.
**Solution:** https://neetcode.io/solutions/distinct-subsequences

---

### 120. Edit Distance 🟡
**Problem:** Minimum operations (insert/delete/replace) to convert s1 to s2.
**Approach:** Classic DP. `dp[i][j]` = edit distance for `s1[0..i-1]` and `s2[0..j-1]`. If chars match: `dp[i-1][j-1]`. Else: `1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`.
**Solution:** https://neetcode.io/solutions/edit-distance

---

### 121. Burst Balloons 🔴
**Problem:** Burst balloons to maximize coins collected.
**Approach:** Interval DP. Instead of "which to burst first", think "which to burst last" in an interval. `dp[l][r]` = max coins from bursting all balloons in (l,r).
**Solution:** https://neetcode.io/solutions/burst-balloons

---

### 122. Regular Expression Matching 🔴
**Problem:** Implement regex matching with `.` and `*`.
**Approach:** 2D DP. Handle `*` by: use it 0 times (`dp[i][j-2]`) or use it to extend (`dp[i-1][j]` if pattern char matches).
**Solution:** https://neetcode.io/solutions/regular-expression-matching

---

## Section 15 — Greedy

> **Core idea:** Make the locally optimal choice at each step. Prove (or convince yourself) that local optimum leads to global optimum. Often involves sorting first.

---

### 123. Maximum Subarray (Kadane's Algorithm) 🟡
**Problem:** Find the contiguous subarray with the largest sum.
**Approach:** Track `currSum` — if it goes negative, reset to 0. Update `maxSum` at each step.
**Solution:** https://neetcode.io/solutions/maximum-subarray

---

### 124. Jump Game 🟡
**Problem:** Can you reach the last index?
**Approach:** Track `maxReach`. For each index, if `i > maxReach` → stuck. Else update `maxReach = max(maxReach, i + nums[i])`.
**Solution:** https://neetcode.io/solutions/jump-game

---

### 125. Jump Game II 🟡
**Problem:** Minimum jumps to reach the last index.
**Approach:** Greedy with current jump range and next jump range. When you reach the end of current range, increment jumps and expand to next range.
**Solution:** https://neetcode.io/solutions/jump-game-ii

---

### 126. Gas Station 🟡
**Problem:** Find the starting gas station to complete a circuit.
**Approach:** If total gas ≥ total cost, a solution exists. Greedy: track running sum. If it goes negative, reset start to next station.
**Solution:** https://neetcode.io/solutions/gas-station

---

### 127. Hand of Straights 🟡
**Problem:** Check if cards can be grouped into consecutive groups of size w.
**Approach:** Sort (or use ordered map). Greedily form groups starting from the smallest card. Reduce frequencies.
**Solution:** https://neetcode.io/solutions/hand-of-straights

---

### 128. Merge Triplets to Form Target Triplet 🟡
**Problem:** Select triplets to form a target triplet by taking element-wise max.
**Approach:** Filter out triplets that exceed target in any dimension. Check if the remaining triplets can produce the target.
**Solution:** https://neetcode.io/solutions/merge-triplets-to-form-target-triplet

---

### 129. Partition Labels 🟡
**Problem:** Partition string so each letter appears in at most one part.
**Approach:** For each char, record its last occurrence. Greedily extend current partition's end to include last occurrences of all chars seen so far.
**Solution:** https://neetcode.io/solutions/partition-labels

---

### 130. Valid Parenthesis String 🟡
**Problem:** `(`, `)`, `*` (can be `(`, `)`, or empty) — check validity.
**Approach:** Track range `[minOpen, maxOpen]`. `(` increments both, `)` decrements both, `*` does `min-1, max+1`. Invalid if `maxOpen < 0`.
**Solution:** https://neetcode.io/solutions/valid-parenthesis-string

---

## Section 16 — Intervals

> **Core idea:** Sort by start time. Merge by checking overlap with previous interval. For scheduling, sorting by end time is often optimal (greedy).

---

### 131. Insert Interval 🟡
**Problem:** Insert a new interval into sorted non-overlapping intervals.
**Approach:** Add all intervals ending before new one. Merge all overlapping. Add remaining.
**Solution:** https://neetcode.io/solutions/insert-interval

---

### 132. Merge Intervals 🟡
**Problem:** Merge all overlapping intervals.
**Approach:** Sort by start. Iterate — if current start ≤ last merged end, merge (extend end). Else add a new interval.
**Solution:** https://neetcode.io/solutions/merge-intervals

---

### 133. Non-Overlapping Intervals 🟡
**Problem:** Minimum intervals to remove so none overlap.
**Approach:** Sort by end time. Greedily keep intervals with earliest end. Count those removed (= those that overlap with the last kept).
**Solution:** https://neetcode.io/solutions/non-overlapping-intervals

---

### 134. Meeting Rooms 🟢
**Problem:** Can one person attend all meetings (no overlaps)?
**Approach:** Sort by start time. Check if any adjacent pair overlaps (`meetings[i].start < meetings[i-1].end`).
**Solution:** https://neetcode.io/solutions/meeting-schedule

---

### 135. Meeting Rooms II 🟡
**Problem:** Minimum number of conference rooms needed.
**Approach:** Min-heap of end times. Sort by start time. For each meeting, if its start ≥ heap top (earliest end), reuse that room (pop then push new end). Else add new room.
**Solution:** https://neetcode.io/solutions/meeting-schedule-ii

---

### 136. Minimum Interval to Include Each Query 🔴
**Problem:** For each query, find the smallest interval containing it.
**Approach:** Sort intervals and queries. Process queries in sorted order. Use min-heap sorted by interval size. Add all intervals starting ≤ query, remove expired ones, answer is heap top.
**Solution:** https://neetcode.io/solutions/minimum-interval-to-include-each-query

---

## Section 17 — Math & Geometry

---

### 137. Rotate Image 🟡
**Problem:** Rotate an n×n matrix 90° clockwise in-place.
**Approach:** Transpose (swap `matrix[i][j]` with `matrix[j][i]`), then reverse each row.
**Solution:** https://neetcode.io/solutions/rotate-image

---

### 138. Spiral Matrix 🟡
**Problem:** Return elements in spiral order.
**Approach:** Maintain 4 boundaries (top, bottom, left, right). Peel off one layer at a time — go right, down, left, up. Shrink boundaries after each direction.
**Solution:** https://neetcode.io/solutions/spiral-matrix

---

### 139. Set Matrix Zeroes 🟡
**Problem:** If an element is 0, set its entire row and column to 0 (in-place).
**Approach:** Use first row and column as flags. Record which rows/cols need zeroing. Apply zeroes last (handle first row/col separately).
**Solution:** https://neetcode.io/solutions/set-matrix-zeroes

---

### 140. Happy Number 🟢
**Problem:** Is a number "happy" (sum of squares of digits eventually reaches 1)?
**Approach:** Floyd's cycle detection. If you ever see 1 → happy. If you hit a cycle (not 1) → not happy.
**Solution:** https://neetcode.io/solutions/happy-number

---

### 141. Plus One 🟢
**Problem:** Add 1 to a number represented as a digit array.
**Approach:** Start from the end. If digit < 9, increment and return. Else set to 0 and carry. If all 9s, prepend a 1.
**Solution:** https://neetcode.io/solutions/plus-one

---

### 142. Pow(x, n) 🟡
**Problem:** Implement `pow(x, n)` efficiently.
**Approach:** Fast exponentiation (binary exponentiation). If n is even: `pow(x*x, n/2)`. If odd: `x * pow(x*x, n/2)`. Handle negative n.
**C++ hint:** O(log n) — must know this for interviews.
**Solution:** https://neetcode.io/solutions/pow-x-n

---

### 143. Multiply Strings 🟡
**Problem:** Multiply two non-negative integer strings without converting to int.
**Approach:** Simulate long multiplication. `result[i+j+1] += (a[i]-'0') * (b[j]-'0')`. Handle carries backward.
**Solution:** https://neetcode.io/solutions/multiply-strings

---

### 144. Detect Squares 🟡
**Problem:** Detect axis-aligned squares from a stream of points.
**Approach:** For each query point, find a diagonal opposite, then check for the other two corners using stored point counts.
**Solution:** https://neetcode.io/solutions/detect-squares

---

## Section 18 — Bit Manipulation

> **Core idea:** XOR (`^`), AND (`&`), OR (`|`), NOT (`~`), left shift (`<<`), right shift (`>>`). Key trick: XOR of a number with itself = 0. XOR with 0 = itself.

---

### 145. Single Number 🟢
**Problem:** Find the one element that doesn't appear twice.
**Approach:** XOR all elements. Pairs cancel out (a^a=0). Result is the single number.
**Solution:** https://neetcode.io/solutions/single-number

---

### 146. Number of 1 Bits 🟢
**Problem:** Count the number of 1 bits (Hamming weight).
**Approach:** `while (n) { count += n & 1; n >>= 1; }`. Or Kernighan's trick: `n &= (n-1)` clears the lowest set bit.
**Solution:** https://neetcode.io/solutions/number-of-1-bits

---

### 147. Counting Bits 🟢
**Problem:** For every number from 0 to n, count its 1 bits.
**Approach:** DP. `dp[i] = dp[i >> 1] + (i & 1)`. The number of bits in i = bits in i/2 + the last bit.
**Solution:** https://neetcode.io/solutions/counting-bits

---

### 148. Reverse Bits 🟢
**Problem:** Reverse bits of a 32-bit unsigned integer.
**Approach:** Iterate 32 times. Extract last bit of n, add to result shifted left, right shift n.
**Solution:** https://neetcode.io/solutions/reverse-bits

---

### 149. Missing Number 🟢
**Problem:** Find the missing number in [0, n].
**Approach 1 (XOR):** XOR all indices with all values — missing number remains.
**Approach 2 (Math):** `n*(n+1)/2 - sum(nums)`.
**Solution:** https://neetcode.io/solutions/missing-number

---

### 150. Sum of Two Integers 🟡
**Problem:** Add two integers without `+` or `-`.
**Approach:** `a + b = (a XOR b) + carry`. Carry = `(a AND b) << 1`. Repeat until carry = 0.
**C++ hint:** Handle negative numbers carefully — use unsigned arithmetic or mask.
**Solution:** https://neetcode.io/solutions/sum-of-two-integers

---

---

# 🎯 Bonus — Frequently Asked DSA Problems NOT in NeetCode 150

> These are high-frequency interview problems that appear at FAANG/MAANG companies and top product firms but are not part of the NeetCode 150 list.

---

## B1 — Arrays & Sorting

### B1.1 — Sort Colors (Dutch National Flag) 🟡
**Problem:** Sort an array of 0s, 1s, and 2s in a single pass.
**Approach:** Three pointers: `low`, `mid`, `high`. Invariant: `[0,low)` = 0s, `[low,mid)` = 1s, `(high,n)` = 2s. Swap based on `nums[mid]`.
**LeetCode:** https://leetcode.com/problems/sort-colors/

---

### B1.2 — Count Inversions 🟡
**Problem:** Count the number of inversions in an array (pairs where i < j but arr[i] > arr[j]).
**Approach:** Modified Merge Sort. Count cross-inversions during the merge step.
**C++ hint:** Every time a right-side element is placed before left-side elements, those are inversions.
**LeetCode:** https://leetcode.com/problems/count-of-smaller-numbers-after-self/

---

### B1.3 — First Missing Positive 🔴
**Problem:** Find the smallest missing positive integer in O(n) time, O(1) space.
**Approach:** Use the array as a hash map. Place each number at its correct index (`nums[i]` should be at index `nums[i]-1`). Then scan for the first index where `nums[i] != i+1`.
**LeetCode:** https://leetcode.com/problems/first-missing-positive/

---

### B1.4 — Next Permutation 🟡
**Problem:** Rearrange numbers to the next lexicographically greater permutation.
**Approach:** (1) Find the rightmost ascending adjacent pair (i, i+1) where `nums[i] < nums[i+1]`. (2) Find the smallest element to the right of i that is greater than `nums[i]`, swap them. (3) Reverse the suffix after index i.
**LeetCode:** https://leetcode.com/problems/next-permutation/

---

### B1.5 — Majority Element 🟢
**Problem:** Find the element appearing more than n/2 times.
**Approach:** Boyer-Moore Voting Algorithm. Maintain a `candidate` and `count`. Increment if same, decrement if different, replace if 0.
**LeetCode:** https://leetcode.com/problems/majority-element/

---

## B2 — Strings

### B2.1 — Longest Valid Parentheses 🔴
**Problem:** Find the length of the longest valid parentheses substring.
**Approach:** Stack storing indices. Push `-1` as base. For each `)`, pop — if stack empty push current index (new base), else `result = max(result, i - stack.top())`.
**LeetCode:** https://leetcode.com/problems/longest-valid-parentheses/

---

### B2.2 — Minimum Number of Brackets to Add 🟡
**Problem:** Minimum additions to make a parentheses string valid.
**Approach:** Track `open` count. For `(` increment open. For `)` if open > 0 decrement, else increment result. Final result += open.
**LeetCode:** https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/

---

### B2.3 — Roman to Integer / Integer to Roman 🟢/🟡
**Problem:** Convert between Roman numerals and integers.
**Approach:** Map each symbol to value. For Roman→Int, if current < next, subtract; else add. For Int→Roman, greedily subtract largest symbol values.
**LeetCode:** https://leetcode.com/problems/roman-to-integer/

---

### B2.4 — Zigzag Conversion 🟡
**Problem:** Write a string in zigzag pattern across numRows rows.
**Approach:** Simulate with `numRows` strings. Track current row and direction. Append each char to its row's string.
**LeetCode:** https://leetcode.com/problems/zigzag-conversion/

---

## B3 — Linked List

### B3.1 — Flatten a Multilevel Doubly Linked List 🟡
**Problem:** Flatten a multilevel doubly linked list (nodes can have child pointers).
**Approach:** DFS/Stack. When a node has a child, insert the child list between current and next. Connect pointers carefully.
**LeetCode:** https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/

---

### B3.2 — Odd Even Linked List 🟡
**Problem:** Group odd-indexed nodes before even-indexed nodes.
**Approach:** Two pointers — `odd` and `even`. Connect all odd nodes, then connect the end of odd chain to the head of even chain.
**LeetCode:** https://leetcode.com/problems/odd-even-linked-list/

---

### B3.3 — Swap Nodes in Pairs 🟡
**Problem:** Swap every two adjacent nodes.
**Approach:** Iterative with dummy head, or recursive. For each pair, swap pointers.
**LeetCode:** https://leetcode.com/problems/swap-nodes-in-pairs/

---

## B4 — Trees

### B4.1 — Check if a Binary Tree is Symmetric 🟢
**Problem:** Is a binary tree a mirror of itself?
**Approach:** Recursive helper `isMirror(left, right)`. Both null → true. One null → false. Check `left->val == right->val` and recurse on `(left->left, right->right)` and `(left->right, right->left)`.
**LeetCode:** https://leetcode.com/problems/symmetric-tree/

---

### B4.2 — Path Sum II 🟡
**Problem:** Find all root-to-leaf paths summing to target.
**Approach:** Backtracking DFS. Add to path, recurse, pop from path.
**LeetCode:** https://leetcode.com/problems/path-sum-ii/

---

### B4.3 — Morris Inorder Traversal 🟡
**Problem:** Inorder traversal of binary tree in O(1) space (no stack/recursion).
**Approach:** Morris traversal — use the rightmost node of left subtree to create temporary thread back to current node. Key interview topic at senior levels.
**LeetCode:** https://leetcode.com/problems/binary-tree-inorder-traversal/

---

### B4.4 — Vertical Order Traversal 🔴
**Problem:** Print binary tree in vertical order.
**Approach:** BFS/DFS storing `(node, col, row)`. Group by column, sort within column by row, then value. Use `map<int, map<int, multiset<int>>>`.
**LeetCode:** https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/

---

## B5 — Heap & Priority Queue

### B5.1 — Reorganize String 🟡
**Problem:** Rearrange string so no two adjacent characters are the same.
**Approach:** Max-heap by frequency. Each step: pop most frequent, append, hold the previous char (cool down one step), push it back.
**LeetCode:** https://leetcode.com/problems/reorganize-string/

---

### B5.2 — Find K Pairs with Smallest Sums 🟡
**Problem:** Find k pairs (u,v) with smallest sums from two sorted arrays.
**Approach:** Min-heap initialized with `(nums1[i], nums2[0])` for all i. Pop min pair, push next pair from nums2.
**LeetCode:** https://leetcode.com/problems/find-k-pairs-with-smallest-sums/

---

## B6 — Graphs

### B6.1 — Detect Cycle in Undirected Graph 🟡
**Problem:** Detect if an undirected graph has a cycle.
**Approach:** DFS tracking parent node. If you visit an already-visited node that isn't the parent → cycle. Union-Find also works cleanly.
**LeetCode:** https://leetcode.com/problems/graph-valid-tree/ *(same problem)*

---

### B6.2 — Flood Fill 🟢
**Problem:** Perform a flood fill on an image from a starting pixel.
**Approach:** DFS/BFS from start pixel. Change color if it matches the original color. Skip if already the new color.
**LeetCode:** https://leetcode.com/problems/flood-fill/

---

### B6.3 — Find if Path Exists in Graph 🟢
**Problem:** Check if a path exists between source and destination.
**Approach:** Union-Find or BFS/DFS. Simple connectivity check.
**LeetCode:** https://leetcode.com/problems/find-if-path-exists-in-graph/

---

### B6.4 — Minimum Spanning Tree (Kruskal's) 🟡
**Problem:** Find MST using Kruskal's algorithm.
**Approach:** Sort edges by weight. Use Union-Find to add edges that don't create cycles. Stop when n-1 edges are added.
**LeetCode:** https://leetcode.com/problems/min-cost-to-connect-all-points/ *(Prim's/Kruskal's both work)*

---

## B7 — Dynamic Programming (Extra)

### B7.1 — Jump Game III 🟡
**Problem:** Can you reach any index with value 0? (From index i, jump i±arr[i])
**Approach:** BFS/DFS. Visit reachable indices. Mark visited to avoid cycles.
**LeetCode:** https://leetcode.com/problems/jump-game-iii/

---

### B7.2 — Maximal Square 🟡
**Problem:** Find the largest square of 1s in a binary matrix.
**Approach:** DP. `dp[i][j]` = side of largest square with `(i,j)` as bottom-right corner. `dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1` if `matrix[i][j] == '1'`.
**LeetCode:** https://leetcode.com/problems/maximal-square/

---

### B7.3 — Maximum Sum of Non-Adjacent Elements (Staircase DP) 🟡
**Problem:** Max sum picking no two adjacent elements — classic House Robber pattern.
**Approach:** Same as House Robber. Know this pattern cold.
**LeetCode:** https://leetcode.com/problems/house-robber/

---

### B7.4 — Wildcard Matching 🔴
**Problem:** Match pattern with `?` (any single char) and `*` (any sequence including empty).
**Approach:** 2D DP. `dp[i][j]` = does `s[0..i-1]` match `p[0..j-1]`. For `*`: `dp[i][j] = dp[i-1][j] || dp[i][j-1]`. For `?` or match: `dp[i-1][j-1]`.
**LeetCode:** https://leetcode.com/problems/wildcard-matching/

---

### B7.5 — Matrix Chain Multiplication / Minimum Path Sum 🟡
**Problem:** Minimum path sum from top-left to bottom-right in a grid.
**Approach:** `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`. Classic 2D DP warmup.
**LeetCode:** https://leetcode.com/problems/minimum-path-sum/

---

## B8 — Math & Classic Problems

### B8.1 — Find Peak Element 🟡
**Problem:** Find a peak element (greater than neighbors). Multiple peaks may exist.
**Approach:** Binary search. If `nums[mid] < nums[mid+1]`, peak is to the right. Else it's to the left or at mid.
**LeetCode:** https://leetcode.com/problems/find-peak-element/

---

### B8.2 — Excel Sheet Column Number 🟢
**Problem:** Convert Excel column title to its column number (A=1, Z=26, AA=27).
**Approach:** Treat as base-26. `result = result * 26 + (c - 'A' + 1)`.
**LeetCode:** https://leetcode.com/problems/excel-sheet-column-number/

---

### B8.3 — Reverse Integer 🟡
**Problem:** Reverse digits of an integer (handle overflow).
**Approach:** Pop last digit each iteration (`n % 10`), add to reversed. Check overflow before adding: `if (rev > INT_MAX/10 || rev < INT_MIN/10) return 0`.
**LeetCode:** https://leetcode.com/problems/reverse-integer/

---

### B8.4 — GCD and LCM Applications 🟢
**Problem:** Many interview problems reduce to GCD/LCM — know `__gcd(a, b)` in C++.
**C++ hint:** `#include <algorithm>` then `__gcd(a, b)`. LCM = `a / __gcd(a,b) * b`.

---

---

# 📋 Master Reference

## Topic → Problem Count

| Topic | NeetCode 150 | Bonus |
|---|---|---|
| Arrays & Hashing | 9 | 5 |
| Two Pointers | 5 | — |
| Sliding Window | 6 | — |
| Stack | 7 | 2 |
| Binary Search | 7 | 1 |
| Linked List | 11 | 3 |
| Trees | 15 | 4 |
| Tries | 3 | — |
| Heap | 7 | 2 |
| Backtracking | 10 | — |
| Graphs | 13 | 4 |
| Advanced Graphs | 6 | 1 |
| 1D DP | 12 | — |
| 2D DP | 11 | 5 |
| Greedy | 8 | — |
| Intervals | 6 | — |
| Math & Geometry | 8 | 4 |
| Bit Manipulation | 7 | — |

---

## C++ Cheat Sheet for Interviews

```cpp
// Containers
unordered_map<int,int> mp;
unordered_set<int> st;
priority_queue<int> maxHeap;                              // default max
priority_queue<int, vector<int>, greater<int>> minHeap;  // min-heap
deque<int> dq;
stack<int> sk;
queue<int> q;

// String ops
sort(s.begin(), s.end());
reverse(s.begin(), s.end());
s.substr(start, length);

// Vector
sort(v.begin(), v.end());
sort(v.begin(), v.end(), greater<int>());   // descending
auto it = lower_bound(v.begin(), v.end(), x);  // first >= x
auto it = upper_bound(v.begin(), v.end(), x);  // first > x

// Useful functions
__gcd(a, b)           // GCD
abs(x)                // absolute value
INT_MAX, INT_MIN, LONG_MAX, LONG_MIN   // limits
accumulate(v.begin(), v.end(), 0)      // sum

// Lambda sort
sort(v.begin(), v.end(), [](auto& a, auto& b) {
    return a.second < b.second;
});
```

---

## Complexity Reference

| Structure | Access | Search | Insert | Delete |
|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) |
| Hash Map | — | O(1) avg | O(1) avg | O(1) avg |
| Binary Search Tree | O(log n) | O(log n) | O(log n) | O(log n) |
| Heap | O(1) top | — | O(log n) | O(log n) |
| Linked List | O(n) | O(n) | O(1) | O(1) |

---

## How to Approach Any DSA Problem in an Interview

```
Step 1: Understand
  → Restate the problem in your own words
  → Ask about constraints (n size? negative numbers? duplicates?)
  → Work through 1-2 examples manually

Step 2: Pattern Match
  → Array + distinct values → Hash Map / Set
  → Sorted array + search → Binary Search
  → Subarray/substring → Sliding Window
  → All combinations → Backtracking
  → Shortest path → BFS / Dijkstra
  → Optimal substructure → DP
  → Repeated min/max → Heap

Step 3: Brute Force First
  → State the O(n²) or O(2^n) solution — shows you understand the problem
  → Then optimize

Step 4: Code
  → Write clean, readable C++ code
  → Use meaningful variable names
  → Handle edge cases (empty input, single element, all same)

Step 5: Test
  → Test with given examples
  → Test edge cases
  → Walk through your code once
```

---

> **Remember:** Interviewers care more about *how you think* than whether you get the optimal solution immediately. Communicate your thought process clearly.
>
> **Practice order:** NeetCode Roadmap sequence (Arrays → Two Pointers → Sliding Window → ... → Bit Manipulation). Don't jump around.
