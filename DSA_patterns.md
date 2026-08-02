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

```cpp
int findMax(vector<int>& nums) {
    int maxVal = INT_MIN;
    for (int n : nums) maxVal = max(maxVal, n);
    return maxVal;
}
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

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, maxProfit = 0;
    for (int p : prices) {
        minPrice = min(minPrice, p);
        maxProfit = max(maxProfit, p - minPrice);
    }
    return maxProfit;
}
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

```cpp
int majorityElement(vector<int>& nums) {
    int count = 0, candidate = 0;
    for (int n : nums) {
        if (count == 0) candidate = n;
        count += (n == candidate) ? 1 : -1;
    }
    return candidate;
}
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

```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    int l = 0, r = numbers.size() - 1;
    while (l < r) {
        int sum = numbers[l] + numbers[r];
        if (sum == target) return {l + 1, r + 1};
        else if (sum < target) l++;
        else r--;
    }
    return {};
}
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

```cpp
int maxArea(vector<int>& height) {
    int l = 0, r = height.size() - 1, res = 0;
    while (l < r) {
        res = max(res, min(height[l], height[r]) * (r - l));
        if (height[l] < height[r]) l++;
        else r--;
    }
    return res;
}
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

```cpp
void moveZeroes(vector<int>& nums) {
    int pos = 0;
    for (int n : nums) if (n != 0) nums[pos++] = n;
    while (pos < nums.size()) nums[pos++] = 0;
}
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

```cpp
double findMaxAverage(vector<int>& nums, int k) {
    double sum = 0;
    for (int i = 0; i < k; i++) sum += nums[i];
    double maxSum = sum;
    for (int i = k; i < nums.size(); i++) {
        sum += nums[i] - nums[i - k];
        maxSum = max(maxSum, sum);
    }
    return maxSum / k;
}
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

```cpp
long long maximumSubarraySum(vector<int>& nums, int k) {
    unordered_map<int,int> freq;
    long long sum = 0, res = 0;
    for (int i = 0; i < nums.size(); i++) {
        freq[nums[i]]++; sum += nums[i];
        if (i >= k) {
            freq[nums[i-k]]--;
            if (freq[nums[i-k]] == 0) freq.erase(nums[i-k]);
            sum -= nums[i-k];
        }
        if (i >= k-1 && freq.size() == k) res = max(res, sum);
    }
    return res;
}
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

```cpp
int lengthOfLongestSubstring(string s) {
    unordered_map<char,int> mp;
    int l = 0, res = 0;
    for (int r = 0; r < s.size(); r++) {
        if (mp.count(s[r])) l = max(l, mp[s[r]] + 1);
        mp[s[r]] = r;
        res = max(res, r - l + 1);
    }
    return res;
}
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

```cpp
int minSubArrayLen(int target, vector<int>& nums) {
    int l = 0, sum = 0, res = INT_MAX;
    for (int r = 0; r < nums.size(); r++) {
        sum += nums[r];
        while (sum >= target) {
            res = min(res, r - l + 1);
            sum -= nums[l++];
        }
    }
    return res == INT_MAX ? 0 : res;
}
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

```cpp
int totalFruit(vector<int>& fruits) {
    unordered_map<int,int> basket;
    int l = 0, res = 0;
    for (int r = 0; r < fruits.size(); r++) {
        basket[fruits[r]]++;
        while (basket.size() > 2) {
            basket[fruits[l]]--;
            if (basket[fruits[l]] == 0) basket.erase(fruits[l]);
            l++;
        }
        res = max(res, r - l + 1);
    }
    return res;
}
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

```cpp
class NumArray {
    vector<int> prefix;
public:
    NumArray(vector<int>& nums) {
        prefix.resize(nums.size() + 1, 0);
        for (int i = 0; i < nums.size(); i++)
            prefix[i+1] = prefix[i] + nums[i];
    }
    int sumRange(int left, int right) {
        return prefix[right+1] - prefix[left];
    }
};
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

```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int,int> mp{{0,1}};
    int sum = 0, res = 0;
    for (int n : nums) {
        sum += n;
        res += mp[sum - k];
        mp[sum]++;
    }
    return res;
}
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

```cpp
bool checkSubarraySum(vector<int>& nums, int k) {
    unordered_map<int,int> mp{{0,-1}};
    int sum = 0;
    for (int i = 0; i < nums.size(); i++) {
        sum = (sum + nums[i]) % k;
        if (mp.count(sum)) {
            if (i - mp[sum] >= 2) return true;
        } else mp[sum] = i;
    }
    return false;
}
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

```cpp
vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
    vector<int> diff(n + 1, 0);
    for (auto& b : bookings) {
        diff[b[0]-1] += b[2];
        if (b[1] < n) diff[b[1]] -= b[2];
    }
    for (int i = 1; i < n; i++) diff[i] += diff[i-1];
    return diff;
}
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

```cpp
bool carPooling(vector<vector<int>>& trips, int capacity) {
    vector<int> diff(1001, 0);
    for (auto& t : trips) {
        diff[t[1]] += t[0];
        diff[t[2]] -= t[0];
    }
    int cur = 0;
    for (int d : diff) {
        cur += d;
        if (cur > capacity) return false;
    }
    return true;
}
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

```cpp
int maxSubArray(vector<int>& nums) {
    int cur = nums[0], res = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        cur = max(nums[i], cur + nums[i]);
        res = max(res, cur);
    }
    return res;
}
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

```cpp
int maxAbsoluteSum(vector<int>& nums) {
    int maxSum = 0, minSum = 0, curMax = 0, curMin = 0;
    for (int n : nums) {
        curMax = max(n, curMax + n);
        curMin = min(n, curMin + n);
        maxSum = max(maxSum, curMax);
        minSum = min(minSum, curMin);
    }
    return max(maxSum, -minSum);
}
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

```cpp
int search(vector<int>& nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}
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

```cpp
int searchInsert(vector<int>& nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return l;
}
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

```cpp
int search(vector<int>& nums, int target) {
    int l = 0, r = nums.size() - 1;
    while (l <= r) {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        if (nums[l] <= nums[mid]) {
            if (nums[l] <= target && target < nums[mid]) r = mid - 1;
            else l = mid + 1;
        } else {
            if (nums[mid] < target && target <= nums[r]) l = mid + 1;
            else r = mid - 1;
        }
    }
    return -1;
}
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

```cpp
int minEatingSpeed(vector<int>& piles, int h) {
    int l = 1, r = *max_element(piles.begin(), piles.end());
    while (l < r) {
        int mid = l + (r - l) / 2;
        long long hours = 0;
        for (int p : piles) hours += (p + mid - 1) / mid;
        if (hours <= h) r = mid;
        else l = mid + 1;
    }
    return l;
}
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

```cpp
int shipWithinDays(vector<int>& weights, int days) {
    int l = *max_element(weights.begin(), weights.end());
    int r = accumulate(weights.begin(), weights.end(), 0);
    while (l < r) {
        int mid = l + (r - l) / 2, need = 1, cur = 0;
        for (int w : weights) {
            if (cur + w > mid) { need++; cur = 0; }
            cur += w;
        }
        if (need <= days) r = mid;
        else l = mid + 1;
    }
    return l;
}
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

```cpp
int splitArray(vector<int>& nums, int k) {
    int l = *max_element(nums.begin(), nums.end());
    int r = accumulate(nums.begin(), nums.end(), 0);
    while (l < r) {
        int mid = l + (r - l) / 2, parts = 1, cur = 0;
        for (int n : nums) {
            if (cur + n > mid) { parts++; cur = 0; }
            cur += n;
        }
        if (parts <= k) r = mid;
        else l = mid + 1;
    }
    return l;
}
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

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    for (int i = 0; i < nums.size(); i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        int l = i+1, r = nums.size()-1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                res.push_back({nums[i], nums[l], nums[r]});
                while (l < r && nums[l] == nums[l+1]) l++;
                while (l < r && nums[r] == nums[r-1]) r--;
                l++; r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return res;
}
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

```cpp
vector<vector<int>> fourSum(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    int n = nums.size();
    for (int i = 0; i < n-3; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        for (int j = i+1; j < n-2; j++) {
            if (j > i+1 && nums[j] == nums[j-1]) continue;
            int l = j+1, r = n-1;
            while (l < r) {
                long long sum = (long long)nums[i]+nums[j]+nums[l]+nums[r];
                if (sum == target) {
                    res.push_back({nums[i],nums[j],nums[l],nums[r]});
                    while (l < r && nums[l]==nums[l+1]) l++;
                    while (l < r && nums[r]==nums[r-1]) r--;
                    l++; r--;
                } else if (sum < target) l++;
                else r--;
            }
        }
    }
    return res;
}
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

```cpp
string largestNumber(vector<int>& nums) {
    vector<string> strs;
    for (int n : nums) strs.push_back(to_string(n));
    sort(strs.begin(), strs.end(), [](string& a, string& b){ return a+b > b+a; });
    if (strs[0] == "0") return "0";
    string res;
    for (string& s : strs) res += s;
    return res;
}
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

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> res;
    for (auto& iv : intervals) {
        if (!res.empty() && iv[0] <= res.back()[1])
            res.back()[1] = max(res.back()[1], iv[1]);
        else res.push_back(iv);
    }
    return res;
}
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

```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    vector<vector<int>> res;
    int i = 0, n = intervals.size();
    while (i < n && intervals[i][1] < newInterval[0]) res.push_back(intervals[i++]);
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = min(newInterval[0], intervals[i][0]);
        newInterval[1] = max(newInterval[1], intervals[i][1]);
        i++;
    }
    res.push_back(newInterval);
    while (i < n) res.push_back(intervals[i++]);
    return res;
}
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

```cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end(), [](auto& a, auto& b){ return a[1] < b[1]; });
    int res = 0, prevEnd = INT_MIN;
    for (auto& iv : intervals) {
        if (iv[0] >= prevEnd) prevEnd = iv[1];
        else res++;
    }
    return res;
}
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

```cpp
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> res;
    int top=0, bottom=matrix.size()-1, left=0, right=matrix[0].size()-1;
    while (top<=bottom && left<=right) {
        for (int i=left;i<=right;i++) res.push_back(matrix[top][i]); top++;
        for (int i=top;i<=bottom;i++) res.push_back(matrix[i][right]); right--;
        if (top<=bottom) { for (int i=right;i>=left;i--) res.push_back(matrix[bottom][i]); bottom--; }
        if (left<=right) { for (int i=bottom;i>=top;i--) res.push_back(matrix[i][left]); left++; }
    }
    return res;
}
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

```cpp
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for (int i=0;i<n;i++) for (int j=i+1;j<n;j++) swap(matrix[i][j], matrix[j][i]);
    for (int i=0;i<n;i++) reverse(matrix[i].begin(), matrix[i].end());
}
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

```cpp
void setZeroes(vector<vector<int>>& matrix) {
    int m=matrix.size(), n=matrix[0].size();
    bool firstRow=false, firstCol=false;
    for (int j=0;j<n;j++) if (matrix[0][j]==0) firstRow=true;
    for (int i=0;i<m;i++) if (matrix[i][0]==0) firstCol=true;
    for (int i=1;i<m;i++) for (int j=1;j<n;j++)
        if (matrix[i][j]==0) { matrix[i][0]=0; matrix[0][j]=0; }
    for (int i=1;i<m;i++) for (int j=1;j<n;j++)
        if (matrix[i][0]==0 || matrix[0][j]==0) matrix[i][j]=0;
    if (firstRow) for (int j=0;j<n;j++) matrix[0][j]=0;
    if (firstCol) for (int i=0;i<m;i++) matrix[i][0]=0;
}
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

```cpp
void sortColors(vector<int>& nums) {
    int lo=0, mid=0, hi=nums.size()-1;
    while (mid<=hi) {
        if (nums[mid]==0) swap(nums[lo++], nums[mid++]);
        else if (nums[mid]==1) mid++;
        else swap(nums[mid], nums[hi--]);
    }
}
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

```cpp
int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    for (int i=0;i<n;i++)
        while (nums[i]>0 && nums[i]<=n && nums[nums[i]-1]!=nums[i])
            swap(nums[i], nums[nums[i]-1]);
    for (int i=0;i<n;i++) if (nums[i]!=i+1) return i+1;
    return n+1;
}
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

```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {
    for (int i=0;i<nums.size();i++) {
        int idx = abs(nums[i])-1;
        if (nums[idx]>0) nums[idx] = -nums[idx];
    }
    vector<int> res;
    for (int i=0;i<nums.size();i++) if (nums[i]>0) res.push_back(i+1);
    return res;
}
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

```cpp
int findDuplicate(vector<int>& nums) {
    int slow = nums[0], fast = nums[0];
    do { slow = nums[slow]; fast = nums[nums[fast]]; } while (slow != fast);
    slow = nums[0];
    while (slow != fast) { slow = nums[slow]; fast = nums[fast]; }
    return slow;
}
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

```cpp
bool isPalindrome(string s) {
    int l=0, r=s.size()-1;
    while (l<r) {
        while (l<r && !isalnum(s[l])) l++;
        while (l<r && !isalnum(s[r])) r--;
        if (tolower(s[l++]) != tolower(s[r--])) return false;
    }
    return true;
}
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

```cpp
void reverseString(vector<char>& s) {
    int l=0, r=s.size()-1;
    while (l<r) swap(s[l++], s[r--]);
}
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

```cpp
string reverseVowels(string s) {
    string vowels = "aeiouAEIOU";
    int l=0, r=s.size()-1;
    while (l<r) {
        while (l<r && vowels.find(s[l])==string::npos) l++;
        while (l<r && vowels.find(s[r])==string::npos) r--;
        if (l<r) swap(s[l++], s[r--]);
    }
    return s;
}
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

```cpp
string minWindow(string s, string t) {
    unordered_map<char,int> need, have;
    for (char c : t) need[c]++;
    int formed=0, required=need.size(), l=0, minLen=INT_MAX, start=0;
    for (int r=0;r<s.size();r++) {
        char c = s[r];
        have[c]++;
        if (need.count(c) && have[c]==need[c]) formed++;
        while (formed==required) {
            if (r-l+1 < minLen) { minLen=r-l+1; start=l; }
            have[s[l]]--;
            if (need.count(s[l]) && have[s[l]]<need[s[l]]) formed--;
            l++;
        }
    }
    return minLen==INT_MAX ? "" : s.substr(start, minLen);
}
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

```cpp
bool checkInclusion(string s1, string s2) {
    if (s1.size() > s2.size()) return false;
    vector<int> need(26,0), have(26,0);
    for (char c : s1) need[c-'a']++;
    for (int i=0;i<s1.size();i++) have[s2[i]-'a']++;
    if (need==have) return true;
    for (int i=s1.size();i<s2.size();i++) {
        have[s2[i]-'a']++;
        have[s2[i-s1.size()]-'a']--;
        if (need==have) return true;
    }
    return false;
}
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

```cpp
bool isAnagram(string s, string t) {
    if (s.size()!=t.size()) return false;
    vector<int> cnt(26,0);
    for (int i=0;i<s.size();i++) { cnt[s[i]-'a']++; cnt[t[i]-'a']--; }
    return *max_element(cnt.begin(),cnt.end())==0 && *min_element(cnt.begin(),cnt.end())==0;
}
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

```cpp
bool canConstruct(string ransomNote, string magazine) {
    vector<int> cnt(26,0);
    for (char c : magazine) cnt[c-'a']++;
    for (char c : ransomNote) if (--cnt[c-'a'] < 0) return false;
    return true;
}
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

```cpp
char findTheDifference(string s, string t) {
    char res = 0;
    for (char c : s) res ^= c;
    for (char c : t) res ^= c;
    return res;
}
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

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for (string& s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        mp[key].push_back(s);
    }
    vector<vector<string>> res;
    for (auto& [k,v] : mp) res.push_back(v);
    return res;
}
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

```cpp
bool isIsomorphic(string s, string t) {
    unordered_map<char,char> s2t, t2s;
    for (int i=0;i<s.size();i++) {
        if (s2t.count(s[i]) && s2t[s[i]]!=t[i]) return false;
        if (t2s.count(t[i]) && t2s[t[i]]!=s[i]) return false;
        s2t[s[i]]=t[i]; t2s[t[i]]=s[i];
    }
    return true;
}
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

```cpp
bool wordPattern(string pattern, string s) {
    unordered_map<char,string> c2w;
    unordered_map<string,char> w2c;
    istringstream iss(s);
    string word; int i=0;
    while (iss >> word) {
        if (i >= pattern.size()) return false;
        char c = pattern[i++];
        if (c2w.count(c) && c2w[c]!=word) return false;
        if (w2c.count(word) && w2c[word]!=c) return false;
        c2w[c]=word; w2c[word]=c;
    }
    return i == pattern.size();
}
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

```cpp
int myAtoi(string s) {
    int i=0, sign=1; long res=0;
    while (i<s.size() && s[i]==' ') i++;
    if (i<s.size() && (s[i]=='-'||s[i]=='+')) sign=(s[i++]=='+') ? 1 : -1;
    while (i<s.size() && isdigit(s[i])) {
        res = res*10 + (s[i++]-'0');
        if (res*sign > INT_MAX) return INT_MAX;
        if (res*sign < INT_MIN) return INT_MIN;
    }
    return res*sign;
}
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

```cpp
string decodeString(string s) {
    stack<int> counts;
    stack<string> stk;
    string cur = "";
    int k = 0;
    for (char c : s) {
        if (isdigit(c)) k = k*10 + (c-'0');
        else if (c == '[') { counts.push(k); stk.push(cur); k=0; cur=""; }
        else if (c == ']') {
            int n = counts.top(); counts.pop();
            string prev = stk.top(); stk.pop();
            while (n--) prev += cur;
            cur = prev;
        } else cur += c;
    }
    return cur;
}
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

```cpp
int strStr(string haystack, string needle) {
    int m=needle.size(), n=haystack.size();
    vector<int> lps(m,0);
    for (int i=1,j=0;i<m;) {
        if (needle[i]==needle[j]) lps[i++]=++j;
        else if (j) j=lps[j-1];
        else lps[i++]=0;
    }
    for (int i=0,j=0;i<n;) {
        if (haystack[i]==needle[j]) { i++; j++; }
        if (j==m) return i-j;
        else if (i<n && haystack[i]!=needle[j]) {
            if (j) j=lps[j-1]; else i++;
        }
    }
    return -1;
}
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

```cpp
struct TrieNode {
    TrieNode* children[26] = {};
    bool isEnd = false;
};
class Trie {
    TrieNode* root = new TrieNode();
public:
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c-'a']) node->children[c-'a'] = new TrieNode();
            node = node->children[c-'a'];
        }
        node->isEnd = true;
    }
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c-'a']) return false;
            node = node->children[c-'a'];
        }
        return node->isEnd;
    }
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            if (!node->children[c-'a']) return false;
            node = node->children[c-'a'];
        }
        return true;
    }
};
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

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int,int> freq;
    for (int n : nums) freq[n]++;
    vector<vector<int>> bucket(nums.size()+1);
    for (auto& [n,f] : freq) bucket[f].push_back(n);
    vector<int> res;
    for (int i=bucket.size()-1;i>=0&&res.size()<k;i--)
        for (int n : bucket[i]) res.push_back(n);
    return res;
}
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

```cpp
string frequencySort(string s) {
    unordered_map<char,int> freq;
    for (char c : s) freq[c]++;
    priority_queue<pair<int,char>> pq;
    for (auto& [c,f] : freq) pq.push({f,c});
    string res;
    while (!pq.empty()) {
        auto [f,c] = pq.top(); pq.pop();
        res += string(f, c);
    }
    return res;
}
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

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> mp;
    for (int i=0;i<nums.size();i++) {
        int comp = target - nums[i];
        if (mp.count(comp)) return {mp[comp], i};
        mp[nums[i]] = i;
    }
    return {};
}
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

```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> seen;
    for (int n : nums) {
        if (seen.count(n)) return true;
        seen.insert(n);
    }
    return false;
}
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

```cpp
bool isHappy(int n) {
    unordered_set<int> seen;
    while (n != 1) {
        int sum = 0;
        while (n) { sum += (n%10)*(n%10); n /= 10; }
        if (seen.count(sum)) return false;
        seen.insert(sum);
        n = sum;
    }
    return true;
}
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

```cpp
int findMaxLength(vector<int>& nums) {
    unordered_map<int,int> mp{{0,-1}};
    int sum=0, res=0;
    for (int i=0;i<nums.size();i++) {
        sum += nums[i]==1 ? 1 : -1;
        if (mp.count(sum)) res = max(res, i-mp[sum]);
        else mp[sum] = i;
    }
    return res;
}
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

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> st(nums.begin(), nums.end());
    int res = 0;
    for (int n : st) {
        if (!st.count(n-1)) {
            int len = 1;
            while (st.count(n+len)) len++;
            res = max(res, len);
        }
    }
    return res;
}
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

```cpp
vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
    unordered_set<int> s(nums1.begin(), nums1.end());
    vector<int> res;
    for (int n : nums2) if (s.count(n)) { res.push_back(n); s.erase(n); }
    return res;
}
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

```cpp
vector<vector<string>> findDuplicate(vector<string>& paths) {
    unordered_map<string, vector<string>> mp;
    for (string& path : paths) {
        istringstream iss(path);
        string dir, file;
        iss >> dir;
        while (iss >> file) {
            int l = file.find('('), r = file.find(')');
            string content = file.substr(l+1, r-l-1);
            string name = dir + "/" + file.substr(0, l);
            mp[content].push_back(name);
        }
    }
    vector<vector<string>> res;
    for (auto& [k,v] : mp) if (v.size() > 1) res.push_back(v);
    return res;
}
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

```cpp
bool hasCycle(ListNode *head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
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

```cpp
ListNode* middleNode(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) { slow = slow->next; fast = fast->next->next; }
    return slow;
}
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

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode *prev = nullptr, *cur = head;
    while (cur) { ListNode* nxt = cur->next; cur->next = prev; prev = cur; cur = nxt; }
    return prev;
}
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

```cpp
ListNode* reverseBetween(ListNode* head, int left, int right) {
    ListNode dummy(0, head);
    ListNode* pre = &dummy;
    for (int i=1;i<left;i++) pre = pre->next;
    ListNode* cur = pre->next;
    for (int i=0;i<right-left;i++) {
        ListNode* nxt = cur->next;
        cur->next = nxt->next;
        nxt->next = pre->next;
        pre->next = nxt;
    }
    return dummy.next;
}
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

```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
    ListNode* cur = head;
    int count = 0;
    while (cur && count < k) { cur = cur->next; count++; }
    if (count < k) return head;
    ListNode *prev = nullptr, *node = head;
    for (int i=0;i<k;i++) { ListNode* nxt = node->next; node->next = prev; prev = node; node = nxt; }
    head->next = reverseKGroup(node, k);
    return prev;
}
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

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* cur = &dummy;
    while (l1 && l2) {
        if (l1->val <= l2->val) { cur->next = l1; l1 = l1->next; }
        else { cur->next = l2; l2 = l2->next; }
        cur = cur->next;
    }
    cur->next = l1 ? l1 : l2;
    return dummy.next;
}
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

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<pair<int,ListNode*>, vector<pair<int,ListNode*>>, greater<>> pq;
    for (ListNode* l : lists) if (l) pq.push({l->val, l});
    ListNode dummy(0);
    ListNode* cur = &dummy;
    while (!pq.empty()) {
        auto [val, node] = pq.top(); pq.pop();
        cur->next = node; cur = cur->next;
        if (node->next) pq.push({node->next->val, node->next});
    }
    return dummy.next;
}
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

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0, head);
    ListNode *fast = &dummy, *slow = &dummy;
    for (int i=0;i<=n;i++) fast = fast->next;
    while (fast) { fast = fast->next; slow = slow->next; }
    slow->next = slow->next->next;
    return dummy.next;
}
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

```cpp
ListNode* swapPairs(ListNode* head) {
    ListNode dummy(0, head);
    ListNode* pre = &dummy;
    while (pre->next && pre->next->next) {
        ListNode *a = pre->next, *b = pre->next->next;
        pre->next = b; a->next = b->next; b->next = a;
        pre = a;
    }
    return dummy.next;
}
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

```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c=='('||c=='{'||c=='[') st.push(c);
        else {
            if (st.empty()) return false;
            if (c==')' && st.top()!='(') return false;
            if (c=='}' && st.top()!='{') return false;
            if (c==']' && st.top()!='[') return false;
            st.pop();
        }
    }
    return st.empty();
}
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

```cpp
class MinStack {
    stack<pair<int,int>> st;
public:
    void push(int val) {
        int minVal = st.empty() ? val : min(val, st.top().second);
        st.push({val, minVal});
    }
    void pop() { st.pop(); }
    int top() { return st.top().first; }
    int getMin() { return st.top().second; }
};
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

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int,int> nge;
    stack<int> st;
    for (int n : nums2) {
        while (!st.empty() && st.top() < n) { nge[st.top()] = n; st.pop(); }
        st.push(n);
    }
    vector<int> res;
    for (int n : nums1) res.push_back(nge.count(n) ? nge[n] : -1);
    return res;
}
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

```cpp
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> res(n, 0);
    stack<int> st;
    for (int i=0;i<n;i++) {
        while (!st.empty() && temperatures[i] > temperatures[st.top()]) {
            res[st.top()] = i - st.top(); st.pop();
        }
        st.push(i);
    }
    return res;
}
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

```cpp
int largestRectangleArea(vector<int>& heights) {
    stack<int> st;
    int res = 0;
    heights.push_back(0);
    for (int i=0;i<heights.size();i++) {
        while (!st.empty() && heights[i] < heights[st.top()]) {
            int h = heights[st.top()]; st.pop();
            int w = st.empty() ? i : i - st.top() - 1;
            res = max(res, h * w);
        }
        st.push(i);
    }
    return res;
}
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

```cpp
int evalRPN(vector<string>& tokens) {
    stack<long long> st;
    for (string& t : tokens) {
        if (t=="+"||t=="-"||t=="*"||t=="/") {
            long long b=st.top(); st.pop();
            long long a=st.top(); st.pop();
            if (t=="+") st.push(a+b);
            else if (t=="-") st.push(a-b);
            else if (t=="*") st.push(a*b);
            else st.push(a/b);
        } else st.push(stoll(t));
    }
    return st.top();
}
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

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();
        vector<int> level;
        for (int i=0;i<sz;i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        res.push_back(level);
    }
    return res;
}
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

```cpp
int orangesRotting(vector<vector<int>>& grid) {
    int m=grid.size(), n=grid[0].size(), fresh=0, time=0;
    queue<pair<int,int>> q;
    for (int i=0;i<m;i++) for (int j=0;j<n;j++) {
        if (grid[i][j]==2) q.push({i,j});
        if (grid[i][j]==1) fresh++;
    }
    int dirs[][2]={{0,1},{0,-1},{1,0},{-1,0}};
    while (!q.empty() && fresh) {
        time++;
        for (int sz=q.size();sz--;) {
            auto [r,c] = q.front(); q.pop();
            for (auto& d : dirs) {
                int nr=r+d[0], nc=c+d[1];
                if (nr>=0&&nr<m&&nc>=0&&nc<n&&grid[nr][nc]==1) {
                    grid[nr][nc]=2; fresh--; q.push({nr,nc});
                }
            }
        }
    }
    return fresh ? -1 : time;
}
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

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> res;
    for (int i=0;i<nums.size();i++) {
        while (!dq.empty() && dq.front() < i-k+1) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] < nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k-1) res.push_back(nums[dq.front()]);
    }
    return res;
}
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

```cpp
vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
    priority_queue<pair<int,int>> pq;
    for (int i=0;i<points.size();i++) {
        int d = points[i][0]*points[i][0] + points[i][1]*points[i][1];
        pq.push({d, i});
        if (pq.size() > k) pq.pop();
    }
    vector<vector<int>> res;
    while (!pq.empty()) { res.push_back(points[pq.top().second]); pq.pop(); }
    return res;
}
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

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;
    for (int n : nums) {
        pq.push(n);
        if (pq.size() > k) pq.pop();
    }
    return pq.top();
}
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

```cpp
class MedianFinder {
    priority_queue<int> lo;
    priority_queue<int, vector<int>, greater<int>> hi;
public:
    void addNum(int num) {
        lo.push(num);
        hi.push(lo.top()); lo.pop();
        if (hi.size() > lo.size()) { lo.push(hi.top()); hi.pop(); }
    }
    double findMedian() {
        return lo.size() > hi.size() ? lo.top() : (lo.top() + hi.top()) / 2.0;
    }
};
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

```cpp
int findMaximizedCapital(int k, int w, vector<int>& profits, vector<int>& capital) {
    int n = profits.size();
    vector<pair<int,int>> projects(n);
    for (int i=0;i<n;i++) projects[i] = {capital[i], profits[i]};
    sort(projects.begin(), projects.end());
    priority_queue<int> pq;
    int i = 0;
    while (k--) {
        while (i < n && projects[i].first <= w) pq.push(projects[i++].second);
        if (pq.empty()) break;
        w += pq.top(); pq.pop();
    }
    return w;
}
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

```cpp
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
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

```cpp
bool hasPathSum(TreeNode* root, int targetSum) {
    if (!root) return false;
    if (!root->left && !root->right) return root->val == targetSum;
    return hasPathSum(root->left, targetSum - root->val) ||
           hasPathSum(root->right, targetSum - root->val);
}
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

```cpp
int diameterOfBinaryTree(TreeNode* root) {
    int res = 0;
    function<int(TreeNode*)> dfs = [&](TreeNode* node) -> int {
        if (!node) return 0;
        int l = dfs(node->left), r = dfs(node->right);
        res = max(res, l + r);
        return 1 + max(l, r);
    };
    dfs(root);
    return res;
}
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

```cpp
vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    if (!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        for (int i=q.size();i>0;i--) {
            TreeNode* node = q.front(); q.pop();
            if (i==1) res.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    return res;
}
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

```cpp
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    bool leftToRight = true;
    while (!q.empty()) {
        int sz = q.size();
        vector<int> level(sz);
        for (int i=0;i<sz;i++) {
            TreeNode* node = q.front(); q.pop();
            int idx = leftToRight ? i : sz-1-i;
            level[idx] = node->val;
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        res.push_back(level);
        leftToRight = !leftToRight;
    }
    return res;
}
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

```cpp
bool isValidBST(TreeNode* root, long lo=LONG_MIN, long hi=LONG_MAX) {
    if (!root) return true;
    if (root->val <= lo || root->val >= hi) return false;
    return isValidBST(root->left, lo, root->val) &&
           isValidBST(root->right, root->val, hi);
}
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

```cpp
int kthSmallest(TreeNode* root, int k) {
    int res = 0;
    function<void(TreeNode*)> inorder = [&](TreeNode* node) {
        if (!node) return;
        inorder(node->left);
        if (--k == 0) res = node->val;
        inorder(node->right);
    };
    inorder(root);
    return res;
}
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

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (p->val < root->val && q->val < root->val) return lowestCommonAncestor(root->left, p, q);
    if (p->val > root->val && q->val > root->val) return lowestCommonAncestor(root->right, p, q);
    return root;
}
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

```cpp
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    unordered_map<int,int> idx;
    for (int i=0;i<inorder.size();i++) idx[inorder[i]] = i;
    int pre = 0;
    function<TreeNode*(int,int)> build = [&](int l, int r) -> TreeNode* {
        if (l > r) return nullptr;
        int val = preorder[pre++];
        TreeNode* node = new TreeNode(val);
        node->left = build(l, idx[val]-1);
        node->right = build(idx[val]+1, r);
        return node;
    };
    return build(0, inorder.size()-1);
}
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

```cpp
int rob(TreeNode* root) {
    function<pair<int,int>(TreeNode*)> dfs = [&](TreeNode* node) -> pair<int,int> {
        if (!node) return {0, 0};
        auto [ll, lr] = dfs(node->left);
        auto [rl, rr] = dfs(node->right);
        int rob = node->val + ll + rl;
        int skip = max(ll, lr) + max(rl, rr);
        return {rob, skip};
    };
    auto [r, s] = dfs(root);
    return max(r, s);
}
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

```cpp
int maxPathSum(TreeNode* root) {
    int res = INT_MIN;
    function<int(TreeNode*)> dfs = [&](TreeNode* node) -> int {
        if (!node) return 0;
        int l = max(0, dfs(node->left));
        int r = max(0, dfs(node->right));
        res = max(res, node->val + l + r);
        return node->val + max(l, r);
    };
    dfs(root);
    return res;
}
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

```cpp
int numIslands(vector<vector<char>>& grid) {
    int m=grid.size(), n=grid[0].size(), res=0;
    function<void(int,int)> dfs = [&](int r, int c) {
        if (r<0||r>=m||c<0||c>=n||grid[r][c]!='1') return;
        grid[r][c] = '0';
        dfs(r+1,c); dfs(r-1,c); dfs(r,c+1); dfs(r,c-1);
    };
    for (int i=0;i<m;i++) for (int j=0;j<n;j++)
        if (grid[i][j]=='1') { res++; dfs(i,j); }
    return res;
}
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

```cpp
Node* cloneGraph(Node* node) {
    unordered_map<Node*, Node*> mp;
    function<Node*(Node*)> dfs = [&](Node* n) -> Node* {
        if (!n) return nullptr;
        if (mp.count(n)) return mp[n];
        mp[n] = new Node(n->val);
        for (Node* nb : n->neighbors) mp[n]->neighbors.push_back(dfs(nb));
        return mp[n];
    };
    return dfs(node);
}
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

```cpp
int maxAreaOfIsland(vector<vector<int>>& grid) {
    int m=grid.size(), n=grid[0].size(), res=0;
    function<int(int,int)> dfs = [&](int r, int c) -> int {
        if (r<0||r>=m||c<0||c>=n||grid[r][c]==0) return 0;
        grid[r][c] = 0;
        return 1 + dfs(r+1,c) + dfs(r-1,c) + dfs(r,c+1) + dfs(r,c-1);
    };
    for (int i=0;i<m;i++) for (int j=0;j<n;j++)
        res = max(res, dfs(i,j));
    return res;
}
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

```cpp
int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    unordered_set<string> wordSet(wordList.begin(), wordList.end());
    if (!wordSet.count(endWord)) return 0;
    queue<string> q;
    q.push(beginWord);
    int steps = 1;
    while (!q.empty()) {
        for (int sz=q.size();sz--;) {
            string word = q.front(); q.pop();
            if (word == endWord) return steps;
            for (int i=0;i<word.size();i++) {
                char orig = word[i];
                for (char c='a';c<='z';c++) {
                    word[i] = c;
                    if (wordSet.count(word)) { q.push(word); wordSet.erase(word); }
                }
                word[i] = orig;
            }
        }
        steps++;
    }
    return 0;
}
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

```cpp
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adj(numCourses);
    vector<int> indegree(numCourses, 0);
    for (auto& p : prerequisites) { adj[p[1]].push_back(p[0]); indegree[p[0]]++; }
    queue<int> q;
    for (int i=0;i<numCourses;i++) if (indegree[i]==0) q.push(i);
    int count = 0;
    while (!q.empty()) {
        int node = q.front(); q.pop(); count++;
        for (int nb : adj[node]) if (--indegree[nb]==0) q.push(nb);
    }
    return count == numCourses;
}
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

```cpp
vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adj(numCourses);
    vector<int> indegree(numCourses, 0);
    for (auto& p : prerequisites) { adj[p[1]].push_back(p[0]); indegree[p[0]]++; }
    queue<int> q;
    for (int i=0;i<numCourses;i++) if (indegree[i]==0) q.push(i);
    vector<int> order;
    while (!q.empty()) {
        int node = q.front(); q.pop();
        order.push_back(node);
        for (int nb : adj[node]) if (--indegree[nb]==0) q.push(nb);
    }
    return order.size()==numCourses ? order : vector<int>{};
}
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

```cpp
vector<int> findRedundantConnection(vector<vector<int>>& edges) {
    int n = edges.size();
    vector<int> parent(n+1);
    iota(parent.begin(), parent.end(), 0);
    function<int(int)> find = [&](int x) {
        return parent[x]==x ? x : parent[x]=find(parent[x]);
    };
    for (auto& e : edges) {
        int u=find(e[0]), v=find(e[1]);
        if (u==v) return e;
        parent[u] = v;
    }
    return {};
}
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

```cpp
int findCircleNum(vector<vector<int>>& isConnected) {
    int n = isConnected.size();
    vector<int> parent(n);
    iota(parent.begin(), parent.end(), 0);
    function<int(int)> find = [&](int x) {
        return parent[x]==x ? x : parent[x]=find(parent[x]);
    };
    int res = n;
    for (int i=0;i<n;i++) for (int j=i+1;j<n;j++)
        if (isConnected[i][j]) {
            int u=find(i), v=find(j);
            if (u!=v) { parent[u]=v; res--; }
        }
    return res;
}
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

```cpp
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<vector<pair<int,int>>> adj(n+1);
    for (auto& t : times) adj[t[0]].push_back({t[1], t[2]});
    vector<int> dist(n+1, INT_MAX);
    dist[k] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, k});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    int res = *max_element(dist.begin()+1, dist.end());
    return res == INT_MAX ? -1 : res;
}
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

```cpp
int minimumEffortPath(vector<vector<int>>& heights) {
    int m=heights.size(), n=heights[0].size();
    vector<vector<int>> effort(m, vector<int>(n, INT_MAX));
    effort[0][0] = 0;
    priority_queue<tuple<int,int,int>, vector<tuple<int,int,int>>, greater<>> pq;
    pq.push({0,0,0});
    int dirs[][2]={{0,1},{0,-1},{1,0},{-1,0}};
    while (!pq.empty()) {
        auto [e,r,c] = pq.top(); pq.pop();
        if (r==m-1 && c==n-1) return e;
        if (e > effort[r][c]) continue;
        for (auto& d : dirs) {
            int nr=r+d[0], nc=c+d[1];
            if (nr>=0&&nr<m&&nc>=0&&nc<n) {
                int ne = max(e, abs(heights[nr][nc]-heights[r][c]));
                if (ne < effort[nr][nc]) {
                    effort[nr][nc] = ne;
                    pq.push({ne,nr,nc});
                }
            }
        }
    }
    return 0;
}
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

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> cur;
    function<void(int)> bt = [&](int start) {
        res.push_back(cur);
        for (int i=start;i<nums.size();i++) {
            cur.push_back(nums[i]);
            bt(i+1);
            cur.pop_back();
        }
    };
    bt(0);
    return res;
}
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

```cpp
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    vector<int> cur;
    function<void(int)> bt = [&](int start) {
        res.push_back(cur);
        for (int i=start;i<nums.size();i++) {
            if (i>start && nums[i]==nums[i-1]) continue;
            cur.push_back(nums[i]);
            bt(i+1);
            cur.pop_back();
        }
    };
    bt(0);
    return res;
}
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

```cpp
vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> res;
    function<void(int)> bt = [&](int start) {
        if (start == nums.size()) { res.push_back(nums); return; }
        for (int i=start;i<nums.size();i++) {
            swap(nums[start], nums[i]);
            bt(start+1);
            swap(nums[start], nums[i]);
        }
    };
    bt(0);
    return res;
}
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

```cpp
vector<vector<int>> permuteUnique(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    vector<bool> used(nums.size(), false);
    vector<int> cur;
    function<void()> bt = [&]() {
        if (cur.size()==nums.size()) { res.push_back(cur); return; }
        for (int i=0;i<nums.size();i++) {
            if (used[i]) continue;
            if (i>0 && nums[i]==nums[i-1] && !used[i-1]) continue;
            used[i]=true; cur.push_back(nums[i]);
            bt();
            used[i]=false; cur.pop_back();
        }
    };
    bt();
    return res;
}
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

```cpp
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int>> res;
    vector<int> cur;
    function<void(int,int)> bt = [&](int start, int rem) {
        if (rem==0) { res.push_back(cur); return; }
        for (int i=start;i<candidates.size();i++) {
            if (candidates[i] > rem) break;
            cur.push_back(candidates[i]);
            bt(i, rem-candidates[i]);
            cur.pop_back();
        }
    };
    sort(candidates.begin(), candidates.end());
    bt(0, target);
    return res;
}
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

```cpp
bool exist(vector<vector<char>>& board, string word) {
    int m=board.size(), n=board[0].size();
    function<bool(int,int,int)> dfs = [&](int r, int c, int idx) -> bool {
        if (idx==word.size()) return true;
        if (r<0||r>=m||c<0||c>=n||board[r][c]!=word[idx]) return false;
        char tmp = board[r][c]; board[r][c] = '#';
        bool found = dfs(r+1,c,idx+1)||dfs(r-1,c,idx+1)||dfs(r,c+1,idx+1)||dfs(r,c-1,idx+1);
        board[r][c] = tmp;
        return found;
    };
    for (int i=0;i<m;i++) for (int j=0;j<n;j++) if (dfs(i,j,0)) return true;
    return false;
}
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

```cpp
vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> res;
    vector<string> board(n, string(n, '.'));
    set<int> cols, diag1, diag2;
    function<void(int)> bt = [&](int row) {
        if (row==n) { res.push_back(board); return; }
        for (int col=0;col<n;col++) {
            if (cols.count(col)||diag1.count(row-col)||diag2.count(row+col)) continue;
            cols.insert(col); diag1.insert(row-col); diag2.insert(row+col);
            board[row][col] = 'Q';
            bt(row+1);
            cols.erase(col); diag1.erase(row-col); diag2.erase(row+col);
            board[row][col] = '.';
        }
    };
    bt(0);
    return res;
}
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

```cpp
int climbStairs(int n) {
    if (n<=2) return n;
    int a=1, b=2;
    for (int i=3;i<=n;i++) { int c=a+b; a=b; b=c; }
    return b;
}
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

```cpp
int rob(vector<int>& nums) {
    int prev=0, cur=0;
    for (int n : nums) { int tmp = max(cur, prev+n); prev=cur; cur=tmp; }
    return cur;
}
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

```cpp
int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount+1, amount+1);
    dp[0] = 0;
    for (int i=1;i<=amount;i++)
        for (int c : coins)
            if (c<=i) dp[i] = min(dp[i], dp[i-c]+1);
    return dp[amount] > amount ? -1 : dp[amount];
}
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

```cpp
int uniquePaths(int m, int n) {
    vector<int> dp(n, 1);
    for (int i=1;i<m;i++) for (int j=1;j<n;j++) dp[j] += dp[j-1];
    return dp[n-1];
}
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

```cpp
int minDistance(string word1, string word2) {
    int m=word1.size(), n=word2.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    for (int i=0;i<=m;i++) dp[i][0]=i;
    for (int j=0;j<=n;j++) dp[0][j]=j;
    for (int i=1;i<=m;i++) for (int j=1;j<=n;j++) {
        if (word1[i-1]==word2[j-1]) dp[i][j]=dp[i-1][j-1];
        else dp[i][j]=1+min({dp[i-1][j],dp[i][j-1],dp[i-1][j-1]});
    }
    return dp[m][n];
}
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

```cpp
bool canPartition(vector<int>& nums) {
    int sum = accumulate(nums.begin(), nums.end(), 0);
    if (sum % 2) return false;
    int target = sum / 2;
    vector<bool> dp(target+1, false);
    dp[0] = true;
    for (int n : nums) for (int j=target;j>=n;j--) dp[j] = dp[j] || dp[j-n];
    return dp[target];
}
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

```cpp
int findTargetSumWays(vector<int>& nums, int target) {
    unordered_map<int,int> dp;
    dp[0] = 1;
    for (int n : nums) {
        unordered_map<int,int> next;
        for (auto& [sum, cnt] : dp) {
            next[sum+n] += cnt;
            next[sum-n] += cnt;
        }
        dp = next;
    }
    return dp.count(target) ? dp[target] : 0;
}
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

```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> tails;
    for (int n : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), n);
        if (it == tails.end()) tails.push_back(n);
        else *it = n;
    }
    return tails.size();
}
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

```cpp
int longestPalindromeSubseq(string s) {
    int n = s.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int i=0;i<n;i++) dp[i][i]=1;
    for (int len=2;len<=n;len++) for (int i=0;i<=n-len;i++) {
        int j = i+len-1;
        if (s[i]==s[j]) dp[i][j] = dp[i+1][j-1]+2;
        else dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
    }
    return dp[0][n-1];
}
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

```cpp
int countSubstrings(string s) {
    int n=s.size(), res=0;
    for (int center=0;center<2*n-1;center++) {
        int l=center/2, r=l+center%2;
        while (l>=0 && r<n && s[l--]==s[r++]) res++;
    }
    return res;
}
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

```cpp
int maxCoins(vector<int>& nums) {
    nums.insert(nums.begin(), 1); nums.push_back(1);
    int n = nums.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len=2;len<n;len++) for (int l=0;l<n-len;l++) {
        int r = l+len;
        for (int k=l+1;k<r;k++)
            dp[l][r] = max(dp[l][r], nums[l]*nums[k]*nums[r]+dp[l][k]+dp[k][r]);
    }
    return dp[0][n-1];
}
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

```cpp
int findMinArrowShots(vector<vector<int>>& points) {
    sort(points.begin(), points.end(), [](auto& a, auto& b){ return a[1] < b[1]; });
    int arrows=1; long long end=points[0][1];
    for (int i=1;i<points.size();i++) {
        if (points[i][0] > end) { arrows++; end=points[i][1]; }
    }
    return arrows;
}
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

```cpp
bool canJump(vector<int>& nums) {
    int maxReach = 0;
    for (int i=0;i<nums.size();i++) {
        if (i > maxReach) return false;
        maxReach = max(maxReach, i + nums[i]);
    }
    return true;
}
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

```cpp
int jump(vector<int>& nums) {
    int jumps=0, curEnd=0, farthest=0;
    for (int i=0;i<nums.size()-1;i++) {
        farthest = max(farthest, i+nums[i]);
        if (i==curEnd) { jumps++; curEnd=farthest; }
    }
    return jumps;
}
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

```cpp
int singleNumber(vector<int>& nums) {
    int res = 0;
    for (int n : nums) res ^= n;
    return res;
}
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

```cpp
int missingNumber(vector<int>& nums) {
    int res = nums.size();
    for (int i=0;i<nums.size();i++) res ^= i ^ nums[i];
    return res;
}
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

```cpp
vector<int> singleNumber(vector<int>& nums) {
    int xorAll = 0;
    for (int n : nums) xorAll ^= n;
    int bit = xorAll & (-xorAll);
    int a=0, b=0;
    for (int n : nums) {
        if (n & bit) a ^= n;
        else b ^= n;
    }
    return {a, b};
}
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

```cpp
vector<int> countBits(int n) {
    vector<int> dp(n+1, 0);
    for (int i=1;i<=n;i++) dp[i] = dp[i>>1] + (i&1);
    return dp;
}
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

```cpp
int maxProduct(vector<string>& words) {
    int n=words.size();
    vector<int> mask(n, 0);
    for (int i=0;i<n;i++) for (char c : words[i]) mask[i] |= (1<<(c-'a'));
    int res = 0;
    for (int i=0;i<n;i++) for (int j=i+1;j<n;j++)
        if (!(mask[i]&mask[j])) res = max(res, (int)(words[i].size()*words[j].size()));
    return res;
}
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

```cpp
class NumArray {
    int n;
    vector<int> bit;
    void update(int i, int delta) { for (i++;i<=n;i+=i&(-i)) bit[i]+=delta; }
    int query(int i) { int s=0; for (i++;i>0;i-=i&(-i)) s+=bit[i]; return s; }
public:
    vector<int> nums;
    NumArray(vector<int>& nums) : n(nums.size()), bit(nums.size()+1,0), nums(nums) {
        for (int i=0;i<n;i++) update(i, nums[i]);
    }
    void update(int i, int val) { update(i, val-nums[i]); nums[i]=val; }
    int sumRange(int l, int r) { return query(r) - (l>0 ? query(l-1) : 0); }
};
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

```cpp
vector<int> countSmaller(vector<int>& nums) {
    int n=nums.size();
    vector<int> sorted_nums = nums;
    sort(sorted_nums.begin(), sorted_nums.end());
    sorted_nums.erase(unique(sorted_nums.begin(), sorted_nums.end()), sorted_nums.end());
    int m = sorted_nums.size();
    vector<int> bit(m+1, 0), res(n);
    auto getIdx = [&](int v) { return lower_bound(sorted_nums.begin(), sorted_nums.end(), v) - sorted_nums.begin() + 1; };
    auto update = [&](int i) { for (;i<=m;i+=i&(-i)) bit[i]++; };
    auto query = [&](int i) { int s=0; for (;i>0;i-=i&(-i)) s+=bit[i]; return s; };
    for (int i=n-1;i>=0;i--) {
        int idx = getIdx(nums[i]);
        res[i] = query(idx-1);
        update(idx);
    }
    return res;
}
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

```cpp
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int m=mat.size(), n=mat[0].size();
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    queue<pair<int,int>> q;
    for (int i=0;i<m;i++) for (int j=0;j<n;j++)
        if (mat[i][j]==0) { dist[i][j]=0; q.push({i,j}); }
    int dirs[][2]={{0,1},{0,-1},{1,0},{-1,0}};
    while (!q.empty()) {
        auto [r,c] = q.front(); q.pop();
        for (auto& d : dirs) {
            int nr=r+d[0], nc=c+d[1];
            if (nr>=0&&nr<m&&nc>=0&&nc<n&&dist[nr][nc]>dist[r][c]+1) {
                dist[nr][nc]=dist[r][c]+1; q.push({nr,nc});
            }
        }
    }
    return dist;
}
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

```cpp
vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
    int m=heights.size(), n=heights[0].size();
    vector<vector<bool>> pac(m,vector<bool>(n,false)), atl(m,vector<bool>(n,false));
    queue<pair<int,int>> pq, aq;
    for (int i=0;i<m;i++) { pq.push({i,0}); pac[i][0]=true; aq.push({i,n-1}); atl[i][n-1]=true; }
    for (int j=0;j<n;j++) { pq.push({0,j}); pac[0][j]=true; aq.push({m-1,j}); atl[m-1][j]=true; }
    int dirs[][2]={{0,1},{0,-1},{1,0},{-1,0}};
    auto bfs = [&](queue<pair<int,int>>& q, vector<vector<bool>>& visited) {
        while (!q.empty()) {
            auto [r,c]=q.front(); q.pop();
            for (auto& d : dirs) {
                int nr=r+d[0], nc=c+d[1];
                if (nr>=0&&nr<m&&nc>=0&&nc<n&&!visited[nr][nc]&&heights[nr][nc]>=heights[r][c])
                { visited[nr][nc]=true; q.push({nr,nc}); }
            }
        }
    };
    bfs(pq,pac); bfs(aq,atl);
    vector<vector<int>> res;
    for (int i=0;i<m;i++) for (int j=0;j<n;j++) if (pac[i][j]&&atl[i][j]) res.push_back({i,j});
    return res;
}
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

```cpp
int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
    vector<int> prices(n, INT_MAX);
    prices[src] = 0;
    for (int i=0;i<=k;i++) {
        vector<int> tmp = prices;
        for (auto& f : flights) {
            if (prices[f[0]]!=INT_MAX && prices[f[0]]+f[2]<tmp[f[1]])
                tmp[f[1]] = prices[f[0]]+f[2];
        }
        prices = tmp;
    }
    return prices[dst]==INT_MAX ? -1 : prices[dst];
}
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

```cpp
int minCostConnectPoints(vector<vector<int>>& points) {
    int n = points.size();
    vector<int> parent(n);
    iota(parent.begin(), parent.end(), 0);
    function<int(int)> find = [&](int x) {
        return parent[x]==x ? x : parent[x]=find(parent[x]);
    };
    vector<tuple<int,int,int>> edges;
    for (int i=0;i<n;i++) for (int j=i+1;j<n;j++) {
        int dist = abs(points[i][0]-points[j][0]) + abs(points[i][1]-points[j][1]);
        edges.push_back({dist,i,j});
    }
    sort(edges.begin(), edges.end());
    int res=0, cnt=0;
    for (auto& [d,u,v] : edges) {
        int pu=find(u), pv=find(v);
        if (pu!=pv) { parent[pu]=pv; res+=d; cnt++; }
        if (cnt==n-1) break;
    }
    return res;
}
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

```cpp
int shortestPathLength(vector<vector<int>>& graph) {
    int n = graph.size(), full = (1<<n)-1;
    queue<tuple<int,int,int>> q;
    set<pair<int,int>> visited;
    for (int i=0;i<n;i++) { q.push({i, 1<<i, 0}); visited.insert({i, 1<<i}); }
    while (!q.empty()) {
        auto [node, mask, dist] = q.front(); q.pop();
        if (mask == full) return dist;
        for (int nb : graph[node]) {
            int newMask = mask | (1<<nb);
            if (!visited.count({nb, newMask})) {
                visited.insert({nb, newMask});
                q.push({nb, newMask, dist+1});
            }
        }
    }
    return -1;
}
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

```cpp
// Already covered above under Cyclic Sort section — Floyd's variant:
int findDuplicate(vector<int>& nums) {
    int slow = nums[0], fast = nums[0];
    do { slow = nums[slow]; fast = nums[nums[fast]]; } while (slow != fast);
    slow = nums[0];
    while (slow != fast) { slow = nums[slow]; fast = nums[fast]; }
    return slow;
}
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
