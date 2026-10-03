# DSA Interview Patterns — Complete Problem Reference
---

## Table of Contents

- [1. Arrays](#1-arrays)
  - [Linear Traversal](#pattern-linear-traversal)
  - [Two Pointers](#pattern-two-pointers)
  - [Sliding Window Fixed](#pattern-sliding-window-fixed)
  - [Sliding Window Variable](#pattern-sliding-window-variable)
  - [Prefix Sum](#pattern-prefix-sum)
  - [Difference Array](#pattern-difference-array)
  - [Kadane's Algorithm](#pattern-kadanes-algorithm)
  - [Binary Search](#pattern-binary-search)
  - [Binary Search on Answer](#pattern-binary-search-on-answer)
  - [Sorting Based](#pattern-sorting-based)
  - [Merge Intervals](#pattern-merge-intervals)
  - [Matrix Traversal](#pattern-matrix-traversal)
  - [Dutch National Flag](#pattern-dutch-national-flag)
  - [Cyclic Sort](#pattern-cyclic-sort)

- [2. Strings](#2-strings)
  - [Two Pointers (Strings)](#pattern-two-pointers-strings)
  - [Sliding Window (Strings)](#pattern-sliding-window-strings)
  - [Character Frequency](#pattern-character-frequency)
  - [Hashing (Strings)](#pattern-hashing-strings)
  - [String Parsing](#pattern-string-parsing)
  - [KMP](#pattern-kmp)
  - [Trie](#pattern-trie)

- [3. Hashing](#3-hashing)
  - [Frequency Count](#pattern-frequency-count)
  - [HashMap Lookup](#pattern-hashmap-lookup)
  - [Prefix Sum + HashMap](#pattern-prefix-sum--hashmap)
  - [HashSet](#pattern-hashset)
  - [Grouping](#pattern-grouping)

- [4. Linked List](#4-linked-list)
  - [Fast & Slow Pointer](#pattern-fast--slow-pointer)
  - [Reverse](#pattern-reverse)
  - [Merge](#pattern-merge)
  - [Dummy Node](#pattern-dummy-node)

- [5. Stack](#5-stack)
  - [Basic Stack](#pattern-basic-stack)
  - [Monotonic Stack Decreasing](#pattern-monotonic-stack-decreasing)
  - [Monotonic Stack Increasing](#pattern-monotonic-stack-increasing)
  - [Expression Evaluation](#pattern-expression-evaluation)

- [6. Queue / Deque](#6-queue--deque)
  - [BFS Queue](#pattern-bfs-queue)
  - [Monotonic Deque](#pattern-monotonic-deque)

- [7. Heap](#7-heap)
  - [Top K](#pattern-top-k)
  - [Running Median](#pattern-running-median)
  - [Greedy Heap](#pattern-greedy-heap)

- [8. Trees](#8-trees)
  - [DFS (Trees)](#pattern-dfs-trees)
  - [BFS (Trees)](#pattern-bfs-trees)
  - [BST](#pattern-bst)
  - [Tree Construction](#pattern-tree-construction)
  - [Tree DP](#pattern-tree-dp)

- [9. Graphs](#9-graphs)
  - [DFS (Graphs)](#pattern-dfs-graphs)
  - [BFS (Graphs)](#pattern-bfs-graphs)
  - [Topological Sort](#pattern-topological-sort)
  - [Union Find](#pattern-union-find)
  - [Dijkstra](#pattern-dijkstra)

- [10. Backtracking](#10-backtracking)
  - [Subsets](#pattern-subsets)
  - [Permutations](#pattern-permutations)
  - [Combination](#pattern-combination)
  - [Grid Backtracking](#pattern-grid-backtracking)

- [11. Dynamic Programming](#11-dynamic-programming)
  - [1D DP](#pattern-1d-dp)
  - [2D DP](#pattern-2d-dp)
  - [Knapsack](#pattern-knapsack)
  - [LIS](#pattern-lis)
  - [DP on Strings](#pattern-dp-on-strings)
  - [Interval DP](#pattern-interval-dp)

- [12. Greedy](#12-greedy)
  - [Interval Greedy](#pattern-interval-greedy)
  - [Jump Problems](#pattern-jump-problems)

- [13. Bit Manipulation](#13-bit-manipulation)
  - [XOR](#pattern-xor)
  - [Bitmask](#pattern-bitmask)

- [14. Segment Tree / BIT](#14-segment-tree--bit-fenwick-tree)
  - [Range Queries](#pattern-range-queries)

- [15. Multi-Source BFS](#15-multi-source-bfs)

- [16. Bellman-Ford](#16-bellman-ford)

- [17. Minimum Spanning Tree](#17-minimum-spanning-tree)

- [18. DP + Bitmask](#18-dp--bitmask)

- [19. Floyd's Cycle Detection](#19-floyds-cycle-detection-standalone)

---

## Problem Index (by LeetCode number)

| # | Problem | Pattern | Section |
|---|---------|---------|---------|
| 1 | Two Sum | HashMap Lookup | [Hashing](#pattern-hashmap-lookup) |
| 3 | Longest Substring Without Repeating Characters | Sliding Window Variable | [Arrays](#pattern-sliding-window-variable) |
| 8 | String to Integer (atoi) | String Parsing | [Strings](#pattern-string-parsing) |
| 11 | Container With Most Water | Two Pointers | [Arrays](#pattern-two-pointers) |
| 15 | 3Sum | Sorting Based | [Arrays](#pattern-sorting-based) |
| 18 | 4Sum | Sorting Based | [Arrays](#pattern-sorting-based) |
| 19 | Remove Nth Node From End | Dummy Node | [Linked List](#pattern-dummy-node) |
| 20 | Valid Parentheses | Basic Stack | [Stack](#pattern-basic-stack) |
| 21 | Merge Two Sorted Lists | Merge | [Linked List](#pattern-merge) |
| 23 | Merge K Sorted Lists | Merge / K-way | [Linked List](#pattern-merge) |
| 24 | Swap Nodes in Pairs | Dummy Node | [Linked List](#pattern-dummy-node) |
| 25 | Reverse Nodes in k-Group | Reverse | [Linked List](#pattern-reverse) |
| 28 | Find Index of First Occurrence | KMP | [Strings](#pattern-kmp) |
| 33 | Search in Rotated Sorted Array | Binary Search | [Arrays](#pattern-binary-search) |
| 35 | Search Insert Position | Binary Search | [Arrays](#pattern-binary-search) |
| 37 | Sudoku Solver | Grid Backtracking | [Backtracking](#pattern-grid-backtracking) |
| 39 | Combination Sum | Combination | [Backtracking](#pattern-combination) |
| 40 | Combination Sum II | Combination | [Backtracking](#pattern-combination) |
| 41 | First Missing Positive | Cyclic Sort | [Arrays](#pattern-cyclic-sort) |
| 45 | Jump Game II | Jump Problems | [Greedy](#pattern-jump-problems) |
| 46 | Permutations | Permutations | [Backtracking](#pattern-permutations) |
| 47 | Permutations II | Permutations | [Backtracking](#pattern-permutations) |
| 48 | Rotate Image | Matrix Traversal | [Arrays](#pattern-matrix-traversal) |
| 49 | Group Anagrams | Hashing | [Strings](#pattern-hashing-strings) |
| 51 | N-Queens | Grid Backtracking | [Backtracking](#pattern-grid-backtracking) |
| 53 | Maximum Subarray | Kadane's | [Arrays](#pattern-kadanes-algorithm) |
| 54 | Spiral Matrix | Matrix Traversal | [Arrays](#pattern-matrix-traversal) |
| 55 | Jump Game | Jump Problems | [Greedy](#pattern-jump-problems) |
| 56 | Merge Intervals | Merge Intervals | [Arrays](#pattern-merge-intervals) |
| 57 | Insert Interval | Merge Intervals | [Arrays](#pattern-merge-intervals) |
| 62 | Unique Paths | 2D DP | [DP](#pattern-2d-dp) |
| 70 | Climbing Stairs | 1D DP | [DP](#pattern-1d-dp) |
| 72 | Edit Distance | 2D DP | [DP](#pattern-2d-dp) |
| 73 | Set Matrix Zeroes | Matrix Traversal | [Arrays](#pattern-matrix-traversal) |
| 75 | Sort Colors | Dutch National Flag | [Arrays](#pattern-dutch-national-flag) |
| 76 | Minimum Window Substring | Sliding Window | [Strings](#pattern-sliding-window-strings) |
| 78 | Subsets | Subsets | [Backtracking](#pattern-subsets) |
| 79 | Word Search | Grid Backtracking | [Backtracking](#pattern-grid-backtracking) |
| 84 | Largest Rectangle in Histogram | Monotonic Stack | [Stack](#pattern-monotonic-stack-increasing) |
| 90 | Subsets II | Subsets | [Backtracking](#pattern-subsets) |
| 92 | Reverse Linked List II | Reverse | [Linked List](#pattern-reverse) |
| 98 | Validate BST | BST | [Trees](#pattern-bst) |
| 102 | Binary Tree Level Order Traversal | BFS | [Trees](#pattern-bfs-trees) |
| 103 | Zigzag Level Order Traversal | BFS | [Trees](#pattern-bfs-trees) |
| 104 | Maximum Depth of Binary Tree | DFS | [Trees](#pattern-dfs-trees) |
| 105 | Construct Binary Tree | Tree Construction | [Trees](#pattern-tree-construction) |
| 112 | Path Sum | DFS | [Trees](#pattern-dfs-trees) |
| 124 | Binary Tree Maximum Path Sum | Tree DP | [Trees](#pattern-tree-dp) |
| 125 | Valid Palindrome | Two Pointers | [Strings](#pattern-two-pointers-strings) |
| 127 | Word Ladder | BFS | [Graphs](#pattern-bfs-graphs) |
| 128 | Longest Consecutive Sequence | HashSet | [Hashing](#pattern-hashset) |
| 133 | Clone Graph | DFS | [Graphs](#pattern-dfs-graphs) |
| 136 | Single Number | XOR | [Bit Manipulation](#pattern-xor) |
| 141 | Linked List Cycle | Fast & Slow Pointer | [Linked List](#pattern-fast--slow-pointer) |
| 150 | Evaluate Reverse Polish Notation | Expression Eval | [Stack](#pattern-expression-evaluation) |
| 155 | Min Stack | Basic Stack | [Stack](#pattern-basic-stack) |
| 167 | Two Sum II | Two Pointers | [Arrays](#pattern-two-pointers) |
| 169 | Majority Element | Linear Traversal | [Arrays](#pattern-linear-traversal) |
| 179 | Largest Number | Sorting Based | [Arrays](#pattern-sorting-based) |
| 197 | House Robber | 1D DP | [DP](#pattern-1d-dp) |
| 199 | Binary Tree Right Side View | BFS | [Trees](#pattern-bfs-trees) |
| 200 | Number of Islands | DFS | [Graphs](#pattern-dfs-graphs) |
| 202 | Happy Number | HashMap Lookup | [Hashing](#pattern-hashmap-lookup) |
| 205 | Isomorphic Strings | Hashing | [Strings](#pattern-hashing-strings) |
| 206 | Reverse Linked List | Reverse | [Linked List](#pattern-reverse) |
| 207 | Course Schedule | Topological Sort | [Graphs](#pattern-topological-sort) |
| 208 | Implement Trie | Trie | [Strings](#pattern-trie) |
| 209 | Minimum Size Subarray Sum | Sliding Window | [Arrays](#pattern-sliding-window-variable) |
| 210 | Course Schedule II | Topological Sort | [Graphs](#pattern-topological-sort) |
| 215 | Kth Largest Element | Top K | [Heap](#pattern-top-k) |
| 217 | Contains Duplicate | HashMap Lookup | [Hashing](#pattern-hashmap-lookup) |
| 230 | Kth Smallest in BST | BST | [Trees](#pattern-bst) |
| 235 | LCA of BST | BST | [Trees](#pattern-bst) |
| 239 | Sliding Window Maximum | Monotonic Deque | [Queue](#pattern-monotonic-deque) |
| 242 | Valid Anagram | Character Frequency | [Strings](#pattern-character-frequency) |
| 260 | Single Number III | XOR | [Bit Manipulation](#pattern-xor) |
| 268 | Missing Number | XOR | [Bit Manipulation](#pattern-xor) |
| 283 | Move Zeroes | Two Pointers | [Arrays](#pattern-two-pointers) |
| 287 | Find the Duplicate Number | Cyclic Sort / Floyd's | [Arrays](#pattern-cyclic-sort) |
| 290 | Word Pattern | Hashing | [Strings](#pattern-hashing-strings) |
| 295 | Find Median from Data Stream | Running Median | [Heap](#pattern-running-median) |
| 300 | Longest Increasing Subsequence | LIS | [DP](#pattern-lis) |
| 303 | Range Sum Query | Prefix Sum | [Arrays](#pattern-prefix-sum) |
| 307 | Range Sum Query Mutable | Segment Tree/BIT | [Segment Tree](#pattern-range-queries) |
| 312 | Burst Balloons | Interval DP | [DP](#pattern-interval-dp) |
| 315 | Count of Smaller Numbers After Self | BIT | [Segment Tree](#pattern-range-queries) |
| 318 | Maximum Product of Word Lengths | Bitmask | [Bit Manipulation](#pattern-bitmask) |
| 322 | Coin Change | 1D DP | [DP](#pattern-1d-dp) |
| 337 | House Robber III | Tree DP | [Trees](#pattern-tree-dp) |
| 338 | Counting Bits | Bitmask | [Bit Manipulation](#pattern-bitmask) |
| 344 | Reverse String | Two Pointers | [Strings](#pattern-two-pointers-strings) |
| 345 | Reverse Vowels | Two Pointers | [Strings](#pattern-two-pointers-strings) |
| 347 | Top K Frequent Elements | Frequency Count / Top K | [Hashing](#pattern-frequency-count) |
| 349 | Intersection of Two Arrays | HashSet | [Hashing](#pattern-hashset) |
| 383 | Ransom Note | Character Frequency | [Strings](#pattern-character-frequency) |
| 389 | Find the Difference | Character Frequency | [Strings](#pattern-character-frequency) |
| 394 | Decode String | String Parsing | [Strings](#pattern-string-parsing) |
| 410 | Split Array Largest Sum | Binary Search on Answer | [Arrays](#pattern-binary-search-on-answer) |
| 416 | Partition Equal Subset Sum | Knapsack | [DP](#pattern-knapsack) |
| 417 | Pacific Atlantic Water Flow | Multi-Source BFS | [Multi-Source BFS](#15-multi-source-bfs) |
| 435 | Non-overlapping Intervals | Merge Intervals | [Arrays](#pattern-merge-intervals) |
| 448 | Find All Disappeared Numbers | Cyclic Sort | [Arrays](#pattern-cyclic-sort) |
| 451 | Sort Characters by Frequency | Frequency Count | [Hashing](#pattern-frequency-count) |
| 452 | Min Arrows to Burst Balloons | Interval Greedy | [Greedy](#pattern-interval-greedy) |
| 494 | Target Sum | Knapsack | [DP](#pattern-knapsack) |
| 496 | Next Greater Element I | Monotonic Stack | [Stack](#pattern-monotonic-stack-decreasing) |
| 502 | IPO | Greedy Heap | [Heap](#pattern-greedy-heap) |
| 516 | Longest Palindromic Subsequence | DP on Strings | [DP](#pattern-dp-on-strings) |
| 523 | Continuous Subarray Sum | Prefix Sum | [Arrays](#pattern-prefix-sum) |
| 525 | Contiguous Array | Prefix Sum + HashMap | [Hashing](#pattern-prefix-sum--hashmap) |
| 542 | 01 Matrix | Multi-Source BFS | [Multi-Source BFS](#15-multi-source-bfs) |
| 543 | Diameter of Binary Tree | DFS | [Trees](#pattern-dfs-trees) |
| 547 | Number of Provinces | Union Find | [Graphs](#pattern-union-find) |
| 560 | Subarray Sum Equals K | Prefix Sum + HashMap | [Arrays](#pattern-prefix-sum) |
| 567 | Permutation in String | Sliding Window | [Strings](#pattern-sliding-window-strings) |
| 609 | Find Duplicate File in System | Grouping | [Hashing](#pattern-grouping) |
| 643 | Maximum Average Subarray I | Sliding Window Fixed | [Arrays](#pattern-sliding-window-fixed) |
| 647 | Palindromic Substrings | DP on Strings | [DP](#pattern-dp-on-strings) |
| 684 | Redundant Connection | Union Find | [Graphs](#pattern-union-find) |
| 695 | Max Area of Island | DFS | [Graphs](#pattern-dfs-graphs) |
| 704 | Binary Search | Binary Search | [Arrays](#pattern-binary-search) |
| 721 | Accounts Merge | Union Find | [Graphs](#pattern-union-find) |
| 739 | Daily Temperatures | Monotonic Stack | [Stack](#pattern-monotonic-stack-decreasing) |
| 743 | Network Delay Time | Dijkstra | [Graphs](#pattern-dijkstra) |
| 787 | Cheapest Flights Within K Stops | Bellman-Ford | [Bellman-Ford](#16-bellman-ford) |
| 847 | Shortest Path Visiting All Nodes | DP + Bitmask | [DP + Bitmask](#18-dp--bitmask) |
| 875 | Koko Eating Bananas | Binary Search on Answer | [Arrays](#pattern-binary-search-on-answer) |
| 876 | Middle of Linked List | Fast & Slow Pointer | [Linked List](#pattern-fast--slow-pointer) |
| 904 | Fruit Into Baskets | Sliding Window Variable | [Arrays](#pattern-sliding-window-variable) |
| 973 | K Closest Points to Origin | Top K | [Heap](#pattern-top-k) |
| 994 | Rotting Oranges | BFS Queue | [Queue](#pattern-bfs-queue) |
| 1011 | Capacity to Ship Packages | Binary Search on Answer | [Arrays](#pattern-binary-search-on-answer) |
| 1094 | Car Pooling | Difference Array | [Arrays](#pattern-difference-array) |
| 1109 | Corporate Flight Bookings | Difference Array | [Arrays](#pattern-difference-array) |
| 1631 | Path With Minimum Effort | Dijkstra | [Graphs](#pattern-dijkstra) |
| 1642 | Furthest Building You Can Reach | Greedy Heap | [Heap](#pattern-greedy-heap) |
| 1749 | Maximum Absolute Sum | Kadane's | [Arrays](#pattern-kadanes-algorithm) |
| 1868 | Min Cost to Connect All Points | MST (Kruskal) | [MST](#17-minimum-spanning-tree) |
| 2461 | Max Sum of Distinct Subarrays | Sliding Window Fixed | [Arrays](#pattern-sliding-window-fixed) |
## 1. ARRAYS

### Pattern: Linear Traversal

---

#### Find Maximum in Array
**Link:** [https://leetcode.com/problems/find-maximum-in-array/](https://leetcode.com/problems/find-maximum-in-array/)  
**Problem:** Given an integer array `nums`, return the maximum element.

```python
def findMax(nums: list[int]) -> int:
    max_val = float('-inf')
    for n in nums:
        max_val = max(max_val, n)
    return max_val
```

**Example 1:**
**Input:** `nums = [1, 5, 3, 9, 2]`
**Output:** `9`
**Explanation:** The maximum value in the array is 9.

**Example 2:**
**Input:** `nums = [-1, -5, -3]`
**Output:** `-1`
**Explanation:** The maximum value in the array is -1.

---

#### 121. Best Time to Buy and Sell Stock
**Link:** [https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)  
**Problem:** You are given an array `prices` where `prices[i]` is the price of a given stock on the ith day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

```python
def maxProfit(prices: list[int]) -> int:
    min_price = float('inf')
    max_profit = 0
    for p in prices:
        min_price = min(min_price, p)
        max_profit = max(max_profit, p - min_price)
    return max_profit
```

**Example 1:**
**Input:** `prices = [7,1,5,3,6,4]`
**Output:** `5`
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**
**Input:** `prices = [7,6,4,3,1]`
**Output:** `0`
**Explanation:** In this case, no transactions are done and the max profit = 0.

---

#### 169. Majority Element
**Link:** [https://leetcode.com/problems/majority-element/](https://leetcode.com/problems/majority-element/)  
**Problem:** Given an array `nums` of size `n`, return the majority element. The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

```python
def majorityElement(nums: list[int]) -> int:
    count = 0
    candidate = 0
    for n in nums:
        if count == 0:
            candidate = n
        count += 1 if n == candidate else -1
    return candidate
```

**Example 1:**
**Input:** `nums = [3,2,3]`
**Output:** `3`
**Explanation:** The element 3 appears 2 times, which is > 3/2.

**Example 2:**
**Input:** `nums = [2,2,1,1,1,2,2]`
**Output:** `2`
**Explanation:** The element 2 appears 4 times, which is > 7/2.

---

### Pattern: Two Pointers

---

#### 167. Two Sum II
**Link:** [https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)  
**Problem:** Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Return the indices of the two numbers as an integer array `[index1, index2]` of length 2.

```python
def twoSum(numbers: list[int], target: int) -> list[int]:
    l, r = 0, len(numbers) - 1
    while l < r:
        s = numbers[l] + numbers[r]
        if s == target:
            return [l + 1, r + 1]
        elif s < target:
            l += 1
        else:
            r -= 1
    return []
```

**Example 1:**
**Input:** `numbers = [2,7,11,15], target = 9`
**Output:** `[1,2]`
**Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

**Example 2:**
**Input:** `numbers = [2,3,4], target = 6`
**Output:** `[1,3]`
**Explanation:** The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].

---

#### 11. Container With Most Water
**Link:** [https://leetcode.com/problems/container-with-most-water/](https://leetcode.com/problems/container-with-most-water/)  
**Problem:** You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the ith line are `(i, 0)` and `(i, height[i])`. Find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum amount of water a container can store.

```python
def maxArea(height: list[int]) -> int:
    l, r = 0, len(height) - 1
    res = 0
    while l < r:
        res = max(res, min(height[l], height[r]) * (r - l))
        if height[l] < height[r]:
            l += 1
        else:
            r -= 1
    return res
```

**Example 1:**
**Input:** `height = [1,8,6,2,5,4,8,3,7]`
**Output:** `49`
**Explanation:** The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Example 2:**
**Input:** `height = [1,1]`
**Output:** `1`
**Explanation:** The max area is 1 * 1 = 1.

---

#### 283. Move Zeroes
**Link:** [https://leetcode.com/problems/move-zeroes/](https://leetcode.com/problems/move-zeroes/)  
**Problem:** Given an integer array `nums`, move all 0's to the end of it while maintaining the relative order of the non-zero elements. Note that you must do this in-place without making a copy of the array.

```python
def moveZeroes(nums: list[int]) -> None:
    pos = 0
    for n in nums:
        if n != 0:
            nums[pos] = n
            pos += 1
    while pos < len(nums):
        nums[pos] = 0
        pos += 1
```

**Example 1:**
**Input:** `nums = [0,1,0,3,12]`
**Output:** `[1,3,12,0,0]`
**Explanation:** Move all zeros to the end, keeping the original order of 1, 3, 12.

**Example 2:**
**Input:** `nums = [0]`
**Output:** `[0]`
**Explanation:** A single zero remains as is.

---

### Pattern: Sliding Window (Fixed)

---

#### 643. Maximum Average Subarray I
**Link:** [https://leetcode.com/problems/maximum-average-subarray-i/](https://leetcode.com/problems/maximum-average-subarray-i/)  
**Problem:** You are given an integer array `nums` consisting of `n` elements, and an integer `k`. Find a contiguous subarray whose length is equal to `k` that has the maximum average value and return this value.

```python
def findMaxAverage(nums: list[int], k: int) -> float:
    cur_sum = sum(nums[:k])
    max_sum = cur_sum
    for i in range(k, len(nums)):
        cur_sum += nums[i] - nums[i - k]
        max_sum = max(max_sum, cur_sum)
    return max_sum / k
```

**Example 1:**
**Input:** `nums = [1,12,-5,-6,50,3], k = 4`
**Output:** `12.75000`
**Explanation:** Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75

**Example 2:**
**Input:** `nums = [5], k = 1`
**Output:** `5.00000`
**Explanation:** Maximum average is 5 / 1 = 5.

---

#### 2461. Maximum Sum of Distinct Subarrays With Length K
**Link:** [https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)  
**Problem:** You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions: the length of the subarray is `k`, and all the elements of the subarray are distinct. Return the maximum subarray sum of all the subarrays that meet the conditions. If no subarray meets the conditions, return 0.

```python
from collections import defaultdict

def maximumSubarraySum(nums: list[int], k: int) -> int:
    freq = defaultdict(int)
    cur_sum = 0
    res = 0
    for i in range(len(nums)):
        freq[nums[i]] += 1
        cur_sum += nums[i]
        if i >= k:
            out = nums[i - k]
            freq[out] -= 1
            if freq[out] == 0:
                del freq[out]
            cur_sum -= out
        if i >= k - 1 and len(freq) == k:
            res = max(res, cur_sum)
    return res
```

**Example 1:**
**Input:** `nums = [1,5,4,2,9,9,9], k = 3`
**Output:** `15`
**Explanation:** The subarrays of nums with length 3 are:
- [1,5,4] which meets the requirements and has a sum of 10.
- [5,4,2] which meets the requirements and has a sum of 11.
- [4,2,9] which meets the requirements and has a sum of 15.
- [2,9,9] which does not meet the requirements because the element 9 is repeated.
- [9,9,9] which does not meet the requirements because the element 9 is repeated.
We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions.

**Example 2:**
**Input:** `nums = [4,4,4], k = 3`
**Output:** `0`
**Explanation:** The subarrays of nums with length 3 are:
- [4,4,4] which does not meet the requirements because the element 4 is repeated.
We return 0 because no subarrays meet the conditions.

---

### Pattern: Sliding Window (Variable)

---

#### 3. Longest Substring Without Repeating Characters
**Link:** [https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)  
**Problem:** Given a string `s`, find the length of the longest substring without repeating characters.

```python
def lengthOfLongestSubstring(s: str) -> int:
    mp = {}
    l = 0
    res = 0
    for r in range(len(s)):
        if s[r] in mp:
            l = max(l, mp[s[r]] + 1)
        mp[s[r]] = r
        res = max(res, r - l + 1)
    return res
```

**Example 1:**
**Input:** `s = "abcabcbb"`
**Output:** `3`
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**
**Input:** `s = "bbbbb"`
**Output:** `1`
**Explanation:** The answer is "b", with the length of 1.

---

#### 209. Minimum Size Subarray Sum
**Link:** [https://leetcode.com/problems/minimum-size-subarray-sum/](https://leetcode.com/problems/minimum-size-subarray-sum/)  
**Problem:** Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is greater than or equal to `target`. If there is no such subarray, return 0 instead.

```python
def minSubArrayLen(target: int, nums: list[int]) -> int:
    l = 0
    cur_sum = 0
    res = float('inf')
    for r in range(len(nums)):
        cur_sum += nums[r]
        while cur_sum >= target:
            res = min(res, r - l + 1)
            cur_sum -= nums[l]
            l += 1
    return 0 if res == float('inf') else res
```

**Example 1:**
**Input:** `target = 7, nums = [2,3,1,2,4,3]`
**Output:** `2`
**Explanation:** The subarray [4,3] has the minimal length under the problem constraint.

**Example 2:**
**Input:** `target = 4, nums = [1,4,4]`
**Output:** `1`
**Explanation:** The subarray [4] has length 1.

---

#### 904. Fruit Into Baskets
**Link:** [https://leetcode.com/problems/fruit-into-baskets/](https://leetcode.com/problems/fruit-into-baskets/)  
**Problem:** You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the ith tree produces. You want to collect as much fruit as possible with two baskets, where each basket can only hold a single type of fruit. Starting from any tree, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. Return the maximum number of fruits you can pick.

```python
from collections import defaultdict

def totalFruit(fruits: list[int]) -> int:
    basket = defaultdict(int)
    l = 0
    res = 0
    for r in range(len(fruits)):
        basket[fruits[r]] += 1
        while len(basket) > 2:
            basket[fruits[l]] -= 1
            if basket[fruits[l]] == 0:
                del basket[fruits[l]]
            l += 1
        res = max(res, r - l + 1)
    return res
```

**Example 1:**
**Input:** `fruits = [1,2,1]`
**Output:** `3`
**Explanation:** We can pick from all 3 trees.

**Example 2:**
**Input:** `fruits = [0,1,2,2]`
**Output:** `3`
**Explanation:** We can pick from trees [1,2,2].

---

### Pattern: Prefix Sum

---

#### 303. Range Sum Query - Immutable
**Link:** [https://leetcode.com/problems/range-sum-query-immutable/](https://leetcode.com/problems/range-sum-query-immutable/)  
**Problem:** Given an integer array `nums`, handle multiple queries of the following type: Calculate the sum of the elements of `nums` between indices `left` and `right` inclusive where `left <= right`.

```python
class NumArray:
    def __init__(self, nums: list[int]):
        self.prefix = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            self.prefix[i + 1] = self.prefix[i] + nums[i]

    def sumRange(self, left: int, right: int) -> int:
        return self.prefix[right + 1] - self.prefix[left]
```

**Example 1:**
**Input:** 
`["NumArray", "sumRange", "sumRange", "sumRange"]`
`[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]`
**Output:** `[null, 1, -1, -3]`
**Explanation:** 
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3

---

#### 560. Subarray Sum Equals K
**Link:** [https://leetcode.com/problems/subarray-sum-equals-k/](https://leetcode.com/problems/subarray-sum-equals-k/)  
**Problem:** Given an array of integers `nums` and an integer `k`, return the total number of subarrays whose sum equals to `k`.

```python
from collections import defaultdict

def subarraySum(nums: list[int], k: int) -> int:
    mp = defaultdict(int)
    mp[0] = 1
    cur_sum = 0
    res = 0
    for n in nums:
        cur_sum += n
        res += mp[cur_sum - k]
        mp[cur_sum] += 1
    return res
```

**Example 1:**
**Input:** `nums = [1,1,1], k = 2`
**Output:** `2`
**Explanation:** Subarrays are [1,1] from index 0-1 and [1,1] from index 1-2.

**Example 2:**
**Input:** `nums = [1,2,3], k = 3`
**Output:** `2`
**Explanation:** Subarrays are [1,2] and [3].

---

#### 523. Continuous Subarray Sum
**Link:** [https://leetcode.com/problems/continuous-subarray-sum/](https://leetcode.com/problems/continuous-subarray-sum/)  
**Problem:** Given an integer array `nums` and an integer `k`, return true if `nums` has a good subarray or false otherwise. A good subarray is a subarray where its length is at least two, and the sum of the elements of the subarray is a multiple of `k`.

```python
def checkSubarraySum(nums: list[int], k: int) -> bool:
    mp = {0: -1}
    cur_sum = 0
    for i in range(len(nums)):
        cur_sum = (cur_sum + nums[i]) % k
        if cur_sum in mp:
            if i - mp[cur_sum] >= 2:
                return True
        else:
            mp[cur_sum] = i
    return False
```

**Example 1:**
**Input:** `nums = [23,2,4,6,7], k = 6`
**Output:** `true`
**Explanation:** [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.

**Example 2:**
**Input:** `nums = [23,2,6,4,7], k = 13`
**Output:** `false`
**Explanation:** No subarray has sum multiple of 13.

---

### Pattern: Difference Array

---

#### 1109. Corporate Flight Bookings
**Link:** [https://leetcode.com/problems/corporate-flight-bookings/](https://leetcode.com/problems/corporate-flight-bookings/)  
**Problem:** There are `n` flights and they are labeled from 1 to `n`. You are given an array of flight bookings `bookings`, where `bookings[i] = [firsti, lasti, seatsi]` represents a booking for flights `firsti` through `lasti` (inclusive) with `seatsi` seats reserved for each flight. Return an array `answer` of length `n`, where `answer[i]` is the total number of seats reserved for flight `i`.

```python
def corpFlightBookings(bookings: list[list[int]], n: int) -> list[int]:
    diff = [0] * (n + 1)
    for first, last, seats in bookings:
        diff[first - 1] += seats
        if last < n:
            diff[last] -= seats
    for i in range(1, n):
        diff[i] += diff[i - 1]
    return diff[:n]
```

**Example 1:**
**Input:** `bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5`
**Output:** `[10,55,45,25,25]`
**Explanation:** 
Flight labels:        1   2   3   4   5
Booking 1 reserved:  10  10
Booking 2 reserved:      20  20
Booking 3 reserved:      25  25  25  25
Total seats:         10  55  45  25  25

---

#### 1094. Car Pooling
**Link:** [https://leetcode.com/problems/car-pooling/](https://leetcode.com/problems/car-pooling/)  
**Problem:** There is a car with `capacity` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west). You are given the integer `capacity` and an array `trips` where `trips[i] = [numPassengersi, fromi, toi]` indicates that the ith trip has `numPassengersi` passengers and the passengers must be picked up at location `fromi` and dropped off at location `toi`. Return true if it is possible to pick up and drop off all passengers for all the given trips, or false otherwise.

```python
def carPooling(trips: list[list[int]], capacity: int) -> bool:
    diff = [0] * 1001
    for passengers, start, end in trips:
        diff[start] += passengers
        diff[end] -= passengers
    cur = 0
    for d in diff:
        cur += d
        if cur > capacity:
            return False
    return True
```

**Example 1:**
**Input:** `trips = [[2,1,5],[3,3,7]], capacity = 4`
**Output:** `false`
**Explanation:** We pick up 2 passengers at 1. We pick up 3 passengers at 3 (total 5), which exceeds capacity 4.

**Example 2:**
**Input:** `trips = [[2,1,5],[3,3,7]], capacity = 5`
**Output:** `true`

---

### Pattern: Kadane's Algorithm

---

#### 53. Maximum Subarray
**Link:** [https://leetcode.com/problems/maximum-subarray/](https://leetcode.com/problems/maximum-subarray/)  
**Problem:** Given an integer array `nums`, find the subarray with the largest sum, and return its sum.

```python
def maxSubArray(nums: list[int]) -> int:
    cur = res = nums[0]
    for i in range(1, len(nums)):
        cur = max(nums[i], cur + nums[i])
        res = max(res, cur)
    return res
```

**Example 1:**
**Input:** `nums = [-2,1,-3,4,-1,2,1,-5,4]`
**Output:** `6`
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**
**Input:** `nums = [1]`
**Output:** `1`
**Explanation:** The subarray [1] has the largest sum 1.

---

#### 1749. Maximum Absolute Sum of Any Subarray
**Link:** [https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/)  
**Problem:** You are given an integer array `nums`. The absolute sum of a subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is `abs(numsl + numsl+1 + ... + numsr-1 + numsr)`. Return the maximum absolute sum of any (possibly empty) subarray of `nums`.

```python
def maxAbsoluteSum(nums: list[int]) -> int:
    max_sum = min_sum = 0
    cur_max = cur_min = 0
    for n in nums:
        cur_max = max(n, cur_max + n)
        cur_min = min(n, cur_min + n)
        max_sum = max(max_sum, cur_max)
        min_sum = min(min_sum, cur_min)
    return max(max_sum, -min_sum)
```

**Example 1:**
**Input:** `nums = [1,-3,2,3,-4]`
**Output:** `5`
**Explanation:** The absolute sum of subarray [2,3] is |5| = 5.

**Example 2:**
**Input:** `nums = [2,-5,1,-4,3,-2]`
**Output:** `8`
**Explanation:** The absolute sum of subarray [-5,1,-4] is |-8| = 8.

---

### Pattern: Binary Search

---

#### 704. Binary Search
**Link:** [https://leetcode.com/problems/binary-search/](https://leetcode.com/problems/binary-search/)  
**Problem:** Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

```python
def search(nums: list[int], target: int) -> int:
    l, r = 0, len(nums) - 1
    while l <= r:
        mid = l + (r - l) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            l = mid + 1
        else:
            r = mid - 1
    return -1
```

**Example 1:**
**Input:** `nums = [-1,0,3,5,9,12], target = 9`
**Output:** `4`
**Explanation:** 9 exists in nums and its index is 4.

**Example 2:**
**Input:** `nums = [-1,0,3,5,9,12], target = 2`
**Output:** `-1`
**Explanation:** 2 does not exist in nums so return -1.

---

#### 35. Search Insert Position
**Link:** [https://leetcode.com/problems/search-insert-position/](https://leetcode.com/problems/search-insert-position/)  
**Problem:** Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

```python
def searchInsert(nums: list[int], target: int) -> int:
    l, r = 0, len(nums) - 1
    while l <= r:
        mid = l + (r - l) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            l = mid + 1
        else:
            r = mid - 1
    return l
```

**Example 1:**
**Input:** `nums = [1,3,5,6], target = 5`
**Output:** `2`

**Example 2:**
**Input:** `nums = [1,3,5,6], target = 2`
**Output:** `1`

---

#### 33. Search in Rotated Sorted Array
**Link:** [https://leetcode.com/problems/search-in-rotated-sorted-array/](https://leetcode.com/problems/search-in-rotated-sorted-array/)  
**Problem:** There is an integer array `nums` sorted in ascending order (with distinct values). Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k`. Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or -1 if it is not in `nums`.

```python
def search(nums: list[int], target: int) -> int:
    l, r = 0, len(nums) - 1
    while l <= r:
        mid = l + (r - l) // 2
        if nums[mid] == target:
            return mid
        if nums[l] <= nums[mid]:
            if nums[l] <= target < nums[mid]:
                r = mid - 1
            else:
                l = mid + 1
        else:
            if nums[mid] < target <= nums[r]:
                l = mid + 1
            else:
                r = mid - 1
    return -1
```

**Example 1:**
**Input:** `nums = [4,5,6,7,0,1,2], target = 0`
**Output:** `4`

**Example 2:**
**Input:** `nums = [4,5,6,7,0,1,2], target = 3`
**Output:** `-1`

---

### Pattern: Binary Search on Answer

---

#### 875. Koko Eating Bananas
**Link:** [https://leetcode.com/problems/koko-eating-bananas/](https://leetcode.com/problems/koko-eating-bananas/)  
**Problem:** Koko loves to eat bananas. There are `n` piles of bananas, the ith pile has `piles[i]` bananas. Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. She likes to eat slowly but still wants to finish eating all the bananas before the guards return in `h` hours. Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

```python
def minEatingSpeed(piles: list[int], h: int) -> int:
    l, r = 1, max(piles)
    while l < r:
        mid = l + (r - l) // 2
        hours = sum((p + mid - 1) // mid for p in piles)
        if hours <= h:
            r = mid
        else:
            l = mid + 1
    return l
```

**Example 1:**
**Input:** `piles = [3,6,7,11], h = 8`
**Output:** `4`

**Example 2:**
**Input:** `piles = [30,11,23,4,20], h = 5`
**Output:** `30`

---

#### 1011. Capacity to Ship Packages Within D Days
**Link:** [https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)  
**Problem:** A conveyor belt has packages that must be shipped from one port to another within `days` days. The ith package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship. Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

```python
def shipWithinDays(weights: list[int], days: int) -> int:
    l, r = max(weights), sum(weights)
    while l < r:
        mid = l + (r - l) // 2
        need, cur = 1, 0
        for w in weights:
            if cur + w > mid:
                need += 1
                cur = 0
            cur += w
        if need <= days:
            r = mid
        else:
            l = mid + 1
    return l
```

**Example 1:**
**Input:** `weights = [1,2,3,4,5,6,7,8,9,10], days = 5`
**Output:** `15`
**Explanation:** A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

**Example 2:**
**Input:** `weights = [3,2,2,4,1,4], days = 3`
**Output:** `6`

---

#### 410. Split Array Largest Sum
**Link:** [https://leetcode.com/problems/split-array-largest-sum/](https://leetcode.com/problems/split-array-largest-sum/)  
**Problem:** Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is minimized. Return the minimized largest sum of the split.

```python
def splitArray(nums: list[int], k: int) -> int:
    l, r = max(nums), sum(nums)
    while l < r:
        mid = l + (r - l) // 2
        parts, cur = 1, 0
        for n in nums:
            if cur + n > mid:
                parts += 1
                cur = 0
            cur += n
        if parts <= k:
            r = mid
        else:
            l = mid + 1
    return l
```

**Example 1:**
**Input:** `nums = [7,2,5,10,8], k = 2`
**Output:** `18`
**Explanation:** There are four ways to split nums into two subarrays. The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

**Example 2:**
**Input:** `nums = [1,2,3,4,5], k = 2`
**Output:** `9`

---

### Pattern: Sorting Based

---

#### 15. 3Sum
**Link:** [https://leetcode.com/problems/3sum/](https://leetcode.com/problems/3sum/)  
**Problem:** Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`. Notice that the solution set must not contain duplicate triplets.

```python
def threeSum(nums: list[int]) -> list[list[int]]:
    nums.sort()
    res = []
    for i in range(len(nums)):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        l, r = i + 1, len(nums) - 1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s == 0:
                res.append([nums[i], nums[l], nums[r]])
                while l < r and nums[l] == nums[l + 1]:
                    l += 1
                while l < r and nums[r] == nums[r - 1]:
                    r -= 1
                l += 1
                r -= 1
            elif s < 0:
                l += 1
            else:
                r -= 1
    return res
```

**Example 1:**
**Input:** `nums = [-1,0,1,2,-1,-4]`
**Output:** `[[-1,-1,2],[-1,0,1]]`

**Example 2:**
**Input:** `nums = [0,1,1]`
**Output:** `[]`

---

#### 18. 4Sum
**Link:** [https://leetcode.com/problems/4sum/](https://leetcode.com/problems/4sum/)  
**Problem:** Given an array `nums` of `n` integers and an integer `target`, return an array of all the unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that their sum equals `target`.

```python
def fourSum(nums: list[int], target: int) -> list[list[int]]:
    nums.sort()
    res = []
    n = len(nums)
    for i in range(n - 3):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        for j in range(i + 1, n - 2):
            if j > i + 1 and nums[j] == nums[j - 1]:
                continue
            l, r = j + 1, n - 1
            while l < r:
                s = nums[i] + nums[j] + nums[l] + nums[r]
                if s == target:
                    res.append([nums[i], nums[j], nums[l], nums[r]])
                    while l < r and nums[l] == nums[l + 1]:
                        l += 1
                    while l < r and nums[r] == nums[r - 1]:
                        r -= 1
                    l += 1
                    r -= 1
                elif s < target:
                    l += 1
                else:
                    r -= 1
    return res
```

**Example 1:**
**Input:** `nums = [1,0,-1,0,-2,2], target = 0`
**Output:** `[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]`

**Example 2:**
**Input:** `nums = [2,2,2,2,2], target = 8`
**Output:** `[[2,2,2,2]]`

---

#### 179. Largest Number
**Link:** [https://leetcode.com/problems/largest-number/](https://leetcode.com/problems/largest-number/)  
**Problem:** Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it as a string. Since the result may be very large, so you need to return a string instead of an integer.

```python
from functools import cmp_to_key

def largestNumber(nums: list[int]) -> str:
    strs = [str(n) for n in nums]
    strs.sort(key=cmp_to_key(lambda a, b: 1 if a + b < b + a else -1))
    if strs[0] == "0":
        return "0"
    return "".join(strs)
```

**Example 1:**
**Input:** `nums = [10,2]`
**Output:** `"210"`

**Example 2:**
**Input:** `nums = [3,30,34,5,9]`
**Output:** `"9534330"`

---

### Pattern: Merge Intervals

---

#### 56. Merge Intervals
**Link:** [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/)  
**Problem:** Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

```python
def merge(intervals: list[list[int]]) -> list[list[int]]:
    intervals.sort(key=lambda x: x[0])
    res = []
    for iv in intervals:
        if res and iv[0] <= res[-1][1]:
            res[-1][1] = max(res[-1][1], iv[1])
        else:
            res.append(iv)
    return res
```

**Example 1:**
**Input:** `intervals = [[1,3],[2,6],[8,10],[15,18]]`
**Output:** `[[1,6],[8,10],[15,18]]`
**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

**Example 2:**
**Input:** `intervals = [[1,4],[4,5]]`
**Output:** `[[1,5]]`
**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

---

#### 57. Insert Interval
**Link:** [https://leetcode.com/problems/insert-interval/](https://leetcode.com/problems/insert-interval/)  
**Problem:** You are given an array of non-overlapping intervals `intervals` sorted in ascending order by start point and a new interval `newInterval`. Insert `newInterval` into `intervals` such that the intervals remain sorted and non-overlapping (merge overlapping intervals if necessary). Return the resulting array of intervals.

```python
def insert(intervals: list[list[int]], newInterval: list[int]) -> list[list[int]]:
    res = []
    i = 0
    n = len(intervals)
    while i < n and intervals[i][1] < newInterval[0]:
        res.append(intervals[i])
        i += 1
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    res.append(newInterval)
    while i < n:
        res.append(intervals[i])
        i += 1
    return res
```

**Example 1:**
**Input:** `intervals = [[1,3],[6,9]], newInterval = [2,5]`
**Output:** `[[1,5],[6,9]]`

**Example 2:**
**Input:** `intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]`
**Output:** `[[1,2],[3,10],[12,16]]`
**Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

---

#### 435. Non-overlapping Intervals
**Link:** [https://leetcode.com/problems/non-overlapping-intervals/](https://leetcode.com/problems/non-overlapping-intervals/)  
**Problem:** Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

```python
def eraseOverlapIntervals(intervals: list[list[int]]) -> int:
    intervals.sort(key=lambda x: x[1])
    res = 0
    prev_end = float('-inf')
    for iv in intervals:
        if iv[0] >= prev_end:
            prev_end = iv[1]
        else:
            res += 1
    return res
```

**Example 1:**
**Input:** `intervals = [[1,2],[2,3],[3,4],[1,3]]`
**Output:** `1`
**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

**Example 2:**
**Input:** `intervals = [[1,2],[1,2],[1,2]]`
**Output:** `2`

---

### Pattern: Matrix Traversal

---

#### 54. Spiral Matrix
**Link:** [https://leetcode.com/problems/spiral-matrix/](https://leetcode.com/problems/spiral-matrix/)  
**Problem:** Given an `m x n` matrix, return all elements of the matrix in spiral order.

```python
def spiralOrder(matrix: list[list[int]]) -> list[int]:
    res = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    while top <= bottom and left <= right:
        for i in range(left, right + 1):
            res.append(matrix[top][i])
        top += 1
        for i in range(top, bottom + 1):
            res.append(matrix[i][right])
        right -= 1
        if top <= bottom:
            for i in range(right, left - 1, -1):
                res.append(matrix[bottom][i])
            bottom -= 1
        if left <= right:
            for i in range(bottom, top - 1, -1):
                res.append(matrix[i][left])
            left += 1
    return res
```

**Example 1:**
**Input:** `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
**Output:** `[1,2,3,6,9,8,7,4,5]`

**Example 2:**
**Input:** `matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]`
**Output:** `[1,2,3,4,8,12,11,10,9,5,6,7]`

---

#### 48. Rotate Image
**Link:** [https://leetcode.com/problems/rotate-image/](https://leetcode.com/problems/rotate-image/)  
**Problem:** You are given an `n x n` 2D `matrix` representing an image, rotate the image by 90 degrees (clockwise). You have to rotate the image in-place.

```python
def rotate(matrix: list[list[int]]) -> None:
    n = len(matrix)
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    for i in range(n):
        matrix[i].reverse()
```

**Example 1:**
**Input:** `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
**Output:** `[[7,4,1],[8,5,2],[9,6,3]]`

**Example 2:**
**Input:** `matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]`
**Output:** `[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]`

---

#### 73. Set Matrix Zeroes
**Link:** [https://leetcode.com/problems/set-matrix-zeroes/](https://leetcode.com/problems/set-matrix-zeroes/)  
**Problem:** Given an `m x n` integer matrix, if an element is 0, set its entire row and column to 0's. You must do it in place.

```python
def setZeroes(matrix: list[list[int]]) -> None:
    m, n = len(matrix), len(matrix[0])
    first_row = any(matrix[0][j] == 0 for j in range(n))
    first_col = any(matrix[i][0] == 0 for i in range(m))
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[i][0] = 0
                matrix[0][j] = 0
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0
    if first_row:
        for j in range(n):
            matrix[0][j] = 0
    if first_col:
        for i in range(m):
            matrix[i][0] = 0
```

**Example 1:**
**Input:** `matrix = [[1,1,1],[1,0,1],[1,1,1]]`
**Output:** `[[1,0,1],[0,0,0],[1,0,1]]`

**Example 2:**
**Input:** `matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
**Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

---

### Pattern: Dutch National Flag

---

#### 75. Sort Colors
**Link:** [https://leetcode.com/problems/sort-colors/](https://leetcode.com/problems/sort-colors/)  
**Problem:** Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue. We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

```python
def sortColors(nums: list[int]) -> None:
    lo, mid, hi = 0, 0, len(nums) - 1
    while mid <= hi:
        if nums[mid] == 0:
            nums[lo], nums[mid] = nums[mid], nums[lo]
            lo += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[mid], nums[hi] = nums[hi], nums[mid]
            hi -= 1
```

**Example 1:**
**Input:** `nums = [2,0,2,1,1,0]`
**Output:** `[0,0,1,1,2,2]`

**Example 2:**
**Input:** `nums = [2,0,1]`
**Output:** `[0,1,2]`

---

### Pattern: Cyclic Sort

---

#### 41. First Missing Positive
**Link:** [https://leetcode.com/problems/first-missing-positive/](https://leetcode.com/problems/first-missing-positive/)  
**Problem:** Given an unsorted integer array `nums`, return the smallest missing positive integer. You must implement an algorithm that runs in O(n) time and uses O(1) auxiliary space.

```python
def firstMissingPositive(nums: list[int]) -> int:
    n = len(nums)
    for i in range(n):
        while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
            target_idx = nums[i] - 1
            nums[i], nums[target_idx] = nums[target_idx], nums[i]
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    return n + 1
```

**Example 1:**
**Input:** `nums = [1,2,0]`
**Output:** `3`
**Explanation:** The numbers in the range [1,2] are all in the array.

**Example 2:**
**Input:** `nums = [3,4,-1,1]`
**Output:** `2`
**Explanation:** 1 is in the array but 2 is missing.

---

#### 448. Find All Numbers Disappeared in an Array
**Link:** [https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)  
**Problem:** Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

```python
def findDisappearedNumbers(nums: list[int]) -> list[int]:
    for i in range(len(nums)):
        idx = abs(nums[i]) - 1
        if nums[idx] > 0:
            nums[idx] = -nums[idx]
    return [i + 1 for i in range(len(nums)) if nums[i] > 0]
```

**Example 1:**
**Input:** `nums = [4,3,2,7,8,2,3,1]`
**Output:** `[5,6]`

**Example 2:**
**Input:** `nums = [1,1]`
**Output:** `[2]`

---

#### 287. Find the Duplicate Number
**Link:** [https://leetcode.com/problems/find-the-duplicate-number/](https://leetcode.com/problems/find-the-duplicate-number/)  
**Problem:** Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive, there is only one repeated number in `nums`, return this repeated number. You must solve the problem without modifying the array and uses only constant extra space.

```python
def findDuplicate(nums: list[int]) -> int:
    slow = nums[0]
    fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```

**Example 1:**
**Input:** `nums = [1,3,4,2,2]`
**Output:** `2`

**Example 2:**
**Input:** `nums = [3,1,3,4,2]`
**Output:** `3`

---

## 2. STRINGS

### Pattern: Two Pointers

---

#### 125. Valid Palindrome
**Link:** [https://leetcode.com/problems/valid-palindrome/](https://leetcode.com/problems/valid-palindrome/)  
**Problem:** A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Given a string `s`, return true if it is a palindrome, or false otherwise.

```python
def isPalindrome(s: str) -> bool:
    l, r = 0, len(s) - 1
    while l < r:
        while l < r and not s[l].isalnum():
            l += 1
        while l < r and not s[r].isalnum():
            r -= 1
        if s[l].lower() != s[r].lower():
            return False
        l += 1
        r -= 1
    return True
```

**Example 1:**
**Input:** `s = "A man, a plan, a canal: Panama"`
**Output:** `true`
**Explanation:** "amanaplanacanalpanama" is a palindrome.

**Example 2:**
**Input:** `s = "race a car"`
**Output:** `false`
**Explanation:** "raceacar" is not a palindrome.

---

#### 344. Reverse String
**Link:** [https://leetcode.com/problems/reverse-string/](https://leetcode.com/problems/reverse-string/)  
**Problem:** Write a function that reverses a string. The input string is given as an array of characters `s`. You must do this by modifying the input array in-place with O(1) extra memory.

```python
def reverseString(s: list[str]) -> None:
    l, r = 0, len(s) - 1
    while l < r:
        s[l], s[r] = s[r], s[l]
        l += 1
        r -= 1
```

**Example 1:**
**Input:** `s = ["h","e","l","l","o"]`
**Output:** `["o","l","l","e","h"]`

**Example 2:**
**Input:** `s = ["H","a","n","n","a","h"]`
**Output:** `["h","a","n","n","a","H"]`

---

#### 345. Reverse Vowels of a String
**Link:** [https://leetcode.com/problems/reverse-vowels-of-a-string/](https://leetcode.com/problems/reverse-vowels-of-a-string/)  
**Problem:** Given a string `s`, reverse only all the vowels in the string and return it.

```python
def reverseVowels(s: str) -> str:
    vowels = set("aeiouAEIOU")
    chars = list(s)
    l, r = 0, len(chars) - 1
    while l < r:
        while l < r and chars[l] not in vowels:
            l += 1
        while l < r and chars[r] not in vowels:
            r -= 1
        if l < r:
            chars[l], chars[r] = chars[r], chars[l]
            l += 1
            r -= 1
    return "".join(chars)
```

**Example 1:**
**Input:** `s = "hello"`
**Output:** `"holle"`

**Example 2:**
**Input:** `s = "leetcode"`
**Output:** `"leotcede"`

---

### Pattern: Sliding Window (Strings)

---

#### 76. Minimum Window Substring
**Link:** [https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/)  
**Problem:** Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string "".

```python
from collections import Counter, defaultdict

def minWindow(s: str, t: str) -> str:
    need = Counter(t)
    have = defaultdict(int)
    formed, required = 0, len(need)
    l, min_len, start = 0, float('inf'), 0
    for r in range(len(s)):
        c = s[r]
        have[c] += 1
        if c in need and have[c] == need[c]:
            formed += 1
        while formed == required:
            if r - l + 1 < min_len:
                min_len = r - l + 1
                start = l
            have[s[l]] -= 1
            if s[l] in need and have[s[l]] < need[s[l]]:
                formed -= 1
            l += 1
    return "" if min_len == float('inf') else s[start:start + min_len]
```

**Example 1:**
**Input:** `s = "ADOBECODEBANC", t = "ABC"`
**Output:** `"BANC"`
**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example 2:**
**Input:** `s = "a", t = "a"`
**Output:** `"a"`

---

#### 567. Permutation in String
**Link:** [https://leetcode.com/problems/permutation-in-string/](https://leetcode.com/problems/permutation-in-string/)  
**Problem:** Given two strings `s1` and `s2`, return true if `s2` contains a permutation of `s1`, or false otherwise. In other words, return true if one of `s1`'s permutations is the substring of `s2`.

```python
def checkInclusion(s1: str, s2: str) -> bool:
    if len(s1) > len(s2):
        return False
    need = [0] * 26
    have = [0] * 26
    for c in s1:
        need[ord(c) - ord('a')] += 1
    for i in range(len(s1)):
        have[ord(s2[i]) - ord('a')] += 1
    if need == have:
        return True
    for i in range(len(s1), len(s2)):
        have[ord(s2[i]) - ord('a')] += 1
        have[ord(s2[i - len(s1)]) - ord('a')] -= 1
        if need == have:
            return True
    return False
```

**Example 1:**
**Input:** `s1 = "ab", s2 = "eidbaooo"`
**Output:** `true`
**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**
**Input:** `s1 = "ab", s2 = "eidboaoo"`
**Output:** `false`

---

### Pattern: Character Frequency

---

#### 242. Valid Anagram
**Link:** [https://leetcode.com/problems/valid-anagram/](https://leetcode.com/problems/valid-anagram/)  
**Problem:** Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.

```python
from collections import Counter

def isAnagram(s: str, t: str) -> bool:
    return Counter(s) == Counter(t)
```

**Example 1:**
**Input:** `s = "anagram", t = "nagaram"`
**Output:** `true`

**Example 2:**
**Input:** `s = "rat", t = "car"`
**Output:** `false`

---

#### 383. Ransom Note
**Link:** [https://leetcode.com/problems/ransom-note/](https://leetcode.com/problems/ransom-note/)  
**Problem:** Given two strings `ransomNote` and `magazine`, return true if `ransomNote` can be constructed by using the letters from `magazine` and false otherwise. Each letter in `magazine` can only be used once in `ransomNote`.

```python
from collections import Counter

def canConstruct(ransomNote: str, magazine: str) -> bool:
    cnt = Counter(magazine)
    for c in ransomNote:
        if cnt[c] <= 0:
            return False
        cnt[c] -= 1
    return True
```

**Example 1:**
**Input:** `ransomNote = "a", magazine = "b"`
**Output:** `false`

**Example 2:**
**Input:** `ransomNote = "aa", magazine = "ab"`
**Output:** `false`

**Example 3:**
**Input:** `ransomNote = "aa", magazine = "aab"`
**Output:** `true`

---

#### 389. Find the Difference
**Link:** [https://leetcode.com/problems/find-the-difference/](https://leetcode.com/problems/find-the-difference/)  
**Problem:** You are given two strings `s` and `t`. String `t` is generated by random shuffling string `s` and then add one more letter at a random position. Return the letter that was added to `t`.

```python
def findTheDifference(s: str, t: str) -> str:
    res = 0
    for c in s:
        res ^= ord(c)
    for c in t:
        res ^= ord(c)
    return chr(res)
```

**Example 1:**
**Input:** `s = "abcd", t = "abcde"`
**Output:** `"e"`
**Explanation:** 'e' is the letter that was added.

**Example 2:**
**Input:** `s = "", t = "y"`
**Output:** `"y"`

---

### Pattern: Hashing (Strings)

---

#### 49. Group Anagrams
**Link:** [https://leetcode.com/problems/group-anagrams/](https://leetcode.com/problems/group-anagrams/)  
**Problem:** Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

```python
from collections import defaultdict

def groupAnagrams(strs: list[str]) -> list[list[str]]:
    mp = defaultdict(list)
    for s in strs:
        key = "".join(sorted(s))
        mp[key].append(s)
    return list(mp.values())
```

**Example 1:**
**Input:** `strs = ["eat","tea","tan","ate","nat","bat"]`
**Output:** `[["bat"],["nat","tan"],["ate","eat","tea"]]`

**Example 2:**
**Input:** `strs = [""]`
**Output:** `[[""]]`

**Example 3:**
**Input:** `strs = ["a"]`
**Output:** `[["a"]]`

---

#### 205. Isomorphic Strings
**Link:** [https://leetcode.com/problems/isomorphic-strings/](https://leetcode.com/problems/isomorphic-strings/)  
**Problem:** Given two strings `s` and `t`, determine if they are isomorphic. Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

```python
def isIsomorphic(s: str, t: str) -> bool:
    s2t, t2s = {}, {}
    for c1, c2 in zip(s, t):
        if (c1 in s2t and s2t[c1] != c2) or (c2 in t2s and t2s[c2] != c1):
            return False
        s2t[c1] = c2
        t2s[c2] = c1
    return True
```

**Example 1:**
**Input:** `s = "egg", t = "add"`
**Output:** `true`

**Example 2:**
**Input:** `s = "foo", t = "bar"`
**Output:** `false`

**Example 3:**
**Input:** `s = "paper", t = "title"`
**Output:** `true`

---

#### 290. Word Pattern
**Link:** [https://leetcode.com/problems/word-pattern/](https://leetcode.com/problems/word-pattern/)  
**Problem:** Given a `pattern` and a string `s`, find if `s` follows the same pattern. Here, "follow" means a full match, such that there is a bijection between a letter in `pattern` and a non-empty word in `s`.

```python
def wordPattern(pattern: str, s: str) -> bool:
    words = s.split()
    if len(pattern) != len(words):
        return False
    c2w, w2c = {}, {}
    for c, w in zip(pattern, words):
        if (c in c2w and c2w[c] != w) or (w in w2c and w2c[w] != c):
            return False
        c2w[c] = w
        w2c[w] = c
    return True
```

**Example 1:**
**Input:** `pattern = "abba", s = "dog cat cat dog"`
**Output:** `true`

**Example 2:**
**Input:** `pattern = "abba", s = "dog cat cat fish"`
**Output:** `false`

**Example 3:**
**Input:** `pattern = "aaaa", s = "dog cat cat dog"`
**Output:** `false`

---

### Pattern: String Parsing

---

#### 8. String to Integer (atoi)
**Link:** [https://leetcode.com/problems/string-to-integer-atoi/](https://leetcode.com/problems/string-to-integer-atoi/)  
**Problem:** Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer.

```python
def myAtoi(s: str) -> int:
    i = 0
    sign = 1
    res = 0
    INT_MAX = 2**31 - 1
    INT_MIN = -2**31
    while i < len(s) and s[i] == ' ':
        i += 1
    if i < len(s) and s[i] in ('-', '+'):
        sign = 1 if s[i] == '+' else -1
        i += 1
    while i < len(s) and s[i].isdigit():
        res = res * 10 + int(s[i])
        if res * sign > INT_MAX:
            return INT_MAX
        if res * sign < INT_MIN:
            return INT_MIN
        i += 1
    return res * sign
```

**Example 1:**
**Input:** `s = "42"`
**Output:** `42`

**Example 2:**
**Input:** `s = "   -42"`
**Output:** `-42`

**Example 3:**
**Input:** `s = "4193 with words"`
**Output:** `4193`

---

#### 394. Decode String
**Link:** [https://leetcode.com/problems/decode-string/](https://leetcode.com/problems/decode-string/)  
**Problem:** Given an encoded string, return its decoded string. The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times.

```python
def decodeString(s: str) -> str:
    counts = []
    stk = []
    cur = ""
    k = 0
    for c in s:
        if c.isdigit():
            k = k * 10 + int(c)
        elif c == '[':
            counts.append(k)
            stk.append(cur)
            k = 0
            cur = ""
        elif c == ']':
            count = counts.pop()
            prev = stk.pop()
            cur = prev + cur * count
        else:
            cur += c
    return cur
```

**Example 1:**
**Input:** `s = "3[a]2[bc]"`
**Output:** `"aaabcbc"`

**Example 2:**
**Input:** `s = "3[a2[c]]"`
**Output:** `"accaccacc"`

**Example 3:**
**Input:** `s = "2[abc]3[cd]ef"`
**Output:** `"abcabccdcdcdef"`

---

### Pattern: KMP

---

#### 28. Find the Index of the First Occurrence in a String
**Link:** [https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)  
**Problem:** Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or -1 if `needle` is not part of `haystack`.

```python
def strStr(haystack: str, needle: str) -> int:
    m, n = len(needle), len(haystack)
    if m == 0:
        return 0
    lps = [0] * m
    j = 0
    for i in range(1, m):
        while j > 0 and needle[i] != needle[j]:
            j = lps[j - 1]
        if needle[i] == needle[j]:
            j += 1
            lps[i] = j
    j = 0
    for i in range(n):
        while j > 0 and haystack[i] != needle[j]:
            j = lps[j - 1]
        if haystack[i] == needle[j]:
            j += 1
        if j == m:
            return i - m + 1
    return -1
```

**Example 1:**
**Input:** `haystack = "sadbutsad", needle = "sad"`
**Output:** `0`
**Explanation:** "sad" occurs at index 0 and 6. The first occurrence is at index 0, so we return 0.

**Example 2:**
**Input:** `haystack = "leetcode", needle = "leeto"`
**Output:** `-1`
**Explanation:** "leeto" did not occur in "leetcode", so we return -1.

---

### Pattern: Trie

---

#### 208. Implement Trie (Prefix Tree)
**Link:** [https://leetcode.com/problems/implement-trie-prefix-tree/](https://leetcode.com/problems/implement-trie-prefix-tree/)  
**Problem:** A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. Implement the Trie class with `insert`, `search`, and `startsWith` methods.

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for c in word:
            if c not in node.children:
                node.children[c] = TrieNode()
            node = node.children[c]
        node.is_end = True

    def search(self, word: str) -> bool:
        node = self.root
        for c in word:
            if c not in node.children:
                return False
            node = node.children[c]
        return node.is_end

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for c in prefix:
            if c not in node.children:
                return False
            node = node.children[c]
        return True
```

**Example 1:**
**Input:**
`["Trie", "insert", "search", "search", "startsWith", "insert", "search"]`
`[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]`
**Output:**
`[null, null, true, false, true, null, true]`
**Explanation:**
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True

---

## 3. HASHING

### Pattern: Frequency Count

---

#### 347. Top K Frequent Elements
**Link:** [https://leetcode.com/problems/top-k-frequent-elements/](https://leetcode.com/problems/top-k-frequent-elements/)  
**Problem:** Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

```python
from collections import Counter

def topKFrequent(nums: list[int], k: int) -> list[int]:
    freq = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for n, f in freq.items():
        buckets[f].append(n)
    res = []
    for i in range(len(buckets) - 1, -1, -1):
        for n in buckets[i]:
            res.append(n)
            if len(res) == k:
                return res
    return res
```

**Example 1:**
**Input:** `nums = [1,1,1,2,2,3], k = 2`
**Output:** `[1,2]`

**Example 2:**
**Input:** `nums = [1], k = 1`
**Output:** `[1]`

---

#### 451. Sort Characters By Frequency
**Link:** [https://leetcode.com/problems/sort-characters-by-frequency/](https://leetcode.com/problems/sort-characters-by-frequency/)  
**Problem:** Given a string `s`, sort it in decreasing order based on the frequency of the characters. The frequency of a character is the number of times it appears in the string. Return the sorted string. If there are multiple answers, return any of them.

```python
from collections import Counter

def frequencySort(s: str) -> str:
    freq = Counter(s)
    return "".join(c * f for c, f in freq.most_common())
```

**Example 1:**
**Input:** `s = "tree"`
**Output:** `"eert"`
**Explanation:** 'e' appears twice while 'r' and 't' both appear once. So 'e' must appear before both 'r' and 't'.

**Example 2:**
**Input:** `s = "cccaaa"`
**Output:** `"aaaccc"`
**Explanation:** Both 'c' and 'a' appear three times, so both "cccaaa" and "aaaccc" are valid answers.

**Example 3:**
**Input:** `s = "Aabb"`
**Output:** `"bbAa"`

---

### Pattern: HashMap Lookup

---

#### 1. Two Sum
**Link:** [https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)  
**Problem:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`. You may assume that each input would have exactly one solution, and you may not use the same element twice.

```python
def twoSum(nums: list[int], target: int) -> list[int]:
    mp = {}
    for i, n in enumerate(nums):
        comp = target - n
        if comp in mp:
            return [mp[comp], i]
        mp[n] = i
    return []
```

**Example 1:**
**Input:** `nums = [2,7,11,15], target = 9`
**Output:** `[0,1]`
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**
**Input:** `nums = [3,2,4], target = 6`
**Output:** `[1,2]`

**Example 3:**
**Input:** `nums = [3,3], target = 6`
**Output:** `[0,1]`

---

#### 217. Contains Duplicate
**Link:** [https://leetcode.com/problems/contains-duplicate/](https://leetcode.com/problems/contains-duplicate/)  
**Problem:** Given an integer array `nums`, return true if any value appears at least twice in the array, and return false if every element is distinct.

```python
def containsDuplicate(nums: list[int]) -> bool:
    seen = set()
    for n in nums:
        if n in seen:
            return True
        seen.add(n)
    return False
```

**Example 1:**
**Input:** `nums = [1,2,3,1]`
**Output:** `true`

**Example 2:**
**Input:** `nums = [1,2,3,4]`
**Output:** `false`

**Example 3:**
**Input:** `nums = [1,1,1,3,3,4,3,2,4,2]`
**Output:** `true`

---

#### 202. Happy Number
**Link:** [https://leetcode.com/problems/happy-number/](https://leetcode.com/problems/happy-number/)  
**Problem:** Write an algorithm to determine if a number `n` is happy. A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.

```python
def isHappy(n: int) -> bool:
    seen = set()
    while n != 1:
        total = 0
        while n > 0:
            total += (n % 10) ** 2
            n //= 10
        if total in seen:
            return False
        seen.add(total)
        n = total
    return True
```

**Example 1:**
**Input:** `n = 19`
**Output:** `true`
**Explanation:**
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1

**Example 2:**
**Input:** `n = 2`
**Output:** `false`

---

### Pattern: Prefix Sum + HashMap

---

#### 525. Contiguous Array
**Link:** [https://leetcode.com/problems/contiguous-array/](https://leetcode.com/problems/contiguous-array/)  
**Problem:** Given a binary array `nums`, return the maximum length of a contiguous subarray with an equal number of 0 and 1.

```python
def findMaxLength(nums: list[int]) -> int:
    mp = {0: -1}
    cur_sum = 0
    res = 0
    for i, n in enumerate(nums):
        cur_sum += 1 if n == 1 else -1
        if cur_sum in mp:
            res = max(res, i - mp[cur_sum])
        else:
            mp[cur_sum] = i
    return res
```

**Example 1:**
**Input:** `nums = [0,1]`
**Output:** `2`
**Explanation:** [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.

**Example 2:**
**Input:** `nums = [0,1,0]`
**Output:** `2`
**Explanation:** [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.

---

### Pattern: HashSet

---

#### 128. Longest Consecutive Sequence
**Link:** [https://leetcode.com/problems/longest-consecutive-sequence/](https://leetcode.com/problems/longest-consecutive-sequence/)  
**Problem:** Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence. You must write an algorithm that runs in O(n) time.

```python
def longestConsecutive(nums: list[int]) -> int:
    st = set(nums)
    res = 0
    for n in st:
        if n - 1 not in st:
            cur = n
            length = 1
            while cur + 1 in st:
                cur += 1
                length += 1
            res = max(res, length)
    return res
```

**Example 1:**
**Input:** `nums = [100,4,200,1,3,2]`
**Output:** `4`
**Explanation:** The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

**Example 2:**
**Input:** `nums = [0,3,7,2,5,8,4,6,0,1]`
**Output:** `9`

---

#### 349. Intersection of Two Arrays
**Link:** [https://leetcode.com/problems/intersection-of-two-arrays/](https://leetcode.com/problems/intersection-of-two-arrays/)  
**Problem:** Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

```python
def intersection(nums1: list[int], nums2: list[int]) -> list[int]:
    return list(set(nums1) & set(nums2))
```

**Example 1:**
**Input:** `nums1 = [1,2,2,1], nums2 = [2,2]`
**Output:** `[2]`

**Example 2:**
**Input:** `nums1 = [4,9,5], nums2 = [9,4,9,8,4]`
**Output:** `[9,4]`
**Explanation:** [4,9] is also accepted.

---

### Pattern: Grouping

---

#### 609. Find Duplicate File in System
**Link:** [https://leetcode.com/problems/find-duplicate-file-in-system/](https://leetcode.com/problems/find-duplicate-file-in-system/)  
**Problem:** Given a list `paths` of directory info, find all the groups of duplicate files. A group of duplicate files consists of at least two files that have the same content.

```python
from collections import defaultdict

def findDuplicate(paths: list[str]) -> list[list[str]]:
    mp = defaultdict(list)
    for path in paths:
        parts = path.split()
        directory = parts[0]
        for file in parts[1:]:
            l = file.find('(')
            r = file.find(')')
            content = file[l + 1:r]
            file_name = directory + "/" + file[:l]
            mp[content].append(file_name)
    return [v for v in mp.values() if len(v) > 1]
```

**Example 1:**
**Input:** `paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)","root 4.txt(efgh)"]`
**Output:** `[["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]`

**Example 2:**
**Input:** `paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)"]`
**Output:** `[["root/a/2.txt","root/c/d/4.txt"],["root/a/1.txt","root/c/3.txt"]]`

---

## 4. LINKED LIST

### Pattern: Fast & Slow Pointer

---

#### 141. Linked List Cycle
**Link:** [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)  
**Problem:** Given `head`, the head of a linked list, determine if the linked list has a cycle in it. Return true if there is a cycle in the linked list, otherwise, return false.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def hasCycle(head: ListNode | None) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

**Example 1:**
**Input:** `head = [3,2,0,-4], pos = 1`
**Output:** `true`
**Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**
**Input:** `head = [1,2], pos = 0`
**Output:** `true`
**Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**
**Input:** `head = [1], pos = -1`
**Output:** `false`

---

#### 876. Middle of the Linked List
**Link:** [https://leetcode.com/problems/middle-of-the-linked-list/](https://leetcode.com/problems/middle-of-the-linked-list/)  
**Problem:** Given the `head` of a singly linked list, return the middle node of the linked list. If there are two middle nodes, return the second middle node.

```python
def middleNode(head: ListNode | None) -> ListNode | None:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

**Example 1:**
**Input:** `head = [1,2,3,4,5]`
**Output:** `[3,4,5]`
**Explanation:** The middle node of the list is node 3.

**Example 2:**
**Input:** `head = [1,2,3,4,5,6]`
**Output:** `[4,5,6]`
**Explanation:** Since the list has two middle nodes with values 3 and 4, we return the second one.

---

### Pattern: Reverse

---

#### 206. Reverse Linked List
**Link:** [https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)  
**Problem:** Given the `head` of a singly linked list, reverse the list, and return the reversed list.

```python
def reverseList(head: ListNode | None) -> ListNode | None:
    prev = None
    cur = head
    while cur:
        nxt = cur.next
        cur.next = prev
        prev = cur
        cur = nxt
    return prev
```

**Example 1:**
**Input:** `head = [1,2,3,4,5]`
**Output:** `[5,4,3,2,1]`

**Example 2:**
**Input:** `head = [1,2]`
**Output:** `[2,1]`

**Example 3:**
**Input:** `head = []`
**Output:** `[]`

---

#### 92. Reverse Linked List II
**Link:** [https://leetcode.com/problems/reverse-linked-list-ii/](https://leetcode.com/problems/reverse-linked-list-ii/)  
**Problem:** Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

```python
def reverseBetween(head: ListNode | None, left: int, right: int) -> ListNode | None:
    dummy = ListNode(0, head)
    pre = dummy
    for _ in range(left - 1):
        pre = pre.next
    cur = pre.next
    for _ in range(right - left):
        nxt = cur.next
        cur.next = nxt.next
        nxt.next = pre.next
        pre.next = nxt
    return dummy.next
```

**Example 1:**
**Input:** `head = [1,2,3,4,5], left = 2, right = 4`
**Output:** `[1,4,3,2,5]`

**Example 2:**
**Input:** `head = [5], left = 1, right = 1`
**Output:** `[5]`

---

#### 25. Reverse Nodes in k-Group
**Link:** [https://leetcode.com/problems/reverse-nodes-in-k-group/](https://leetcode.com/problems/reverse-nodes-in-k-group/)  
**Problem:** Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list. If the number of nodes is not a multiple of `k` then left-out nodes in the end should remain as it is.

```python
def reverseKGroup(head: ListNode | None, k: int) -> ListNode | None:
    cur = head
    count = 0
    while cur and count < k:
        cur = cur.next
        count += 1
    if count < k:
        return head
    prev = None
    node = head
    for _ in range(k):
        nxt = node.next
        node.next = prev
        prev = node
        node = nxt
    head.next = reverseKGroup(node, k)
    return prev
```

**Example 1:**
**Input:** `head = [1,2,3,4,5], k = 2`
**Output:** `[2,1,4,3,5]`

**Example 2:**
**Input:** `head = [1,2,3,4,5], k = 3`
**Output:** `[3,2,1,4,5]`

---

### Pattern: Merge

---

#### 21. Merge Two Sorted Lists
**Link:** [https://leetcode.com/problems/merge-two-sorted-lists/](https://leetcode.com/problems/merge-two-sorted-lists/)  
**Problem:** You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists. Return the head of the merged linked list.

```python
def mergeTwoLists(l1: ListNode | None, l2: ListNode | None) -> ListNode | None:
    dummy = ListNode(0)
    cur = dummy
    while l1 and l2:
        if l1.val <= l2.val:
            cur.next = l1
            l1 = l1.next
        else:
            cur.next = l2
            l2 = l2.next
        cur = cur.next
    cur.next = l1 if l1 else l2
    return dummy.next
```

**Example 1:**
**Input:** `list1 = [1,2,4], list2 = [1,3,4]`
**Output:** `[1,1,2,3,4,4]`

**Example 2:**
**Input:** `list1 = [], list2 = []`
**Output:** `[]`

**Example 3:**
**Input:** `list1 = [], list2 = [0]`
**Output:** `[0]`

---

#### 23. Merge K Sorted Lists
**Link:** [https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/)  
**Problem:** You are given an array of `k` linked-lists lists, each linked-list is sorted in ascending order. Merge all the linked-lists into one sorted linked-list and return it.

```python
import heapq

def mergeKLists(lists: list[ListNode | None]) -> ListNode | None:
    pq = []
    for i, node in enumerate(lists):
        if node:
            heapq.heappush(pq, (node.val, i, node))
    dummy = ListNode(0)
    cur = dummy
    while pq:
        val, i, node = heapq.heappop(pq)
        cur.next = node
        cur = cur.next
        if node.next:
            heapq.heappush(pq, (node.next.val, i, node.next))
    return dummy.next
```

**Example 1:**
**Input:** `lists = [[1,4,5],[1,3,4],[2,6]]`
**Output:** `[1,1,2,3,4,4,5,6]`
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

**Example 2:**
**Input:** `lists = []`
**Output:** `[]`

**Example 3:**
**Input:** `lists = [[]]`
**Output:** `[]`

---

### Pattern: Dummy Node

---

#### 19. Remove Nth Node From End of List
**Link:** [https://leetcode.com/problems/remove-nth-node-from-end-of-list/](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
**Problem:** Given the `head` of a linked list, remove the nth node from the end of the list and return its head.

```python
def removeNthFromEnd(head: ListNode | None, n: int) -> ListNode | None:
    dummy = ListNode(0, head)
    fast = slow = dummy
    for _ in range(n + 1):
        fast = fast.next
    while fast:
        fast = fast.next
        slow = slow.next
    slow.next = slow.next.next
    return dummy.next
```

**Example 1:**
**Input:** `head = [1,2,3,4,5], n = 2`
**Output:** `[1,2,3,5]`

**Example 2:**
**Input:** `head = [1], n = 1`
**Output:** `[]`

**Example 3:**
**Input:** `head = [1,2], n = 1`
**Output:** `[1]`

---

#### 24. Swap Nodes in Pairs
**Link:** [https://leetcode.com/problems/swap-nodes-in-pairs/](https://leetcode.com/problems/swap-nodes-in-pairs/)  
**Problem:** Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes.

```python
def swapPairs(head: ListNode | None) -> ListNode | None:
    dummy = ListNode(0, head)
    pre = dummy
    while pre.next and pre.next.next:
        a = pre.next
        b = pre.next.next
        pre.next = b
        a.next = b.next
        b.next = a
        pre = a
    return dummy.next
```

**Example 1:**
**Input:** `head = [1,2,3,4]`
**Output:** `[2,1,4,3]`

**Example 2:**
**Input:** `head = []`
**Output:** `[]`

**Example 3:**
**Input:** `head = [1]`
**Output:** `[1]`

---

## 5. STACK

### Pattern: Basic Stack

---

#### 20. Valid Parentheses
**Link:** [https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/)  
**Problem:** Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

```python
def isValid(s: str) -> bool:
    st = []
    mapping = {')': '(', '}': '{', ']': '['}
    for c in s:
        if c in '({[':
            st.append(c)
        else:
            if not st or st[-1] != mapping[c]:
                return False
            st.pop()
    return len(st) == 0
```

**Example 1:**
**Input:** `s = "()"`
**Output:** `true`

**Example 2:**
**Input:** `s = "()[]{}"`
**Output:** `true`

**Example 3:**
**Input:** `s = "(]"`
**Output:** `false`

---

#### 155. Min Stack
**Link:** [https://leetcode.com/problems/min-stack/](https://leetcode.com/problems/min-stack/)  
**Problem:** Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

```python
class MinStack:
    def __init__(self):
        self.st = []

    def push(self, val: int) -> None:
        min_val = val if not self.st else min(val, self.st[-1][1])
        self.st.append((val, min_val))

    def pop(self) -> None:
        self.st.pop()

    def top(self) -> int:
        return self.st[-1][0]

    def getMin(self) -> int:
        return self.st[-1][1]
```

**Example 1:**
**Input:**
`["MinStack","push","push","push","getMin","pop","top","getMin"]`
`[[],[-2],[0],[-3],[],[],[],[]]`
**Output:**
`[null,null,null,null,-3,null,0,-2]`
**Explanation:**
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2

---

### Pattern: Monotonic Stack (Decreasing)

---

#### 496. Next Greater Element I
**Link:** [https://leetcode.com/problems/next-greater-element-i/](https://leetcode.com/problems/next-greater-element-i/)  
**Problem:** The next greater element of some element `x` in an array is the first greater element that is to the right of `x` in the same array. Given two distinct 0-indexed integer arrays `nums1` and `nums2`, return an array `ans` of length `nums1.length` such that `ans[i]` is the next greater element of `nums1[i]` in `nums2`.

```python
def nextGreaterElement(nums1: list[int], nums2: list[int]) -> list[int]:
    nge = {}
    st = []
    for n in nums2:
        while st and st[-1] < n:
            nge[st.pop()] = n
        st.append(n)
    return [nge.get(n, -1) for n in nums1]
```

**Example 1:**
**Input:** `nums1 = [4,1,2], nums2 = [1,3,4,2]`
**Output:** `[-1,3,-1]`
**Explanation:**
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**Example 2:**
**Input:** `nums1 = [2,4], nums2 = [1,2,3,4]`
**Output:** `[3,-1]`

---

#### 739. Daily Temperatures
**Link:** [https://leetcode.com/problems/daily-temperatures/](https://leetcode.com/problems/daily-temperatures/)  
**Problem:** Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

```python
def dailyTemperatures(temperatures: list[int]) -> list[int]:
    n = len(temperatures)
    res = [0] * n
    st = []
    for i, t in enumerate(temperatures):
        while st and t > temperatures[st[-1]]:
            prev_i = st.pop()
            res[prev_i] = i - prev_i
        st.append(i)
    return res
```

**Example 1:**
**Input:** `temperatures = [73,74,75,71,69,72,76,73]`
**Output:** `[1,1,4,2,1,1,0,0]`

**Example 2:**
**Input:** `temperatures = [30,40,50,60]`
**Output:** `[1,1,1,0]`

**Example 3:**
**Input:** `temperatures = [30,60,90]`
**Output:** `[1,1,0]`

---

### Pattern: Monotonic Stack (Increasing)

---

#### 84. Largest Rectangle in Histogram
**Link:** [https://leetcode.com/problems/largest-rectangle-in-histogram/](https://leetcode.com/problems/largest-rectangle-in-histogram/)  
**Problem:** Given an array of integers `heights` representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

```python
def largestRectangleArea(heights: list[int]) -> int:
    st = []
    res = 0
    h_extended = heights + [0]
    for i, h in enumerate(h_extended):
        while st and h < h_extended[st[-1]]:
            height = h_extended[st.pop()]
            width = i if not st else i - st[-1] - 1
            res = max(res, height * width)
        st.append(i)
    return res
```

**Example 1:**
**Input:** `heights = [2,1,5,6,2,3]`
**Output:** `10`
**Explanation:** The above is a histogram where width of each bar is 1. The largest rectangle is shown in the red area, which has an area = 10 units.

**Example 2:**
**Input:** `heights = [2,4]`
**Output:** `4`

---

### Pattern: Expression Evaluation

---

#### 150. Evaluate Reverse Polish Notation
**Link:** [https://leetcode.com/problems/evaluate-reverse-polish-notation/](https://leetcode.com/problems/evaluate-reverse-polish-notation/)  
**Problem:** You are given an array of strings `tokens` that represents an arithmetic expression in Reverse Polish Notation. Evaluate the expression. Return an integer that represents the value of the expression.

```python
def evalRPN(tokens: list[str]) -> int:
    st = []
    for t in tokens:
        if t in ("+", "-", "*", "/"):
            b = st.pop()
            a = st.pop()
            if t == "+":
                st.append(a + b)
            elif t == "-":
                st.append(a - b)
            elif t == "*":
                st.append(a * b)
            else:
                # truncate toward zero
                st.append(int(a / b))
        else:
            st.append(int(t))
    return st[0]
```

**Example 1:**
**Input:** `tokens = ["2","1","+","3","*"]`
**Output:** `9`
**Explanation:** ((2 + 1) * 3) = 9

**Example 2:**
**Input:** `tokens = ["4","13","5","/","+"]`
**Output:** `6`
**Explanation:** (4 + (13 / 5)) = 6

**Example 3:**
**Input:** `tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]`
**Output:** `22`
**Explanation:** ((10 * (6 / ((9 + 3) * -11))) + 17) + 5

---

## 6. QUEUE / DEQUE

### Pattern: BFS Queue

---

#### 102. Binary Tree Level Order Traversal
**Link:** [https://leetcode.com/problems/binary-tree-level-order-traversal/](https://leetcode.com/problems/binary-tree-level-order-traversal/)  
**Problem:** Given the `root` of a binary tree, return the level order traversal of its nodes' values (i.e., from left to right, level by level).

```python
from collections import deque

def levelOrder(root: TreeNode | None) -> list[list[int]]:
    res = []
    if not root:
        return res
    q = deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(level)
    return res
```

**Example 1:**
**Input:** `root = [3,9,20,null,null,15,7]`
**Output:** `[[3],[9,20],[15,7]]`

**Example 2:**
**Input:** `root = [1]`
**Output:** `[[1]]`

**Example 3:**
**Input:** `root = []`
**Output:** `[]`

---

#### 994. Rotting Oranges
**Link:** [https://leetcode.com/problems/rotting-oranges/](https://leetcode.com/problems/rotting-oranges/)  
**Problem:** You are given an `m x n` grid where each cell can have one of three values: 0 (empty), 1 (fresh orange), or 2 (rotten orange). Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten. Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

```python
from collections import deque

def orangesRotting(grid: list[list[int]]) -> int:
    m, n = len(grid), len(grid[0])
    q = deque()
    fresh = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                q.append((i, j))
            elif grid[i][j] == 1:
                fresh += 1
    time = 0
    dirs = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    while q and fresh:
        time += 1
        for _ in range(len(q)):
            r, c = q.popleft()
            for dr, dc in dirs:
                nr, nc = r + dr, c + dc
                if 0 <= nr < m and 0 <= nc < n and grid[nr][nc] == 1:
                    grid[nr][nc] = 2
                    fresh -= 1
                    q.append((nr, nc))
    return -1 if fresh else time
```

**Example 1:**
**Input:** `grid = [[2,1,1],[1,1,0],[0,1,1]]`
**Output:** `4`

**Example 2:**
**Input:** `grid = [[2,1,1],[0,1,1],[1,0,1]]`
**Output:** `-1`
**Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**
**Input:** `grid = [[0,2]]`
**Output:** `0`

---

### Pattern: Monotonic Deque

---

#### 239. Sliding Window Maximum
**Link:** [https://leetcode.com/problems/sliding-window-maximum/](https://leetcode.com/problems/sliding-window-maximum/)  
**Problem:** You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

```python
from collections import deque

def maxSlidingWindow(nums: list[int], k: int) -> list[int]:
    dq = deque()
    res = []
    for i, n in enumerate(nums):
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        while dq and nums[dq[-1]] < n:
            dq.pop()
        dq.append(i)
        if i >= k - 1:
            res.append(nums[dq[0]])
    return res
```

**Example 1:**
**Input:** `nums = [1,3,-1,-3,5,3,6,7], k = 3`
**Output:** `[3,3,5,5,6,7]`
**Explanation:**
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

**Example 2:**
**Input:** `nums = [1], k = 1`
**Output:** `[1]`

---

## 7. HEAP

### Pattern: Top K

---

#### 973. K Closest Points to Origin
**Link:** [https://leetcode.com/problems/k-closest-points-to-origin/](https://leetcode.com/problems/k-closest-points-to-origin/)  
**Problem:** Given an array of `points` where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin (0, 0). The distance between two points on the X-Y plane is the Euclidean distance.

```python
import heapq

def kClosest(points: list[list[int]], k: int) -> list[list[int]]:
    # Max-heap storing (-dist, point) to keep top k smallest distances
    pq = []
    for x, y in points:
        d = -(x * x + y * y)
        heapq.heappush(pq, (d, [x, y]))
        if len(pq) > k:
            heapq.heappop(pq)
    return [pt for d, pt in pq]
```

**Example 1:**
**Input:** `points = [[1,3],[-2,2]], k = 1`
**Output:** `[[-2,2]]`
**Explanation:**
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.

**Example 2:**
**Input:** `points = [[3,3],[5,-1],[-2,4]], k = 2`
**Output:** `[[3,3],[-2,4]]`

---

#### 215. Kth Largest Element in an Array
**Link:** [https://leetcode.com/problems/kth-largest-element-in-an-array/](https://leetcode.com/problems/kth-largest-element-in-an-array/)  
**Problem:** Given an integer array `nums` and an integer `k`, return the kth largest element in the array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

```python
import heapq

def findKthLargest(nums: list[int], k: int) -> int:
    pq = []
    for n in nums:
        heapq.heappush(pq, n)
        if len(pq) > k:
            heapq.heappop(pq)
    return pq[0]
```

**Example 1:**
**Input:** `nums = [3,2,1,5,6,4], k = 2`
**Output:** `5`

**Example 2:**
**Input:** `nums = [3,2,3,1,2,4,5,5,6], k = 4`
**Output:** `4`

---

### Pattern: Running Median

---

#### 295. Find Median from Data Stream
**Link:** [https://leetcode.com/problems/find-median-from-data-stream/](https://leetcode.com/problems/find-median-from-data-stream/)  
**Problem:** The MedianFinder class finds the median of a data stream. Implement the `addNum` and `findMedian` methods.

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.lo = []  # max-heap (invert values)
        self.hi = []  # min-heap

    def addNum(self, num: int) -> None:
        heapq.heappush(self.lo, -num)
        heapq.heappush(self.hi, -heapq.heappop(self.lo))
        if len(self.hi) > len(self.lo):
            heapq.heappush(self.lo, -heapq.heappop(self.hi))

    def findMedian(self) -> float:
        if len(self.lo) > len(self.hi):
            return float(-self.lo[0])
        return (-self.lo[0] + self.hi[0]) / 2.0
```

**Example 1:**
**Input:**
`["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]`
`[[], [1], [2], [], [3], []]`
**Output:**
`[null, null, null, 1.5, null, 2.0]`
**Explanation:**
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0

---

### Pattern: Greedy Heap

---

#### 502. IPO
**Link:** [https://leetcode.com/problems/ipo/](https://leetcode.com/problems/ipo/)  
**Problem:** Suppose LeetCode will start its IPO soon. In order to sell a good price of its shares to Venture Capital, LeetCode would like to work on some projects to increase its capital before the IPO. Given `n` projects where the ith project has a pure profit `profits[i]` and a minimum capital `capital[i]` required, design a way to maximize the final capital, doing at most `k` distinct projects.

```python
import heapq

def findMaximizedCapital(k: int, w: int, profits: list[int], capital: list[int]) -> int:
    projects = sorted(zip(capital, profits))
    pq = []  # max-heap of profits
    i = 0
    n = len(projects)
    for _ in range(k):
        while i < n and projects[i][0] <= w:
            heapq.heappush(pq, -projects[i][1])
            i += 1
        if not pq:
            break
        w += -heapq.heappop(pq)
    return w
```

**Example 1:**
**Input:** `k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]`
**Output:** `4`
**Explanation:**
Since your initial capital is 0, you can only start the project indexed 0.
After finishing it you will obtain profit 1 and your capital becomes 1.
With capital 1, you can either start the project indexed 1 or the project indexed 2.
Since you can choose at most 2 projects, you need to finish the project indexed 2 to get the maximum capital.
Therefore, output the final maximized capital, which is 0 + 1 + 3 = 4.

**Example 2:**
**Input:** `k = 3, w = 0, profits = [1,2,3], capital = [0,1,2]`
**Output:** `6`

---

## 8. TREES

### Pattern: DFS

---

#### 104. Maximum Depth of Binary Tree
**Link:** [https://leetcode.com/problems/maximum-depth-of-binary-tree/](https://leetcode.com/problems/maximum-depth-of-binary-tree/)  
**Problem:** Given the `root` of a binary tree, return its maximum depth. A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

```python
def maxDepth(root: TreeNode | None) -> int:
    if not root:
        return 0
    return 1 + max(maxDepth(root.left), maxDepth(root.right))
```

**Example 1:**
**Input:** `root = [3,9,20,null,null,15,7]`
**Output:** `3`

**Example 2:**
**Input:** `root = [1,null,2]`
**Output:** `2`

---

#### 112. Path Sum
**Link:** [https://leetcode.com/problems/path-sum/](https://leetcode.com/problems/path-sum/)  
**Problem:** Given the `root` of a binary tree and an integer `targetSum`, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals `targetSum`.

```python
def hasPathSum(root: TreeNode | None, targetSum: int) -> bool:
    if not root:
        return False
    if not root.left and not root.right:
        return root.val == targetSum
    return hasPathSum(root.left, targetSum - root.val) or hasPathSum(root.right, targetSum - root.val)
```

**Example 1:**
**Input:** `root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22`
**Output:** `true`
**Explanation:** The root-to-leaf path with the target sum is shown.

**Example 2:**
**Input:** `root = [1,2,3], targetSum = 5`
**Output:** `false`

**Example 3:**
**Input:** `root = [], targetSum = 0`
**Output:** `false`

---

#### 543. Diameter of Binary Tree
**Link:** [https://leetcode.com/problems/diameter-of-binary-tree/](https://leetcode.com/problems/diameter-of-binary-tree/)  
**Problem:** Given the `root` of a binary tree, return the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

```python
def diameterOfBinaryTree(root: TreeNode | None) -> int:
    res = 0

    def dfs(node: TreeNode | None) -> int:
        nonlocal res
        if not node:
            return 0
        l = dfs(node.left)
        r = dfs(node.right)
        res = max(res, l + r)
        return 1 + max(l, r)

    dfs(root)
    return res
```

**Example 1:**
**Input:** `root = [1,2,3,4,5]`
**Output:** `3`
**Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3].

**Example 2:**
**Input:** `root = [1,2]`
**Output:** `1`

---

### Pattern: BFS (Trees)

---

#### 199. Binary Tree Right Side View
**Link:** [https://leetcode.com/problems/binary-tree-right-side-view/](https://leetcode.com/problems/binary-tree-right-side-view/)  
**Problem:** Given the `root` of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

```python
from collections import deque

def rightSideView(root: TreeNode | None) -> list[int]:
    res = []
    if not root:
        return res
    q = deque([root])
    while q:
        for i in range(len(q), 0, -1):
            node = q.popleft()
            if i == 1:
                res.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
    return res
```

**Example 1:**
**Input:** `root = [1,2,3,null,5,null,4]`
**Output:** `[1,3,4]`

**Example 2:**
**Input:** `root = [1,null,3]`
**Output:** `[1,3]`

**Example 3:**
**Input:** `root = []`
**Output:** `[]`

---

#### 103. Binary Tree Zigzag Level Order Traversal
**Link:** [https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)  
**Problem:** Given the `root` of a binary tree, return the zigzag level order traversal of its nodes' values (i.e., from left to right, then right to left for the next level and alternate between).

```python
from collections import deque

def zigzagLevelOrder(root: TreeNode | None) -> list[list[int]]:
    res = []
    if not root:
        return res
    q = deque([root])
    left_to_right = True
    while q:
        sz = len(q)
        level = [0] * sz
        for i in range(sz):
            node = q.popleft()
            idx = i if left_to_right else sz - 1 - i
            level[idx] = node.val
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(level)
        left_to_right = not left_to_right
    return res
```

**Example 1:**
**Input:** `root = [3,9,20,null,null,15,7]`
**Output:** `[[3],[20,9],[15,7]]`

**Example 2:**
**Input:** `root = [1]`
**Output:** `[[1]]`

**Example 3:**
**Input:** `root = []`
**Output:** `[]`

---

### Pattern: BST

---

#### 98. Validate Binary Search Tree
**Link:** [https://leetcode.com/problems/validate-binary-search-tree/](https://leetcode.com/problems/validate-binary-search-tree/)  
**Problem:** Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

```python
def isValidBST(root: TreeNode | None, lo: float = float('-inf'), hi: float = float('inf')) -> bool:
    if not root:
        return True
    if root.val <= lo or root.val >= hi:
        return False
    return isValidBST(root.left, lo, root.val) and isValidBST(root.right, root.val, hi)
```

**Example 1:**
**Input:** `root = [2,1,3]`
**Output:** `true`

**Example 2:**
**Input:** `root = [5,1,4,null,null,3,6]`
**Output:** `false`
**Explanation:** The root node's value is 5 but its right child's value is 4.

---

#### 230. Kth Smallest Element in a BST
**Link:** [https://leetcode.com/problems/kth-smallest-element-in-a-bst/](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)  
**Problem:** Given the `root` of a binary search tree, and an integer `k`, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

```python
def kthSmallest(root: TreeNode | None, k: int) -> int:
    res = 0

    def inorder(node: TreeNode | None) -> None:
        nonlocal k, res
        if not node or k <= 0:
            return
        inorder(node.left)
        k -= 1
        if k == 0:
            res = node.val
            return
        inorder(node.right)

    inorder(root)
    return res
```

**Example 1:**
**Input:** `root = [3,1,4,null,2], k = 1`
**Output:** `1`

**Example 2:**
**Input:** `root = [5,3,6,2,4,null,null,1], k = 3`
**Output:** `3`

---

#### 235. Lowest Common Ancestor of a BST
**Link:** [https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)  
**Problem:** Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

```python
def lowestCommonAncestor(root: TreeNode | None, p: TreeNode, q: TreeNode) -> TreeNode | None:
    if p.val < root.val and q.val < root.val:
        return lowestCommonAncestor(root.left, p, q)
    if p.val > root.val and q.val > root.val:
        return lowestCommonAncestor(root.right, p, q)
    return root
```

**Example 1:**
**Input:** `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8`
**Output:** `6`
**Explanation:** The LCA of nodes 2 and 8 is 6.

**Example 2:**
**Input:** `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4`
**Output:** `2`
**Explanation:** The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

---

### Pattern: Tree Construction

---

#### 105. Construct Binary Tree from Preorder and Inorder Traversal
**Link:** [https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)  
**Problem:** Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.

```python
def buildTree(preorder: list[int], inorder: list[int]) -> TreeNode | None:
    idx_map = {v: i for i, v in enumerate(inorder)}
    pre = 0

    def build(l: int, r: int) -> TreeNode | None:
        nonlocal pre
        if l > r:
            return None
        val = preorder[pre]
        pre += 1
        node = TreeNode(val)
        node.left = build(l, idx_map[val] - 1)
        node.right = build(idx_map[val] + 1, r)
        return node

    return build(0, len(inorder) - 1)
```

**Example 1:**
**Input:** `preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]`
**Output:** `[3,9,20,null,null,15,7]`

**Example 2:**
**Input:** `preorder = [-1], inorder = [-1]`
**Output:** `[-1]`

---

### Pattern: Tree DP

---

#### 337. House Robber III
**Link:** [https://leetcode.com/problems/house-robber-iii/](https://leetcode.com/problems/house-robber-iii/)  
**Problem:** The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`. Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. Determine the maximum amount of money the thief can rob tonight without alerting the police.

```python
def rob(root: TreeNode | None) -> int:
    def dfs(node: TreeNode | None) -> tuple[int, int]:
        if not node:
            return 0, 0
        ll, lr = dfs(node.left)
        rl, rr = dfs(node.right)
        rob_val = node.val + lr + rr
        skip_val = max(ll, lr) + max(rl, rr)
        return rob_val, skip_val

    rob_val, skip_val = dfs(root)
    return max(rob_val, skip_val)
```

**Example 1:**
**Input:** `root = [3,2,3,null,3,null,1]`
**Output:** `7`
**Explanation:** Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example 2:**
**Input:** `root = [3,4,5,1,3,null,1]`
**Output:** `9`
**Explanation:** Maximum amount of money the thief can rob = 4 + 5 = 9.

---

#### 124. Binary Tree Maximum Path Sum
**Link:** [https://leetcode.com/problems/binary-tree-maximum-path-sum/](https://leetcode.com/problems/binary-tree-maximum-path-sum/)  
**Problem:** A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root. Given the `root` of a binary tree, return the maximum path sum of any non-empty path.

```python
def maxPathSum(root: TreeNode | None) -> int:
    res = float('-inf')

    def dfs(node: TreeNode | None) -> int:
        nonlocal res
        if not node:
            return 0
        l = max(0, dfs(node.left))
        r = max(0, dfs(node.right))
        res = max(res, node.val + l + r)
        return node.val + max(l, r)

    dfs(root)
    return res
```

**Example 1:**
**Input:** `root = [1,2,3]`
**Output:** `6`
**Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

**Example 2:**
**Input:** `root = [-10,9,20,null,null,15,7]`
**Output:** `42`
**Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

---

## 9. GRAPHS

### Pattern: DFS (Graphs)

---

#### 200. Number of Islands
**Link:** [https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/)  
**Problem:** Given an `m x n` 2D binary grid `grid` which represents a map of '1's (land) and '0's (water), return the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically.

```python
def numIslands(grid: list[list[str]]) -> int:
    m, n = len(grid), len(grid[0])
    res = 0

    def dfs(r: int, c: int) -> None:
        if r < 0 or r >= m or c < 0 or c >= n or grid[r][c] != '1':
            return
        grid[r][c] = '0'
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)

    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                res += 1
                dfs(i, j)
    return res
```

**Example 1:**
**Input:** `grid = [["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]`
**Output:** `1`

**Example 2:**
**Input:** `grid = [["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]`
**Output:** `3`

---

#### 133. Clone Graph
**Link:** [https://leetcode.com/problems/clone-graph/](https://leetcode.com/problems/clone-graph/)  
**Problem:** Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph. Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

```python
def cloneGraph(node: Node | None) -> Node | None:
    mp = {}

    def dfs(n: Node | None) -> Node | None:
        if not n:
            return None
        if n in mp:
            return mp[n]
        clone = Node(n.val)
        mp[n] = clone
        for nb in n.neighbors:
            clone.neighbors.append(dfs(nb))
        return clone

    return dfs(node)
```

**Example 1:**
**Input:** `adjList = [[2,4],[1,3],[2,4],[1,3]]`
**Output:** `[[2,4],[1,3],[2,4],[1,3]]`
**Explanation:** There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

**Example 2:**
**Input:** `adjList = [[]]`
**Output:** `[[]]`

**Example 3:**
**Input:** `adjList = []`
**Output:** `[]`

---

#### 695. Max Area of Island
**Link:** [https://leetcode.com/problems/max-area-of-island/](https://leetcode.com/problems/max-area-of-island/)  
**Problem:** You are given an `m x n` binary matrix `grid`. An island is a group of 1's (representing land) connected 4-directionally. Return the maximum area of an island in `grid`. If there is no island, return 0.

```python
def maxAreaOfIsland(grid: list[list[int]]) -> int:
    m, n = len(grid), len(grid[0])
    res = 0

    def dfs(r: int, c: int) -> int:
        if r < 0 or r >= m or c < 0 or c >= n or grid[r][c] == 0:
            return 0
        grid[r][c] = 0
        return 1 + dfs(r + 1, c) + dfs(r - 1, c) + dfs(r, c + 1) + dfs(r, c - 1)

    for i in range(m):
        for j in range(n):
            res = max(res, dfs(i, j))
    return res
```

**Example 1:**
**Input:** `grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]`
**Output:** `6`
**Explanation:** The answer is not 11, because the island must be connected 4-directionally.

**Example 2:**
**Input:** `grid = [[0,0,0,0,0,0,0,0]]`
**Output:** `0`

---

### Pattern: BFS (Graphs)

---

#### 127. Word Ladder
**Link:** [https://leetcode.com/problems/word-ladder/](https://leetcode.com/problems/word-ladder/)  
**Problem:** A transformation sequence from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words where each adjacent pair differs by a single character, and each word is in the dictionary. Given `beginWord`, `endWord`, and `wordList`, return the number of words in the shortest transformation sequence, or 0 if no such sequence exists.

```python
from collections import deque
import string

def ladderLength(beginWord: str, endWord: str, wordList: list[str]) -> int:
    word_set = set(wordList)
    if endWord not in word_set:
        return 0
    q = deque([beginWord])
    steps = 1
    while q:
        for _ in range(len(q)):
            word = q.popleft()
            if word == endWord:
                return steps
            for i in range(len(word)):
                for c in string.ascii_lowercase:
                    next_word = word[:i] + c + word[i + 1:]
                    if next_word in word_set:
                        q.append(next_word)
                        word_set.remove(next_word)
        steps += 1
    return 0
```

**Example 1:**
**Input:** `beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]`
**Output:** `5`
**Explanation:** One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.

**Example 2:**
**Input:** `beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]`
**Output:** `0`
**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

---

### Pattern: Topological Sort

---

#### 207. Course Schedule
**Link:** [https://leetcode.com/problems/course-schedule/](https://leetcode.com/problems/course-schedule/)  
**Problem:** There are a total of `numCourses` courses you have to take, labeled from 0 to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`. Return true if you can finish all courses.

```python
from collections import deque

def canFinish(numCourses: int, prerequisites: list[list[int]]) -> bool:
    adj = [[] for _ in range(numCourses)]
    indegree = [0] * numCourses
    for dest, src in prerequisites:
        adj[src].append(dest)
        indegree[dest] += 1
    q = deque([i for i in range(numCourses) if indegree[i] == 0])
    count = 0
    while q:
        node = q.popleft()
        count += 1
        for nb in adj[node]:
            indegree[nb] -= 1
            if indegree[nb] == 0:
                q.append(nb)
    return count == numCourses
```

**Example 1:**
**Input:** `numCourses = 2, prerequisites = [[1,0]]`
**Output:** `true`
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

**Example 2:**
**Input:** `numCourses = 2, prerequisites = [[1,0],[0,1]]`
**Output:** `false`
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

---

#### 210. Course Schedule II
**Link:** [https://leetcode.com/problems/course-schedule-ii/](https://leetcode.com/problems/course-schedule-ii/)  
**Problem:** Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

```python
from collections import deque

def findOrder(numCourses: int, prerequisites: list[list[int]]) -> list[int]:
    adj = [[] for _ in range(numCourses)]
    indegree = [0] * numCourses
    for dest, src in prerequisites:
        adj[src].append(dest)
        indegree[dest] += 1
    q = deque([i for i in range(numCourses) if indegree[i] == 0])
    order = []
    while q:
        node = q.popleft()
        order.append(node)
        for nb in adj[node]:
            indegree[nb] -= 1
            if indegree[nb] == 0:
                q.append(nb)
    return order if len(order) == numCourses else []
```

**Example 1:**
**Input:** `numCourses = 2, prerequisites = [[1,0]]`
**Output:** `[0,1]`
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

**Example 2:**
**Input:** `numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]`
**Output:** `[0,2,1,3]`
**Explanation:** There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

**Example 3:**
**Input:** `numCourses = 1, prerequisites = []`
**Output:** `[0]`

---

### Pattern: Union Find

---

#### 684. Redundant Connection
**Link:** [https://leetcode.com/problems/redundant-connection/](https://leetcode.com/problems/redundant-connection/)  
**Problem:** In this problem, a tree is an undirected graph that is connected and has no cycles. You are given a graph that started as a tree with `n` nodes labeled from 1 to `n`, with one additional edge added. Return an edge that can be removed so that the resulting graph is a tree of `n` nodes.

```python
def findRedundantConnection(edges: list[list[int]]) -> list[int]:
    n = len(edges)
    parent = list(range(n + 1))

    def find(x: int) -> int:
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    for u, v in edges:
        pu, pv = find(u), find(v)
        if pu == pv:
            return [u, v]
        parent[pu] = pv
    return []
```

**Example 1:**
**Input:** `edges = [[1,2],[1,3],[2,3]]`
**Output:** `[2,3]`

**Example 2:**
**Input:** `edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]`
**Output:** `[1,4]`

---

#### 547. Number of Provinces
**Link:** [https://leetcode.com/problems/number-of-provinces/](https://leetcode.com/problems/number-of-provinces/)  
**Problem:** There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`. A province is a group of directly or indirectly connected cities. Given an `n x n` matrix `isConnected`, return the total number of provinces.

```python
def findCircleNum(isConnected: list[list[int]]) -> int:
    n = len(isConnected)
    parent = list(range(n))

    def find(x: int) -> int:
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    res = n
    for i in range(n):
        for j in range(i + 1, n):
            if isConnected[i][j]:
                pi, pj = find(i), find(j)
                if pi != pj:
                    parent[pi] = pj
                    res -= 1
    return res
```

**Example 1:**
**Input:** `isConnected = [[1,1,0],[1,1,0],[0,0,1]]`
**Output:** `2`

**Example 2:**
**Input:** `isConnected = [[1,0,0],[0,1,0],[0,0,1]]`
**Output:** `3`

---

### Pattern: Dijkstra

---

#### 743. Network Delay Time
**Link:** [https://leetcode.com/problems/network-delay-time/](https://leetcode.com/problems/network-delay-time/)  
**Problem:** You are given a network of `n` nodes, labeled from 1 to `n`. You are also given times, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target. Return the minimum time it takes for all the `n` nodes to receive the signal. If it is impossible for all the `n` nodes to receive the signal, return -1.

```python
import heapq

def networkDelayTime(times: list[list[int]], n: int, k: int) -> int:
    adj = [[] for _ in range(n + 1)]
    for u, v, w in times:
        adj[u].append((v, w))
    dist = [float('inf')] * (n + 1)
    dist[k] = 0
    pq = [(0, k)]
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        for v, w in adj[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(pq, (dist[v], v))
    res = max(dist[1:])
    return -1 if res == float('inf') else res
```

**Example 1:**
**Input:** `times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2`
**Output:** `2`

**Example 2:**
**Input:** `times = [[1,2,1]], n = 2, k = 1`
**Output:** `1`

**Example 3:**
**Input:** `times = [[1,2,1]], n = 2, k = 2`
**Output:** `-1`

---

#### 1631. Path With Minimum Effort
**Link:** [https://leetcode.com/problems/path-with-minimum-effort/](https://leetcode.com/problems/path-with-minimum-effort/)  
**Problem:** You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. A route's effort is the maximum absolute difference in heights between two consecutive cells of the route. Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

```python
import heapq

def minimumEffortPath(heights: list[list[int]]) -> int:
    m, n = len(heights), len(heights[0])
    effort = [[float('inf')] * n for _ in range(m)]
    effort[0][0] = 0
    pq = [(0, 0, 0)]  # effort, r, c
    dirs = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    while pq:
        e, r, c = heapq.heappop(pq)
        if r == m - 1 and c == n - 1:
            return e
        if e > effort[r][c]:
            continue
        for dr, dc in dirs:
            nr, nc = r + dr, c + dc
            if 0 <= nr < m and 0 <= nc < n:
                ne = max(e, abs(heights[nr][nc] - heights[r][c]))
                if ne < effort[nr][nc]:
                    effort[nr][nc] = ne
                    heapq.heappush(pq, (ne, nr, nc))
    return 0
```

**Example 1:**
**Input:** `heights = [[1,2,2],[3,8,2],[5,3,5]]`
**Output:** `2`
**Explanation:** The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells. This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.

**Example 2:**
**Input:** `heights = [[1,2,3],[3,8,4],[5,3,5]]`
**Output:** `1`
**Explanation:** The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].

**Example 3:**
**Input:** `heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]`
**Output:** `0`
**Explanation:** This route does not require any effort.

---

## 10. BACKTRACKING

### Pattern: Subsets

---

#### 78. Subsets
**Link:** [https://leetcode.com/problems/subsets/](https://leetcode.com/problems/subsets/)  
**Problem:** Given an integer array `nums` of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets. Return the solution in any order.

```python
def subsets(nums: list[int]) -> list[list[int]]:
    res = []
    cur = []

    def bt(start: int) -> None:
        res.append(list(cur))
        for i in range(start, len(nums)):
            cur.append(nums[i])
            bt(i + 1)
            cur.pop()

    bt(0)
    return res
```

**Example 1:**
**Input:** `nums = [1,2,3]`
**Output:** `[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

**Example 2:**
**Input:** `nums = [0]`
**Output:** `[[],[0]]`

---

#### 90. Subsets II
**Link:** [https://leetcode.com/problems/subsets-ii/](https://leetcode.com/problems/subsets-ii/)  
**Problem:** Given an integer array `nums` that may contain duplicates, return all possible subsets (the power set). The solution set must not contain duplicate subsets. Return the solution in any order.

```python
def subsetsWithDup(nums: list[int]) -> list[list[int]]:
    nums.sort()
    res = []
    cur = []

    def bt(start: int) -> None:
        res.append(list(cur))
        for i in range(start, len(nums)):
            if i > start and nums[i] == nums[i - 1]:
                continue
            cur.append(nums[i])
            bt(i + 1)
            cur.pop()

    bt(0)
    return res
```

**Example 1:**
**Input:** `nums = [1,2,2]`
**Output:** `[[],[1],[1,2],[1,2,2],[2],[2,2]]`

**Example 2:**
**Input:** `nums = [0]`
**Output:** `[[],[0]]`

---

### Pattern: Permutations

---

#### 46. Permutations
**Link:** [https://leetcode.com/problems/permutations/](https://leetcode.com/problems/permutations/)  
**Problem:** Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.

```python
def permute(nums: list[int]) -> list[list[int]]:
    res = []

    def bt(start: int) -> None:
        if start == len(nums):
            res.append(list(nums))
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]
            bt(start + 1)
            nums[start], nums[i] = nums[i], nums[start]

    bt(0)
    return res
```

**Example 1:**
**Input:** `nums = [1,2,3]`
**Output:** `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

**Example 2:**
**Input:** `nums = [0,1]`
**Output:** `[[0,1],[1,0]]`

**Example 3:**
**Input:** `nums = [1]`
**Output:** `[[1]]`

---

#### 47. Permutations II
**Link:** [https://leetcode.com/problems/permutations-ii/](https://leetcode.com/problems/permutations-ii/)  
**Problem:** Given a collection of numbers, `nums`, that might contain duplicates, return all possible unique permutations in any order.

```python
def permuteUnique(nums: list[int]) -> list[list[int]]:
    nums.sort()
    res = []
    used = [False] * len(nums)
    cur = []

    def bt() -> None:
        if len(cur) == len(nums):
            res.append(list(cur))
            return
        for i in range(len(nums)):
            if used[i]:
                continue
            if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                continue
            used[i] = True
            cur.append(nums[i])
            bt()
            used[i] = False
            cur.pop()

    bt()
    return res
```

**Example 1:**
**Input:** `nums = [1,1,2]`
**Output:** `[[1,1,2],[1,2,1],[2,1,1]]`

**Example 2:**
**Input:** `nums = [1,2,3]`
**Output:** `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

---

### Pattern: Combination

---

#### 39. Combination Sum
**Link:** [https://leetcode.com/problems/combination-sum/](https://leetcode.com/problems/combination-sum/)  
**Problem:** Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order. The same number may be chosen from candidates an unlimited number of times.

```python
def combinationSum(candidates: list[int], target: int) -> list[list[int]]:
    candidates.sort()
    res = []
    cur = []

    def bt(start: int, rem: int) -> None:
        if rem == 0:
            res.append(list(cur))
            return
        for i in range(start, len(candidates)):
            if candidates[i] > rem:
                break
            cur.append(candidates[i])
            bt(i, rem - candidates[i])
            cur.pop()

    bt(0, target)
    return res
```

**Example 1:**
**Input:** `candidates = [2,3,6,7], target = 7`
**Output:** `[[2,2,3],[7]]`
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**
**Input:** `candidates = [2,3,5], target = 8`
**Output:** `[[2,2,2,2],[2,3,3],[3,5]]`

**Example 3:**
**Input:** `candidates = [2], target = 1`
**Output:** `[]`

---

### Pattern: Grid Backtracking

---

#### 79. Word Search
**Link:** [https://leetcode.com/problems/word-search/](https://leetcode.com/problems/word-search/)  
**Problem:** Given an `m x n` grid of characters `board` and a string `word`, return true if `word` exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

```python
def exist(board: list[list[str]], word: str) -> bool:
    m, n = len(board), len(board[0])

    def dfs(r: int, c: int, idx: int) -> bool:
        if idx == len(word):
            return True
        if r < 0 or r >= m or c < 0 or c >= n or board[r][c] != word[idx]:
            return False
        tmp = board[r][c]
        board[r][c] = '#'
        found = (dfs(r + 1, c, idx + 1) or
                 dfs(r - 1, c, idx + 1) or
                 dfs(r, c + 1, idx + 1) or
                 dfs(r, c - 1, idx + 1))
        board[r][c] = tmp
        return found

    for i in range(m):
        for j in range(n):
            if dfs(i, j, 0):
                return True
    return False
```

**Example 1:**
**Input:** `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"`
**Output:** `true`

**Example 2:**
**Input:** `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"`
**Output:** `true`

**Example 3:**
**Input:** `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"`
**Output:** `false`

---

#### 51. N-Queens
**Link:** [https://leetcode.com/problems/n-queens/](https://leetcode.com/problems/n-queens/)  
**Problem:** The n-queens puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other. Given an integer `n`, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

```python
def solveNQueens(n: int) -> list[list[str]]:
    res = []
    board = [['.'] * n for _ in range(n)]
    cols = set()
    diag1 = set()  # r - c
    diag2 = set()  # r + c

    def bt(row: int) -> None:
        if row == n:
            res.append(["".join(r) for r in board])
            return
        for col in range(n):
            if col in cols or (row - col) in diag1 or (row + col) in diag2:
                continue
            cols.add(col)
            diag1.add(row - col)
            diag2.add(row + col)
            board[row][col] = 'Q'
            bt(row + 1)
            cols.remove(col)
            diag1.remove(row - col)
            diag2.remove(row + col)
            board[row][col] = '.'

    bt(0)
    return res
```

**Example 1:**
**Input:** `n = 4`
**Output:** `[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above.

**Example 2:**
**Input:** `n = 1`
**Output:** `[["Q"]]`

---

## 11. DYNAMIC PROGRAMMING

### Pattern: 1D DP

---

#### 70. Climbing Stairs
**Link:** [https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/)  
**Problem:** You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```python
def climbStairs(n: int) -> int:
    if n <= 2:
        return n
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b
```

**Example 1:**
**Input:** `n = 2`
**Output:** `2`
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**
**Input:** `n = 3`
**Output:** `3`
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

---

#### 198. House Robber
**Link:** [https://leetcode.com/problems/house-robber/](https://leetcode.com/problems/house-robber/)  
**Problem:** You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. You cannot rob two adjacent houses. Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

```python
def rob(nums: list[int]) -> int:
    prev = cur = 0
    for n in nums:
        prev, cur = cur, max(cur, prev + n)
    return cur
```

**Example 1:**
**Input:** `nums = [1,2,3,1]`
**Output:** `4`
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 2:**
**Input:** `nums = [2,7,9,3,1]`
**Output:** `12`
**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.

---

#### 322. Coin Change
**Link:** [https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)  
**Problem:** You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money. Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

```python
def coinChange(coins: list[int], amount: int) -> int:
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for i in range(1, amount + 1):
        for c in coins:
            if c <= i:
                dp[i] = min(dp[i], dp[i - c] + 1)
    return -1 if dp[amount] == float('inf') else dp[amount]
```

**Example 1:**
**Input:** `coins = [1,2,5], amount = 11`
**Output:** `3`
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**
**Input:** `coins = [2], amount = 3`
**Output:** `-1`

**Example 3:**
**Input:** `coins = [1], amount = 0`
**Output:** `0`

---

### Pattern: 2D DP

---

#### 62. Unique Paths
**Link:** [https://leetcode.com/problems/unique-paths/](https://leetcode.com/problems/unique-paths/)  
**Problem:** There is a robot on an `m x n` grid. The robot is initially located at the top-left corner and wants to reach the bottom-right corner. The robot can only move either down or right at any point in time. How many possible unique paths are there?

```python
def uniquePaths(m: int, n: int) -> int:
    dp = [1] * n
    for _ in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j - 1]
    return dp[-1]
```

**Example 1:**
**Input:** `m = 3, n = 7`
**Output:** `28`

**Example 2:**
**Input:** `m = 3, n = 2`
**Output:** `3`
**Explanation:** From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

---

#### 72. Edit Distance
**Link:** [https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)  
**Problem:** Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`. You have three operations: insert a character, delete a character, or replace a character.

```python
def minDistance(word1: str, word2: str) -> int:
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[m][n]
```

**Example 1:**
**Input:** `word1 = "horse", word2 = "ros"`
**Output:** `3`
**Explanation:**
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Example 2:**
**Input:** `word1 = "intention", word2 = "execution"`
**Output:** `5`
**Explanation:**
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

---

### Pattern: Knapsack

---

#### 416. Partition Equal Subset Sum
**Link:** [https://leetcode.com/problems/partition-equal-subset-sum/](https://leetcode.com/problems/partition-equal-subset-sum/)  
**Problem:** Given an integer array `nums`, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.

```python
def canPartition(nums: list[int]) -> bool:
    total = sum(nums)
    if total % 2 != 0:
        return False
    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True
    for n in nums:
        for j in range(target, n - 1, -1):
            dp[j] = dp[j] or dp[j - n]
    return dp[target]
```

**Example 1:**
**Input:** `nums = [1,5,11,5]`
**Output:** `true`
**Explanation:** The array can be partitioned as [1, 5, 5] and [11].

**Example 2:**
**Input:** `nums = [1,2,3,5]`
**Output:** `false`
**Explanation:** The array cannot be partitioned into equal sum subsets.

---

#### 494. Target Sum
**Link:** [https://leetcode.com/problems/target-sum/](https://leetcode.com/problems/target-sum/)  
**Problem:** You are given an integer array `nums` and an integer `target`. You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers. Return the number of different expressions that you can build which evaluates to `target`.

```python
from collections import defaultdict

def findTargetSumWays(nums: list[int], target: int) -> int:
    dp = {0: 1}
    for n in nums:
        nxt = defaultdict(int)
        for s, cnt in dp.items():
            nxt[s + n] += cnt
            nxt[s - n] += cnt
        dp = nxt
    return dp.get(target, 0)
```

**Example 1:**
**Input:** `nums = [1,1,1,1,1], target = 3`
**Output:** `5`
**Explanation:** There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

**Example 2:**
**Input:** `nums = [1], target = 1`
**Output:** `1`

---

### Pattern: LIS

---

#### 300. Longest Increasing Subsequence
**Link:** [https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/)  
**Problem:** Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

```python
import bisect

def lengthOfLIS(nums: list[int]) -> int:
    tails = []
    for n in nums:
        idx = bisect.bisect_left(tails, n)
        if idx == len(tails):
            tails.append(n)
        else:
            tails[idx] = n
    return len(tails)
```

**Example 1:**
**Input:** `nums = [10,9,2,5,3,7,101,18]`
**Output:** `4`
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**
**Input:** `nums = [0,1,0,3,2,3]`
**Output:** `4`

**Example 3:**
**Input:** `nums = [7,7,7,7,7,7,7]`
**Output:** `1`

---

### Pattern: DP on Strings

---

#### 516. Longest Palindromic Subsequence
**Link:** [https://leetcode.com/problems/longest-palindromic-subsequence/](https://leetcode.com/problems/longest-palindromic-subsequence/)  
**Problem:** Given a string `s`, find the longest palindromic subsequence's length in `s`.

```python
def longestPalindromeSubseq(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1] + 2
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]
```

**Example 1:**
**Input:** `s = "bbbab"`
**Output:** `4`
**Explanation:** One possible longest palindromic subsequence is "bbbb".

**Example 2:**
**Input:** `s = "cbbd"`
**Output:** `2`
**Explanation:** One possible longest palindromic subsequence is "bb".

---

#### 647. Palindromic Substrings
**Link:** [https://leetcode.com/problems/palindromic-substrings/](https://leetcode.com/problems/palindromic-substrings/)  
**Problem:** Given a string `s`, return the number of palindromic substrings in it. A string is a palindrome when it reads the same backward as forward.

```python
def countSubstrings(s: str) -> int:
    n = len(s)
    res = 0
    for center in range(2 * n - 1):
        l = center // 2
        r = l + center % 2
        while l >= 0 and r < n and s[l] == s[r]:
            res += 1
            l -= 1
            r += 1
    return res
```

**Example 1:**
**Input:** `s = "abc"`
**Output:** `3`
**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**
**Input:** `s = "aaa"`
**Output:** `6`
**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

---

### Pattern: Interval DP

---

#### 312. Burst Balloons
**Link:** [https://leetcode.com/problems/burst-balloons/](https://leetcode.com/problems/burst-balloons/)  
**Problem:** You are given `n` balloons, indexed from 0 to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons. If you burst the ith balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. Return the maximum coins you can collect by bursting the balloons wisely.

```python
def maxCoins(nums: list[int]) -> int:
    nums = [1] + nums + [1]
    n = len(nums)
    dp = [[0] * n for _ in range(n)]
    for length in range(2, n):
        for l in range(n - length):
            r = l + length
            for k in range(l + 1, r):
                dp[l][r] = max(dp[l][r], nums[l] * nums[k] * nums[r] + dp[l][k] + dp[k][r])
    return dp[0][n - 1]
```

**Example 1:**
**Input:** `nums = [3,1,5,8]`
**Output:** `167`
**Explanation:**
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167

**Example 2:**
**Input:** `nums = [1,5]`
**Output:** `10`

---

## 12. GREEDY

### Pattern: Interval Greedy

---

#### 452. Minimum Number of Arrows to Burst Balloons
**Link:** [https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)  
**Problem:** There are some spherical balloons taped onto a flat wall. The balloons are represented by a 2D array `points` where `points[i] = [xstart, xend]`. An arrow can be shot up at any x-coordinate. A balloon with `xstart` and `xend` is burst by an arrow shot at x if `xstart <= x <= xend`. Return the minimum number of arrows that must be shot to burst all balloons.

```python
def findMinArrowShots(points: list[list[int]]) -> int:
    points.sort(key=lambda x: x[1])
    arrows = 1
    end = points[0][1]
    for i in range(1, len(points)):
        if points[i][0] > end:
            arrows += 1
            end = points[i][1]
    return arrows
```

**Example 1:**
**Input:** `points = [[10,16],[2,8],[1,6],[7,12]]`
**Output:** `2`
**Explanation:** The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].

**Example 2:**
**Input:** `points = [[1,2],[3,4],[5,6],[7,8]]`
**Output:** `4`

---

### Pattern: Jump Problems

---

#### 55. Jump Game
**Link:** [https://leetcode.com/problems/jump-game/](https://leetcode.com/problems/jump-game/)  
**Problem:** You are given an integer array `nums`. You are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Return true if you can reach the last index, or false otherwise.

```python
def canJump(nums: list[int]) -> bool:
    max_reach = 0
    for i, n in enumerate(nums):
        if i > max_reach:
            return False
        max_reach = max(max_reach, i + n)
    return True
```

**Example 1:**
**Input:** `nums = [2,3,1,1,4]`
**Output:** `true`
**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**
**Input:** `nums = [3,2,1,0,4]`
**Output:** `false`
**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

---

#### 45. Jump Game II
**Link:** [https://leetcode.com/problems/jump-game-ii/](https://leetcode.com/problems/jump-game-ii/)  
**Problem:** You are given a 0-indexed array of integers `nums` of length `n`. You are initially positioned at `nums[0]`. Each element `nums[i]` represents the maximum length of a forward jump from index `i`. Return the minimum number of jumps to reach `nums[n - 1]`.

```python
def jump(nums: list[int]) -> int:
    jumps = 0
    cur_end = 0
    farthest = 0
    for i in range(len(nums) - 1):
        farthest = max(farthest, i + nums[i])
        if i == cur_end:
            jumps += 1
            cur_end = farthest
    return jumps
```

**Example 1:**
**Input:** `nums = [2,3,1,1,4]`
**Output:** `2`
**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**
**Input:** `nums = [2,3,0,1,4]`
**Output:** `2`

---

## 13. BIT MANIPULATION

### Pattern: XOR

---

#### 136. Single Number
**Link:** [https://leetcode.com/problems/single-number/](https://leetcode.com/problems/single-number/)  
**Problem:** Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one. You must implement a solution with a linear runtime complexity and use only constant extra space.

```python
def singleNumber(nums: list[int]) -> int:
    res = 0
    for n in nums:
        res ^= n
    return res
```

**Example 1:**
**Input:** `nums = [2,2,1]`
**Output:** `1`

**Example 2:**
**Input:** `nums = [4,1,2,1,2]`
**Output:** `4`

---

#### 268. Missing Number
**Link:** [https://leetcode.com/problems/missing-number/](https://leetcode.com/problems/missing-number/)  
**Problem:** Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

```python
def missingNumber(nums: list[int]) -> int:
    res = len(nums)
    for i, n in enumerate(nums):
        res ^= i ^ n
    return res
```

**Example 1:**
**Input:** `nums = [3,0,1]`
**Output:** `2`

**Example 2:**
**Input:** `nums = [0,1]`
**Output:** `2`

---

#### 260. Single Number III
**Link:** [https://leetcode.com/problems/single-number-iii/](https://leetcode.com/problems/single-number-iii/)  
**Problem:** Given an integer array `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in any order.

```python
def singleNumber(nums: list[int]) -> list[int]:
    xor_all = 0
    for n in nums:
        xor_all ^= n
    bit = xor_all & (-xor_all)
    a, b = 0, 0
    for n in nums:
        if n & bit:
            a ^= n
        else:
            b ^= n
    return [a, b]
```

**Example 1:**
**Input:** `nums = [1,2,1,3,2,5]`
**Output:** `[3,5]`
**Explanation:**  [5, 3] is also a valid answer.

**Example 2:**
**Input:** `nums = [-1,0]`
**Output:** `[-1,0]`

---

### Pattern: Bitmask

---

#### 338. Counting Bits
**Link:** [https://leetcode.com/problems/counting-bits/](https://leetcode.com/problems/counting-bits/)  
**Problem:** Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (0 <= i <= n), `ans[i]` is the number of 1's in the binary representation of `i`.

```python
def countBits(n: int) -> list[int]:
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i >> 1] + (i & 1)
    return dp
```

**Example 1:**
**Input:** `n = 2`
**Output:** `[0,1,1]`
**Explanation:**
0 --> 0
1 --> 1
2 --> 10

**Example 2:**
**Input:** `n = 5`
**Output:** `[0,1,1,2,1,2]`

---

#### 318. Maximum Product of Word Lengths
**Link:** [https://leetcode.com/problems/maximum-product-of-word-lengths/](https://leetcode.com/problems/maximum-product-of-word-lengths/)  
**Problem:** Given a string array `words`, return the maximum value of `length(word[i]) * length(word[j])` where the two words do not share common letters. If no such two words exist, return 0.

```python
def maxProduct(words: list[str]) -> int:
    n = len(words)
    mask = [0] * n
    for i, w in enumerate(words):
        for c in w:
            mask[i] |= 1 << (ord(c) - ord('a'))
    res = 0
    for i in range(n):
        for j in range(i + 1, n):
            if not (mask[i] & mask[j]):
                res = max(res, len(words[i]) * len(words[j]))
    return res
```

**Example 1:**
**Input:** `words = ["abcw","baz","foo","bar","xtfn","abcdef"]`
**Output:** `16`
**Explanation:** The two words can be "abcw", "xtfn".

**Example 2:**
**Input:** `words = ["a","ab","abc","d","cd","bcd","abcd"]`
**Output:** `4`
**Explanation:** The two words can be "ab", "cd".

---

## 14. SEGMENT TREE / BIT (FENWICK TREE)

### Pattern: Range Queries

---

#### 307. Range Sum Query - Mutable
**Link:** [https://leetcode.com/problems/range-sum-query-mutable/](https://leetcode.com/problems/range-sum-query-mutable/)  
**Problem:** Given an integer array `nums`, handle multiple queries of the following types: update the value of an element in `nums`, and calculate the sum of the elements of `nums` between indices `left` and `right` inclusive.

```python
class NumArray:
    def __init__(self, nums: list[int]):
        self.n = len(nums)
        self.nums = list(nums)
        self.bit = [0] * (self.n + 1)
        for i, val in enumerate(nums):
            self._add(i, val)

    def _add(self, i: int, delta: int) -> None:
        idx = i + 1
        while idx <= self.n:
            self.bit[idx] += delta
            idx += idx & (-idx)

    def _query(self, i: int) -> int:
        s = 0
        idx = i + 1
        while idx > 0:
            s += self.bit[idx]
            idx -= idx & (-idx)
        return s

    def update(self, index: int, val: int) -> None:
        delta = val - self.nums[index]
        self.nums[index] = val
        self._add(index, delta)

    def sumRange(self, left: int, right: int) -> int:
        return self._query(right) - (self._query(left - 1) if left > 0 else 0)
```

**Example 1:**
**Input:**
`["NumArray", "sumRange", "update", "sumRange"]`
`[[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]`
**Output:**
`[null, 9, null, 8]`
**Explanation:**
NumArray numArray = new NumArray([1, 3, 5]);
numArray.sumRange(0, 2); // return 1 + 3 + 5 = 9
numArray.update(1, 2);   // nums = [1, 2, 5]
numArray.sumRange(0, 2); // return 1 + 2 + 5 = 8

---

#### 315. Count of Smaller Numbers After Self
**Link:** [https://leetcode.com/problems/count-of-smaller-numbers-after-self/](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)  
**Problem:** Given an integer array `nums`, return an integer array `counts` where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

```python
import bisect

def countSmaller(nums: list[int]) -> list[int]:
    sorted_nums = sorted(set(nums))
    m = len(sorted_nums)
    bit = [0] * (m + 1)
    res = [0] * len(nums)

    def update(idx: int) -> None:
        while idx <= m:
            bit[idx] += 1
            idx += idx & (-idx)

    def query(idx: int) -> int:
        s = 0
        while idx > 0:
            s += bit[idx]
            idx -= idx & (-idx)
        return s

    for i in range(len(nums) - 1, -1, -1):
        idx = bisect.bisect_left(sorted_nums, nums[i]) + 1
        res[i] = query(idx - 1)
        update(idx)

    return res
```

**Example 1:**
**Input:** `nums = [5,2,6,1]`
**Output:** `[2,1,1,0]`
**Explanation:**
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.

**Example 2:**
**Input:** `nums = [-1]`
**Output:** `[0]`

**Example 3:**
**Input:** `nums = [-1,-1]`
**Output:** `[0,0]`

---

## 15. MULTI-SOURCE BFS

---

#### 542. 01 Matrix
**Link:** [https://leetcode.com/problems/01-matrix/](https://leetcode.com/problems/01-matrix/)  
**Problem:** Given an `m x n` binary matrix `mat`, return the distance of the nearest 0 for each cell. The distance between two adjacent cells is 1.

```python
from collections import deque

def updateMatrix(mat: list[list[int]]) -> list[list[int]]:
    m, n = len(mat), len(mat[0])
    dist = [[float('inf')] * n for _ in range(m)]
    q = deque()
    for i in range(m):
        for j in range(n):
            if mat[i][j] == 0:
                dist[i][j] = 0
                q.append((i, j))
    dirs = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    while q:
        r, c = q.popleft()
        for dr, dc in dirs:
            nr, nc = r + dr, c + dc
            if 0 <= nr < m and 0 <= nc < n and dist[nr][nc] > dist[r][c] + 1:
                dist[nr][nc] = dist[r][c] + 1
                q.append((nr, nc))
    return dist
```

**Example 1:**
**Input:** `mat = [[0,0,0],[0,1,0],[0,0,0]]`
**Output:** `[[0,0,0],[0,1,0],[0,0,0]]`

**Example 2:**
**Input:** `mat = [[0,0,0],[0,1,0],[1,1,1]]`
**Output:** `[[0,0,0],[0,1,0],[1,2,1]]`

---

#### 417. Pacific Atlantic Water Flow
**Link:** [https://leetcode.com/problems/pacific-atlantic-water-flow/](https://leetcode.com/problems/pacific-atlantic-water-flow/)  
**Problem:** There is an `m x n` rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges. Water can flow to a neighboring cell (north, south, east, west) if the neighboring cell's height is less than or equal to the current cell's height. Return a list of grid coordinates `result` where `result[i] = [ri, ci]` denotes that rain water can flow from cell `(ri, ci)` to both the Pacific and Atlantic oceans.

```python
from collections import deque

def pacificAtlantic(heights: list[list[int]]) -> list[list[int]]:
    m, n = len(heights), len(heights[0])
    pac = [[False] * n for _ in range(m)]
    atl = [[False] * n for _ in range(m)]
    pq = deque()
    aq = deque()
    for i in range(m):
        pq.append((i, 0))
        pac[i][0] = True
        aq.append((i, n - 1))
        atl[i][n - 1] = True
    for j in range(n):
        pq.append((0, j))
        pac[0][j] = True
        aq.append((m - 1, j))
        atl[m - 1][j] = True
    dirs = [(0, 1), (0, -1), (1, 0), (-1, 0)]

    def bfs(q: deque, visited: list[list[bool]]) -> None:
        while q:
            r, c = q.popleft()
            for dr, dc in dirs:
                nr, nc = r + dr, c + dc
                if 0 <= nr < m and 0 <= nc < n and not visited[nr][nc] and heights[nr][nc] >= heights[r][c]:
                    visited[nr][nc] = True
                    q.append((nr, nc))

    bfs(pq, pac)
    bfs(aq, atl)
    res = []
    for i in range(m):
        for j in range(n):
            if pac[i][j] and atl[i][j]:
                res.append([i, j])
    return res
```

**Example 1:**
**Input:** `heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]`
**Output:** `[[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]`

**Example 2:**
**Input:** `heights = [[1]]`
**Output:** `[[0,0]]`

---

## 16. BELLMAN-FORD

---

#### 787. Cheapest Flights Within K Stops
**Link:** [https://leetcode.com/problems/cheapest-flights-within-k-stops/](https://leetcode.com/problems/cheapest-flights-within-k-stops/)  
**Problem:** There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]`. You are also given three integers `src`, `dst`, and `k`. Return the cheapest price from `src` to `dst` with at most `k` stops. If there is no such route, return -1.

```python
def findCheapestPrice(n: int, flights: list[list[int]], src: int, dst: int, k: int) -> int:
    prices = [float('inf')] * n
    prices[src] = 0
    for _ in range(k + 1):
        tmp = list(prices)
        for u, v, price in flights:
            if prices[u] != float('inf') and prices[u] + price < tmp[v]:
                tmp[v] = prices[u] + price
        prices = tmp
    return -1 if prices[dst] == float('inf') else prices[dst]
```

**Example 1:**
**Input:** `n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1`
**Output:** `700`

**Example 2:**
**Input:** `n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1`
**Output:** `200`

**Example 3:**
**Input:** `n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0`
**Output:** `500`

---

## 17. MINIMUM SPANNING TREE

---

#### Kruskal's MST (classic template)
**Link:** [https://leetcode.com/problems/min-cost-to-connect-all-points/](https://leetcode.com/problems/min-cost-to-connect-all-points/)  
**Problem:** You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`. The cost of connecting two points is the Manhattan distance between them. Return the minimum cost to make all points connected.

```python
def minCostConnectPoints(points: list[list[int]]) -> int:
    n = len(points)
    parent = list(range(n))

    def find(x: int) -> int:
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    edges = []
    for i in range(n):
        for j in range(i + 1, n):
            dist = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
            edges.append((dist, i, j))
    edges.sort()
    res = 0
    cnt = 0
    for d, u, v in edges:
        pu, pv = find(u), find(v)
        if pu != pv:
            parent[pu] = pv
            res += d
            cnt += 1
            if cnt == n - 1:
                break
    return res
```

**Example 1:**
**Input:** `points = [[0,0],[2,2],[3,10],[5,2],[7,0]]`
**Output:** `20`

**Example 2:**
**Input:** `points = [[3,12],[-2,5],[-4,1]]`
**Output:** `18`

---

## 18. DP + BITMASK

---

#### 847. Shortest Path Visiting All Nodes
**Link:** [https://leetcode.com/problems/shortest-path-visiting-all-nodes/](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)  
**Problem:** You have an undirected, connected graph of `n` nodes labeled from 0 to `n - 1`. Given an array `graph` where `graph[i]` is a list of all the nodes connected with node `i` by an edge, return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you can reuse edges.

```python
from collections import deque

def shortestPathLength(graph: list[list[int]]) -> int:
    n = len(graph)
    full = (1 << n) - 1
    q = deque()
    visited = set()
    for i in range(n):
        state = (i, 1 << i)
        q.append((i, 1 << i, 0))
        visited.add(state)
    while q:
        node, mask, dist = q.popleft()
        if mask == full:
            return dist
        for nb in graph[node]:
            new_mask = mask | (1 << nb)
            state = (nb, new_mask)
            if state not in visited:
                visited.add(state)
                q.append((nb, new_mask, dist + 1))
    return -1
```

**Example 1:**
**Input:** `graph = [[1,2,3],[0],[0],[0]]`
**Output:** `4`
**Explanation:** One possible path is [1,0,2,0,3]

**Example 2:**
**Input:** `graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]`
**Output:** `4`
**Explanation:** One possible path is [0,1,4,2,3]

---

## 19. FLOYD'S CYCLE DETECTION (Standalone)

---

#### 287. Find the Duplicate Number (Floyd's)
**Link:** [https://leetcode.com/problems/find-the-duplicate-number/](https://leetcode.com/problems/find-the-duplicate-number/)  
**Problem:** Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]`, there is only one repeated number — find it using Floyd's cycle detection without modifying the array.

```python
# Already covered above under Cyclic Sort section — Floyd's variant:
def findDuplicate(nums: list[int]) -> int:
    slow = nums[0]
    fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```

**Example 1:**
**Input:** `nums = [1,3,4,2,2]`
**Output:** `2`

**Example 2:**
**Input:** `nums = [3,1,3,4,2]`
**Output:** `3`

**Example 3:**
**Input:** `nums = [3,3,3,3,3]`
**Output:** `3`

---
