# 📚 DSA Interview Mastery — NeetCode 150 + Bonus Problems

> **Your complete DSA roadmap.** All 150 NeetCode problems + bonus frequently-asked problems — with approach tags, detailed explanations, LeetCode links, and video solution links.

---

## 📖 How to Use This Guide

- **Approach first, code second.** Read the approach, try the problem yourself for 20–30 min, then watch the solution.
- **Difficulty tags:** 🟢 Easy | 🟡 Medium | 🔴 Hard
- **Pattern matters more than the problem.** Once you recognize the pattern, 80% of the problem is solved.
- **Check off problems as you solve them** — use this as your progress tracker.
- **C++ tip:** Always prefer `unordered_map` over `map` (O(1) vs O(log n)), `vector` over arrays, and know when to use `auto`.

---

## 🧩 The 18 Core Patterns (Master These)

| # | Pattern |
|---|---------|
| 1 | Array / Hash Map tricks |
| 2 | Two Pointers |
| 3 | Sliding Window |
| 4 | Stack (Monotonic Stack) |
| 5 | Binary Search (on answer) |
| 6 | Linked List (fast/slow pointers) |
| 7 | Trees (DFS / BFS) |
| 8 | Tries (Prefix Trees) |
| 9 | Heap / Priority Queue |
| 10 | Backtracking (explore & undo) |
| 11 | Graphs (BFS / DFS / Union-Find) |
| 12 | Advanced Graphs (Dijkstra, Bellman-Ford, Topological Sort) |
| 13 | 1D Dynamic Programming |
| 14 | 2D Dynamic Programming (Grid / String) |
| 15 | Greedy |
| 16 | Intervals |
| 17 | Math & Geometry |
| 18 | Bit Manipulation |

---

## 📊 Progress Tracker

| Topic | Total | Done |
|-------|-------|------|
| Arrays & Hashing | 9 | track manually |
| Two Pointers | 5 | track manually |
| Sliding Window | 6 | track manually |
| Stack | 7 | track manually |
| Binary Search | 7 | track manually |
| Linked List | 11 | track manually |
| Trees | 15 | track manually |
| Tries | 3 | track manually |
| Heap / Priority Queue | 7 | track manually |
| Backtracking | 10 | track manually |
| Graphs | 13 | track manually |
| Advanced Graphs | 6 | track manually |
| 1D DP | 12 | track manually |
| 2D DP | 11 | track manually |
| Greedy | 8 | track manually |
| Intervals | 6 | track manually |
| Math & Geometry | 8 | track manually |
| Bit Manipulation | 7 | track manually |
| **Bonus Problems** | **28** | track manually |
| **TOTAL** | **178** | — |

---

&nbsp;

## Section 1 — Arrays & Hashing

> **Core idea:** Use a hash map (`unordered_map` / `unordered_set`) to reduce O(n²) brute force to O(n).
> Ask yourself: *"Can I trade space for time?"*

&nbsp;

---

### 1. Contains Duplicate 🟢

- [ ] Solved

**Approach Tag:** `Hash Set`

**Problem:** Given an integer array `nums`, return `true` if any value appears **at least twice**, and `false` if every element is distinct.

**Detailed Approach:**
- Insert elements one by one into an `unordered_set`.
- Before inserting, check if the element already exists in the set.
- If it does → duplicate found, return `true`.
- If you finish the loop without finding a duplicate → return `false`.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
unordered_set<int> seen;
for (int n : nums) {
    if (seen.count(n)) return true;
    seen.insert(n);
}
return false;
```

🔗 [LeetCode](https://leetcode.com/problems/contains-duplicate/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=3OamzN90kPg)

---

&nbsp;

### 2. Valid Anagram 🟢

- [ ] Solved

**Approach Tag:** `Hash Map` | `Frequency Count`

**Problem:** Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise. An anagram uses all original letters exactly once.

**Detailed Approach:**
- If lengths differ → return `false` immediately.
- Count character frequencies using an `int freq[26]` array (or `unordered_map<char,int>`).
- Increment freq for each char in `s`, decrement for each char in `t`.
- If all values in freq are 0 → valid anagram.
- Time: O(n) | Space: O(1) (fixed 26-char alphabet)

**C++ Hint:**
```cpp
if (s.size() != t.size()) return false;
int freq[26] = {};
for (int i = 0; i < s.size(); i++) {
    freq[s[i] - 'a']++;
    freq[t[i] - 'a']--;
}
for (int f : freq) if (f != 0) return false;
return true;
```

🔗 [LeetCode](https://leetcode.com/problems/valid-anagram/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=9UtInBqnCgA)

---

&nbsp;

### 3. Two Sum 🟢

- [ ] Solved

**Approach Tag:** `Hash Map` | `One-Pass`

**Problem:** Given an array of integers `nums` and an integer `target`, return the **indices** of the two numbers that add up to `target`. Exactly one solution exists.

**Detailed Approach:**
- Use a hash map to store `value → index` as you iterate.
- For each element, compute `complement = target - nums[i]`.
- If `complement` exists in the map → return `{map[complement], i}`.
- Otherwise, store `nums[i] → i` in the map.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
unordered_map<int,int> seen;
for (int i = 0; i < nums.size(); i++) {
    int comp = target - nums[i];
    if (seen.count(comp)) return {seen[comp], i};
    seen[nums[i]] = i;
}
return {};
```

🔗 [LeetCode](https://leetcode.com/problems/two-sum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=KLlXCFG5TnA)

---

&nbsp;

### 4. Group Anagrams 🟡

- [ ] Solved

**Approach Tag:** `Hash Map` | `Sorting as Key`

**Problem:** Given an array of strings `strs`, group the anagrams together and return them in any order.

**Detailed Approach:**
- For each string, sort its characters → this sorted string is the unique **key** for its anagram group.
- Use an `unordered_map<string, vector<string>>` to group strings by their sorted key.
- Return all values of the map.
- Time: O(n · k log k) where k = max string length | Space: O(n · k)

**C++ Hint:**
```cpp
unordered_map<string, vector<string>> mp;
for (string& w : strs) {
    string key = w;
    sort(key.begin(), key.end());
    mp[key].push_back(w);
}
vector<vector<string>> res;
for (auto& [k, v] : mp) res.push_back(v);
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/group-anagrams/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=vzdNOK2oB2E)

---

&nbsp;

### 5. Top K Frequent Elements 🟡

- [ ] Solved

**Approach Tag:** `Bucket Sort` | `Heap`

**Problem:** Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Detailed Approach (Bucket Sort — O(n)):**
- Count frequencies using an `unordered_map`.
- Create a `bucket` array of size `n+1` where `bucket[freq]` stores all numbers with that frequency.
- Scan buckets from highest to lowest frequency, collecting elements until you have `k`.

**Detailed Approach (Min-Heap — O(n log k)):**
- Build frequency map.
- Maintain a min-heap of size `k`. If size exceeds `k`, pop the least frequent.
- Remaining heap contains the top `k` frequent elements.

**C++ Hint (Bucket Sort):**
```cpp
unordered_map<int,int> freq;
for (int n : nums) freq[n]++;
vector<vector<int>> bucket(nums.size() + 1);
for (auto& [num, f] : freq) bucket[f].push_back(num);
vector<int> res;
for (int i = bucket.size()-1; i >= 0 && res.size() < k; i--)
    for (int n : bucket[i]) res.push_back(n);
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/top-k-frequent-elements/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=YPTqKIgVk-k)

---

&nbsp;

### 6. Encode and Decode Strings 🟡

- [ ] Solved

**Approach Tag:** `String Encoding` | `Length-Prefix`

**Problem:** Design an algorithm to serialize a list of strings into a single string, and deserialize it back. The strings may contain any character including the delimiter.

**Detailed Approach:**
- **Encode:** Prefix each string with its length followed by a `#` delimiter: `"hello"` → `"5#hello"`.
- **Decode:** Read characters until `#`, parse the number as length `L`, then read exactly the next `L` characters as the string. Repeat.
- This handles strings containing any characters including `#` itself.
- Time: O(n) for both | Space: O(n)

**C++ Hint:**
```cpp
// Encode
string encode(vector<string>& strs) {
    string res;
    for (string& s : strs) res += to_string(s.size()) + "#" + s;
    return res;
}
// Decode
vector<string> decode(string s) {
    vector<string> res;
    int i = 0;
    while (i < s.size()) {
        int j = s.find('#', i);
        int len = stoi(s.substr(i, j - i));
        res.push_back(s.substr(j + 1, len));
        i = j + 1 + len;
    }
    return res;
}
```

🔗 [LeetCode](https://leetcode.com/problems/encode-and-decode-strings/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=B1k_sxOSgv8)

---

&nbsp;

### 7. Product of Array Except Self 🟡

- [ ] Solved

**Approach Tag:** `Prefix Product` | `Suffix Product`

**Problem:** Given an integer array `nums`, return an array `answer` where `answer[i]` is the product of all elements **except** `nums[i]`. Must run in O(n) without using the division operator.

**Detailed Approach:**
- **Left pass:** Build `res[i]` = product of all elements to the **left** of index `i`. Start with `res[0] = 1`.
- **Right pass:** Use a running variable `right = 1`. Multiply `res[i] *= right`, then update `right *= nums[i]`, scanning right to left.
- No division needed. Time: O(n) | Space: O(1) extra (output array doesn't count)

**C++ Hint:**
```cpp
int n = nums.size();
vector<int> res(n, 1);
for (int i = 1; i < n; i++) res[i] = res[i-1] * nums[i-1];
int right = 1;
for (int i = n-1; i >= 0; i--) { res[i] *= right; right *= nums[i]; }
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/product-of-array-except-self/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=bNvIQI2wAjk)

---

&nbsp;

### 8. Valid Sudoku 🟡

- [ ] Solved

**Approach Tag:** `Hash Set` | `Matrix Indexing`

**Problem:** Determine if a 9×9 Sudoku board is valid based on the rules: each row, column, and 3×3 sub-box must contain digits 1–9 with no repetition. Empty cells are marked `'.'`.

**Detailed Approach:**
- Use three sets: `rows[9]`, `cols[9]`, `boxes[9]`.
- For each cell `(r, c)` with a digit:
  - Compute box index: `(r/3)*3 + (c/3)`.
  - Check if the digit already exists in `rows[r]`, `cols[c]`, or `boxes[box]`.
  - If yes → invalid. If no → insert into all three sets.
- Time: O(81) = O(1) | Space: O(81) = O(1)

**C++ Hint:**
```cpp
unordered_set<string> seen;
for (int r = 0; r < 9; r++)
    for (int c = 0; c < 9; c++) {
        if (board[r][c] == '.') continue;
        char d = board[r][c];
        int box = (r/3)*3 + c/3;
        if (!seen.insert("r"+to_string(r)+d).second) return false;
        if (!seen.insert("c"+to_string(c)+d).second) return false;
        if (!seen.insert("b"+to_string(box)+d).second) return false;
    }
return true;
```

🔗 [LeetCode](https://leetcode.com/problems/valid-sudoku/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=TjFXEUCMqI8)

---

&nbsp;

### 9. Longest Consecutive Sequence 🟡

- [ ] Solved

**Approach Tag:** `Hash Set` | `Sequence Start Detection`

**Problem:** Given an unsorted array of integers, return the length of the **longest consecutive elements sequence** in O(n) time.

**Detailed Approach:**
- Put all numbers in an `unordered_set` for O(1) lookup.
- For each number, only **start counting** if `num - 1` is NOT in the set (meaning it's the start of a sequence).
- Count upward (`num, num+1, num+2, ...`) as long as the next number exists in the set.
- Track the global maximum length.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
unordered_set<int> s(nums.begin(), nums.end());
int res = 0;
for (int n : s) {
    if (!s.count(n - 1)) {
        int len = 1;
        while (s.count(n + len)) len++;
        res = max(res, len);
    }
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/longest-consecutive-sequence/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=P6RZZMu_maU)

---

&nbsp;

## Section 2 — Two Pointers

> **Core idea:** Use two indices moving toward each other (or in the same direction) to avoid O(n²) nested loops.
> Works best when the array is **sorted** or when you need **pairs/triplets**.

&nbsp;

---

### 10. Valid Palindrome 🟢

- [ ] Solved

**Approach Tag:** `Two Pointers`

**Problem:** Given a string `s`, return `true` if it is a palindrome after **converting to lowercase** and **removing all non-alphanumeric characters**.

**Detailed Approach:**
- Place one pointer at the start (`l = 0`) and one at the end (`r = s.size()-1`).
- Skip non-alphanumeric characters by advancing the respective pointer.
- Compare `tolower(s[l])` and `tolower(s[r])`. If they differ → not a palindrome.
- Move both pointers inward. If the loop completes → palindrome.
- Time: O(n) | Space: O(1)

**C++ Hint:**
```cpp
int l = 0, r = s.size() - 1;
while (l < r) {
    while (l < r && !isalnum(s[l])) l++;
    while (l < r && !isalnum(s[r])) r--;
    if (tolower(s[l]) != tolower(s[r])) return false;
    l++; r--;
}
return true;
```

🔗 [LeetCode](https://leetcode.com/problems/valid-palindrome/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=jJXJ16kPFWg)

---

&nbsp;

### 11. Two Sum II — Input Array Is Sorted 🟡

- [ ] Solved

**Approach Tag:** `Two Pointers` | `Sorted Array`

**Problem:** Given a **1-indexed** sorted array of integers, find two numbers that add up to `target`. Return their indices. Exactly one solution exists. Must use O(1) extra space.

**Detailed Approach:**
- Left pointer at index 0, right pointer at last index.
- Compute `sum = nums[l] + nums[r]`.
  - If `sum == target` → found, return `{l+1, r+1}`.
  - If `sum < target` → need a bigger sum, move `l` right.
  - If `sum > target` → need a smaller sum, move `r` left.
- Time: O(n) | Space: O(1)

**C++ Hint:**
```cpp
int l = 0, r = numbers.size() - 1;
while (l < r) {
    int sum = numbers[l] + numbers[r];
    if (sum == target) return {l+1, r+1};
    else if (sum < target) l++;
    else r--;
}
return {};
```

🔗 [LeetCode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=cQ1Oz4ckceM)

---

&nbsp;

### 12. 3Sum 🟡

- [ ] Solved

**Approach Tag:** `Two Pointers` | `Sort + Fix One Element`

**Problem:** Given an integer array `nums`, return all triplets `[nums[i], nums[j], nums[k]]` such that `i != j != k` and `nums[i] + nums[j] + nums[k] == 0`. No duplicate triplets.

**Detailed Approach:**
- Sort the array first.
- Fix one element `nums[i]` in a loop. For the remaining subarray, use two pointers.
- Skip duplicate values of `nums[i]` to avoid duplicate triplets.
- Inside the two-pointer pass, also skip duplicates when a valid triplet is found.
- Time: O(n²) | Space: O(1) extra

**C++ Hint:**
```cpp
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
```

🔗 [LeetCode](https://leetcode.com/problems/3sum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=jzZsG8n2R9A)

---

&nbsp;

### 13. Container With Most Water 🟡

- [ ] Solved

**Approach Tag:** `Two Pointers` | `Greedy`

**Problem:** Given an array `height` of `n` integers where `height[i]` is the height of a vertical line at index `i`, find two lines that together with the x-axis form a container that holds the **most water**.

**Detailed Approach:**
- Two pointers at both ends. Area = `min(height[l], height[r]) * (r - l)`.
- Always move the pointer pointing to the **shorter** line inward.
  - Moving the taller line inward can only decrease the width without guaranteeing a taller minimum.
  - Moving the shorter line gives a chance to find a taller one.
- Track maximum area throughout.
- Time: O(n) | Space: O(1)

**C++ Hint:**
```cpp
int l = 0, r = height.size()-1, res = 0;
while (l < r) {
    res = max(res, min(height[l], height[r]) * (r - l));
    if (height[l] < height[r]) l++;
    else r--;
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/container-with-most-water/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=UuiTKBwPgAo)

---

&nbsp;

### 14. Trapping Rain Water 🔴

- [ ] Solved

**Approach Tag:** `Two Pointers` | `Max Prefix/Suffix`

**Problem:** Given `n` non-negative integers representing an elevation map, compute how much water it can trap after raining.

**Detailed Approach:**
- Maintain `maxLeft` and `maxRight` as running maximums from each side.
- Use two pointers. At each step, process the side with the **smaller max**.
  - Water at that position = `min(maxLeft, maxRight) - height[i]`.
  - The smaller max is the bottleneck — we know for certain how much water is trapped there.
- Move the pointer of the smaller side inward.
- Time: O(n) | Space: O(1) — no extra arrays needed.

**C++ Hint:**
```cpp
int l = 0, r = height.size()-1, maxL = 0, maxR = 0, res = 0;
while (l < r) {
    if (height[l] <= height[r]) {
        maxL = max(maxL, height[l]);
        res += maxL - height[l];
        l++;
    } else {
        maxR = max(maxR, height[r]);
        res += maxR - height[r];
        r--;
    }
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/trapping-rain-water/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=ZI2z5pq0TqA)

---

&nbsp;

## Section 3 — Sliding Window

> **Core idea:** Maintain a window `[l, r]` and expand/shrink it based on a condition.
> Use a hash map or frequency array to track window contents. Avoid recomputing from scratch on every step.

&nbsp;

---

### 15. Best Time to Buy and Sell Stock 🟢

- [ ] Solved

**Approach Tag:** `Sliding Window` | `Greedy` | `One Pass`

**Problem:** Given an array `prices` where `prices[i]` is the price of a stock on day `i`, return the **maximum profit** you can achieve from a single buy and sell. Return 0 if no profit is possible.

**Detailed Approach:**
- Track the minimum price seen so far (`minPrice`).
- For each price, compute potential profit = `price - minPrice`.
- Update `maxProfit` if this profit is higher.
- Update `minPrice` if current price is lower.
- Time: O(n) | Space: O(1)

**C++ Hint:**
```cpp
int minPrice = INT_MAX, maxProfit = 0;
for (int p : prices) {
    maxProfit = max(maxProfit, p - minPrice);
    minPrice = min(minPrice, p);
}
return maxProfit;
```

🔗 [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=1pkOgXD63yU)

---

&nbsp;

### 16. Longest Substring Without Repeating Characters 🟡

- [ ] Solved

**Approach Tag:** `Sliding Window` | `Hash Map`

**Problem:** Given a string `s`, find the length of the **longest substring** without repeating characters.

**Detailed Approach:**
- Use a sliding window `[l, r]` and an `unordered_map<char, int>` storing the **last seen index** of each character.
- Expand `r`. If `s[r]` was already seen **inside** the current window (`lastSeen[s[r]] >= l`), move `l` to `lastSeen[s[r]] + 1`.
- Always update `lastSeen[s[r]] = r` and track max window size `r - l + 1`.
- Time: O(n) | Space: O(min(n, 128)) for ASCII

**C++ Hint:**
```cpp
unordered_map<char,int> last;
int l = 0, res = 0;
for (int r = 0; r < s.size(); r++) {
    if (last.count(s[r]) && last[s[r]] >= l) l = last[s[r]] + 1;
    last[s[r]] = r;
    res = max(res, r - l + 1);
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=wiGpQwVHdE0)

---

&nbsp;

### 17. Longest Repeating Character Replacement 🟡

- [ ] Solved

**Approach Tag:** `Sliding Window` | `Frequency Count`

**Problem:** Given a string `s` and integer `k`, you can replace at most `k` characters in any substring. Return the length of the **longest substring** containing the same letter after at most `k` replacements.

**Detailed Approach:**
- Sliding window. Track character frequencies in the window with `int count[26]`.
- Track `maxFreq` — the frequency of the **most common character** in the window.
- Window is **valid** if `(windowSize - maxFreq) <= k` (remaining chars can be replaced).
- If invalid, shrink from the left.
- Key insight: you don't need to recompute `maxFreq` when shrinking — just letting it stay slightly inflated is fine for correctness.
- Time: O(n) | Space: O(1)

**C++ Hint:**
```cpp
int count[26] = {}, maxFreq = 0, l = 0, res = 0;
for (int r = 0; r < s.size(); r++) {
    maxFreq = max(maxFreq, ++count[s[r]-'A']);
    while ((r - l + 1) - maxFreq > k) count[s[l++]-'A']--;
    res = max(res, r - l + 1);
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/longest-repeating-character-replacement/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=gqXU1UyA8pk)

---

&nbsp;

### 18. Permutation in String 🟡

- [ ] Solved

**Approach Tag:** `Sliding Window` | `Fixed Window` | `Frequency Matching`

**Problem:** Given strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1` as a substring.

**Detailed Approach:**
- Use a **fixed-size sliding window** of length `s1.size()` over `s2`.
- Maintain frequency arrays for `s1` and the current window in `s2`.
- Use a `matches` counter (how many of the 26 characters have matching frequencies).
- On each slide: update the window, adjust `matches`, check if `matches == 26`.
- Time: O(n) | Space: O(1)

**C++ Hint:**
```cpp
if (s1.size() > s2.size()) return false;
int freq1[26]={}, freq2[26]={}, matches=0;
for (int i=0; i<s1.size(); i++) { freq1[s1[i]-'a']++; freq2[s2[i]-'a']++; }
for (int i=0;i<26;i++) if(freq1[i]==freq2[i]) matches++;
for (int r=s1.size(); r<s2.size(); r++) {
    if (matches==26) return true;
    int add=s2[r]-'a', rem=s2[r-s1.size()]-'a';
    if(++freq2[add]==freq1[add]) matches++; else if(freq2[add]-1==freq1[add]) matches--;
    if(--freq2[rem]==freq1[rem]) matches++; else if(freq2[rem]+1==freq1[rem]) matches--;
}
return matches==26;
```

🔗 [LeetCode](https://leetcode.com/problems/permutation-in-string/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=UbyhOgBN834)

---

&nbsp;

### 19. Minimum Window Substring 🔴

- [ ] Solved

**Approach Tag:** `Sliding Window` | `Two Frequency Maps`

**Problem:** Given strings `s` and `t`, return the **minimum window substring** of `s` that contains all characters of `t`. If no such window exists, return `""`.

**Detailed Approach:**
- Maintain a frequency map for `t` (`need`) and a frequency map for the current window (`have`).
- Track `formed` = number of characters in the window that meet their required frequency.
- Expand `r`. When `formed == t's unique chars` → valid window found. Record it, then try to shrink from `l`.
- Track `(minLen, minL)` for the result.
- This is the canonical hardest sliding window — understand every line.
- Time: O(|s| + |t|) | Space: O(|s| + |t|)

**C++ Hint:**
```cpp
unordered_map<char,int> need, have;
for (char c : t) need[c]++;
int formed=0, required=need.size(), l=0, minLen=INT_MAX, minL=0;
for (int r=0; r<s.size(); r++) {
    char c=s[r]; have[c]++;
    if (need.count(c) && have[c]==need[c]) formed++;
    while (formed==required) {
        if (r-l+1 < minLen) { minLen=r-l+1; minL=l; }
        have[s[l]]--;
        if (need.count(s[l]) && have[s[l]]<need[s[l]]) formed--;
        l++;
    }
}
return minLen==INT_MAX ? "" : s.substr(minL, minLen);
```

🔗 [LeetCode](https://leetcode.com/problems/minimum-window-substring/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=jSto0O4AJbM)

---

&nbsp;

### 20. Sliding Window Maximum 🔴

- [ ] Solved

**Approach Tag:** `Monotonic Deque` | `Sliding Window`

**Problem:** Given an array `nums` and an integer `k`, return the **maximum value in each sliding window** of size `k`.

**Detailed Approach:**
- Use a **monotonic decreasing deque** (stores indices, not values).
- For each new element `r`:
  - Remove indices from the **front** of the deque if they're outside the current window (`index <= r - k`).
  - Remove indices from the **back** of the deque while `nums[deque.back()] <= nums[r]` (they can never be the max).
  - Push `r` to the back.
  - Once `r >= k-1`, the front of the deque is the current window's maximum.
- Time: O(n) | Space: O(k)

**C++ Hint:**
```cpp
deque<int> dq;
vector<int> res;
for (int r=0; r<nums.size(); r++) {
    while (!dq.empty() && dq.front() <= r-k) dq.pop_front();
    while (!dq.empty() && nums[dq.back()] <= nums[r]) dq.pop_back();
    dq.push_back(r);
    if (r >= k-1) res.push_back(nums[dq.front()]);
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/sliding-window-maximum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=DfljaUwZsOk)

---

&nbsp;

## Section 4 — Stack

> **Core idea:** Stack for "last in, first out" problems — matching brackets, next greater element, monotonic sequences.
> Ask yourself: *"Does the current element affect something I saw earlier?"*

&nbsp;

---

### 21. Valid Parentheses 🟢

- [ ] Solved

**Approach Tag:** `Stack` | `Bracket Matching`

**Problem:** Given a string containing just `(`, `)`, `{`, `}`, `[`, `]`, determine if the input string is valid. Every open bracket must be closed in the correct order.

**Detailed Approach:**
- Push every opening bracket onto the stack.
- For every closing bracket, check if the top of the stack is the matching opener.
  - If yes → pop. If no or stack is empty → invalid.
- After processing all characters, valid if and only if the stack is empty.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
stack<char> st;
unordered_map<char,char> match = {{')','{{'[0]},  // use your preferred pair map
for (char c : s) {
    if (c=='(' || c=='[' || c=='{') st.push(c);
    else {
        if (st.empty()) return false;
        if ((c==')' && st.top()!='(') ||
            (c==']' && st.top()!='[') ||
            (c=='}' && st.top()!='{')) return false;
        st.pop();
    }
}
return st.empty();
```

🔗 [LeetCode](https://leetcode.com/problems/valid-parentheses/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=WTzjTskDFMg)

---

&nbsp;

### 22. Min Stack 🟡

- [ ] Solved

**Approach Tag:** `Stack` | `Auxiliary Stack`

**Problem:** Design a stack that supports `push`, `pop`, `top`, and `getMin` — all in **O(1)** time.

**Detailed Approach:**
- Maintain two stacks: `mainStack` and `minStack`.
- On `push(val)`: push to `mainStack`. Push to `minStack` only if `val <= minStack.top()` (or minStack is empty).
- On `pop()`: pop from `mainStack`. If popped value equals `minStack.top()`, pop from `minStack` too.
- `getMin()` returns `minStack.top()`.
- Time: O(1) all operations | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/min-stack/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=qkLl7nAwDPo)

---

&nbsp;

### 23. Evaluate Reverse Polish Notation 🟡

- [ ] Solved

**Approach Tag:** `Stack` | `Expression Evaluation`

**Problem:** Evaluate the value of an arithmetic expression in **Reverse Polish Notation** (postfix). Valid operators are `+`, `-`, `*`, `/`. Division truncates toward zero.

**Detailed Approach:**
- Iterate through tokens.
- If token is a number → push to stack.
- If token is an operator → pop two values (`b` then `a`), apply `a op b`, push result.
- The final answer is the only value left on the stack.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
stack<long long> st;
for (string& t : tokens) {
    if (t=="+"||t=="-"||t=="*"||t=="/") {
        long long b=st.top(); st.pop();
        long long a=st.top(); st.pop();
        if(t=="+") st.push(a+b);
        else if(t=="-") st.push(a-b);
        else if(t=="*") st.push(a*b);
        else st.push(a/b);
    } else st.push(stoll(t));
}
return st.top();
```

🔗 [LeetCode](https://leetcode.com/problems/evaluate-reverse-polish-notation/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=iu0082c4HDE)

---

&nbsp;

### 24. Generate Parentheses 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Stack`

**Problem:** Given `n` pairs of parentheses, generate all combinations of **well-formed parentheses**.

**Detailed Approach:**
- Backtracking. Maintain counts of `open` and `close` brackets used so far.
- At each step:
  - Add `(` if `open < n`.
  - Add `)` if `close < open`.
- Base case: when `open == close == n`, add the built string to results.
- Time: O(4^n / sqrt(n)) — Catalan number | Space: O(n)

**C++ Hint:**
```cpp
vector<string> res;
function<void(string,int,int)> bt = [&](string cur, int open, int close) {
    if (open==n && close==n) { res.push_back(cur); return; }
    if (open < n) bt(cur+"(", open+1, close);
    if (close < open) bt(cur+")", open, close+1);
};
bt("", 0, 0);
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/generate-parentheses/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=s9fokUqJ76A)

---

&nbsp;

### 25. Daily Temperatures 🟡

- [ ] Solved

**Approach Tag:** `Monotonic Stack` | `Next Greater Element`

**Problem:** Given an array `temperatures`, return an array `answer` where `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If no future day is warmer, `answer[i] = 0`.

**Detailed Approach:**
- Use a **monotonic decreasing stack** (stores indices of days with unresolved "next warmer day").
- For each day `i`, pop all indices from the stack where `temperatures[stack.top()] < temperatures[i]`.
- For each popped index `j`: `answer[j] = i - j`.
- Push current index `i` onto the stack.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
stack<int> st;
vector<int> res(temperatures.size(), 0);
for (int i=0; i<temperatures.size(); i++) {
    while (!st.empty() && temperatures[i] > temperatures[st.top()]) {
        res[st.top()] = i - st.top();
        st.pop();
    }
    st.push(i);
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/daily-temperatures/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=cTBiBSnjO3c)

---

&nbsp;

### 26. Car Fleet 🟡

- [ ] Solved

**Approach Tag:** `Stack` | `Sorting` | `Simulation`

**Problem:** `n` cars are headed to the same destination. Given arrays `position` and `speed`, find how many **car fleets** will arrive. A fleet forms when a faster car catches up to a slower one ahead.

**Detailed Approach:**
- Sort cars by starting position in **descending** order (closest to destination first).
- For each car, compute `time = (target - position[i]) / speed[i]` (time to reach destination).
- Use a stack. If current car's time **>** stack top's time → it can't catch up → new fleet (push it).
- If current car's time **<=** stack top → it catches up and merges into the fleet ahead (don't push).
- Stack size at the end = number of fleets.
- Time: O(n log n) | Space: O(n)

**C++ Hint:**
```cpp
vector<pair<int,int>> cars;
for (int i=0; i<n; i++) cars.push_back({position[i], speed[i]});
sort(cars.rbegin(), cars.rend());
stack<double> st;
for (auto [pos, spd] : cars) {
    double t = (double)(target - pos) / spd;
    if (st.empty() || t > st.top()) st.push(t);
}
return st.size();
```

🔗 [LeetCode](https://leetcode.com/problems/car-fleet/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Pr6T-3yB9RM)

---

&nbsp;

### 27. Largest Rectangle in Histogram 🔴

- [ ] Solved

**Approach Tag:** `Monotonic Stack` | `Area Calculation`

**Problem:** Given an array of integers `heights` representing histogram bar heights, return the **area of the largest rectangle** that can be formed.

**Detailed Approach:**
- Use a **monotonic increasing stack** storing `(index, height)` pairs.
- For each bar:
  - While the current bar is shorter than the stack top, pop and compute area:
    `area = height * (current_index - popped_index)`.
  - The width extends back to the popped bar's original index (because all bars between were taller).
  - Push the current bar with the **start index** of the last popped element.
- After the loop, process remaining bars in stack (they extend to the end).
- Add sentinel `0` at the end to flush the stack cleanly.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
stack<pair<int,int>> st; // {index, height}
int res = 0;
for (int i=0; i<=heights.size(); i++) {
    int h = (i==heights.size()) ? 0 : heights[i];
    int start = i;
    while (!st.empty() && st.top().second > h) {
        auto [idx, ht] = st.top(); st.pop();
        res = max(res, ht * (i - idx));
        start = idx;
    }
    st.push({start, h});
}
return res;
```

🔗 [LeetCode](https://leetcode.com/problems/largest-rectangle-in-histogram/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=zx5Sw9130L0)

---

&nbsp;

## Section 5 — Binary Search

> **Core idea:** Binary search isn't just for sorted arrays — it works any time you have a **monotonic condition**.
> *"If X is possible, then X-1 is also possible."* This is the "binary search on the answer" technique.

&nbsp;

---

### 28. Binary Search 🟢

- [ ] Solved

**Approach Tag:** `Binary Search`

**Problem:** Given a sorted array of integers and a target, return the index of the target. If not found, return `-1`. Must run in O(log n).

**Detailed Approach:**
- Set `lo = 0`, `hi = n-1`.
- While `lo <= hi`:
  - Compute `mid = lo + (hi - lo) / 2` (avoids integer overflow).
  - If `nums[mid] == target` → return `mid`.
  - If `nums[mid] < target` → search right: `lo = mid + 1`.
  - If `nums[mid] > target` → search left: `hi = mid - 1`.
- Return `-1` if not found.
- Time: O(log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/binary-search/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=s4DPM8ct1pI)

---

&nbsp;

### 29. Search a 2D Matrix 🟡

- [ ] Solved

**Approach Tag:** `Binary Search` | `Matrix as Flat Array`

**Problem:** Given an `m x n` matrix where each row is sorted and the first integer of each row is greater than the last of the previous row, search for a target value.

**Detailed Approach:**
- Treat the entire matrix as a **flattened sorted array** of size `m*n`.
- Binary search with `lo = 0`, `hi = m*n - 1`.
- Map `mid` to 2D: `row = mid / cols`, `col = mid % cols`.
- Compare `matrix[row][col]` to target and adjust.
- Time: O(log(m*n)) | Space: O(1)

**C++ Hint:**
```cpp
int m=matrix.size(), n=matrix[0].size(), lo=0, hi=m*n-1;
while (lo<=hi) {
    int mid=lo+(hi-lo)/2, val=matrix[mid/n][mid%n];
    if (val==target) return true;
    else if (val<target) lo=mid+1;
    else hi=mid-1;
}
return false;
```

🔗 [LeetCode](https://leetcode.com/problems/search-a-2d-matrix/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Ber2pi2C0j0)

---

&nbsp;

### 30. Koko Eating Bananas 🟡

- [ ] Solved

**Approach Tag:** `Binary Search on Answer`

**Problem:** Koko has `n` piles of bananas. She has `h` hours and can eat at speed `k` bananas/hour (one pile per hour, leftover carries to next hour). Find the **minimum** `k` that lets her finish in `h` hours.

**Detailed Approach:**
- Binary search on the **answer** — the speed `k` — ranging from `1` to `max(piles)`.
- For a given speed `k`, total hours = `sum of ceil(pile / k)` for all piles.
- Use `(pile + k - 1) / k` for integer ceiling division.
- If `totalHours <= h` → speed is feasible, try lower (`hi = mid`).
- If `totalHours > h` → too slow, increase speed (`lo = mid + 1`).
- Time: O(n log(max(piles))) | Space: O(1)

**C++ Hint:**
```cpp
int lo=1, hi=*max_element(piles.begin(),piles.end());
while (lo<hi) {
    int mid=lo+(hi-lo)/2, hours=0;
    for (int p : piles) hours += (p+mid-1)/mid;
    if (hours<=h) hi=mid;
    else lo=mid+1;
}
return lo;
```

🔗 [LeetCode](https://leetcode.com/problems/koko-eating-bananas/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=U2SozAs9RzA)

---

&nbsp;

### 31. Find Minimum in Rotated Sorted Array 🟡

- [ ] Solved

**Approach Tag:** `Binary Search` | `Rotated Array`

**Problem:** Given a sorted array that has been rotated between 1 and `n` times, find the **minimum element** in O(log n).

**Detailed Approach:**
- Binary search. At each step, determine which half is unsorted (that's where the minimum is).
- If `nums[mid] > nums[right]` → the left half is sorted, minimum is in the right half → `lo = mid + 1`.
- Else → the right half is sorted (or mid is the minimum) → `hi = mid`.
- Loop ends when `lo == hi` — that's the minimum.
- Time: O(log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=nIVW4P8b1VA)

---

&nbsp;

### 32. Search in Rotated Sorted Array 🟡

- [ ] Solved

**Approach Tag:** `Binary Search` | `Rotated Array`

**Problem:** Given a rotated sorted array with **unique** values, find the target's index or return `-1`. Must run in O(log n).

**Detailed Approach:**
- Binary search. At each step, **one of the two halves must be sorted**.
- Check if the left half is sorted (`nums[lo] <= nums[mid]`):
  - If target is in that sorted range → search left, else search right.
- Otherwise the right half is sorted:
  - If target is in that sorted range → search right, else search left.
- Time: O(log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=U8XENwh8Oy8)

---

&nbsp;

### 33. Time Based Key-Value Store 🟡

- [ ] Solved

**Approach Tag:** `Binary Search` | `Design` | `Hash Map`

**Problem:** Design a data structure that stores key-value pairs with timestamps, and retrieves the value with the **largest timestamp ≤ given timestamp**.

**Detailed Approach:**
- Store data as `unordered_map<string, vector<pair<int,string>>>` where each key maps to a sorted list of `(timestamp, value)`.
- Since timestamps are always increasing, the vector stays sorted automatically.
- For `get(key, timestamp)`, binary search the vector using `upper_bound` on timestamp, then return the element just before it.
- Time: O(1) set, O(log n) get | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/time-based-key-value-store/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=fu2cD_6E8Hw)

---

&nbsp;

### 34. Median of Two Sorted Arrays 🔴

- [ ] Solved

**Approach Tag:** `Binary Search` | `Partition`

**Problem:** Given two sorted arrays `nums1` and `nums2` of sizes `m` and `n`, return the **median** of the combined sorted array. Must run in O(log(m+n)).

**Detailed Approach:**
- Binary search on the **smaller array**. Find a partition point `i` in `nums1` and derive `j = (m+n+1)/2 - i` in `nums2`.
- Ensure `maxLeft1 <= minRight2` and `maxLeft2 <= minRight1`.
- Adjust partition based on comparisons. Handle edge cases with `INT_MIN`/`INT_MAX` for boundaries.
- This is the hardest binary search — study the partition logic carefully and trace through examples.
- Time: O(log(min(m,n))) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=q6IEA26hvXc)

---

&nbsp;

## Section 6 — Linked List

> **Core idea:** Most linked list problems use the **fast/slow pointer** technique or require careful pointer manipulation.
> Always draw out pointer reassignments before coding.

&nbsp;

---

### 35. Reverse Linked List 🟢

- [ ] Solved

**Approach Tag:** `Linked List` | `Iterative` | `Recursive`

**Problem:** Given the head of a singly linked list, reverse the list and return the new head.

**Detailed Approach (Iterative):**
- Three pointers: `prev = nullptr`, `curr = head`, `next = nullptr`.
- Each iteration: save `next = curr->next`, point `curr->next = prev`, advance `prev = curr`, `curr = next`.
- Return `prev` (new head).

**Detailed Approach (Recursive):**
- `reverse(head->next)` returns the new head of the reversed sublist.
- Then `head->next->next = head` and `head->next = nullptr`.
- Time: O(n) | Space: O(1) iterative, O(n) recursive

🔗 [LeetCode](https://leetcode.com/problems/reverse-linked-list/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=G0_I-ZF0S38)

---

&nbsp;

### 36. Merge Two Sorted Lists 🟢

- [ ] Solved

**Approach Tag:** `Linked List` | `Two Pointer` | `Dummy Head`

**Problem:** Given the heads of two sorted linked lists, merge them into one sorted list and return its head.

**Detailed Approach:**
- Create a `dummy` node. Maintain a `curr` pointer starting at dummy.
- Compare `l1->val` and `l2->val`. Attach the smaller node to `curr->next`, advance that list's pointer and `curr`.
- After the loop, attach whichever list still has remaining nodes.
- Return `dummy->next`.
- Time: O(m+n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=XIdigk956u0)

---

&nbsp;

### 37. Reorder List 🟡

- [ ] Solved

**Approach Tag:** `Linked List` | `Fast/Slow Pointer` | `Reverse` | `Merge`

**Problem:** Given the head of a linked list `L0→L1→…→Ln`, reorder it to `L0→Ln→L1→Ln-1→L2→Ln-2→…` in-place.

**Detailed Approach (3 Steps):**
1. **Find middle:** Use slow/fast pointers. Slow ends at the middle.
2. **Reverse second half:** Reverse the list from `slow->next` onwards.
3. **Merge two halves:** Interleave nodes from the first half and the reversed second half.

- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/reorder-list/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=S5bfdUTrKLM)

---

&nbsp;

### 38. Remove Nth Node From End of List 🟡

- [ ] Solved

**Approach Tag:** `Linked List` | `Two Pointer` | `Dummy Head`

**Problem:** Given the head of a linked list, remove the `n`th node from the **end** of the list and return its head.

**Detailed Approach:**
- Use a `dummy` node pointing to `head` (handles edge case of removing head).
- Advance `fast` pointer `n+1` steps from `dummy`.
- Move both `slow` (starting at dummy) and `fast` until `fast` reaches `nullptr`.
- `slow` is now just before the node to remove. Set `slow->next = slow->next->next`.
- Time: O(L) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=XVuQxVej6y8)

---

&nbsp;

### 39. Copy List with Random Pointer 🟡

- [ ] Solved

**Approach Tag:** `Linked List` | `Hash Map` | `Deep Copy`

**Problem:** Given a linked list where each node has a `next` and a `random` pointer (can point to any node or null), return a **deep copy** of the list.

**Detailed Approach:**
- Use `unordered_map<Node*, Node*>` to map each original node to its clone.
- **Pass 1:** Iterate the list, creating a new clone node for each original node. Store in map.
- **Pass 2:** Iterate again, setting `clone->next = map[original->next]` and `clone->random = map[original->random]`.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/copy-list-with-random-pointer/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=5Y2EiZST97Y)

---

&nbsp;

### 40. Add Two Numbers 🟡

- [ ] Solved

**Approach Tag:** `Linked List` | `Simulation` | `Carry`

**Problem:** Two non-negative integers are stored as **reversed** linked lists. Add the two numbers and return the sum as a reversed linked list.

**Detailed Approach:**
- Simulate digit-by-digit addition with a `carry` variable.
- While either list has nodes or carry > 0:
  - Sum = `(l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry`.
  - New digit = `sum % 10`, new carry = `sum / 10`.
  - Create a new node, advance both pointers.
- Time: O(max(m,n)) | Space: O(max(m,n))

🔗 [LeetCode](https://leetcode.com/problems/add-two-numbers/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=wgFPrzTjm7s)

---

&nbsp;

### 41. Linked List Cycle 🟢

- [ ] Solved

**Approach Tag:** `Fast/Slow Pointer` | `Floyd's Cycle Detection`

**Problem:** Given the head of a linked list, determine if the list has a **cycle** in it.

**Detailed Approach:**
- Use Floyd's cycle detection: `slow` moves 1 step, `fast` moves 2 steps.
- If they ever meet → cycle exists.
- If `fast` or `fast->next` becomes null → no cycle.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/linked-list-cycle/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=gBTe7lFR3vc)

---

&nbsp;

### 42. Find the Duplicate Number 🟡

- [ ] Solved

**Approach Tag:** `Fast/Slow Pointer` | `Floyd's Cycle Detection` | `Array as Linked List`

**Problem:** Given an array of `n+1` integers where each value is between `1` and `n`, find the **one duplicate** number without modifying the array and using only O(1) extra space.

**Detailed Approach:**
- Treat the array as a linked list where index `i` points to `nums[i]`.
- There must be a cycle because of the duplicate (two indices point to the same value).
- Use Floyd's cycle detection to find the **cycle entrance** — that's the duplicate.
  1. Find meeting point of slow and fast pointers.
  2. Reset one pointer to `nums[0]`, advance both 1 step at a time — they meet at the duplicate.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/find-the-duplicate-number/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=wjYnzkAhcNk)

---

&nbsp;

### 43. LRU Cache 🟡

- [ ] Solved

**Approach Tag:** `Design` | `Doubly Linked List` | `Hash Map`

**Problem:** Design a data structure that follows the **Least Recently Used** cache eviction policy. Both `get` and `put` must run in O(1).

**Detailed Approach:**
- Use a `doubly linked list` to maintain order (most recently used at front, LRU at back).
- Use an `unordered_map<int, list<pair<int,int>>::iterator>` to map keys to list iterators.
- **`get(key)`:** If key exists, move its node to the front, return value. Else return -1.
- **`put(key, value)`:** If key exists, update and move to front. Else insert at front. If over capacity, remove the back node and erase from map.
- `std::list` provides O(1) splice/move when you have an iterator.
- Time: O(1) | Space: O(capacity)

🔗 [LeetCode](https://leetcode.com/problems/lru-cache/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=7ABFKPK2hD4)

---

&nbsp;

### 44. Merge K Sorted Lists 🔴

- [ ] Solved

**Approach Tag:** `Min-Heap` | `Priority Queue` | `Divide and Conquer`

**Problem:** Given an array of `k` sorted linked lists, merge all of them into one sorted linked list.

**Detailed Approach (Min-Heap):**
- Initialize a min-heap with the **head node** of each non-null list.
- Heap stores `(value, list_index)` or `(value, node_ptr)`.
- Pop the minimum node, append to result list, push the next node from that same list.
- Repeat until the heap is empty.
- Time: O(N log k) where N = total nodes | Space: O(k)

🔗 [LeetCode](https://leetcode.com/problems/merge-k-sorted-lists/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=q5a5OiGbT6Q)

---

&nbsp;

### 45. Reverse Nodes in k-Group 🔴

- [ ] Solved

**Approach Tag:** `Linked List` | `Recursion` | `In-place Reversal`

**Problem:** Given the head of a linked list, reverse the nodes of the list `k` at a time and return the modified list. If nodes remaining < k, leave them as is.

**Detailed Approach:**
- Check if at least `k` nodes remain. If not, return head as-is.
- Reverse the first `k` nodes (standard iterative reversal).
- Recursively call for the rest of the list starting at the `k+1`th node.
- Connect the end of the reversed group to the recursive result.
- Time: O(n) | Space: O(n/k) recursive stack

🔗 [LeetCode](https://leetcode.com/problems/reverse-nodes-in-k-group/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=1UOPsfP85V4)

---

&nbsp;

## Section 7 — Trees

> **Core idea:** Most tree problems are solved with **DFS (recursion)** or **BFS (level-order with a queue)**.
> Ask yourself: *"What do I need from my children to compute the answer at the parent?"*

&nbsp;

---

### 46. Invert Binary Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `Recursion`

**Problem:** Given the root of a binary tree, invert the tree (mirror it) and return its root.

**Detailed Approach:**
- Recursively swap the left and right children of every node.
- Base case: if node is null, return null.
- `swap(root->left, root->right)`, then recurse on both children.
- Time: O(n) | Space: O(h) where h = height

🔗 [LeetCode](https://leetcode.com/problems/invert-binary-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=OnSn2XEQ4MY)

---

&nbsp;

### 47. Maximum Depth of Binary Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `BFS`

**Problem:** Given the root of a binary tree, return its **maximum depth** (number of nodes along the longest path from root to a leaf).

**Detailed Approach (DFS):**
- `return 1 + max(maxDepth(root->left), maxDepth(root->right))`
- Base case: null node returns 0.

**Detailed Approach (BFS):**
- Level-order traversal. Count levels.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=hTM3phVI6YQ)

---

&nbsp;

### 48. Diameter of Binary Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `Post-order`

**Problem:** Given the root of a binary tree, return the **length of the diameter** — the longest path between any two nodes (does not need to pass through root).

**Detailed Approach:**
- DFS returning the **height** of each subtree.
- At each node, the diameter **through** that node = `leftHeight + rightHeight`.
- Update a global `maxDiameter` with this value.
- Return `1 + max(leftHeight, rightHeight)` to the parent.
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=bkxqA8Rfv04)

---

&nbsp;

### 49. Balanced Binary Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `Height Checking`

**Problem:** Given the root of a binary tree, determine if it is **height-balanced** (depth of the two subtrees of every node never differs by more than one).

**Detailed Approach:**
- DFS returning height. If unbalanced, propagate `-1` as a sentinel value.
- At each node: if left or right returns `-1` → propagate `-1`.
- If `|leftH - rightH| > 1` → return `-1`.
- Otherwise return `1 + max(leftH, rightH)`.
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/balanced-binary-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=QfJsau0ItOY)

---

&nbsp;

### 50. Same Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `Recursion`

**Problem:** Given the roots of two binary trees, check if they are **structurally identical** and have the same node values.

**Detailed Approach:**
- Base cases: both null → true. One null → false. Values differ → false.
- Recursively check: `isSameTree(p->left, q->left) && isSameTree(p->right, q->right)`.
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/same-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=vRbbcKXCxOw)

---

&nbsp;

### 51. Subtree of Another Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `Tree Matching`

**Problem:** Given roots of two trees `root` and `subRoot`, return `true` if `subRoot` is a subtree of `root`.

**Detailed Approach:**
- For each node in `root`, check `isSameTree(node, subRoot)`.
- If any match → return true.
- Recurse on left and right children.
- Time: O(m * n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/subtree-of-another-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=E36O5SWp-LE)

---

&nbsp;

### 52. Lowest Common Ancestor of BST 🟡

- [ ] Solved

**Approach Tag:** `BST Property` | `DFS`

**Problem:** Given a BST and two nodes `p` and `q`, find their **Lowest Common Ancestor** (the deepest node that is an ancestor of both).

**Detailed Approach:**
- Exploit BST property:
  - If both `p->val` and `q->val` are less than `root->val` → LCA is in the left subtree.
  - If both are greater → LCA is in the right subtree.
  - Otherwise (they split, or one equals root) → current root is the LCA.
- Time: O(h) | Space: O(1) iterative

🔗 [LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=gs2LMfuOR9k)

---

&nbsp;

### 53. Binary Tree Level Order Traversal 🟡

- [ ] Solved

**Approach Tag:** `BFS` | `Queue`

**Problem:** Given the root of a binary tree, return the **level-order traversal** of its nodes' values (left to right, level by level).

**Detailed Approach:**
- Use a queue. Push root. While queue is not empty:
  - Record `size = queue.size()` (number of nodes at this level).
  - Process exactly `size` nodes: record values, push their children.
  - Add current level's values to result.
- Time: O(n) | Space: O(n)

**C++ Hint:**
```cpp
queue<TreeNode*> q;
q.push(root);
while (!q.empty()) {
    int sz = q.size();
    vector<int> level;
    for (int i=0; i<sz; i++) {
        auto node = q.front(); q.pop();
        level.push_back(node->val);
        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
    res.push_back(level);
}
```

🔗 [LeetCode](https://leetcode.com/problems/binary-tree-level-order-traversal/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=6ZnyEApgFYg)

---

&nbsp;

### 54. Binary Tree Right Side View 🟡

- [ ] Solved

**Approach Tag:** `BFS` | `Level Order`

**Problem:** Given the root of a binary tree, return the values of nodes you can see from the **right side** (rightmost node at each level).

**Detailed Approach:**
- BFS level-order traversal. At each level, the **last node** in the level's group is visible from the right.
- Add `queue.back()`'s value at the end of each level's processing.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/binary-tree-right-side-view/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=d4zLyf32e3I)

---

&nbsp;

### 55. Count Good Nodes in Binary Tree 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Path Maximum`

**Problem:** Given a binary tree, a node `X` is **good** if there is no node with a value greater than `X->val` on the path from root to `X`. Count all good nodes.

**Detailed Approach:**
- DFS, passing `maxSoFar` (maximum value seen on the path from root to current node).
- A node is good if `node->val >= maxSoFar`. Increment count if so.
- Recurse on children with `max(maxSoFar, node->val)`.
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/count-good-nodes-in-binary-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=7cp5imvDzl4)

---

&nbsp;

### 56. Validate Binary Search Tree 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Range Bounds`

**Problem:** Given the root of a binary tree, determine if it is a **valid BST** (every node's value is strictly greater than all left descendants and strictly less than all right descendants).

**Detailed Approach:**
- DFS with `(long min, long max)` bounds passed down.
- Each node must satisfy `min < node->val < max`.
- For left child: `max = node->val`.
- For right child: `min = node->val`.
- Start with `(LONG_MIN, LONG_MAX)` to handle edge values.
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/validate-binary-search-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=s6ATEkipzow)

---

&nbsp;

### 57. Kth Smallest Element in BST 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Inorder Traversal` | `BST`

**Problem:** Given the root of a BST and an integer `k`, return the `k`th smallest value in the tree.

**Detailed Approach:**
- **Inorder traversal** (left → root → right) of a BST produces values in sorted order.
- Maintain a counter. When counter reaches `k`, return the current node's value.
- Can be done iteratively with an explicit stack for O(h) space.
- Time: O(h + k) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=5LUXSvjmGCw)

---

&nbsp;

### 58. Construct Binary Tree from Preorder and Inorder 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Divide and Conquer` | `Hash Map`

**Problem:** Given `preorder` and `inorder` traversal arrays of a binary tree, construct and return the binary tree.

**Detailed Approach:**
- First element of `preorder` is always the **root**.
- Find the root's position in `inorder` — everything to the left is the left subtree, to the right is the right subtree.
- Recursively build left and right subtrees.
- Use a `unordered_map<int,int>` for O(1) lookup of root position in inorder.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=ihj4IQGZ2zc)

---

&nbsp;

### 59. Binary Tree Maximum Path Sum 🔴

- [ ] Solved

**Approach Tag:** `DFS` | `Post-order` | `Global Maximum`

**Problem:** Given the root of a binary tree, return the **maximum path sum** of any non-empty path (a path can start and end at any node).

**Detailed Approach:**
- DFS. At each node, compute the maximum "gain" from left and right subtrees (use 0 if negative — skip that branch).
- The path **through** current node = `node->val + leftGain + rightGain`. Update global max.
- Return to parent: `node->val + max(leftGain, rightGain)` (a path can only go in one direction upward).
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Hr5cWUld4vU)

---

&nbsp;

### 60. Serialize and Deserialize Binary Tree 🔴

- [ ] Solved

**Approach Tag:** `BFS` | `DFS` | `Design`

**Problem:** Design an algorithm to **serialize** a binary tree to a string and **deserialize** that string back to the original tree structure.

**Detailed Approach (Preorder DFS):**
- **Serialize:** DFS preorder. Append node value or `"N"` for null, separated by commas.
- **Deserialize:** Split by comma into a queue/index. Recursively rebuild: read next value, if `"N"` return null, else create node and recurse for left/right.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=u4JAi2JJhI8)

---

&nbsp;

## Section 8 — Tries

> **Core idea:** A Trie (prefix tree) stores strings where each path from root to a node represents a prefix.
> Each node has up to 26 children. Best for prefix-search and word-search problems.

```cpp
struct TrieNode {
    TrieNode* children[26] = {};
    bool isEnd = false;
};
```

&nbsp;

---

### 61. Implement Trie (Prefix Tree) 🟡

- [ ] Solved

**Approach Tag:** `Trie`

**Problem:** Implement a trie with `insert`, `search`, and `startsWith` methods.

**Detailed Approach:**
- Each node has an array of 26 child pointers (one per letter) and an `isEnd` flag.
- **Insert:** Traverse char by char, creating nodes as needed. Set `isEnd = true` at the last char.
- **Search:** Traverse char by char. If any char is missing → false. At the end, check `isEnd`.
- **StartsWith:** Same as search but don't check `isEnd` — just confirm the path exists.
- Time: O(L) per op where L = word length | Space: O(26 * L * n)

🔗 [LeetCode](https://leetcode.com/problems/implement-trie-prefix-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=oobqoCJlHA0)

---

&nbsp;

### 62. Design Add and Search Words Data Structure 🟡

- [ ] Solved

**Approach Tag:** `Trie` | `DFS` | `Wildcard`

**Problem:** Design a data structure that supports `addWord` and `search` where search can use wildcard `.` to match any single character.

**Detailed Approach:**
- Normal Trie for `addWord`.
- For `search`: standard traversal for regular characters, but when `.` is encountered, **recursively try all 26 children** from that position.
- Time: O(L) insert, O(26^L) worst case search with wildcards | Space: O(26 * L * n)

🔗 [LeetCode](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=BTf05gs_8iU)

---

&nbsp;

### 63. Word Search II 🔴

- [ ] Solved

**Approach Tag:** `Trie` | `DFS Backtracking` | `Board Search`

**Problem:** Given an `m x n` board of characters and a list of words, return all words that can be found in the board (letters must be adjacent horizontally/vertically, no cell reused).

**Detailed Approach:**
- Build a Trie from all the words.
- DFS from every cell on the board, traversing the Trie simultaneously.
- When `isEnd` is reached → word found, add to result.
- **Pruning:** Mark cells as visited during DFS (e.g., set to `#`), restore after. Remove found words from the Trie to avoid duplicates and prune further.
- Time: O(m * n * 4^L) in theory, much better with pruning | Space: O(total chars in words)

🔗 [LeetCode](https://leetcode.com/problems/word-search-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=asbcE9mZz_U)

---

&nbsp;

## Section 9 — Heap / Priority Queue

> **Core idea:** Use a heap when you need repeated access to the min or max element efficiently.
> In C++: `priority_queue<int>` = max-heap | `priority_queue<int, vector<int>, greater<int>>` = min-heap.

&nbsp;

---

### 64. Kth Largest Element in an Array 🟡

- [ ] Solved

**Approach Tag:** `Min-Heap` | `QuickSelect`

**Problem:** Given an integer array and an integer `k`, return the `k`th largest element (not the `k`th distinct element).

**Detailed Approach (Min-Heap — O(n log k)):**
- Maintain a min-heap of size `k`. For each element, push it. If size > k, pop the minimum.
- The top of the heap after processing all elements is the kth largest.

**Detailed Approach (QuickSelect — O(n) avg):**
- Partition like QuickSort around a pivot.
- If pivot lands at index `n-k` → found. Else recurse only on the relevant half.
- Time: O(n log k) heap / O(n) avg QuickSelect | Space: O(k) heap / O(1) QuickSelect

🔗 [LeetCode](https://leetcode.com/problems/kth-largest-element-in-an-array/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=XEmy13g1Qxc)

---

&nbsp;

### 65. IPO 🔴

- [ ] Solved

**Approach Tag:** `Greedy` | `Max-Heap` | `Sorting`

**Problem:** You can complete at most `k` projects. Each project has a `profit` and required `capital`. You start with `w` capital. Maximize your final capital.

**Detailed Approach:**
- Sort projects by required capital.
- Use a max-heap for available profits (projects you can currently afford).
- For each of `k` rounds:
  - Push all projects with `capital <= w` into the max-heap.
  - Pop the highest profit project and add it to `w`.
- Greedy: always take the most profitable available project.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/ipo/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=IOBXATCVaSU)

---

&nbsp;

### 66. Find Median from Data Stream 🔴

- [ ] Solved

**Approach Tag:** `Two Heaps` | `Max-Heap + Min-Heap`

**Problem:** Design a data structure that supports `addNum` (add a number) and `findMedian` (return the current median).

**Detailed Approach:**
- Maintain two heaps:
  - `maxHeap` (lower half) — max-heap, stores the smaller half of numbers.
  - `minHeap` (upper half) — min-heap, stores the larger half.
- **Balance invariant:** `maxHeap.size() == minHeap.size()` or `maxHeap.size() == minHeap.size() + 1`.
- On `addNum`: push to `maxHeap`, rebalance if needed.
- `findMedian`: if sizes equal → average of both tops. Else → `maxHeap.top()`.
- Time: O(log n) add, O(1) findMedian | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/find-median-from-data-stream/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=SvTVHTorqu4)

---

&nbsp;

### 67. Task Scheduler 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Max-Heap` | `Queue`

**Problem:** Given a list of tasks and a cooldown `n`, find the **minimum number of CPU intervals** needed to finish all tasks (same task must have `n` intervals gap).

**Detailed Approach:**
- Max-heap of task frequencies.
- Use a queue to track cooling tasks with their `(frequency, available_at_time)`.
- Each step: pop the most frequent available task, decrement its count, add to cooldown queue.
- When no task is available, idle for one cycle (time advances regardless).
- Time: O(n log n) | Space: O(1) (max 26 distinct tasks)

🔗 [LeetCode](https://leetcode.com/problems/task-scheduler/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=s8p8ukTyA2I)

---

&nbsp;

### 68. Design Twitter 🟡

- [ ] Solved

**Approach Tag:** `Design` | `Heap` | `Hash Map`

**Problem:** Design a simplified Twitter with `postTweet`, `getNewsFeed` (10 most recent from followed users), `follow`, and `unfollow`.

**Detailed Approach:**
- `unordered_map<int, unordered_set<int>> follows` — who each user follows.
- `unordered_map<int, vector<pair<int,int>>> tweets` — userId → list of `(timestamp, tweetId)`.
- For `getNewsFeed`: use a max-heap on timestamps. Merge tweets from self and all followed users, return top 10.
- Time: O(k log k) for feed where k = total tweets from followed users | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/design-twitter/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=pNichitDD2E)

---

&nbsp;

### 69. Last Stone Weight 🟢

- [ ] Solved

**Approach Tag:** `Max-Heap`

**Problem:** You have stones with various weights. Each turn, smash the two heaviest. If equal, both destroyed; if not, the heavier survives with weight difference. Return the last stone's weight or 0.

**Detailed Approach:**
- Build a max-heap from all stone weights.
- While heap has more than 1 stone:
  - Pop two heaviest `y` and `x` (`y >= x`).
  - If `y != x`, push `y - x` back.
- Return heap top if non-empty, else 0.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/last-stone-weight/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=B-QCq79-Vfw)

---

&nbsp;

### 70. K Closest Points to Origin 🟡

- [ ] Solved

**Approach Tag:** `Max-Heap` | `QuickSelect`

**Problem:** Given an array of `points` on a 2D plane, return the `k` closest points to the origin `(0, 0)`.

**Detailed Approach (Max-Heap — O(n log k)):**
- Maintain a max-heap of size `k` keyed by squared Euclidean distance.
- For each point, push it. If size > k, pop the farthest.
- Remaining heap contains the k closest.

**Detailed Approach (QuickSelect — O(n) avg):**
- Partition by distance like QuickSort. Recurse only on relevant side.
- Time: O(n log k) heap | Space: O(k)

🔗 [LeetCode](https://leetcode.com/problems/k-closest-points-to-origin/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=rI2EBUEMfTk)

---

&nbsp;

## Section 10 — Backtracking

> **Core idea:** Explore all possibilities by making a choice, recursing, then **undoing** the choice.
> Use when you need all combinations/permutations or when brute-force is unavoidable but must be pruned.

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

&nbsp;

---

### 71. Subsets 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Include/Exclude`

**Problem:** Given an integer array `nums` of unique elements, return all possible subsets (the power set).

**Detailed Approach:**
- At each index, make a binary choice: **include** `nums[i]` or **exclude** it.
- Recurse on the next index for both choices.
- Base case: when index reaches end, add current subset to result.
- Total 2^n subsets.
- Time: O(n * 2^n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/subsets/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=REOH22Xwdkk)

---

&nbsp;

### 72. Combination Sum 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Unlimited Reuse`

**Problem:** Given an array of distinct integers `candidates` and a `target`, return all unique combinations that sum to `target`. Each number can be used **unlimited times**.

**Detailed Approach:**
- Backtracking. At each step, try including `candidates[i]` (stay at index `i` to allow reuse).
- If current sum > target → prune (return early).
- If current sum == target → add to result.
- Increment start index to avoid duplicate combinations (not duplicate elements).
- Time: O(2^(t/min)) where t = target | Space: O(t/min) recursion depth

🔗 [LeetCode](https://leetcode.com/problems/combination-sum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=GBKI9VSKdGg)

---

&nbsp;

### 73. Permutations 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Visited Array`

**Problem:** Given an array `nums` of distinct integers, return all possible permutations.

**Detailed Approach:**
- Backtracking with a `bool visited[n]` array.
- At each position, try all unvisited elements.
- Mark as visited before recursing, unmark (backtrack) after.
- Base case: when current permutation has n elements → add to result.
- Time: O(n * n!) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/permutations/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=s7AvT7cGdSo)

---

&nbsp;

### 74. Subsets II 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Deduplication` | `Sort`

**Problem:** Given an integer array `nums` that may contain **duplicates**, return all possible subsets with no duplicate subsets.

**Detailed Approach:**
- **Sort** the array first.
- Same backtracking as Subsets, but skip duplicate elements **at the same recursion depth**:
  `if (i > start && nums[i] == nums[i-1]) continue;`
- This avoids generating duplicate subsets.
- Time: O(n * 2^n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/subsets-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Vn2v6ajA7U0)

---

&nbsp;

### 75. Combination Sum II 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Deduplication` | `Sort`

**Problem:** Given a collection `candidates` (may have duplicates) and a `target`, find all unique combinations where each number is used **at most once** and sum equals target.

**Detailed Approach:**
- Sort the array.
- Backtrack: skip duplicate elements at the same depth (same logic as Subsets II).
- Move to `i+1` (not `i`) to prevent reusing the same element.
- Prune if current sum > target.
- Time: O(2^n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/combination-sum-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=rSA3t6BDDwg)

---

&nbsp;

### 76. Word Search 🟡

- [ ] Solved

**Approach Tag:** `DFS Backtracking` | `Grid`

**Problem:** Given an `m x n` grid of characters and a string `word`, return `true` if the word exists in the grid (adjacent cells, no cell used twice).

**Detailed Approach:**
- DFS from every cell as a potential starting point.
- At each step, check if current cell matches the current character of `word`.
- Mark cell as visited by temporarily replacing it with `#`.
- Explore all 4 directions. Backtrack by restoring the cell.
- Time: O(m * n * 4^L) | Space: O(L) recursion

🔗 [LeetCode](https://leetcode.com/problems/word-search/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=pfiQ_PS1g8E)

---

&nbsp;

### 77. Palindrome Partitioning 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Palindrome Check`

**Problem:** Given a string `s`, partition it such that every substring of the partition is a palindrome. Return all possible partitioning.

**Detailed Approach:**
- Backtracking. At each position `start`, try every possible substring `s[start..end]`.
- If `s[start..end]` is a palindrome → add it to the current partition, recurse from `end+1`.
- Backtrack by removing from the current partition.
- Base case: `start == s.size()` → add current partition to result.
- Time: O(n * 2^n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/palindrome-partitioning/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=3jvWodd7ht0)

---

&nbsp;

### 78. Letter Combinations of a Phone Number 🟡

- [ ] Solved

**Approach Tag:** `Backtracking` | `Digit-to-Letter Mapping`

**Problem:** Given a string of digits `2-9`, return all possible letter combinations that the number could represent (like T9 phone keyboard). Return empty list for empty input.

**Detailed Approach:**
- Map each digit to its letters: `{'2': "abc", '3': "def", ...}`.
- Backtracking: at each position in the digit string, try all corresponding letters.
- When the current combination has length == input length → add to result.
- Time: O(4^n * n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=0snEunUacZY)

---

&nbsp;

### 79. N-Queens 🔴

- [ ] Solved

**Approach Tag:** `Backtracking` | `Set-Based Conflict Checking`

**Problem:** Place `n` queens on an `n x n` chessboard such that no two queens attack each other. Return all distinct solutions.

**Detailed Approach:**
- Place queens row by row. For each row, try each column.
- Track conflicts using three sets:
  - `cols` — columns already occupied.
  - `diag1` — diagonals (`row - col`).
  - `diag2` — anti-diagonals (`row + col`).
- If a position conflicts with any set → skip.
- Else place, recurse on next row, then backtrack (remove from sets).
- Time: O(n!) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/n-queens/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Ph95IHmRp5M)

---

&nbsp;

### 80. N-Queens II 🔴

- [ ] Solved

**Approach Tag:** `Backtracking` | `Count Only`

**Problem:** Same as N-Queens but return the **count** of distinct solutions instead of the actual boards.

**Detailed Approach:**
- Identical to N-Queens backtracking.
- Instead of recording the board configuration, just increment a counter when a complete placement is found.
- Time: O(n!) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/n-queens-ii/) | 🎥 [YouTube Search](https://www.youtube.com/results?search_query=n+queens+II+leetcode)

---

&nbsp;

## Section 11 — Graphs

> **Core idea:** Represent as adjacency list. DFS for exploration/connectivity. BFS for shortest paths (unweighted). Union-Find for connectivity queries. Topological Sort for dependency ordering.

&nbsp;

---

### 81. Number of Islands 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `BFS` | `Grid`

**Problem:** Given an `m x n` binary grid of `'1'`s (land) and `'0'`s (water), return the number of islands. An island is surrounded by water and formed by connecting adjacent lands horizontally/vertically.

**Detailed Approach:**
- Iterate every cell. When you find an unvisited `'1'`:
  - Increment island count.
  - DFS/BFS from that cell, marking all connected `'1'`s as visited (set to `'0'` or use visited array).
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/number-of-islands/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=pV2kpPD66nE)

---

&nbsp;

### 82. Max Area of Island 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Grid`

**Problem:** Given a binary matrix, return the **maximum area** of an island (or 0 if no island exists).

**Detailed Approach:**
- Same as Number of Islands.
- DFS returns the **size** of the island found.
- Track the global maximum.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/max-area-of-island/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=iJGr1OtmH0c)

---

&nbsp;

### 83. Clone Graph 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `BFS` | `Hash Map`

**Problem:** Given a reference to a node in a **connected undirected graph**, return a **deep copy** of the graph.

**Detailed Approach:**
- DFS with `unordered_map<Node*, Node*>` mapping original nodes to their clones.
- Before recursing on a neighbor, **create the clone first** (to handle cycles).
- If a node is already in the map, return its clone (memoization).
- Time: O(V+E) | Space: O(V)

🔗 [LeetCode](https://leetcode.com/problems/clone-graph/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=mQeF6bN8hMk)

---

&nbsp;

### 84. Walls and Gates 🟡

- [ ] Solved

**Approach Tag:** `Multi-Source BFS`

**Problem:** Given a grid with walls (`-1`), gates (`0`), and empty rooms (`INF`), fill each empty room with the distance to its nearest gate.

**Detailed Approach:**
- **Multi-source BFS**: instead of BFS from each room, start BFS simultaneously from **all gates** at once.
- Add all gate positions to the queue initially.
- BFS outward — when visiting a neighbor, if its value > current distance + 1, update and add to queue.
- BFS guarantees minimum distance.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/walls-and-gates/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=e69C6xhiSQE)

---

&nbsp;

### 85. Rotting Oranges 🟡

- [ ] Solved

**Approach Tag:** `Multi-Source BFS` | `Grid`

**Problem:** Given a grid where `0` = empty, `1` = fresh orange, `2` = rotten orange. Each minute, rotten oranges infect adjacent fresh ones. Return minutes until no fresh oranges remain, or `-1` if impossible.

**Detailed Approach:**
- Multi-source BFS from all initially rotten oranges.
- Count fresh oranges. Each BFS level = 1 minute.
- Decrement fresh count as oranges are infected.
- After BFS: if any fresh oranges remain → return `-1`. Else return elapsed minutes.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/rotting-oranges/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=y704fEOx0s0)

---

&nbsp;

### 86. Pacific Atlantic Water Flow 🟡

- [ ] Solved

**Approach Tag:** `Reverse BFS/DFS` | `Two-Ocean Reachability`

**Problem:** Given an `m x n` rainfall matrix, water flows to adjacent cells with equal or lower height. Find all cells from which water can flow to **both** the Pacific and Atlantic oceans.

**Detailed Approach:**
- **Reverse thinking:** instead of simulating flow downward, run BFS/DFS **uphill** from each ocean's border.
- BFS from all Pacific border cells → mark cells reachable from Pacific.
- BFS from all Atlantic border cells → mark cells reachable from Atlantic.
- Answer: cells marked by both passes.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/pacific-atlantic-water-flow/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=s-VkcjyB55s)

---

&nbsp;

### 87. Surrounded Regions 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Border-Connected Regions`

**Problem:** Given an `m x n` grid of `'X'` and `'O'`, capture all `'O'` regions not connected to the border (flip them to `'X'`). Border-connected regions remain.

**Detailed Approach:**
- DFS/BFS from all `'O'` cells **on the border**, marking them as safe (e.g., `'S'`).
- After DFS: all remaining `'O'`s are surrounded → flip to `'X'`. Flip safe `'S'`s back to `'O'`.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/surrounded-regions/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=9z2BunfoZ5Y)

---

&nbsp;

### 88. Course Schedule 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Cycle Detection` | `Directed Graph`

**Problem:** There are `n` courses and a list of prerequisites `[a, b]` meaning you must take `b` before `a`. Return `true` if it's possible to finish all courses (no cycle).

**Detailed Approach:**
- Build adjacency list. DFS with 3 node states: `0` = unvisited, `1` = in current path (visiting), `2` = fully processed (safe).
- If you reach a node with state `1` → cycle detected → return false.
- Mark node as `1` before DFS on neighbors, mark as `2` after.
- Time: O(V+E) | Space: O(V+E)

🔗 [LeetCode](https://leetcode.com/problems/course-schedule/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=EgI5nU9etnU)

---

&nbsp;

### 89. Course Schedule II 🟡

- [ ] Solved

**Approach Tag:** `Topological Sort` | `Kahn's Algorithm (BFS)` | `DFS Postorder`

**Problem:** Same setup as Course Schedule. Return a valid ordering of courses, or empty array if impossible.

**Detailed Approach (Kahn's BFS):**
- Compute in-degrees for all nodes.
- Queue all nodes with in-degree 0.
- Process: dequeue a node, add to result, reduce in-degrees of its neighbors. If any reach 0, enqueue them.
- If result has all n nodes → valid order. Else → cycle exists.
- Time: O(V+E) | Space: O(V+E)

🔗 [LeetCode](https://leetcode.com/problems/course-schedule-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Akt3glAwyfY)

---

&nbsp;

### 90. Graph Valid Tree 🟡

- [ ] Solved

**Approach Tag:** `Union-Find` | `DFS` | `Tree Properties`

**Problem:** Given `n` nodes labeled `0` to `n-1` and a list of edges, determine if the edges form a **valid tree** (connected + no cycles).

**Detailed Approach:**
- A valid tree has exactly `n-1` edges and is fully connected.
- Check `edges.size() == n-1` first.
- **Union-Find:** process each edge. If both endpoints are already in the same component → cycle → return false. Else union them.
- After processing all edges, all nodes should be in one component.
- Time: O(n * α(n)) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/graph-valid-tree/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=bXsUuownnoQ)

---

&nbsp;

### 91. Number of Connected Components 🟡

- [ ] Solved

**Approach Tag:** `Union-Find` | `DFS`

**Problem:** Given `n` nodes and a list of undirected edges, return the **number of connected components**.

**Detailed Approach (Union-Find):**
- Initialize `n` components. For each edge, union the two nodes. If they were in different components → decrement component count.
- Final component count is the answer.

**Detailed Approach (DFS):**
- For each unvisited node, DFS and mark all reachable nodes as visited. Each DFS call = one new component.
- Time: O(n * α(n)) Union-Find | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=8f1XPm4WOUc)

---

&nbsp;

### 92. Redundant Connection 🟡

- [ ] Solved

**Approach Tag:** `Union-Find` | `Cycle Detection`

**Problem:** Given an undirected graph that started as a tree with one extra edge added, return the **redundant edge** that creates a cycle.

**Detailed Approach:**
- Process edges one by one using Union-Find.
- For each edge `(u, v)`: if `find(u) == find(v)` → they're already connected → this edge is redundant.
- Else → union them.
- Return the first redundant edge found.
- Time: O(n * α(n)) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/redundant-connection/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=FXWRE4VzXEo)

---

&nbsp;

### 93. Word Ladder 🔴

- [ ] Solved

**Approach Tag:** `BFS` | `Word Graph`

**Problem:** Given `beginWord`, `endWord`, and a `wordList`, return the **minimum number of transformations** to go from begin to end, changing one letter at a time (each intermediate word must be in the list).

**Detailed Approach:**
- BFS. Start with `beginWord` at distance 1.
- For each word, try changing every character to `a-z`. If the new word is in the word set and unvisited:
  - If it equals `endWord` → return `currentDistance + 1`.
  - Else add to queue, remove from word set (mark visited).
- Use `unordered_set` for O(1) lookups.
- Time: O(n * L * 26) where L = word length | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/word-ladder/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=h9iTnkgv05E)

---

&nbsp;

## Section 12 — Advanced Graphs

> **Core idea:** Dijkstra for weighted shortest paths (no negative edges). Bellman-Ford for negative weights or edge-count constraints. Prim's/Kruskal's for MST. Topological Sort for DAGs.

&nbsp;

---

### 94. Reconstruct Itinerary 🔴

- [ ] Solved

**Approach Tag:** `Hierholzer's Algorithm` | `Eulerian Path` | `DFS`

**Problem:** Given a list of airline tickets `[from, to]`, reconstruct the itinerary in lexical order. Must use all tickets exactly once. Start from `"JFK"`.

**Detailed Approach:**
- Build adjacency list using `map<string, multiset<string>>` (multiset keeps destinations sorted lexicographically).
- DFS (Hierholzer's): while current airport has destinations, visit the smallest one (pop from multiset), recurse.
- Append current airport to result **after** all outgoing edges are consumed (postorder).
- Reverse the result at the end.
- Time: O(E log E) | Space: O(E)

🔗 [LeetCode](https://leetcode.com/problems/reconstruct-itinerary/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=ZyB_gQ8vqGA)

---

&nbsp;

### 95. Min Cost to Connect All Points 🟡

- [ ] Solved

**Approach Tag:** `Prim's Algorithm` | `MST` | `Min-Heap`

**Problem:** Given an array of points on a 2D plane, return the minimum cost to connect all points using the Manhattan distance between them (each connection is an edge).

**Detailed Approach (Prim's — O(n² log n)):**
- Start from any point. Maintain a min-heap of `(cost, point)` pairs.
- Track `visited` set. At each step, pop the cheapest edge to an unvisited point, add its cost, push its edges to unvisited neighbors.
- Repeat until all points are visited.
- Time: O(n² log n) | Space: O(n²)

🔗 [LeetCode](https://leetcode.com/problems/min-cost-to-connect-all-points/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=f7JOBJIC-NA)

---

&nbsp;

### 96. Network Delay Time 🟡

- [ ] Solved

**Approach Tag:** `Dijkstra's Algorithm` | `Shortest Path`

**Problem:** Given a network of `n` nodes and weighted directed edges, find how long it takes for a signal from node `k` to reach **all nodes**. Return `-1` if some nodes are unreachable.

**Detailed Approach (Dijkstra):**
- Min-heap of `(distance, node)`. Start with `(0, k)`.
- Maintain `dist[]` array initialized to infinity.
- Process: pop min, if already visited skip. Else update distances to neighbors and push to heap.
- Answer is `max(dist[1..n])`. If any is infinity → return `-1`.
- Time: O(E log V) | Space: O(V+E)

🔗 [LeetCode](https://leetcode.com/problems/network-delay-time/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=EaphyqKU4PQ)

---

&nbsp;

### 97. Swim in Rising Water 🔴

- [ ] Solved

**Approach Tag:** `Dijkstra` | `Binary Search + BFS`

**Problem:** Given an `n x n` grid where `grid[i][j]` is the elevation, find the minimum time `t` such that you can travel from `(0,0)` to `(n-1,n-1)` (you can only move through cells with elevation ≤ t).

**Detailed Approach (Dijkstra-like):**
- Min-heap of `(maxElevationOnPath, r, c)`. Start with `(grid[0][0], 0, 0)`.
- For each cell, propagate to neighbors with `max(currentCost, grid[neighbor])`.
- First time you pop `(n-1, n-1)` → that cost is the answer.
- Time: O(n² log n) | Space: O(n²)

🔗 [LeetCode](https://leetcode.com/problems/swim-in-rising-water/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=amvrKlMLuGY)

---

&nbsp;

### 98. Alien Dictionary 🔴

- [ ] Solved

**Approach Tag:** `Topological Sort` | `Graph Building`

**Problem:** Given a sorted list of words in an alien language, determine the order of letters in the alien alphabet. Return `""` if the order is invalid.

**Detailed Approach:**
- Compare adjacent words to derive ordering constraints (first differing character gives an edge `a → b`).
- Handle invalid case: if word `a` is a prefix of word `b` but comes after it → invalid.
- Build directed graph and run topological sort (DFS or Kahn's).
- If cycle detected → return `""`.
- Time: O(C) where C = total chars in all words | Space: O(1) — max 26 nodes

🔗 [LeetCode](https://leetcode.com/problems/alien-dictionary/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=6kTZYvNNyps)

---

&nbsp;

### 99. Cheapest Flights Within K Stops 🟡

- [ ] Solved

**Approach Tag:** `Bellman-Ford` | `BFS with Layer Limit`

**Problem:** Given `n` cities, a list of flights `[from, to, price]`, a source `src`, destination `dst`, and at most `k` stops, return the cheapest price or `-1` if impossible.

**Detailed Approach (Bellman-Ford — k+1 iterations):**
- Standard Dijkstra doesn't work here because of the stop constraint.
- Run Bellman-Ford for exactly `k+1` iterations (relaxing edges `k+1` times = at most k stops).
- Critical: make a **copy** of the distance array at the start of each iteration to avoid using edges from the same iteration.
- Time: O(k * E) | Space: O(V)

🔗 [LeetCode](https://leetcode.com/problems/cheapest-flights-within-k-stops/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=5eIK3zUdYmE)

---

&nbsp;

## Section 13 — 1D Dynamic Programming

> **Core idea:** Break the problem into subproblems. `dp[i]` = answer for input of size `i`.
> Find the recurrence: usually `dp[i]` depends on `dp[i-1]`, `dp[i-2]`, etc.

&nbsp;

---

### 100. Climbing Stairs 🟢

- [ ] Solved

**Approach Tag:** `1D DP` | `Fibonacci`

**Problem:** You can climb 1 or 2 steps at a time. How many distinct ways can you climb `n` stairs?

**Detailed Approach:**
- `dp[i] = dp[i-1] + dp[i-2]` — same recurrence as Fibonacci.
- Optimize to O(1) space with two variables.
- Base cases: `dp[1] = 1`, `dp[2] = 2`.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/climbing-stairs/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Y0lT9Fck7qI)

---

&nbsp;

### 101. Min Cost Climbing Stairs 🟢

- [ ] Solved

**Approach Tag:** `1D DP`

**Problem:** Given `cost[i]` to step on stair `i`, you can start from index 0 or 1, and take 1 or 2 steps. Find the minimum cost to reach the top (beyond the last step).

**Detailed Approach:**
- `dp[i] = cost[i] + min(dp[i-1], dp[i-2])`.
- Answer is `min(dp[n-1], dp[n-2])`.
- Can optimize to O(1) space.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/min-cost-climbing-stairs/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=ktmzAZWkEZ0)

---

&nbsp;

### 102. House Robber 🟡

- [ ] Solved

**Approach Tag:** `1D DP` | `Skip Adjacent`

**Problem:** You can't rob two adjacent houses. Given `nums[i]` = amount in house `i`, find the maximum amount you can rob.

**Detailed Approach:**
- `dp[i] = max(dp[i-1], dp[i-2] + nums[i])` — either skip this house or rob it (and skip previous).
- Optimize to two variables.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/house-robber/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=73r3KWiEvyk)

---

&nbsp;

### 103. House Robber II 🟡

- [ ] Solved

**Approach Tag:** `1D DP` | `Circular Array`

**Problem:** Same as House Robber but houses are arranged in a **circle** (first and last are adjacent).

**Detailed Approach:**
- Can't rob both first and last. Run House Robber DP **twice**:
  1. On `nums[0..n-2]` (exclude last).
  2. On `nums[1..n-1]` (exclude first).
- Return the maximum of both.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/house-robber-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=rWAJCfYYOvM)

---

&nbsp;

### 104. Longest Palindromic Substring 🟡

- [ ] Solved

**Approach Tag:** `Expand Around Center` | `DP`

**Problem:** Given a string `s`, return the **longest palindromic substring**.

**Detailed Approach (Expand Around Center — O(n²)):**
- For each of the `2n-1` possible centers (single chars and between chars), expand outward while characters match.
- Track the start and max length of the longest palindrome found.
- Time: O(n²) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/longest-palindromic-substring/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=XYQecbcd6_c)

---

&nbsp;

### 105. Palindromic Substrings 🟡

- [ ] Solved

**Approach Tag:** `Expand Around Center`

**Problem:** Given a string `s`, return the **number of palindromic substrings** it contains.

**Detailed Approach:**
- Same expand-around-center technique as above.
- Each time the window expands successfully (characters match), increment the count.
- Time: O(n²) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/palindromic-substrings/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=4RACzI5-du8)

---

&nbsp;

### 106. Decode Ways 🟡

- [ ] Solved

**Approach Tag:** `1D DP`

**Problem:** A message encoded as digits (1=A, 2=B, ..., 26=Z). Count the total number of ways to decode a given digit string.

**Detailed Approach:**
- `dp[i]` = number of ways to decode `s[0..i-1]`.
- If `s[i-1] != '0'` → single-digit decode is valid: `dp[i] += dp[i-1]`.
- If `s[i-2..i-1]` forms a valid two-digit number (10–26) → `dp[i] += dp[i-2]`.
- Handle leading zeros carefully.
- Time: O(n) | Space: O(n) or O(1) with rolling variables

🔗 [LeetCode](https://leetcode.com/problems/decode-ways/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=6aEyTjOwlJU)

---

&nbsp;

### 107. Coin Change 🟡

- [ ] Solved

**Approach Tag:** `1D DP` | `Unbounded Knapsack`

**Problem:** Given coin denominations and an `amount`, return the **fewest coins** needed to make that amount. Return `-1` if not possible.

**Detailed Approach:**
- `dp[i]` = min coins to make amount `i`. Initialize `dp[0] = 0`, rest = `INT_MAX`.
- For each amount `i` from 1 to `amount`, try each coin `c`: `dp[i] = min(dp[i], dp[i-c] + 1)` if `i >= c` and `dp[i-c] != INT_MAX`.
- Time: O(amount * coins) | Space: O(amount)

🔗 [LeetCode](https://leetcode.com/problems/coin-change/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=H9bfqozjoqs)

---

&nbsp;

### 108. Maximum Product Subarray 🟡

- [ ] Solved

**Approach Tag:** `1D DP` | `Track Min and Max`

**Problem:** Given an integer array `nums`, find the contiguous subarray that has the **largest product**.

**Detailed Approach:**
- Track both `maxProd` and `minProd` at each position (negative × negative = large positive).
- At each element: `newMax = max(nums[i], maxProd*nums[i], minProd*nums[i])`.
- Similarly compute `newMin`. Update `result = max(result, newMax)`.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/maximum-product-subarray/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=lXVy6YWFcRM)

---

&nbsp;

### 109. Word Break 🟡

- [ ] Solved

**Approach Tag:** `1D DP` | `String Segmentation`

**Problem:** Given a string `s` and a dictionary `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of dictionary words.

**Detailed Approach:**
- `dp[i]` = can we form `s[0..i-1]`.
- For each position `i`, check all `j < i`: if `dp[j]` is true AND `s[j..i-1]` is in the dictionary → `dp[i] = true`.
- Use `unordered_set` for O(1) dictionary lookup.
- Time: O(n²) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/word-break/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Sx9NNgInc3A)

---

&nbsp;

### 110. Longest Increasing Subsequence 🟡

- [ ] Solved

**Approach Tag:** `1D DP` | `Binary Search (Patience Sort)`

**Problem:** Given an integer array `nums`, return the **length of the longest strictly increasing subsequence**.

**Detailed Approach (O(n²) DP):**
- `dp[i]` = LIS ending at index `i`. `dp[i] = max(dp[j] + 1)` for all `j < i` where `nums[j] < nums[i]`.

**Detailed Approach (O(n log n) Patience Sort):**
- Maintain a `tails` array where `tails[i]` = smallest tail of all increasing subsequences of length `i+1`.
- For each number, binary search for its position in `tails` and replace or extend.
- Time: O(n²) or O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=cjWnW0hdF1Y)

---

&nbsp;

### 111. Partition Equal Subset Sum 🟡

- [ ] Solved

**Approach Tag:** `0/1 Knapsack` | `DP`

**Problem:** Given a non-empty integer array `nums`, determine if it can be partitioned into two subsets with **equal sum**.

**Detailed Approach:**
- If total sum is odd → impossible.
- Target = `sum / 2`. This becomes a subset-sum problem.
- `dp[j]` = can we reach sum `j`. Boolean array of size `target+1`.
- For each number, update dp **backwards**: `dp[j] |= dp[j - num]` (backwards prevents reusing).
- Time: O(n * sum) | Space: O(sum)

🔗 [LeetCode](https://leetcode.com/problems/partition-equal-subset-sum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=IsvocB5BJhw)

---

&nbsp;

## Section 14 — 2D Dynamic Programming

> **Core idea:** `dp[i][j]` = answer for subproblem using first `i` elements of one sequence and first `j` of another (or a grid position).
> Fill the table **bottom-up**, deriving each cell from already-computed cells.

&nbsp;

---

### 112. Unique Paths 🟡

- [ ] Solved

**Approach Tag:** `2D DP` | `Grid`

**Problem:** A robot is in a `m x n` grid at the top-left corner and needs to reach the bottom-right. It can only move right or down. Count total unique paths.

**Detailed Approach:**
- `dp[i][j] = dp[i-1][j] + dp[i][j-1]` (came from above or from the left).
- Base cases: first row and first column are all 1s.
- Compress to 1D: `dp[j] += dp[j-1]`.
- Time: O(m*n) | Space: O(n) with 1D compression

🔗 [LeetCode](https://leetcode.com/problems/unique-paths/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=IlEsdxuD4lY)

---

&nbsp;

### 113. Longest Common Subsequence 🟡

- [ ] Solved

**Approach Tag:** `2D DP` | `String`

**Problem:** Given two strings `text1` and `text2`, return the **length of their longest common subsequence**. Return 0 if none.

**Detailed Approach:**
- `dp[i][j]` = LCS of `text1[0..i-1]` and `text2[0..j-1]`.
- If `text1[i-1] == text2[j-1]`: `dp[i][j] = dp[i-1][j-1] + 1`.
- Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
- Time: O(m*n) | Space: O(m*n) or O(n) with row compression

🔗 [LeetCode](https://leetcode.com/problems/longest-common-subsequence/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Ua0GhsJSlWM)

---

&nbsp;

### 114. Best Time to Buy and Sell Stock with Cooldown 🟡

- [ ] Solved

**Approach Tag:** `State Machine DP`

**Problem:** You can buy and sell stock multiple times with a 1-day cooldown after selling (can't buy on the day after a sell). Maximize profit.

**Detailed Approach:**
- Three states: `held` (own stock), `sold` (just sold, in cooldown), `rest` (cooldown over, can buy).
- Transitions:
  - `held = max(held, rest - price)` — hold or buy.
  - `sold = held + price` — sell.
  - `rest = max(rest, sold)` — wait out cooldown.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=I7j0F7AHpb8)

---

&nbsp;

### 115. Coin Change II 🟡

- [ ] Solved

**Approach Tag:** `2D DP` | `Unbounded Knapsack` | `Counting`

**Problem:** Given coin denominations and an `amount`, return the **number of combinations** that make up that amount.

**Detailed Approach:**
- `dp[j]` = number of ways to make amount `j`.
- For each coin (outer loop), update amounts (inner loop): `dp[j] += dp[j - coin]`.
- Order matters! Coins outer, amounts inner → counts combinations (not permutations).
- Time: O(amount * coins) | Space: O(amount)

🔗 [LeetCode](https://leetcode.com/problems/coin-change-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Mjy4hd2xgrs)

---

&nbsp;

### 116. Target Sum 🟡

- [ ] Solved

**Approach Tag:** `2D DP` | `0/1 Knapsack` | `Count Ways`

**Problem:** Given an array `nums` and a `target`, assign `+` or `-` to each number. Count the number of ways to reach `target`.

**Detailed Approach:**
- Use a hash map DP: `dp[sum] = number of ways to reach this sum`.
- For each number, generate new sums by adding or subtracting from each existing sum.
- Can also reduce to subset-sum: if `P` = sum of positives, `N` = sum of negatives, then `P - N = target` and `P + N = totalSum`. So `P = (target + totalSum) / 2`.
- Time: O(n * totalSum) | Space: O(totalSum)

🔗 [LeetCode](https://leetcode.com/problems/target-sum/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=g0npyaQtAQM)

---

&nbsp;

### 117. Interleaving String 🟡

- [ ] Solved

**Approach Tag:** `2D DP`

**Problem:** Given strings `s1`, `s2`, and `s3`, determine if `s3` is formed by an **interleaving** of `s1` and `s2`.

**Detailed Approach:**
- `dp[i][j]` = can `s3[0..i+j-1]` be formed using `s1[0..i-1]` and `s2[0..j-1]`.
- `dp[i][j] = (dp[i-1][j] && s1[i-1]==s3[i+j-1]) || (dp[i][j-1] && s2[j-1]==s3[i+j-1])`.
- Base case: `dp[0][0] = true`.
- Time: O(m*n) | Space: O(m*n) or O(n) with row compression

🔗 [LeetCode](https://leetcode.com/problems/interleaving-string/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=3Rw3p9LrgvE)

---

&nbsp;

### 118. Longest Increasing Path in a Matrix 🔴

- [ ] Solved

**Approach Tag:** `DFS` | `Memoization` | `DAG`

**Problem:** Given an `m x n` integer matrix, return the **length of the longest strictly increasing path** (can move up/down/left/right).

**Detailed Approach:**
- DFS with memoization. `dp[i][j]` = longest increasing path **starting** at `(i,j)`.
- No need to track visited — strictly increasing paths can never cycle.
- For each cell, try all 4 directions. Cache the result.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=wCc_nd-GiEc)

---

&nbsp;

### 119. Distinct Subsequences 🔴

- [ ] Solved

**Approach Tag:** `2D DP`

**Problem:** Given strings `s` and `t`, return the number of distinct subsequences of `s` which equal `t`.

**Detailed Approach:**
- `dp[i][j]` = number of ways to form `t[0..j-1]` from `s[0..i-1]`.
- If `s[i-1] == t[j-1]`: `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]` (use this match + skip it).
- Else: `dp[i][j] = dp[i-1][j]` (skip `s[i-1]`).
- Base case: `dp[i][0] = 1` (empty t always 1 way).
- Time: O(m*n) | Space: O(m*n) or O(n)

🔗 [LeetCode](https://leetcode.com/problems/distinct-subsequences/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=-RDzMJ33nx8)

---

&nbsp;

### 120. Edit Distance 🟡

- [ ] Solved

**Approach Tag:** `2D DP` | `String`

**Problem:** Given two strings `word1` and `word2`, return the **minimum number of operations** (insert, delete, replace) to convert `word1` to `word2`.

**Detailed Approach:**
- `dp[i][j]` = edit distance between `word1[0..i-1]` and `word2[0..j-1]`.
- If chars match: `dp[i][j] = dp[i-1][j-1]`.
- Else: `dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])` (delete, insert, replace).
- Base cases: `dp[i][0] = i`, `dp[0][j] = j`.
- Time: O(m*n) | Space: O(m*n) or O(n)

🔗 [LeetCode](https://leetcode.com/problems/edit-distance/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=XYi2-LPrwm4)

---

&nbsp;

### 121. Burst Balloons 🔴

- [ ] Solved

**Approach Tag:** `Interval DP`

**Problem:** Given `n` balloons with values `nums`, burst them all to maximize coins. Bursting balloon `i` gives `nums[left] * nums[i] * nums[right]` coins.

**Detailed Approach:**
- **Reverse thinking:** instead of "which balloon to burst first", think "which balloon to burst **last**" in each interval.
- `dp[l][r]` = max coins from bursting all balloons in the open interval `(l, r)`.
- Add sentinel `1`s at both ends. For each `k` in `(l, r)`, `dp[l][r] = max(dp[l][r], nums[l]*nums[k]*nums[r] + dp[l][k] + dp[k][r])`.
- Time: O(n³) | Space: O(n²)

🔗 [LeetCode](https://leetcode.com/problems/burst-balloons/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=VFskby7lUbw)

---

&nbsp;

### 122. Regular Expression Matching 🔴

- [ ] Solved

**Approach Tag:** `2D DP` | `Regex`

**Problem:** Implement regex matching supporting `.` (any single char) and `*` (zero or more of preceding). Must match the **entire** string.

**Detailed Approach:**
- `dp[i][j]` = does `s[0..i-1]` match `p[0..j-1]`.
- If `p[j-1] == '*'`:
  - Use zero occurrences: `dp[i][j] |= dp[i][j-2]`.
  - Use one or more: `dp[i][j] |= dp[i-1][j]` if `p[j-2]` matches `s[i-1]`.
- If `p[j-1] == '.'` or `p[j-1] == s[i-1]`: `dp[i][j] = dp[i-1][j-1]`.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/regular-expression-matching/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=HAA8mgxlov8)

---

&nbsp;

## Section 15 — Greedy

> **Core idea:** Make the locally optimal choice at each step. Prove (or convince yourself) that local optimum leads to global optimum.
> Often involves **sorting first**.

&nbsp;

---

### 123. Maximum Subarray (Kadane's Algorithm) 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Kadane's Algorithm`

**Problem:** Given an integer array `nums`, find the contiguous subarray with the **largest sum** and return it.

**Detailed Approach (Kadane's):**
- Track `currSum` = best sum ending at current position.
- If `currSum` goes negative, reset it to 0 (start fresh).
- Update `maxSum = max(maxSum, currSum + nums[i])` at each step.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/maximum-subarray/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=5WZl3MMT0Eg)

---

&nbsp;

### 124. Jump Game 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Reach Tracking`

**Problem:** Given `nums[i]` = max jump length from position `i`, return `true` if you can reach the last index from index 0.

**Detailed Approach:**
- Track `maxReach` = farthest index reachable so far.
- For each index `i`: if `i > maxReach` → stuck, return false.
- Update `maxReach = max(maxReach, i + nums[i])`.
- If loop completes → return true.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/jump-game/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=Yan0cv2cLy8)

---

&nbsp;

### 125. Jump Game II 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `BFS-like Layer`

**Problem:** Same setup as Jump Game, but return the **minimum number of jumps** to reach the last index (guaranteed reachable).

**Detailed Approach:**
- Greedy with `curEnd` (end of current jump range) and `farthest` (farthest reachable within this range).
- When `i == curEnd` → must jump. Increment `jumps`, update `curEnd = farthest`.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/jump-game-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=dJ7sWiOoK7g)

---

&nbsp;

### 126. Gas Station 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Running Sum`

**Problem:** There are `n` gas stations in a circle. `gas[i]` = gas available, `cost[i]` = gas to travel to next station. Find the starting station index to complete the circuit, or `-1` if impossible.

**Detailed Approach:**
- If `sum(gas) < sum(cost)` → impossible, return `-1`.
- Track `tank` = running gas balance. If `tank < 0` → current start is invalid, reset `tank = 0` and set `start = i + 1`.
- The station where `tank` never dips below 0 is the answer.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/gas-station/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=lJwbPZGo05A)

---

&nbsp;

### 127. Hand of Straights 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Ordered Map`

**Problem:** You have a hand of cards. Check if you can rearrange them into groups of `w` consecutive cards.

**Detailed Approach:**
- Use a `map<int,int>` (ordered) to count card frequencies.
- For each smallest available card (front of map), try to form a group of `w` consecutive cards starting from it.
- Decrement frequencies; if any required card has count 0 → impossible.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/hand-of-straights/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=amnrMX4d8MU)

---

&nbsp;

### 128. Merge Triplets to Form Target Triplet 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Filtering`

**Problem:** Given triplets and a target triplet, you can merge any two triplets (element-wise max). Return whether it's possible to form the exact target triplet.

**Detailed Approach:**
- Filter out any triplet that exceeds the target in any dimension (it would corrupt the result).
- Among remaining triplets, check if the element-wise maximum equals the target.
- If yes → target achievable. Else → not.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=kShkQLQZ9K4)

---

&nbsp;

### 129. Partition Labels 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Last Occurrence`

**Problem:** Partition a string into as many parts as possible so that each letter appears in at most one part. Return the sizes of the parts.

**Detailed Approach:**
- First pass: record the **last occurrence index** of each character.
- Second pass: maintain `start` and `end` of current partition.
  - `end = max(end, lastOccurrence[s[i]])`.
  - When `i == end` → partition complete. Record `end - start + 1`, set `start = i + 1`.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/partition-labels/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=B7m8UmZE-vw)

---

&nbsp;

### 130. Valid Parenthesis String 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Range Tracking`

**Problem:** Given a string with `(`, `)`, and `*` (can be `(`, `)`, or empty), determine if it's valid.

**Detailed Approach:**
- Track a range `[minOpen, maxOpen]` of possible open bracket counts.
- `(` → both `min` and `max` +1.
- `)` → both -1. If `maxOpen < 0` → invalid (too many closing).
- `*` → `min - 1`, `max + 1` (all three possibilities).
- Clamp `minOpen = max(0, minOpen)` (can't have negative open count).
- Valid if `minOpen == 0` at end.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/valid-parenthesis-string/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=QhPdNS143Qg)

---

&nbsp;

## Section 16 — Intervals

> **Core idea:** Sort by start time. Merge by checking overlap with the previous interval.
> For scheduling, sorting by **end time** is often optimal (greedy).

&nbsp;

---

### 131. Insert Interval 🟡

- [ ] Solved

**Approach Tag:** `Intervals` | `Linear Scan`

**Problem:** Given a list of sorted non-overlapping intervals and a new interval, insert it and merge any overlapping intervals.

**Detailed Approach:**
- Add all intervals that **end before** the new interval starts (no overlap on the left).
- Merge all intervals that **overlap** with the new interval: `newInterval = [min(starts), max(ends)]`.
- Add the merged interval, then add all remaining intervals.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/insert-interval/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=A8NUOmlwOlM)

---

&nbsp;

### 132. Merge Intervals 🟡

- [ ] Solved

**Approach Tag:** `Intervals` | `Sort`

**Problem:** Given an array of intervals, merge all overlapping intervals and return the result.

**Detailed Approach:**
- Sort intervals by start time.
- Iterate: if current interval's start ≤ last merged interval's end → overlap → extend end: `merged.back()[1] = max(merged.back()[1], current[1])`.
- Else → no overlap → add current as new interval.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/merge-intervals/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=44H3cEC2fFM)

---

&nbsp;

### 133. Non-Overlapping Intervals 🟡

- [ ] Solved

**Approach Tag:** `Greedy` | `Sort by End Time`

**Problem:** Given an array of intervals, find the **minimum number of intervals** to remove so the rest don't overlap.

**Detailed Approach:**
- Sort by **end time** (greedy: keeping intervals that end earliest leaves more room).
- Track `prevEnd`. For each interval:
  - If `start >= prevEnd` → no overlap → keep it, update `prevEnd`.
  - Else → overlap → remove it (increment count), keep `prevEnd` as is (we keep the earlier-ending one).
- Time: O(n log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/non-overlapping-intervals/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=nONCGxWoUfM)

---

&nbsp;

### 134. Meeting Rooms 🟢

- [ ] Solved

**Approach Tag:** `Intervals` | `Sort`

**Problem:** Given meeting intervals, determine if a person can attend **all meetings** (no two overlap).

**Detailed Approach:**
- Sort by start time.
- Check if any adjacent pair overlaps: `intervals[i].start < intervals[i-1].end`.
- If yes → conflict → return false.
- Time: O(n log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/meeting-rooms/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=PaJxqZVPhbg)

---

&nbsp;

### 135. Meeting Rooms II 🟡

- [ ] Solved

**Approach Tag:** `Min-Heap` | `Intervals`

**Problem:** Given meeting intervals, find the **minimum number of conference rooms** required.

**Detailed Approach:**
- Sort meetings by start time.
- Use a min-heap of **end times** (one per room).
- For each meeting: if its start time ≥ heap top (earliest ending room) → reuse that room (pop then push new end). Else → need a new room (just push).
- Heap size at the end = minimum rooms needed.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/meeting-rooms-ii/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=FdzJmTx9fHs)

---

&nbsp;

### 136. Minimum Interval to Include Each Query 🔴

- [ ] Solved

**Approach Tag:** `Min-Heap` | `Sort` | `Offline Queries`

**Problem:** Given intervals and queries, for each query find the size of the **smallest interval** that contains the query point. Size = `right - left + 1`.

**Detailed Approach:**
- Sort both intervals and queries.
- Process queries in sorted order. For each query `q`:
  - Add all intervals with `start <= q` to a min-heap keyed by size.
  - Remove all intervals from the heap whose `end < q` (they don't contain q).
  - Heap top (if any) gives the smallest valid interval.
- Map query results back to original order.
- Time: O((n + q) log n) | Space: O(n + q)

🔗 [LeetCode](https://leetcode.com/problems/minimum-interval-to-include-each-query/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=5hQ5WWW5awQ)

---

&nbsp;

## Section 17 — Math & Geometry

&nbsp;

---

### 137. Rotate Image 🟡

- [ ] Solved

**Approach Tag:** `Math` | `In-place Matrix Rotation`

**Problem:** Given an `n x n` matrix, rotate it **90 degrees clockwise** in-place.

**Detailed Approach:**
- **Step 1: Transpose** — swap `matrix[i][j]` with `matrix[j][i]`.
- **Step 2: Reverse each row** — reverse every row of the transposed matrix.
- Result is a 90° clockwise rotation.
- Time: O(n²) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/rotate-image/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=fMSJSS7eO1w)

---

&nbsp;

### 138. Spiral Matrix 🟡

- [ ] Solved

**Approach Tag:** `Simulation` | `Boundary Shrinking`

**Problem:** Given an `m x n` matrix, return all elements in **spiral order**.

**Detailed Approach:**
- Maintain 4 boundaries: `top`, `bottom`, `left`, `right`.
- Traverse: go right (along top row), go down (along right col), go left (along bottom row), go up (along left col).
- After each direction, shrink the corresponding boundary.
- Stop when boundaries cross.
- Time: O(m*n) | Space: O(1) extra

🔗 [LeetCode](https://leetcode.com/problems/spiral-matrix/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=BJnMZNwUk1M)

---

&nbsp;

### 139. Set Matrix Zeroes 🟡

- [ ] Solved

**Approach Tag:** `Matrix` | `In-place Flags`

**Problem:** Given an `m x n` matrix, if an element is `0`, set its entire row and column to `0`. Do it **in-place**.

**Detailed Approach:**
- Use the **first row and first column** as flag arrays to record which rows/cols need zeroing.
- Separately track whether the first row and first column themselves need zeroing.
- Scan matrix (excluding row 0 and col 0), set flags.
- Apply zeroes based on flags, then handle first row/col last.
- Time: O(m*n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/set-matrix-zeroes/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=T41rL0L3Pnw)

---

&nbsp;

### 140. Happy Number 🟢

- [ ] Solved

**Approach Tag:** `Floyd's Cycle Detection` | `Math`

**Problem:** A number is **happy** if repeatedly replacing it with the sum of squares of its digits eventually reaches 1. Return `true` if happy, `false` if it loops forever.

**Detailed Approach:**
- Use Floyd's cycle detection (slow/fast pointers) on the sequence.
- If the sequence reaches 1 → happy.
- If slow and fast meet (and it's not 1) → cycle → not happy.
- Time: O(log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/happy-number/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=ljz85bxOYJ0)

---

&nbsp;

### 141. Plus One 🟢

- [ ] Solved

**Approach Tag:** `Array` | `Carry Simulation`

**Problem:** Given a large integer represented as a digit array, increment it by one and return the result.

**Detailed Approach:**
- Iterate from the last digit backward.
- If digit < 9 → increment and return.
- If digit == 9 → set to 0, carry continues.
- If all digits were 9 → prepend a `1` (e.g., `[9,9,9]` → `[1,0,0,0]`).
- Time: O(n) | Space: O(1) amortized

🔗 [LeetCode](https://leetcode.com/problems/plus-one/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=jIaA8boiG1s)

---

&nbsp;

### 142. Pow(x, n) 🟡

- [ ] Solved

**Approach Tag:** `Fast Exponentiation` | `Divide and Conquer`

**Problem:** Implement `pow(x, n)` efficiently, where `n` can be negative.

**Detailed Approach:**
- **Binary exponentiation:** if `n` is even → `pow(x*x, n/2)`. If odd → `x * pow(x*x, n/2)`.
- Handle negative `n`: `pow(x, -n) = pow(1/x, n)`.
- Must know this for interviews — O(log n) is expected.
- Time: O(log n) | Space: O(log n) recursive

**C++ Hint:**
```cpp
double myPow(double x, int n) {
    if (n == 0) return 1;
    if (n < 0) { x = 1/x; n = -(long long)n; }
    return n%2 == 0 ? myPow(x*x, n/2) : x * myPow(x*x, n/2);
}
```

🔗 [LeetCode](https://leetcode.com/problems/powx-n/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=g9YQyYi4IQQ)

---

&nbsp;

### 143. Multiply Strings 🟡

- [ ] Solved

**Approach Tag:** `Math` | `String Multiplication Simulation`

**Problem:** Given two non-negative integers represented as strings, return their product as a string. Don't convert directly to integers.

**Detailed Approach:**
- Simulate long multiplication. `result[i+j+1] += (num1[i]-'0') * (num2[j]-'0')`.
- Process carries backward through the result array.
- Strip leading zeros and convert to string.
- Time: O(m*n) | Space: O(m+n)

🔗 [LeetCode](https://leetcode.com/problems/multiply-strings/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=1vZswirL8Y8)

---

&nbsp;

### 144. Detect Squares 🟡

- [ ] Solved

**Approach Tag:** `Hash Map` | `Geometry`

**Problem:** Design a data structure for adding points and counting axis-aligned squares that can be formed with a given query point.

**Detailed Approach:**
- Store a count map of all points.
- For a query point `(px, py)`: iterate over all other stored points `(x, y)` with the same x-coordinate (`x == px`). These form one side of a potential square.
- Check if the two required corner points `(x, py)` and `(px, y)` both exist in the map.
- Multiply counts to get total squares.
- Time: O(n) per query | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/detect-squares/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=bahebearrDc)

---

&nbsp;

## Section 18 — Bit Manipulation

> **Core idea:** XOR (`^`) cancels pairs (a^a=0). AND (`&`) masks bits. OR (`|`) sets bits. Left shift (`<<`) doubles. Right shift (`>>`) halves.
> Key: XOR with 0 = itself. XOR with itself = 0.

&nbsp;

---

### 145. Single Number 🟢

- [ ] Solved

**Approach Tag:** `Bit Manipulation` | `XOR`

**Problem:** Given an array where every element appears **twice** except for one, find that single element. O(1) space.

**Detailed Approach:**
- XOR all elements together. Pairs cancel out (`a ^ a = 0`). The lone element remains.
- `result = nums[0] ^ nums[1] ^ ... ^ nums[n-1]`
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/single-number/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=qMPX1AOa83k)

---

&nbsp;

### 146. Number of 1 Bits 🟢

- [ ] Solved

**Approach Tag:** `Bit Manipulation` | `Hamming Weight`

**Problem:** Given a positive integer, return the number of `1` bits in its binary representation (Hamming weight).

**Detailed Approach (Kernighan's trick — O(k) where k = set bits):**
- `n & (n-1)` clears the **lowest set bit**.
- Loop: while `n != 0`, do `n &= (n-1)`, increment count.

**Brute force:** `while (n) { count += n & 1; n >>= 1; }` — O(32).
- Time: O(1) — max 32 iterations | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/number-of-1-bits/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=5Km3utixwZs)

---

&nbsp;

### 147. Counting Bits 🟢

- [ ] Solved

**Approach Tag:** `Bit Manipulation` | `DP`

**Problem:** Given an integer `n`, return an array of length `n+1` where `ans[i]` is the number of `1` bits in `i`.

**Detailed Approach:**
- DP: `dp[i] = dp[i >> 1] + (i & 1)`.
- Bit count of `i` = bit count of `i/2` + the last bit of `i`.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/counting-bits/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=RyBM56RIWrM)

---

&nbsp;

### 148. Reverse Bits 🟢

- [ ] Solved

**Approach Tag:** `Bit Manipulation`

**Problem:** Reverse bits of a given 32-bit unsigned integer.

**Detailed Approach:**
- Loop 32 times. Each iteration:
  - Extract the last bit of `n`: `bit = n & 1`.
  - Shift `result` left and OR the bit: `result = (result << 1) | bit`.
  - Shift `n` right: `n >>= 1`.
- Time: O(1) — always 32 iterations | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/reverse-bits/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=UcoN6UjAI64)

---

&nbsp;

### 149. Missing Number 🟢

- [ ] Solved

**Approach Tag:** `Bit Manipulation` | `Math`

**Problem:** Given an array of `n` distinct numbers in the range `[0, n]`, find the missing number.

**Detailed Approach (XOR):**
- XOR all indices `0..n` with all values. The missing number is the one without a pair.

**Detailed Approach (Math — Gauss Formula):**
- Expected sum = `n*(n+1)/2`. Subtract actual sum. Difference = missing number.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/missing-number/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=WnPLSRLSANE)

---

&nbsp;

### 150. Sum of Two Integers 🟡

- [ ] Solved

**Approach Tag:** `Bit Manipulation` | `XOR + Carry`

**Problem:** Given two integers `a` and `b`, return their sum **without using `+` or `-`**.

**Detailed Approach:**
- `a ^ b` = sum without carry.
- `(a & b) << 1` = carry.
- Repeat: `a = a ^ b`, `b = (a_prev & b) << 1`, until `b == 0`.
- In C++, use unsigned arithmetic or mask to handle negative numbers (32-bit masking).
- Time: O(1) — max 32 iterations | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/sum-of-two-integers/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=gVUrDV4tZfY)

---

&nbsp;

---

# 🎯 Bonus — Frequently Asked Problems (Not in NeetCode 150)

> High-frequency problems from FAANG / top product companies that complement the NeetCode 150.

&nbsp;

---

## B1 — Arrays & Sorting

&nbsp;

### B1.1 Sort Colors (Dutch National Flag) 🟡

- [ ] Solved

**Approach Tag:** `Two Pointers` | `Three-Way Partition`

**Problem:** Given an array of `0`s, `1`s, and `2`s, sort them **in-place** in a single pass without using a library sort.

**Detailed Approach:**
- Three pointers: `low = 0`, `mid = 0`, `high = n-1`.
- Invariant: `[0, low)` = 0s, `[low, mid)` = 1s, `(high, n)` = 2s.
- Process `nums[mid]`:
  - `0` → swap with `nums[low]`, advance both `low` and `mid`.
  - `1` → advance `mid` only.
  - `2` → swap with `nums[high]`, decrement `high` (don't advance `mid`).
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/sort-colors/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=4xbWSRZHqac)

---

&nbsp;

### B1.2 Count Inversions (Count of Smaller Numbers After Self) 🟡

- [ ] Solved

**Approach Tag:** `Merge Sort` | `Divide and Conquer`

**Problem:** Count the number of inversions in an array — pairs where `i < j` but `arr[i] > arr[j]`.

**Detailed Approach:**
- Modified Merge Sort. During the merge step, whenever an element from the right half is placed before elements from the left half → each remaining left-half element contributes an inversion.
- Count `leftRemaining` inversions each time a right element is picked.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | 🎥 [YouTube](https://www.youtube.com/watch?v=S2nFkAfkqGc)

---

&nbsp;

### B1.3 First Missing Positive 🔴

- [ ] Solved

**Approach Tag:** `Array as Hash Map` | `Cycle Sort`

**Problem:** Given an unsorted integer array, find the **smallest missing positive integer** in O(n) time and O(1) space.

**Detailed Approach:**
- Use the array itself as a hash map.
- For each `nums[i]` in range `[1, n]`, place it at index `nums[i] - 1` (swap into correct position).
- After rearranging, scan for the first index where `nums[i] != i + 1` → that `i+1` is the answer.
- If all are in place → answer is `n+1`.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/first-missing-positive/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=8g78yfzMlao)

---

&nbsp;

### B1.4 Next Permutation 🟡

- [ ] Solved

**Approach Tag:** `Array` | `Two Pointer` | `Greedy`

**Problem:** Find the **next lexicographically greater permutation** of an array in-place. If no such permutation exists, rearrange to the lowest order (sorted ascending).

**Detailed Approach:**
1. Find the largest index `i` such that `nums[i] < nums[i+1]` (rightmost ascending pair). If none → reverse entire array.
2. Find the largest index `j > i` such that `nums[j] > nums[i]`.
3. Swap `nums[i]` and `nums[j]`.
4. Reverse the suffix from `nums[i+1]` onwards.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/next-permutation/) | 🎥 [YouTube](https://www.youtube.com/watch?v=quAS1iydq7U)

---

&nbsp;

### B1.5 Majority Element 🟢

- [ ] Solved

**Approach Tag:** `Boyer-Moore Voting Algorithm`

**Problem:** Given an array of size `n`, find the element that appears **more than ⌊n/2⌋ times**. The majority element always exists.

**Detailed Approach:**
- Boyer-Moore Voting: maintain `candidate` and `count`.
- For each element: if `count == 0` → set `candidate = num`. If `num == candidate` → `count++`. Else → `count--`.
- The candidate at the end is the majority element.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/majority-element/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=7pnhv842keE)

---

&nbsp;

## B2 — Strings

&nbsp;

### B2.1 Longest Valid Parentheses 🔴

- [ ] Solved

**Approach Tag:** `Stack` | `DP`

**Problem:** Given a string containing only `(` and `)`, find the length of the **longest valid parentheses substring**.

**Detailed Approach (Stack):**
- Push index `-1` as base onto the stack.
- For `(` → push its index.
- For `)` → pop top. If stack is empty → push current index (new base). Else `result = max(result, i - stack.top())`.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/longest-valid-parentheses/) | 🎥 [YouTube](https://www.youtube.com/watch?v=VdQuwtEd10M)

---

&nbsp;

### B2.2 Minimum Add to Make Parentheses Valid 🟡

- [ ] Solved

**Approach Tag:** `Stack` | `Greedy`

**Problem:** Given a parentheses string, return the **minimum number of insertions** needed to make it valid.

**Detailed Approach:**
- Track `open` (unmatched open brackets) and `close` (needed insertions).
- For `(` → `open++`.
- For `)` → if `open > 0`: `open--` (matched). Else: `close++` (need to add `(`).
- Result = `open + close` (unmatched opens also need closing brackets).
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/) | 🎥 [YouTube](https://www.youtube.com/results?search_query=minimum+add+make+parentheses+valid+leetcode)

---

&nbsp;

### B2.3 Roman to Integer / Integer to Roman 🟢/🟡

- [ ] Solved

**Approach Tag:** `Hash Map` | `Greedy`

**Problem (Roman to Integer):** Convert a Roman numeral string to an integer.
**Problem (Integer to Roman):** Convert an integer to a Roman numeral string.

**Detailed Approach (Roman to Integer):**
- Map each symbol to value. Scan left to right. If current symbol < next symbol → subtract it; else add it.

**Detailed Approach (Integer to Roman):**
- Maintain a list of values/symbols from largest to smallest. Greedily subtract largest fitting value and append symbol.
- Time: O(1) both | Space: O(1) both

🔗 [Roman to Integer](https://leetcode.com/problems/roman-to-integer/) | 🔗 [Integer to Roman](https://leetcode.com/problems/integer-to-roman/) | 🎥 [YouTube](https://www.youtube.com/watch?v=dlATMslQ6Uc)

---

&nbsp;

### B2.4 Zigzag Conversion 🟡

- [ ] Solved

**Approach Tag:** `String Simulation`

**Problem:** Write a string in a zigzag pattern across `numRows` rows, then read line by line.

**Detailed Approach:**
- Maintain `numRows` string builders. Track `currentRow` and `direction` (`+1` going down, `-1` going up).
- Append each character to `rows[currentRow]`.
- Reverse direction at row 0 and row `numRows-1`.
- Concatenate all rows.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/zigzag-conversion/) | 🎥 [YouTube](https://www.youtube.com/watch?v=Q2Tw6gcVEwc)

---

&nbsp;

## B3 — Linked List

&nbsp;

### B3.1 Flatten a Multilevel Doubly Linked List 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Stack` | `Linked List`

**Problem:** A doubly linked list may have a `child` pointer pointing to a separate doubly linked list. Flatten it so all nodes appear in a single-level list.

**Detailed Approach:**
- DFS/Iterative with a stack. When a node has a `child`:
  - Push the `next` node onto the stack (to process later).
  - Connect `node->next = child`, update `child->prev = node`.
  - Clear `child` pointer. Continue.
- When `next` is null and stack is non-empty → pop and reconnect.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/) | 🎥 [YouTube](https://www.youtube.com/watch?v=nKBB97z3VgQ)

---

&nbsp;

### B3.2 Odd Even Linked List 🟡

- [ ] Solved

**Approach Tag:** `Linked List` | `Two Pointer`

**Problem:** Given a linked list, group all odd-indexed nodes first, then even-indexed nodes. Maintain relative order within each group.

**Detailed Approach:**
- Two pointers: `odd` and `even`. Track `evenHead` (start of even list).
- `odd->next = even->next`, advance `odd`.
- `even->next = odd->next`, advance `even`.
- Repeat until `even` or `even->next` is null.
- Connect `odd->next = evenHead`.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/odd-even-linked-list/) | 🎥 [YouTube](https://www.youtube.com/watch?v=C_LA6SOwVTM)

---

&nbsp;

### B3.3 Swap Nodes in Pairs 🟡

- [ ] Solved

**Approach Tag:** `Linked List` | `Recursion` | `Iterative`

**Problem:** Given a linked list, swap every two adjacent nodes and return the head.

**Detailed Approach (Recursive):**
- Base case: 0 or 1 nodes → return head.
- `second = head->next`. `head->next = swapPairs(second->next)`. `second->next = head`. Return `second`.

**Detailed Approach (Iterative):**
- Use a dummy head. For each pair, adjust the 4 pointers.
- Time: O(n) | Space: O(1) iterative

🔗 [LeetCode](https://leetcode.com/problems/swap-nodes-in-pairs/) | 🎥 [YouTube](https://www.youtube.com/watch?v=o811TZLAWOo)

---

&nbsp;

## B4 — Trees

&nbsp;

### B4.1 Symmetric Tree 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `Recursion`

**Problem:** Given the root of a binary tree, check if it is **a mirror of itself** (symmetric around its center).

**Detailed Approach:**
- Recursive helper `isMirror(left, right)`:
  - Both null → true. One null → false. Values differ → false.
  - Recurse: `isMirror(left->left, right->right) && isMirror(left->right, right->left)`.
- Time: O(n) | Space: O(h)

🔗 [LeetCode](https://leetcode.com/problems/symmetric-tree/) | 🎥 [YouTube](https://www.youtube.com/watch?v=Mao9uzxwvyk)

---

&nbsp;

### B4.2 Path Sum II 🟡

- [ ] Solved

**Approach Tag:** `DFS Backtracking` | `Root-to-Leaf Paths`

**Problem:** Given the root of a binary tree and a `targetSum`, return all **root-to-leaf paths** where the sum equals `targetSum`.

**Detailed Approach:**
- DFS backtracking. At each node, add to current path and subtract from remaining sum.
- At a leaf node, if `remainingSum == 0` → add path to result.
- Backtrack by removing the last node from path.
- Time: O(n²) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/path-sum-ii/) | 🎥 [YouTube](https://www.youtube.com/watch?v=uPTpwHJv_Hc)

---

&nbsp;

### B4.3 Morris Inorder Traversal 🟡

- [ ] Solved

**Approach Tag:** `Morris Traversal` | `O(1) Space Inorder`

**Problem:** Perform inorder traversal of a binary tree in **O(1) space** without recursion or an explicit stack.

**Detailed Approach:**
- For each node `curr`:
  - If `curr->left == null`: visit `curr`, move to `curr->right`.
  - Else: find the **rightmost node** in `curr`'s left subtree (inorder predecessor).
    - If its right is null → create a thread: `predecessor->right = curr`. Move to `curr->left`.
    - If its right is `curr` → remove thread: `predecessor->right = null`. Visit `curr`. Move to `curr->right`.
- Key interview topic at senior levels.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/binary-tree-inorder-traversal/) | 🎥 [YouTube](https://www.youtube.com/watch?v=wGXB9OWhPTg)

---

&nbsp;

### B4.4 Vertical Order Traversal 🔴

- [ ] Solved

**Approach Tag:** `BFS/DFS` | `Map of Map`

**Problem:** Given the root of a binary tree, return nodes in **vertical order** (top to bottom, left to right). Nodes in the same position are sorted by value.

**Detailed Approach:**
- DFS/BFS storing `(node, col, row)` for each node.
- Use `map<col, map<row, multiset<val>>>` to group and sort values.
- Extract results by iterating columns in order.
- Time: O(n log n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/) | 🎥 [YouTube](https://www.youtube.com/watch?v=gVRFGHMRqJs)

---

&nbsp;

## B5 — Heap & Priority Queue

&nbsp;

### B5.1 Reorganize String 🟡

- [ ] Solved

**Approach Tag:** `Max-Heap` | `Greedy`

**Problem:** Given a string `s`, rearrange its characters so that no two **adjacent** characters are the same. Return the rearranged string or `""` if not possible.

**Detailed Approach:**
- Build frequency map. Push all `(freq, char)` pairs into a max-heap.
- Each step: pop the most frequent char, append to result, hold it (cooldown).
- Push the previous (cooled-down) char back if its count > 0.
- If impossible (can't place next char) → return `""`.
- Time: O(n log 26) = O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/reorganize-string/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=2g_b1aYTHeg)

---

&nbsp;

### B5.2 Find K Pairs with Smallest Sums 🟡

- [ ] Solved

**Approach Tag:** `Min-Heap` | `Priority Queue`

**Problem:** Given two sorted integer arrays, find the `k` pairs `(u, v)` with the **smallest sums**.

**Detailed Approach:**
- Initialize min-heap with `(nums1[i] + nums2[0], i, 0)` for all `i` in `nums1`.
- Pop the smallest pair `(sum, i, j)`. Add to result. Push `(nums1[i] + nums2[j+1], i, j+1)` if `j+1 < nums2.size()`.
- Repeat `k` times (or until heap empty).
- Time: O(k log min(n, k)) | Space: O(min(n, k))

🔗 [LeetCode](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=HbB9lSLJqpY)

---

&nbsp;

## B6 — Graphs

&nbsp;

### B6.1 Detect Cycle in Undirected Graph 🟡

- [ ] Solved

**Approach Tag:** `DFS` | `Union-Find`

**Problem:** Given an undirected graph, detect if it contains a **cycle**.

**Detailed Approach (DFS):**
- DFS tracking parent node. If you visit an already-visited node that is **not the parent** → cycle found.

**Detailed Approach (Union-Find):**
- Process each edge. If both endpoints are already in the same component → cycle.
- Time: O(V+E) | Space: O(V)

🔗 [LeetCode](https://leetcode.com/problems/graph-valid-tree/) | 🎥 [YouTube](https://www.youtube.com/results?search_query=detect+cycle+undirected+graph+leetcode)

---

&nbsp;

### B6.2 Flood Fill 🟢

- [ ] Solved

**Approach Tag:** `DFS` | `BFS` | `Grid`

**Problem:** Given an image (2D pixel grid), a starting pixel, and a new color, perform a **flood fill** — change all connected pixels of the same original color to the new color.

**Detailed Approach:**
- DFS/BFS from the starting pixel.
- Only visit neighbors with the same **original color**.
- Handle edge case: if starting pixel already has the new color → return immediately to avoid infinite loop.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/flood-fill/) | 🎥 [YouTube](https://www.youtube.com/watch?v=aehEcTEPtCs)

---

&nbsp;

### B6.3 Find if Path Exists in Graph 🟢

- [ ] Solved

**Approach Tag:** `BFS` | `DFS` | `Union-Find`

**Problem:** Given a graph with `n` nodes and edges, determine if a **path exists** between `source` and `destination`.

**Detailed Approach (Union-Find — simplest):**
- Union all edges. Check `find(source) == find(destination)`.

**Detailed Approach (BFS/DFS):**
- Simple traversal from `source`, check if `destination` is reachable.
- Time: O(V+E) | Space: O(V)

🔗 [LeetCode](https://leetcode.com/problems/find-if-path-exists-in-graph/) | 🎥 [YouTube](https://www.youtube.com/results?search_query=find+path+exists+graph+leetcode)

---

&nbsp;

### B6.4 Minimum Spanning Tree (Kruskal's) 🟡

- [ ] Solved

**Approach Tag:** `Kruskal's Algorithm` | `Union-Find` | `MST`

**Problem:** Find the Minimum Spanning Tree of a graph (minimum total weight to connect all nodes).

**Detailed Approach (Kruskal's):**
- Sort all edges by weight.
- Use Union-Find. For each edge (ascending weight): if endpoints in different components → add to MST, union them. Stop when `n-1` edges added.

**vs Prim's:** Kruskal's is better for sparse graphs; Prim's is better for dense graphs.
- Time: O(E log E) | Space: O(V)

🔗 [LeetCode — Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/) | 🎥 [YouTube](https://www.youtube.com/watch?v=JZBQLXgSGfs)

---

&nbsp;

## B7 — Dynamic Programming (Extra)

&nbsp;

### B7.1 Jump Game III 🟡

- [ ] Solved

**Approach Tag:** `BFS` | `DFS` | `Graph`

**Problem:** Given array `arr` and start index, from index `i` you can jump to `i + arr[i]` or `i - arr[i]`. Can you reach any index with value `0`?

**Detailed Approach:**
- BFS/DFS from `start`. Visit reachable indices.
- Mark visited to avoid cycles.
- Return true if you reach any index where `arr[index] == 0`.
- Time: O(n) | Space: O(n)

🔗 [LeetCode](https://leetcode.com/problems/jump-game-iii/) | 🎥 [YouTube](https://www.youtube.com/results?search_query=jump+game+iii+leetcode)

---

&nbsp;

### B7.2 Maximal Square 🟡

- [ ] Solved

**Approach Tag:** `2D DP`

**Problem:** Given an `m x n` binary matrix of `'0'`s and `'1'`s, find the largest square containing only `'1'`s and return its area.

**Detailed Approach:**
- `dp[i][j]` = side length of the largest all-1s square with `(i,j)` as the **bottom-right corner**.
- If `matrix[i][j] == '1'`: `dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`.
- Track `maxSide = max(maxSide, dp[i][j])`. Return `maxSide²`.
- Time: O(m*n) | Space: O(m*n) or O(n)

🔗 [LeetCode](https://leetcode.com/problems/maximal-square/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=6X7Ha2PrDmM)

---

&nbsp;

### B7.3 Wildcard Matching 🔴

- [ ] Solved

**Approach Tag:** `2D DP`

**Problem:** Implement wildcard pattern matching with `?` (matches any single character) and `*` (matches any sequence, including empty).

**Detailed Approach:**
- `dp[i][j]` = does `s[0..i-1]` match `p[0..j-1]`.
- If `p[j-1] == '*'`: `dp[i][j] = dp[i-1][j] (use *) || dp[i][j-1] (skip *)`.
- If `p[j-1] == '?'` or `p[j-1] == s[i-1]`: `dp[i][j] = dp[i-1][j-1]`.
- Base: `dp[0][0] = true`, `dp[0][j] = dp[0][j-1] && p[j-1]=='*'`.
- Time: O(m*n) | Space: O(m*n)

🔗 [LeetCode](https://leetcode.com/problems/wildcard-matching/) | 🎥 [YouTube](https://www.youtube.com/watch?v=3ZDZ-N0EPV0)

---

&nbsp;

### B7.4 Minimum Path Sum 🟡

- [ ] Solved

**Approach Tag:** `2D DP` | `Grid`

**Problem:** Given an `m x n` grid filled with non-negative numbers, find the path from top-left to bottom-right (only right or down moves) that **minimizes the sum**.

**Detailed Approach:**
- `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.
- Base: first row and first column are prefix sums.
- Can be done in-place modifying the grid.
- Time: O(m*n) | Space: O(1) in-place

🔗 [LeetCode](https://leetcode.com/problems/minimum-path-sum/) | 🎥 [YouTube](https://www.youtube.com/watch?v=pGMsrvt0fpk)

---

&nbsp;

### B7.5 Matrix Chain Multiplication (Minimum Path Sum variant) 🟡

> See Minimum Path Sum above — both are 2D DP warmup problems.

---

&nbsp;

## B8 — Math & Classic Problems

&nbsp;

### B8.1 Find Peak Element 🟡

- [ ] Solved

**Approach Tag:** `Binary Search`

**Problem:** A peak element is strictly greater than its neighbors. Find **any** peak element in O(log n). Multiple peaks may exist.

**Detailed Approach:**
- Binary search. At `mid`:
  - If `nums[mid] < nums[mid+1]` → peak is to the right (ascending slope) → `lo = mid + 1`.
  - Else → peak is at `mid` or to the left → `hi = mid`.
- Return `lo` when `lo == hi`.
- Time: O(log n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/find-peak-element/) | 🎥 [NeetCode Video](https://www.youtube.com/watch?v=kMzJy9es7Hc)

---

&nbsp;

### B8.2 Excel Sheet Column Number 🟢

- [ ] Solved

**Approach Tag:** `Math` | `Base-26`

**Problem:** Convert an Excel column title (`"A"` = 1, `"Z"` = 26, `"AA"` = 27) to its corresponding column number.

**Detailed Approach:**
- Treat as base-26 number system (but 1-indexed, not 0-indexed).
- `result = result * 26 + (c - 'A' + 1)` for each character left to right.
- Time: O(n) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/excel-sheet-column-number/) | 🎥 [YouTube](https://www.youtube.com/watch?v=NHhKDfwsGJI)

---

&nbsp;

### B8.3 Reverse Integer 🟡

- [ ] Solved

**Approach Tag:** `Math` | `Overflow Handling`

**Problem:** Given a signed 32-bit integer `x`, reverse its digits. Return `0` if the reversed integer overflows 32-bit signed range.

**Detailed Approach:**
- Pop last digit: `digit = x % 10`, `x /= 10`.
- Before appending: check overflow: `if (rev > INT_MAX/10 || (rev == INT_MAX/10 && digit > 7))` → return 0.
- `rev = rev * 10 + digit`.
- Time: O(log x) | Space: O(1)

🔗 [LeetCode](https://leetcode.com/problems/reverse-integer/) | 🎥 [YouTube](https://www.youtube.com/watch?v=HAgLH58IgJQ)

---

&nbsp;

### B8.4 GCD and LCM Applications 🟢

- [ ] Know the patterns

**Approach Tag:** `Math` | `Number Theory`

**Concept:** Many interview problems reduce to GCD/LCM calculations. Know these cold.

**Key formulas:**
- GCD (Euclidean): `gcd(a, b) = gcd(b, a % b)`, base case `gcd(a, 0) = a`.
- LCM: `lcm(a, b) = a / gcd(a, b) * b` (divide first to avoid overflow).

**C++ Hint:**
```cpp
#include <numeric>
int g = __gcd(a, b);         // or gcd(a, b) in C++17
long long l = (long long)a / g * b;
```

**Common applications:**
- Arranging items in rows/columns.
- Rope cutting problems.
- Finding patterns in cyclic sequences.

🔗 [GCD Practice](https://leetcode.com/problems/greatest-common-divisor-of-strings/) | 🔗 [LCM Practice](https://leetcode.com/problems/find-the-smallest-common-element-in-all-rows/)

---

&nbsp;

---

# 📋 C++ Cheat Sheet

```cpp
// ── Containers ──────────────────────────────────────────────
unordered_map<int,int> mp;
unordered_set<int> st;
priority_queue<int> maxHeap;                               // max-heap (default)
priority_queue<int, vector<int>, greater<int>> minHeap;    // min-heap
deque<int> dq;
stack<int> sk;
queue<int> q;

// ── String operations ────────────────────────────────────────
sort(s.begin(), s.end());
reverse(s.begin(), s.end());
s.substr(start, length);
stoi(s), stoll(s), to_string(n);

// ── Vector ───────────────────────────────────────────────────
sort(v.begin(), v.end());
sort(v.begin(), v.end(), greater<int>());                  // descending
auto it = lower_bound(v.begin(), v.end(), x);             // first >= x
auto it = upper_bound(v.begin(), v.end(), x);             // first > x
*max_element(v.begin(), v.end());
accumulate(v.begin(), v.end(), 0);                        // sum

// ── Useful functions ─────────────────────────────────────────
__gcd(a, b);                    // GCD
abs(x);
INT_MAX, INT_MIN, LONG_MAX, LONG_MIN;

// ── Lambda comparator ────────────────────────────────────────
sort(v.begin(), v.end(), [](auto& a, auto& b) {
    return a.second < b.second;
});

// ── Bit tricks ───────────────────────────────────────────────
n & 1            // check if odd
n & (n-1)        // clear lowest set bit (Kernighan's)
n | (1 << k)     // set bit k
n & ~(1 << k)    // clear bit k
n ^ (1 << k)     // toggle bit k
__builtin_popcount(n)   // count set bits
```

---

# ⚙️ Complexity Reference

| Structure | Access | Search | Insert | Delete |
|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) |
| Hash Map | — | O(1) avg | O(1) avg | O(1) avg |
| Binary Search | — | O(log n) | — | — |
| Heap | O(1) top | — | O(log n) | O(log n) |
| Linked List | O(n) | O(n) | O(1) | O(1) |
| BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) |
| Trie | — | O(L) | O(L) | O(L) |

---

# 🧠 Interview Problem-Solving Framework

```
Step 1: Understand
  → Restate the problem in your own words
  → Ask about constraints (n size? negatives? duplicates? sorted?)
  → Work through 1–2 examples manually

Step 2: Pattern Match
  → Array + distinct values         → Hash Map / Set
  → Sorted array + search           → Binary Search
  → Subarray / substring problem    → Sliding Window
  → Pairs / triplets                → Two Pointers
  → All combinations / permutations → Backtracking
  → Shortest path (unweighted)      → BFS
  → Shortest path (weighted)        → Dijkstra
  → Optimal substructure            → DP
  → Repeated min / max              → Heap
  → Connectivity                    → Union-Find / DFS

Step 3: Brute Force First
  → State the O(n²) or O(2^n) solution clearly
  → Shows you understand the problem before optimizing

Step 4: Code
  → Write clean, readable code
  → Use meaningful variable names
  → Handle edge cases (empty input, single element, all same, negatives)

Step 5: Test
  → Test with given examples
  → Test edge cases
  → Trace through code once

Step 6: Complexity
  → Always state Time and Space complexity at the end
```

---

> 💡 **Remember:** Interviewers care more about *how you think* than whether you get the optimal solution immediately. Communicate your thought process out loud.
>
> 📌 **Practice order:** Follow the NeetCode roadmap sequence — Arrays → Two Pointers → Sliding Window → Stack → Binary Search → Linked List → Trees → Tries → Heap → Backtracking → Graphs → Advanced Graphs → 1D DP → 2D DP → Greedy → Intervals → Math → Bit Manipulation.
