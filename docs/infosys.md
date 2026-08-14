# Infosys SP / DSE — 2-Day Complete Crash Course
*One file, everything you need. Written for someone seeing this material for the first time.*

---

## 0. The Interview Format (so you know what to expect)

- **Round 1 — Virtual Assessment (3 hrs):** Usually 2-3 DSA coding problems of increasing difficulty (Easy → Medium → Hard), sometimes + MCQs on CS fundamentals. Coding weight is high (~60%+).
- **Round 2 — In-person Assessment + Interview (3 hrs):** More coding (often on paper/notepad — practice writing code by hand), followed by a technical panel interview covering your Round-1 solutions, CS fundamentals (OOP, DBMS, OS, Networking), and your project. Sometimes an HR/behavioral round follows.
- **What they explicitly evaluate:** not just whether you get the right answer, but **how you think out loud**. Always narrate your approach before and while coding.
- **Real 2026 candidate quote:** *"They weren't just looking for the right answer, but how I arrived at it."*

**Difficulty split reported by candidates (Round 1, 3 problems):** Q1 Easy (arrays/strings/basic loops), Q2 Medium (Greedy / two pointers / sliding window), Q3 Medium-Hard (Dynamic Programming, sometimes combined with HashMap/GCD/prime factorization).

**2-day priority order (most likely to matter, in order):**
1. Arrays/Strings/HashMap patterns (near-certain to appear, Easy Q1)
2. Greedy + Sliding Window (Medium Q2)
3. DP — Knapsack/LCS/Palindrome family (Hard Q3 — this is the differentiator)
4. OOP (SOLID + Singleton/Factory) — always asked in the interview round
5. DBMS (ACID, normalization, joins, Nth highest salary, window functions) — always asked
6. OS/Networking basics — asked, but usually only 2-3 questions, lower depth needed
7. Project walkthrough — always asked, prepare this fully
8. GenAI/ML — increasingly asked, know concepts not deep math
9. HR/behavioral — short round, prepare 4-5 stories

---

## 1. DSA — Core Patterns With Explanations

### 1.1 Arrays & Strings (near-guaranteed topic)

**Key idea:** Most Infosys Easy questions are pure array/string manipulation using loops — no fancy data structure needed. They explicitly value **clean, correct logic over cleverness**, and test hidden edge cases (empty array, all duplicates, negative numbers, single element).

**Must-know patterns:**
- **Find largest / second largest element** (handle duplicates!):
```python
def second_largest(arr):
    first = second = float('-inf')
    for num in arr:
        if num > first:
            second = first
            first = num
        elif first > num > second:
            second = num
    return second if second != float('-inf') else -1
```
- **Move zeroes to end, keep relative order** (asked in real 2026 DSE interview):
```python
def move_zeroes(arr):
    insert_pos = 0
    for i in range(len(arr)):
        if arr[i] != 0:
            arr[insert_pos], arr[i] = arr[i], arr[insert_pos]
            insert_pos += 1
    return arr
```
- **Remove duplicates from array** (asked repeatedly)
- **Reverse array/string, check palindrome**
- **Count vowels, check anagram, first non-repeating character** (use a HashMap of character counts)
- **Find missing number in 1 to N** (use sum formula `n*(n+1)/2 - actual_sum`, or XOR trick)
- **Check for a duplicate in an array of size N with values 0 to N-2** (classic — use XOR or a visited-set)
- **Longest substring without repeating characters** (sliding window + hashset)

**Why HashMap matters:** A huge chunk of Easy/Medium questions collapse into "count frequency then do something" — e.g., "given N inputs of a character and an integer, arrange them by character" (real asked question) is just build a `dict`/`map` then sort keys.

### 1.2 Two Pointers & Sliding Window

**When to use:** subarray/substring problems asking for max/min length, sum, or a pair — anything where you'd otherwise use nested loops (O(n²)) can often become O(n) with two pointers.

- **Two Sum (pair with given sum)** — sort + two pointers, or single-pass HashMap.
- **3Sum** — sort array, fix one element, two-pointer on the rest.
- **Max sum subarray of size k** — fixed window, slide and subtract/add.
- **Longest substring without repeat** — variable window with a set/map tracking last-seen index.
- **Container with most water / trapping rain water** — classic two-pointer greedy-ish problems.

Template for sliding window:
```python
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)
    return max_sum
```

### 1.3 Binary Search

**Core idea:** works only on sorted (or "monotonic condition") data. Halve the search space each step → O(log n).

```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```
Know variants: first/last occurrence of a target, search in rotated sorted array, square root of a number using binary search. **Be ready to explain how binary search works verbally** — this exact phrasing has been asked directly ("Explain how binary search works").

### 1.4 Sorting

Know **Merge Sort** and **Quick Sort** well enough to code from memory (both have been directly asked: "Write a program to sort an array using quick sort").

- **Merge Sort:** divide array in half, recursively sort each half, merge two sorted halves. O(n log n) always, stable, needs O(n) extra space.
- **Quick Sort:** pick a pivot, partition array so smaller elements are left / larger are right, recursively sort partitions. O(n log n) average, O(n²) worst case, in-place.

```python
def quick_sort(arr, low, high):
    if low < high:
        pivot = partition(arr, low, high)
        quick_sort(arr, low, pivot - 1)
        quick_sort(arr, pivot + 1, high)

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1
```

### 1.5 Recursion & Backtracking

**Core idea:** solve a problem by breaking it into a smaller version of the same problem, with a base case that stops recursion. Backtracking = recursion + "undo the choice if it doesn't work" (used for generating all valid combinations/permutations).

- **Permutations** (this one directly tripped up a real candidate — practice writing it by hand):
```python
def permute(arr, l, r):
    if l == r:
        print(arr)
        return
    for i in range(l, r + 1):
        arr[l], arr[i] = arr[i], arr[l]   # swap
        permute(arr, l + 1, r)            # recurse
        arr[l], arr[i] = arr[i], arr[l]   # backtrack (undo swap)
```
- **Subsets** — for each element, either include it or don't (2 recursive branches).
- **N-Queens** — place queens row by row, backtrack if a placement is attacked.
- **Reverse a stack using recursion** (no extra space allowed) — asked in real interviews.

### 1.6 Trees

**Core definitions:** A tree is a hierarchical structure of nodes; a **Binary Search Tree (BST)** keeps left child < parent < right child, giving O(log n) search/insert/delete on average.

- **Traversals:** Inorder (Left-Root-Right, gives sorted order for BST), Preorder (Root-Left-Right), Postorder (Left-Right-Root), Level order (BFS using a queue).
```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.val)
        inorder(root.right)
```
- **Height of tree, Lowest Common Ancestor (LCA), diameter of tree** — all common follow-ups.
- Be ready to **draw a tree and trace recursive calls on paper** — one interviewer explicitly asked a candidate to draw the tree and show recursion by hand instead of typing code.

### 1.7 Graphs

- **Representations:** adjacency list (most common, space-efficient) vs adjacency matrix.
- **BFS** (queue-based, level-by-level, finds shortest path in unweighted graphs) vs **DFS** (stack/recursion-based, explores deep first). Know time/space complexity: O(V+E) for both.
```python
def bfs(graph, start):
    visited, queue, order = {start}, [start], []
    while queue:
        node = queue.pop(0)
        order.append(node)
        for nbr in graph[node]:
            if nbr not in visited:
                visited.add(nbr)
                queue.append(nbr)
    return order

def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    for nbr in graph[node]:
        if nbr not in visited:
            dfs(graph, nbr, visited)
    return visited
```
- **Dijkstra's Algorithm** (shortest path, non-negative weights, uses a min-heap/priority queue) — be ready to explain with pseudocode, this has been directly asked.
- **Topological sort** (ordering of a DAG), **cycle detection** (directed vs undirected differs).

### 1.8 Greedy Algorithms (the "Medium" bucket)

**Core idea:** make the locally optimal choice at each step, hoping it leads to a globally optimal solution. Works only when the problem has the "greedy choice property" — always prove to yourself (or the interviewer) why greedy works before coding it.

- **Activity selection** — sort by end time, greedily pick non-overlapping activities.
- **Fractional knapsack** — sort by value/weight ratio, take greedily.
- **Job scheduling with deadlines**
- **Gas station problem**
- Pattern to recognize: "sort array, then make a decision going left to right" is almost always the greedy signature at Infosys Medium level.

---

## 2. Dynamic Programming — The "Hard" Bucket (give this the most time)

**Core idea:** break a problem into overlapping subproblems, solve each subproblem once, and store (memoize) the result to avoid recomputation. Always explain your approach in this order out loud:
1. **Brute force recursion** (identify overlapping subproblems)
2. **Memoization** (top-down, add a cache)
3. **Tabulation** (bottom-up, build a table iteratively — usually the final expected answer)

### 2.1 0/1 Knapsack (the template for a huge DP family)

*Problem:* given items with weights and values, and a capacity W, maximize value without exceeding W (each item used once).
```python
def knapsack(weights, values, W):
    n = len(weights)
    dp = [[0]*(W+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for w in range(W+1):
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i-1][w], values[i-1] + dp[i-1][w-weights[i-1]])
            else:
                dp[i][w] = dp[i-1][w]
    return dp[n][W]
```
Variants to know: **Unbounded knapsack** (item can be reused), **Subset sum** (can a subset sum to target?), **Partition equal subset sum**, **Coin change (min coins)**.

### 2.2 Longest Common Subsequence (LCS) family

*Problem:* given two strings, find the length of the longest subsequence common to both (not necessarily contiguous).
```python
def lcs(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0]*(n+1) for _ in range(m+1)]
    for i in range(1, m+1):
        for j in range(1, n+1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]
```
Related: **Longest Common Substring** (must be contiguous — reset to 0 on mismatch instead of taking max), **Edit Distance** (min insert/delete/replace to convert one string to another — asked directly in interviews), **Word Break**.

### 2.3 Palindromic Subsequence / Substring family

- **Longest Palindromic Substring** — expand around center (O(n²) simple approach) or DP table `dp[i][j] = True if s[i..j] is a palindrome`.
- **Longest Palindromic Subsequence** — this is just **LCS(string, reverse(string))**. Knowing this trick saves you real time in the interview.
- **Minimum insertions to make a string palindrome** — `len(s) - LPS(s)`.

### 2.4 DP on Grids

- **Unique paths** (robot moving right/down only) — `dp[i][j] = dp[i-1][j] + dp[i][j-1]`
- **Minimum path sum**
- **Matrix chain multiplication** (classic interval DP)

### 2.5 DP on Trees / Graphs

- **Diameter of a binary tree** — for each node, longest path = left height + right height; recursively track max.
- **Maximum path sum in a binary tree**
- General pattern: define what "state" you're tracking at each node (e.g., "max path ending at this node" vs "max path through this node"), and combine child states at the parent.

---

## 3. OOP — SOLID Principles & Design Patterns

Infosys interviewers ask **"explain the four pillars of OOP with examples"** and SOLID + Singleton/Factory almost every time. Know both the definition and a concrete example for each — one-line definitions alone will get follow-up probing.

### 3.1 Four Pillars of OOP
| Pillar | Definition | Example |
|---|---|---|
| **Encapsulation** | Bundling data and methods together, restricting direct access to internal state | Private fields with public getters/setters in a `BankAccount` class |
| **Abstraction** | Hiding implementation details, exposing only what's necessary | A `Car` class exposes `drive()` without revealing engine internals |
| **Inheritance** | A class acquiring properties/behavior of another class | `SavingsAccount` inherits from `Account` |
| **Polymorphism** | Same interface, different underlying behavior | `Shape.area()` behaves differently for `Circle` vs `Square` (method overriding); method overloading is compile-time polymorphism |

### 3.2 SOLID Principles
- **S — Single Responsibility:** a class should have only one reason to change (one job).
- **O — Open/Closed:** open for extension, closed for modification — add new behavior via new code, not by editing existing tested code.
- **L — Liskov Substitution:** subclasses should be replaceable for their base class without breaking correctness.
- **I — Interface Segregation:** don't force a class to implement methods it doesn't need — prefer many small interfaces over one large one.
- **D — Dependency Inversion:** depend on abstractions (interfaces), not concrete implementations.

### 3.3 Design Patterns

**Singleton** — ensures a class has only one instance, with a global access point. Common use: database connection managers, logging.
```java
public class Singleton {
    private static Singleton instance;
    private Singleton() {}   // private constructor
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
(Mentioning `synchronized`/thread-safety shows depth — Infosys interviewers like this.)

**Factory** — a method that creates and returns objects without exposing the creation logic to the caller, useful when the exact type of object needed is decided at runtime.
```java
interface Shape { void draw(); }
class Circle implements Shape { public void draw() { System.out.println("Circle"); } }
class Square implements Shape { public void draw() { System.out.println("Square"); } }

class ShapeFactory {
    public Shape getShape(String type) {
        if (type.equals("CIRCLE")) return new Circle();
        if (type.equals("SQUARE")) return new Square();
        return null;
    }
}
```

### 3.4 Exception Handling & Multithreading (Java-specific — asked in real interviews)
- **Exception handling:** `try-catch-finally` handles runtime errors so the program doesn't crash; `finally` always executes (cleanup code). Checked vs unchecked exceptions.
- **Multithreading:** executing multiple threads concurrently; a thread is the smallest unit of CPU execution. Know the difference between a **process** (independent, own memory) and a **thread** (shares memory within a process).

---

## 4. DBMS — Always Asked, Prepare This Fully

### 4.1 Query Execution Order (conceptual, not the order you write SQL in)
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```
Explain it this way: the database first figures out *which rows* to look at (FROM/WHERE), then *groups* them (GROUP BY/HAVING), then decides *what to show* (SELECT), then *orders and limits* the output.

### 4.2 Window Functions & Ranking (very high-yield — practice writing these)

```sql
SELECT name, department, salary,
       RANK()       OVER (PARTITION BY department ORDER BY salary DESC) AS rnk,
       DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rnk,
       ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num
FROM employees;
```
- **RANK()** — skips ranks after a tie (1,2,2,4)
- **DENSE_RANK()** — no gaps after a tie (1,2,2,3)
- **ROW_NUMBER()** — always unique, no ties (1,2,3,4)
- **PARTITION BY** — resets the ranking within each group (e.g., per department)

### 4.3 Nth Highest Salary — Know All Three Ways

```sql
-- 1. Using subquery + DISTINCT
SELECT DISTINCT salary FROM employees e1
WHERE N-1 = (SELECT COUNT(DISTINCT salary) FROM employees e2 WHERE e2.salary > e1.salary);

-- 2. Using DENSE_RANK
SELECT salary FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk FROM employees
) t WHERE rnk = N;

-- 3. Using LIMIT / OFFSET (MySQL/Postgres)
SELECT DISTINCT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET N-1;
```
(For "second highest salary" specifically, set N=2 — this exact question is one of the most repeated SQL questions in Infosys interviews.)

### 4.4 ACID Properties (asked with a real-life example — practice explaining with a bank transaction, exactly as has been asked before)
- **Atomicity** — transaction is all-or-nothing (money debited from A but not credited to B → whole transaction rolls back)
- **Consistency** — database moves from one valid state to another (total money in the system stays the same before/after transfer)
- **Isolation** — concurrent transactions don't interfere with each other (two people transferring money simultaneously don't corrupt balances)
- **Durability** — once committed, changes survive even a system crash

### 4.5 Normalization
- **1NF:** atomic values only, no repeating groups.
- **2NF:** 1NF + no partial dependency (non-key attribute depends on the whole primary key, not part of it).
- **3NF:** 2NF + no transitive dependency (non-key attribute doesn't depend on another non-key attribute).
- **BCNF:** stricter 3NF — every determinant must be a candidate key. (Directly asked: "Explain BCNF.")

### 4.6 Joins
| Join | Returns |
|---|---|
| INNER JOIN | only matching rows in both tables |
| LEFT JOIN | all rows from left table + matches from right (NULL if no match) |
| RIGHT JOIN | all rows from right table + matches from left |
| FULL OUTER JOIN | all rows from both, matched where possible |
| SELF JOIN | a table joined with itself (e.g., employee-manager hierarchy) |

### 4.7 SQL vs NoSQL (directly asked)
- SQL: structured schema, relational, strong consistency (ACID), good for complex queries/joins.
- NoSQL: flexible/schema-less, horizontally scalable, eventual consistency, good for unstructured or high-volume data (e.g., MongoDB, Cassandra).

### 4.8 Indexing & Performance
Indexes speed up read/search (like a book's index) by avoiding full table scans, at the cost of slightly slower writes (index must be updated too) and extra storage.

---

## 5. Operating Systems & Networking Basics

### 5.1 Process vs Thread
A **process** is an independent program in execution with its own memory space; a **thread** is a lightweight unit of execution within a process, sharing memory with other threads of the same process.

### 5.2 Deadlock
Occurs when processes wait on each other's resources in a cycle, and none can proceed. Four necessary conditions: **mutual exclusion, hold and wait, no preemption, circular wait**. Prevention = break any one of these conditions.

### 5.3 Banker's Algorithm (asked directly)
A deadlock-avoidance algorithm that simulates resource allocation ahead of time and only grants a request if the system remains in a "safe state" (i.e., there's still some order in which all processes can finish).

### 5.4 CPU Scheduling
- **FCFS** (First Come First Serve) — simple, can cause long wait (convoy effect).
- **SJF** (Shortest Job First) — minimizes average wait time, but needs to know burst time in advance.
- **Round Robin** — each process gets a fixed time slice, good for time-sharing systems.

### 5.5 Memory: Paging & Demand Paging
**Paging** splits memory into fixed-size blocks (pages) to avoid external fragmentation. **Demand paging** loads a page into memory only when it's actually needed (referenced), not all at once — reduces memory usage and speeds up process start time.

### 5.6 Monolithic vs Microkernel (asked directly)
- **Monolithic kernel:** entire OS (drivers, file system, etc.) runs in one address space — fast but less modular/stable (a bug in one part can crash everything).
- **Microkernel:** only essential services (IPC, basic scheduling) run in kernel space; everything else runs in user space — more stable/modular but has communication overhead.

### 5.7 Networking Basics
- **OSI Model (7 layers):** Physical → Data Link → Network → Transport → Session → Presentation → Application (mnemonic: "Please Do Not Throw Sausage Pizza Away").
- **TCP/IP Model (4 layers):** Network Interface → Internet → Transport → Application (simplified/practical version of OSI).
- **TCP vs UDP:** TCP is connection-oriented, reliable, ordered (e.g., file transfer, web browsing); UDP is connectionless, faster, no guarantee of delivery/order (e.g., video streaming, gaming).
- **IPv4 vs IPv6** (asked directly): IPv4 = 32-bit address (~4.3 billion addresses, format like `192.168.1.1`); IPv6 = 128-bit address (vastly more addresses, format like `2001:0db8::1`), designed to solve IPv4 exhaustion.
- **DNS resolution / "what happens when you type a URL"** (classic question): browser checks cache → DNS lookup resolves domain to IP → TCP connection (3-way handshake) → HTTPS/TLS handshake if secure → browser sends HTTP request → server responds → browser renders page.
- **HTTP status codes:** 200 (OK), 301/302 (redirect), 400 (bad request), 401/403 (unauthorized/forbidden), 404 (not found), 500 (server error).

---

## 6. GenAI & Machine Learning Basics

- **Traditional ML vs GenAI:** traditional ML predicts/classifies from patterns in data (e.g., spam detection); generative AI *creates new content* (text, images, code) by learning the underlying distribution of training data.
- **Supervised vs Unsupervised Learning:** supervised uses labeled data (input→output pairs, e.g., regression/classification); unsupervised finds patterns in unlabeled data (e.g., clustering).
- **Overfitting vs Underfitting:** overfitting = model memorizes training data, performs poorly on new data; underfitting = model is too simple to capture patterns even in training data.
- **Train/test split:** dividing data so you evaluate the model on data it hasn't seen, to estimate real-world performance.
- **LLM basics:** built on the **Transformer** architecture, which uses an **attention mechanism** to weigh the importance of different words in a sequence relative to each other. Text is broken into **tokens**; each token/word is represented as an **embedding** (a vector capturing meaning).
- **Fine-tuning vs Prompting:** fine-tuning retrains part of a model's weights on new data; prompting/prompt engineering guides a frozen model's output purely through the input text, no retraining.
- **RAG (Retrieval-Augmented Generation):** combines an LLM with a retrieval step over an external knowledge base/document store, so the model answers using up-to-date or domain-specific information instead of relying only on what it memorized during training.
- **Be ready with one applied use case:** e.g., "GenAI could summarize customer support tickets and auto-draft responses" — Infosys likes candidates who connect GenAI to real business value, given its AI-first positioning.

---

## 7. REAL PREVIOUSLY ASKED QUESTIONS (from actual candidate interview reports)

### 7.1 Coding Questions Actually Asked
- Find the **second largest element** in an array (handle duplicates)
- **Move all zeroes** to one end of an array, keeping relative order of non-zero elements
- Given N inputs of (character, integer), **arrange/group them by character** (HashMap-based)
- Check if a number is a **palindrome**; if not, **find the next palindrome number**
- **Binary tree traversal** — draw the tree and trace recursion by hand (not always typed code)
- **Longest array without duplicates**
- **Valid permutations** of an array/string
- Prefix sum + sliding window problem
- A problem conceptually close to the **"Alien Dictionary"** problem (topological sort on characters)
- Compute the **LCM of an entire array**, then find the **minimum-length subsequence whose LCM equals that value**
- **Reverse a stack using recursion** (no extra space allowed)
- **Find the duplicate** in an array of size N containing values 0 to N-2
- **Overlapping intervals** check
- **Contiguous subarray with the largest product** (DP)
- **Number of pairs with a given sum** in an array
- **Sort an array using Quick Sort** (write it from scratch)
- Q1/Q2/Q3 examples reported in 2026 assessments: Queue + Greedy (Medium); DP + GCD (Medium-Hard); DP + HashMap + Prime Factorization (Hard)

### 7.2 Technical / Theory Questions Actually Asked
- Explain the **four pillars of OOP** with real-life examples
- Explain **SOLID** principles
- Difference between **HashMap and TreeMap** in terms of time complexity
- Explain **abstraction** with a real-life example (interviewers probe deeper if your example is weak)
- Explain **try-catch / exception handling**
- Explain **multithreading**, difference between process and thread
- Explain **NoSQL vs SQL** with examples
- Explain **BCNF**
- Explain **Banker's Algorithm**
- Explain **demand paging**
- Difference between **Monolithic Kernel and Microkernel**
- Difference between **IPv4 and IPv6**
- Explain how **binary search** works
- Explain **Dijkstra's Algorithm** with example and pseudocode
- Difference between **BFS and DFS**; write code for DFS
- Time and space complexity of **graph traversal techniques**
- Explain **ACID properties** with a real-life bank-transaction example
- Mention the **different languages in DBMS** (DDL, DML, DCL, TCL)
- What are the **ACID properties**? (asked as a standalone question too)
- If your resume mentions a framework (React, Django, PHP, etc.) — expect: "Explain the [framework] project flow, how it works internally, how the database connects"
- "How would you handle a **conflict within a development team** during a deadline?" (behavioral, asked right after technical questions)

### 7.3 Logical / Puzzle Questions Actually Asked
- "You're in a room with infinite ₹5 coins. How would you estimate how many coins would fit in the room?" (tests logical/estimation thinking — approach: use volume of room ÷ volume of one coin, then reasoning about packing efficiency)
- "Given six distinct colors, in how many unique ways can a six-faced cube be painted such that no two faces share a color?" (combinatorics/group-theory style puzzle)

### 7.4 HR / Behavioral Questions Actually Asked
- Introduce yourself / tell me about yourself
- What are your **strengths and weaknesses**?
- Why Infosys? Are you willing to **relocate**?
- Explain your **project end-to-end**: design approach, tech stack, bugs faced and how you solved them
- Questions about internships/work experience — your specific contributions and learnings
- Career goals / long-term goals
- "How would you handle a conflict within your team during a deadline?"

---

## 8. Project Explanation Framework (prepare this fully — used every time)

Structure your answer for **any** project question this way:
1. **Problem statement** (2 sentences: what real problem it solves)
2. **Design approach** (rough architecture — draw it if allowed)
3. **Tech stack** — and *why* each piece was chosen
4. **Implementation highlight** — one technically interesting part (an algorithm, optimization, or tricky bug)
5. **Challenges faced** — specific bug/issue + exactly how you debugged and solved it (interviewers probe hardest here — real candidates were asked to explain bugs they faced)
6. **Outcome** — result/impact, or what you'd improve given more time

If you used a framework (React, Django, Flask, Spring, etc.), be ready to explain **how it works internally end-to-end**, including how the database connects — this was asked directly in a real interview.

---

## 9. Day-Of Checklist
- [ ] Original college ID + government-issued photo ID
- [ ] 2 printed copies of updated resume, in a file
- [ ] Formal attire
- [ ] Pen, paper (practice writing code by hand — several rounds are on paper/notepad, not a computer)
- [ ] Review this file once in the morning — don't learn anything brand new the night before
- [ ] Sleep well — DP and graph questions need a clear head

**Final reminder:** across every real interview report, the recurring theme is that Infosys values **clear thinking narrated out loud** over silent perfect code. When you get a problem: (1) restate it in your own words, (2) state your approach and why, (3) mention time/space complexity, (4) code it, (5) dry-run with an example. Do this even in your last 2 days of practice so it becomes automatic.
