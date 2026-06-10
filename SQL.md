# SQL — Complete Notes for AI/ML Engineers
### Interview-Ready | Placement-Focused | Data Engineering & ML Perspective

> **How to use this:** Read end-to-end once to build the mental model. Every section is designed so the concept explanation *is* the answer to the interview question listed at the top of each block. Code examples use standard SQL (PostgreSQL syntax where flavour matters).

---

## Table of Contents

1. [SQL Fundamentals & Execution Order](#1-sql-fundamentals--execution-order)
2. [Data Types & DDL](#2-data-types--ddl)
3. [Filtering, Sorting & Aggregation](#3-filtering-sorting--aggregation)
4. [JOINs — Deep Dive](#4-joins--deep-dive)
5. [Subqueries & Correlated Subqueries](#5-subqueries--correlated-subqueries)
6. [CTEs — Common Table Expressions](#6-ctes--common-table-expressions)
7. [Window Functions — The Most Interview-Critical Topic](#7-window-functions--the-most-interview-critical-topic)
8. [Set Operations — UNION, INTERSECT, EXCEPT](#8-set-operations--union-intersect-except)
9. [Indexes & Query Optimisation](#9-indexes--query-optimisation)
10. [Transactions & ACID Properties](#10-transactions--acid-properties)
11. [Database Design & Normalisation](#11-database-design--normalisation)
12. [Advanced SQL — Recursive CTEs, PIVOT, JSON, MERGE](#12-advanced-sql--recursive-ctes-pivot-json-merge)
13. [SQL for ML & Feature Engineering](#13-sql-for-ml--feature-engineering)
14. [Classic Interview Problems — Solved](#14-classic-interview-problems--solved)
15. [Interview Q&A — Fundamentals](#15-interview-qa--fundamentals)
16. [Interview Q&A — Window Functions & CTEs](#16-interview-qa--window-functions--ctes)
17. [Interview Q&A — Performance & Design](#17-interview-qa--performance--design)
18. [Quick Reference Cheat Sheet](#18-quick-reference-cheat-sheet)

---

## 1. SQL Fundamentals & Execution Order

> **Interview Q:** *In what order does SQL execute a query? Why does it matter?*

SQL is **declarative** — you describe *what* you want, not *how* to get it. The engine figures out the execution plan. Understanding logical execution order is critical for writing correct queries and debugging errors like "column alias not found in WHERE".

### Logical Execution Order

```
FROM          ← 1. Identify source tables, apply JOINs
WHERE         ← 2. Filter individual rows (before grouping)
GROUP BY      ← 3. Group remaining rows
HAVING        ← 4. Filter groups (after aggregation)
SELECT        ← 5. Compute output columns and aliases
DISTINCT      ← 6. Remove duplicates
ORDER BY      ← 7. Sort results (can use aliases defined in SELECT)
LIMIT/OFFSET  ← 8. Return final subset
```

**Why this matters:**
```sql
-- This FAILS: alias defined in SELECT, used in WHERE
SELECT salary * 12 AS annual_salary
FROM employees
WHERE annual_salary > 100000;   -- ❌ 'annual_salary' not yet defined

-- This WORKS: use the expression directly in WHERE
WHERE salary * 12 > 100000;     -- ✓

-- This WORKS: ORDER BY can use aliases (step 7 > step 5)
ORDER BY annual_salary DESC;    -- ✓
```

**WHERE vs HAVING:**
```sql
-- WHERE filters rows before grouping; can't use aggregates
-- HAVING filters groups after aggregation; can use aggregates

SELECT department, AVG(salary) AS avg_sal
FROM employees
WHERE status = 'active'          -- filter rows before grouping
GROUP BY department
HAVING AVG(salary) > 70000;      -- filter groups after aggregation
```

### SQL Command Categories

| Category | Commands | Purpose |
|---|---|---|
| **DDL** (Data Definition) | CREATE, ALTER, DROP, TRUNCATE | Schema structure |
| **DML** (Data Manipulation) | SELECT, INSERT, UPDATE, DELETE | Data operations |
| **DCL** (Data Control) | GRANT, REVOKE | Permissions |
| **TCL** (Transaction Control) | COMMIT, ROLLBACK, SAVEPOINT | Transaction management |

> **Interview Q:** *What is the difference between DELETE, TRUNCATE, and DROP?*
>
> | | DELETE | TRUNCATE | DROP |
> |---|---|---|---|
> | What goes | Specific rows | All rows | Entire table |
> | WHERE clause | Yes | No | No |
> | Rollback | Yes (DML, logged) | Usually no (DDL) | No |
> | Triggers | Fires row triggers | No | No |
> | Speed | Slow (logged) | Fast | Fast |
> | Auto-increment reset | No | Yes | N/A |

---

## 2. Data Types & DDL

> **Interview Q:** *What data types would you use for storing embeddings or ML feature vectors in SQL?*

### Common Data Types

| Category | Types | Notes |
|---|---|---|
| **Integer** | INT, BIGINT, SMALLINT | BIGINT for IDs at scale (>2.1B rows) |
| **Decimal** | DECIMAL(p,s), NUMERIC, FLOAT, REAL | Use DECIMAL for money (exact); FLOAT for ML scores (approximate) |
| **String** | VARCHAR(n), CHAR(n), TEXT | VARCHAR for variable-length; TEXT for unlimited |
| **Date/Time** | DATE, TIME, TIMESTAMP, TIMESTAMPTZ | Always use TIMESTAMPTZ (timezone-aware) in production |
| **Boolean** | BOOLEAN | True/False |
| **JSON** | JSON, JSONB (PostgreSQL) | JSONB is binary-stored, indexable — prefer over JSON |
| **Array** | ARRAY[] (PostgreSQL) | Useful for embedding vectors in pgvector |
| **Vector** | VECTOR(dim) via pgvector | Store ML embeddings for similarity search |

### DDL Examples

```sql
-- Create a feature store table for ML
CREATE TABLE user_features (
    user_id     BIGINT PRIMARY KEY,
    signup_date DATE NOT NULL,
    country     VARCHAR(50),
    age_bucket  SMALLINT,
    ltv_30d     DECIMAL(10, 2),
    embedding   VECTOR(1536),          -- pgvector for OpenAI embeddings
    updated_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Add a column
ALTER TABLE user_features ADD COLUMN churn_score FLOAT;

-- Add index
CREATE INDEX idx_user_country ON user_features(country);

-- Modify column type
ALTER TABLE user_features ALTER COLUMN age_bucket TYPE INT;
```

> **Interview Q:** *When would you use VARCHAR vs TEXT?*
> Use VARCHAR(n) to enforce a maximum length and communicate intent (e.g., `country VARCHAR(3)` for ISO codes). Use TEXT for arbitrary-length strings. In PostgreSQL, TEXT and VARCHAR without a limit have identical performance — the limit is a constraint, not a storage difference.

---

## 3. Filtering, Sorting & Aggregation

### Essential SELECT Patterns

```sql
-- Basic filtering
SELECT name, salary FROM employees
WHERE department = 'Engineering' AND salary > 80000;

-- Pattern matching
WHERE email LIKE '%@gmail.com'    -- ends with
WHERE name LIKE 'A%'              -- starts with
WHERE name ILIKE 'alice%'         -- case-insensitive (PostgreSQL)

-- IN / NOT IN
WHERE department IN ('Eng', 'Data', 'ML')
WHERE user_id NOT IN (SELECT user_id FROM banned_users)  -- careful: NULLs!

-- BETWEEN (inclusive on both ends)
WHERE salary BETWEEN 60000 AND 90000
WHERE created_at BETWEEN '2025-01-01' AND '2025-12-31'

-- NULL handling
WHERE manager_id IS NULL          -- check for NULL
WHERE manager_id IS NOT NULL
COALESCE(salary, 0)               -- replace NULL with 0
NULLIF(score, 0)                  -- return NULL if score = 0

-- CASE expression (SQL's if-else)
SELECT name,
       CASE
           WHEN salary >= 100000 THEN 'Senior'
           WHEN salary >= 60000  THEN 'Mid'
           ELSE 'Junior'
       END AS level
FROM employees;
```

### Aggregation Functions

```sql
SELECT
    department,
    COUNT(*)                    AS headcount,
    COUNT(DISTINCT manager_id)  AS num_managers,
    SUM(salary)                 AS total_payroll,
    AVG(salary)                 AS avg_salary,
    MIN(salary)                 AS min_salary,
    MAX(salary)                 AS max_salary,
    STDDEV(salary)              AS salary_stddev,    -- ML-useful
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median_salary
FROM employees
GROUP BY department
HAVING COUNT(*) >= 5
ORDER BY avg_salary DESC;
```

> **Interview Q:** *What's the difference between COUNT(*) and COUNT(column)?*
> `COUNT(*)` counts all rows including NULLs. `COUNT(column)` counts only non-NULL values in that column. `COUNT(DISTINCT column)` counts unique non-NULL values.

> **Interview Q:** *Can you use a window function in a WHERE clause?*
> No. Window functions are computed in the SELECT phase (step 5), after WHERE (step 2). To filter on a window function result, wrap it in a CTE or subquery, then filter in the outer query.

---

## 4. JOINs — Deep Dive

> **Interview Q:** *Explain all types of JOINs with examples. When would you use each?*

### Visual Reference

```
Table A: [1, 2, 3]    Table B: [2, 3, 4]

INNER JOIN:   [2, 3]           ← matching rows only
LEFT JOIN:    [1, 2, 3]        ← all A + matching B (NULLs for non-match)
RIGHT JOIN:   [2, 3, 4]        ← all B + matching A
FULL OUTER:   [1, 2, 3, 4]     ← all rows from both sides
CROSS JOIN:   [1×2, 1×3, ...]  ← every combination (cartesian product)
```

### JOIN Syntax Examples

```sql
-- INNER JOIN: only rows with a match in both tables
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- LEFT JOIN: all employees, even those without a department
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- FULL OUTER JOIN: all employees and all departments
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;

-- SELF JOIN: employees and their managers (same table)
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;

-- ANTI-JOIN: find employees without a department
SELECT e.name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
WHERE d.id IS NULL;

-- CROSS JOIN: generate all combinations (use sparingly)
SELECT a.product, b.region
FROM products a
CROSS JOIN regions b;
```

### Handling NULLs in JOINs

```sql
-- DANGER: NOT IN with NULLs
-- If subquery returns any NULL, the whole NOT IN returns empty
-- Use NOT EXISTS instead for safety:

-- UNSAFE
WHERE user_id NOT IN (SELECT user_id FROM churned_users);

-- SAFE
WHERE NOT EXISTS (
    SELECT 1 FROM churned_users c WHERE c.user_id = u.user_id
);
```

### Multiple Table JOINs

```sql
SELECT
    o.order_id,
    c.customer_name,
    p.product_name,
    oi.quantity,
    oi.unit_price * oi.quantity AS line_total
FROM orders o
JOIN customers c   ON o.customer_id = c.id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p    ON oi.product_id = p.id
WHERE o.order_date >= '2025-01-01'
ORDER BY line_total DESC;
```

> **Interview Q:** *What is the difference between a LEFT JOIN and a LEFT ANTI-JOIN?*
> A LEFT JOIN returns all left rows, with NULLs for unmatched right rows. A LEFT ANTI-JOIN (LEFT JOIN + WHERE right.key IS NULL) returns only the left rows that had *no match* on the right — useful for finding orphaned records, customers who never ordered, users not in a churn list, etc.

---

## 5. Subqueries & Correlated Subqueries

> **Interview Q:** *What is a correlated subquery and when would you use it vs a JOIN?*

### Non-Correlated Subquery (runs once)

```sql
-- Find employees earning above the company average
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- IN with subquery
SELECT name FROM customers
WHERE id IN (SELECT customer_id FROM orders WHERE total > 1000);
```

### Correlated Subquery (runs once per row of outer query)

A correlated subquery references a column from the outer query — it's re-executed for each row.

```sql
-- Find employees earning above their own department's average
SELECT name, department_id, salary
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id  -- references outer 'e'
);
```

This is powerful but **expensive** — runs the subquery N times (once per outer row). Often rewritable as a JOIN with a derived table:

```sql
-- Equivalent, but more efficient:
SELECT e.name, e.department_id, e.salary
FROM employees e
JOIN (
    SELECT department_id, AVG(salary) AS dept_avg
    FROM employees
    GROUP BY department_id
) dept_stats ON e.department_id = dept_stats.department_id
WHERE e.salary > dept_stats.dept_avg;
```

### EXISTS vs IN vs JOIN

| Pattern | When to use |
|---|---|
| `IN (subquery)` | Small subquery results; avoid if subquery can return NULLs |
| `EXISTS` | Large subquery — stops scanning as soon as first match found; NULL-safe |
| `JOIN` | When you need columns from both tables or want better performance control |

```sql
-- EXISTS: find customers who placed at least one order
SELECT name FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.id
);
```

---

## 6. CTEs — Common Table Expressions

> **Interview Q:** *What is a CTE and when would you use it instead of a subquery?*

A CTE (defined with `WITH`) is a named temporary result set scoped to a single query. It improves readability, enables reuse, and is the foundation for recursive queries.

### Basic CTE

```sql
WITH high_earners AS (
    SELECT employee_id, name, salary, department_id
    FROM employees
    WHERE salary > 100000
),
dept_summary AS (
    SELECT department_id, COUNT(*) AS cnt, AVG(salary) AS avg_sal
    FROM high_earners
    GROUP BY department_id
)
SELECT d.department_name, s.cnt, s.avg_sal
FROM dept_summary s
JOIN departments d ON s.department_id = d.id
ORDER BY s.avg_sal DESC;
```

### CTE vs Subquery

| | CTE | Subquery |
|---|---|---|
| Readability | High (named, top-level) | Low (nested) |
| Reuse in same query | Yes — reference multiple times | No — must duplicate |
| Recursive | Yes | No |
| Performance | Same as subquery (usually) | Same as CTE |
| Debugging | Easy — test each CTE independently | Hard |

### Recursive CTE — Hierarchical Data

> **Interview Q:** *How do you query a hierarchy (org chart, folder structure) in SQL?*

```sql
-- Employee org chart: find all reports under a manager
WITH RECURSIVE org_tree AS (
    -- Anchor: start with the root manager
    SELECT employee_id, name, manager_id, 1 AS depth
    FROM employees
    WHERE employee_id = 100  -- root node

    UNION ALL

    -- Recursive: join employees to the tree built so far
    SELECT e.employee_id, e.name, e.manager_id, ot.depth + 1
    FROM employees e
    INNER JOIN org_tree ot ON e.manager_id = ot.employee_id
)
SELECT employee_id, name, depth
FROM org_tree
ORDER BY depth, name;
```

**How recursive CTEs work:**
1. **Anchor** — evaluates once, returns the starting rows
2. **Recursive member** — joins new rows to the previous result set
3. Repeats until no new rows are added

Use cases: org charts, file system trees, bill of materials, category hierarchies, graph traversal.

---

## 7. Window Functions — The Most Interview-Critical Topic

> **Interview Q:** *What are window functions and how are they different from GROUP BY aggregations?*

Window functions compute a value across a **window of rows related to the current row** — without collapsing the rows into groups. The original row count is preserved.

```sql
-- GROUP BY: collapses rows — you lose individual rows
SELECT department, AVG(salary) FROM employees GROUP BY department;
-- 3 departments → 3 rows

-- Window function: computes avg per department, but keeps all rows
SELECT name, department, salary,
       AVG(salary) OVER (PARTITION BY department) AS dept_avg
FROM employees;
-- 100 employees → 100 rows, each with their dept avg
```

### Window Function Syntax

```sql
function_name() OVER (
    PARTITION BY col1, col2    -- defines the "group" (like GROUP BY)
    ORDER BY col3 DESC         -- defines ordering within the window
    ROWS BETWEEN ... AND ...   -- defines the frame (optional)
)
```

### Ranking Functions

```sql
SELECT
    name,
    department,
    salary,
    ROW_NUMBER()   OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
    RANK()         OVER (PARTITION BY department ORDER BY salary DESC) AS rnk,
    DENSE_RANK()   OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rnk,
    NTILE(4)       OVER (PARTITION BY department ORDER BY salary DESC) AS quartile,
    PERCENT_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS pct_rank
FROM employees;
```

**ROW_NUMBER vs RANK vs DENSE_RANK — The critical difference:**

| Salaries | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 100K | 1 | 1 | 1 |
| 100K | 2 | 1 | 1 |
| 90K  | 3 | 3 | 2 |
| 80K  | 4 | 4 | 3 |

- `ROW_NUMBER`: always unique, arbitrary tie-breaking
- `RANK`: ties get same rank; next rank skips (1,1,3)
- `DENSE_RANK`: ties get same rank; next rank is consecutive (1,1,2)

> Use `ROW_NUMBER()` when you need exactly one row per group (deduplication). Use `RANK/DENSE_RANK` when you need "top N" and want to handle ties correctly.

### Offset Functions — LAG and LEAD

```sql
SELECT
    order_date,
    revenue,
    LAG(revenue, 1)  OVER (ORDER BY order_date) AS prev_day_revenue,
    LEAD(revenue, 1) OVER (ORDER BY order_date) AS next_day_revenue,
    revenue - LAG(revenue, 1) OVER (ORDER BY order_date) AS day_over_day_change
FROM daily_revenue;
```

LAG/LEAD are essential for time-series analysis, computing growth rates, and detecting changes between consecutive records.

### Window Frames — ROWS BETWEEN

```sql
-- 7-day rolling average (ML feature engineering)
SELECT
    date,
    sales,
    AVG(sales) OVER (
        ORDER BY date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS rolling_7day_avg,

    -- Cumulative sum
    SUM(sales) OVER (
        ORDER BY date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS cumulative_sales,

    -- Running total reset per month
    SUM(sales) OVER (
        PARTITION BY DATE_TRUNC('month', date)
        ORDER BY date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS monthly_running_total
FROM daily_sales;
```

**Frame options:**
```
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  -- all rows up to current
ROWS BETWEEN 6 PRECEDING AND CURRENT ROW          -- last 7 rows
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING          -- 3-row centred window
ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING  -- current to end
```

### Aggregate Window Functions

```sql
SELECT
    name,
    department,
    salary,
    SUM(salary)   OVER (PARTITION BY department) AS dept_total,
    AVG(salary)   OVER (PARTITION BY department) AS dept_avg,
    salary / SUM(salary) OVER (PARTITION BY department) AS salary_share,
    MAX(salary)   OVER () AS company_max,          -- entire table as window
    MIN(salary)   OVER (PARTITION BY department ORDER BY hire_date
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
                       ) AS running_min_by_hire_date
FROM employees;
```

### FIRST_VALUE / LAST_VALUE / NTH_VALUE

```sql
SELECT
    name,
    department,
    salary,
    FIRST_VALUE(name) OVER (PARTITION BY department ORDER BY salary DESC)
        AS highest_earner_in_dept,
    LAST_VALUE(salary) OVER (
        PARTITION BY department ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS lowest_salary_in_dept
FROM employees;
```

> **Warning:** `LAST_VALUE` defaults to `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`, so it shows the last value in the current partial window, not the global last. Always specify `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` when you want the true last.

---

## 8. Set Operations — UNION, INTERSECT, EXCEPT

> **Interview Q:** *What is the difference between UNION and UNION ALL?*

| Operator | Returns | Performance |
|---|---|---|
| `UNION` | Rows in A OR B; removes duplicates | Slower (requires sort/hash to deduplicate) |
| `UNION ALL` | Rows in A OR B; keeps duplicates | Faster — no deduplication |
| `INTERSECT` | Rows in BOTH A AND B | Moderate |
| `EXCEPT` | Rows in A but NOT in B | Moderate |

```sql
-- UNION ALL: combine two sources, keep duplicates
SELECT user_id, 'app' AS source FROM app_logins
UNION ALL
SELECT user_id, 'web' AS source FROM web_logins;

-- UNION: distinct users from either channel
SELECT user_id FROM app_logins
UNION
SELECT user_id FROM web_logins;

-- INTERSECT: users active on both channels
SELECT user_id FROM app_logins
INTERSECT
SELECT user_id FROM web_logins;

-- EXCEPT: users only on app (not web)
SELECT user_id FROM app_logins
EXCEPT
SELECT user_id FROM web_logins;
```

**Rules:** All set operations require the same number of columns with compatible types. Column names come from the first SELECT.

> Always prefer `UNION ALL` unless you specifically need deduplication — it's significantly faster on large tables.

---

## 9. Indexes & Query Optimisation

> **Interview Q:** *What is an index? What are the tradeoffs of adding more indexes?*

An index is a separate data structure that the database maintains alongside a table to speed up data retrieval — like a book's index pointing to page numbers.

### B-Tree Index (Default)

Most indexes are B-Trees (Balanced Trees). They store key values sorted, allowing binary search instead of a full table scan:

```
Table scan: O(n)  — check every row
Index scan: O(log n) — traverse the B-tree
```

```sql
-- Create indexes
CREATE INDEX idx_emp_department ON employees(department_id);
CREATE INDEX idx_emp_salary     ON employees(salary);
CREATE INDEX idx_emp_dept_sal   ON employees(department_id, salary); -- composite

-- Unique index (also enforces constraint)
CREATE UNIQUE INDEX idx_email ON users(email);
```

### Index Types

| Type | Use case |
|---|---|
| **B-Tree** | Default; range queries, equality, sorting |
| **Hash** | Equality only (`=`); not for ranges |
| **GiST / GIN** | Full-text search, JSON, arrays, geometric data |
| **BRIN** | Very large tables with natural physical ordering (timestamps) |
| **Partial** | Index a subset of rows: `WHERE active = true` |
| **Expression** | Index on computed expression: `LOWER(email)` |

```sql
-- Partial index: only index active users (saves space)
CREATE INDEX idx_active_users ON users(email) WHERE active = true;

-- Expression index: for case-insensitive email lookups
CREATE INDEX idx_lower_email ON users(LOWER(email));
```

### Clustered vs Non-Clustered Index

| | Clustered | Non-Clustered |
|---|---|---|
| Data storage | Table rows physically ordered by this index | Separate structure with pointers to rows |
| Count per table | One (it IS the table order) | Multiple allowed |
| Speed | Fastest for range scans on clustered key | One extra pointer hop |
| Example | Primary key index (usually) | Indexes on other columns |

### Query Optimisation Principles

**1. Use EXPLAIN / EXPLAIN ANALYZE**
```sql
EXPLAIN ANALYZE
SELECT * FROM orders WHERE customer_id = 42 AND status = 'pending';
-- Shows: Seq Scan vs Index Scan, actual cost, row estimates, execution time
```

**2. Index Selectivity**
High selectivity = index is useful (e.g., email — mostly unique). Low selectivity = index may be skipped (e.g., boolean column — only 2 values, faster to table-scan).

**3. Composite Index Column Order**
Columns used in WHERE equality first, then range columns:
```sql
-- Query: WHERE department = 'Eng' AND salary > 80000
-- Good composite index:
CREATE INDEX idx_dept_sal ON employees(department, salary);
-- 'department' first (equality filter), 'salary' second (range)
```

**4. Avoid Functions on Indexed Columns (SARGability)**
```sql
-- Non-SARGable: index on 'created_at' is NOT used
WHERE YEAR(created_at) = 2025        -- ❌ function applied to column

-- SARGable: index IS used
WHERE created_at >= '2025-01-01'
  AND created_at <  '2026-01-01'     -- ✓ column untouched
```

**5. SELECT only needed columns**
```sql
SELECT *                              -- ❌ forces full row read
SELECT user_id, email, status         -- ✓ covering index possible
```

**6. Avoid leading wildcards**
```sql
WHERE name LIKE '%smith'              -- ❌ can't use B-tree index (leading %)
WHERE name LIKE 'smith%'              -- ✓ index works (trailing % only)
```

**7. Use covering indexes**
A covering index includes all columns needed by a query — no table lookup needed:
```sql
-- Query: SELECT email, status FROM users WHERE department = 'Eng'
CREATE INDEX idx_covering ON users(department, email, status);
-- All needed columns are in the index → zero table page reads
```

### Tradeoffs of Adding Indexes

**Benefits:** Faster SELECT queries, ORDER BY, GROUP BY on indexed columns.
**Costs:** (1) Each INSERT/UPDATE/DELETE must also update every index — write overhead. (2) Indexes consume disk space. (3) Too many indexes → query planner may choose the wrong one. **Rule:** Only index columns you actually filter/sort on in real queries.

---

## 10. Transactions & ACID Properties

> **Interview Q:** *What are ACID properties? Explain each with an example.*

A transaction is a sequence of SQL operations treated as a single atomic unit. ACID guarantees ensure reliability even with concurrent users and system failures.

### ACID

**A — Atomicity**
"All or nothing." Either every operation in the transaction commits, or none do. If a bank transfer fails halfway, the debit is rolled back.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 500 WHERE id = 1;  -- debit
UPDATE accounts SET balance = balance + 500 WHERE id = 2;  -- credit
-- If second UPDATE fails, first is rolled back automatically on ROLLBACK
COMMIT;   -- both updates become permanent together
```

**C — Consistency**
A transaction brings the database from one valid state to another. Database constraints (foreign keys, CHECK, NOT NULL) are never violated by a committed transaction.

**I — Isolation**
Concurrent transactions are executed as if they were serial. Each transaction sees a consistent snapshot and doesn't see partial results of other in-progress transactions.

**D — Durability**
Once committed, changes persist — even if the server crashes immediately after. Implemented via write-ahead logging (WAL).

### Transaction Commands

```sql
BEGIN;                            -- start transaction
SAVEPOINT after_debit;            -- create a rollback point
UPDATE ...;
ROLLBACK TO SAVEPOINT after_debit; -- undo to savepoint, keep transaction
ROLLBACK;                          -- undo entire transaction
COMMIT;                            -- finalise all changes
```

### Isolation Levels & Read Phenomena

| Problem | Description |
|---|---|
| **Dirty Read** | Reading uncommitted data from another transaction |
| **Non-Repeatable Read** | Reading the same row twice gets different values (another txn committed between reads) |
| **Phantom Read** | Re-running a range query gets different rows (another txn inserted/deleted rows) |

| Isolation Level | Dirty Read | Non-Repeatable | Phantom |
|---|---|---|---|
| READ UNCOMMITTED | Possible | Possible | Possible |
| READ COMMITTED | ✓ Prevented | Possible | Possible |
| REPEATABLE READ | ✓ | ✓ Prevented | Possible |
| SERIALIZABLE | ✓ | ✓ | ✓ Prevented |

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN;
-- operations
COMMIT;
```

> Most production systems use **READ COMMITTED** (PostgreSQL default) or **REPEATABLE READ** (MySQL InnoDB default). SERIALIZABLE is safest but most expensive.

### Deadlocks

A deadlock occurs when two transactions each hold a lock the other needs:
```
Txn A holds lock on Row 1, wants Row 2
Txn B holds lock on Row 2, wants Row 1
→ Both wait forever → deadlock
```

The database detects this and automatically kills one transaction (the victim) and rolls it back. **Prevention:** always acquire locks in the same order across transactions.

---

## 11. Database Design & Normalisation

> **Interview Q:** *What are the normal forms? Why normalise? When would you denormalise?*

Normalisation is the process of structuring a database to reduce **data redundancy** and improve **data integrity** by organising data into well-defined tables with clear relationships.

### Normal Forms

**1NF (First Normal Form)**
- Each column holds atomic (indivisible) values
- No repeating groups or arrays in a column

```sql
-- VIOLATES 1NF: multiple phones in one column
name: "Alice", phones: "1234567, 9876543"

-- 1NF compliant: separate table for phones
CREATE TABLE user_phones (user_id INT, phone VARCHAR(20));
```

**2NF (Second Normal Form)**
- Must be in 1NF
- Every non-key column must depend on the *entire* primary key (no partial dependencies)
- Applies to tables with composite primary keys

```sql
-- VIOLATES 2NF: composite key (order_id, product_id)
-- 'product_name' depends only on product_id, not the full key
order_id | product_id | product_name | quantity

-- 2NF: separate products into their own table
orders(order_id, product_id, quantity)
products(product_id, product_name)
```

**3NF (Third Normal Form)**
- Must be in 2NF
- No transitive dependencies — non-key columns should not depend on other non-key columns

```sql
-- VIOLATES 3NF: 'department_name' depends on 'department_id', not 'employee_id'
employees(employee_id, department_id, department_name, salary)

-- 3NF:
employees(employee_id, department_id, salary)
departments(department_id, department_name)
```

**BCNF (Boyce-Codd NF)** — stricter than 3NF; every determinant must be a candidate key.

### Keys

| Key | Definition |
|---|---|
| **Primary Key** | Uniquely identifies each row; NOT NULL, unique |
| **Foreign Key** | References PK in another table; enforces referential integrity |
| **Composite Key** | PK made of multiple columns |
| **Surrogate Key** | Auto-generated ID (SERIAL, UUID) — no business meaning |
| **Natural Key** | Business-meaningful identifier (email, SSN) |
| **Candidate Key** | Any column/set that could be a PK |

### When to Denormalise

Normalisation is for OLTP (transactional systems — many reads/writes, data integrity critical). For **OLAP** (analytical, read-heavy, data warehousing), denormalisation improves query performance:

- **Star schema:** Fact table (metrics) + denormalised dimension tables — used in data warehouses (Redshift, BigQuery, Snowflake)
- **Wide tables:** Flatten joins into one table — common in ML feature stores
- **Pre-computed aggregates:** Materialised views

```
Normalised (OLTP):         orders JOIN order_items JOIN products JOIN customers
Denormalised (OLAP star):  fact_orders(order_id, customer_name, product_name, qty, revenue)
                                      ← no joins needed for analytics
```

### Star Schema vs Snowflake Schema

| | Star Schema | Snowflake Schema |
|---|---|---|
| Structure | Fact + flat dimension tables | Fact + normalised dimension hierarchy |
| Query speed | Faster (fewer JOINs) | Slower (more JOINs) |
| Storage | More redundancy | Less redundancy |
| Maintenance | Simpler | More complex |
| Use case | BI dashboards, analytics | When storage is a concern |

---

## 12. Advanced SQL — Recursive CTEs, PIVOT, JSON, MERGE

### Recursive CTEs (covered in Section 6 — expanded here)

```sql
-- Generate a date range (very common in analytics)
WITH RECURSIVE date_series AS (
    SELECT '2025-01-01'::DATE AS dt
    UNION ALL
    SELECT dt + INTERVAL '1 day'
    FROM date_series
    WHERE dt < '2025-12-31'
)
SELECT dt FROM date_series;

-- Fibonacci numbers
WITH RECURSIVE fib(n, a, b) AS (
    SELECT 1, 0, 1
    UNION ALL
    SELECT n+1, b, a+b FROM fib WHERE n < 10
)
SELECT n, a AS fibonacci FROM fib;
```

### PIVOT — Rows to Columns

Standard SQL doesn't have a PIVOT keyword, but `CASE + GROUP BY` achieves it:

```sql
-- Convert monthly sales rows into columns
SELECT
    product_id,
    SUM(CASE WHEN month = 1 THEN sales END) AS jan,
    SUM(CASE WHEN month = 2 THEN sales END) AS feb,
    SUM(CASE WHEN month = 3 THEN sales END) AS mar,
    SUM(CASE WHEN month = 4 THEN sales END) AS apr
FROM monthly_sales
GROUP BY product_id;
```

### JSON in SQL (PostgreSQL JSONB)

```sql
-- Store JSON payload
CREATE TABLE events (id SERIAL, payload JSONB);

-- Extract fields
SELECT payload->>'user_id' AS user_id,           -- text output
       (payload->>'score')::FLOAT AS score,       -- cast to float
       payload->'metadata'->>'source' AS source   -- nested access
FROM events;

-- Filter by JSON field
WHERE payload->>'event_type' = 'purchase'

-- Check if key exists
WHERE payload ? 'coupon_code'

-- Array element
WHERE payload->'tags' @> '["ml", "nlp"]'::jsonb  -- contains

-- Create GIN index for JSON search
CREATE INDEX idx_payload ON events USING GIN(payload);
```

### MERGE (Upsert)

MERGE inserts new rows or updates existing ones — the core of ETL upsert patterns:

```sql
-- PostgreSQL: INSERT ... ON CONFLICT (upsert)
INSERT INTO users (user_id, name, email, updated_at)
VALUES (42, 'Alice', 'alice@new.com', NOW())
ON CONFLICT (user_id) DO UPDATE
SET email      = EXCLUDED.email,
    updated_at = EXCLUDED.updated_at;

-- Standard SQL MERGE (SQL Server, Oracle, PostgreSQL 15+)
MERGE INTO target_table AS t
USING source_table AS s ON t.id = s.id
WHEN MATCHED THEN
    UPDATE SET t.value = s.value
WHEN NOT MATCHED THEN
    INSERT (id, value) VALUES (s.id, s.value)
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;
```

### String Functions

```sql
UPPER(name), LOWER(email)
TRIM(name), LTRIM(name), RTRIM(name)
LENGTH(name)
SUBSTRING(name, 1, 3)       -- first 3 characters
CONCAT(first, ' ', last)    -- or: first || ' ' || last
REPLACE(text, 'old', 'new')
SPLIT_PART(email, '@', 2)   -- domain from email
REGEXP_REPLACE(phone, '[^0-9]', '')  -- strip non-digits
```

### Date Functions

```sql
NOW()                              -- current timestamp
CURRENT_DATE                       -- current date
DATE_TRUNC('month', created_at)    -- truncate to month start
DATE_PART('year', created_at)      -- extract year
EXTRACT(DOW FROM created_at)       -- day of week (0=Sunday)
created_at + INTERVAL '30 days'    -- date arithmetic
AGE(CURRENT_DATE, birth_date)      -- interval between dates
TO_CHAR(created_at, 'YYYY-MM')     -- format as string
```

---

## 13. SQL for ML & Feature Engineering

> **Interview Q:** *How would you use SQL for feature engineering in an ML pipeline?*

SQL is often the most efficient tool for computing features at scale — especially on large datasets in a data warehouse (BigQuery, Redshift, Snowflake). Feature engineering in SQL happens before data is passed to a training framework.

### Time-Series Features

```sql
-- Common ML features: lags, rolling stats, counts in windows
SELECT
    user_id,
    event_date,
    -- Lagged features
    LAG(purchase_amount, 1)  OVER w AS amt_1d_ago,
    LAG(purchase_amount, 7)  OVER w AS amt_7d_ago,
    LAG(purchase_amount, 30) OVER w AS amt_30d_ago,

    -- Rolling aggregates
    AVG(purchase_amount) OVER (PARTITION BY user_id
        ORDER BY event_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
        AS rolling_7d_avg_spend,

    SUM(purchase_amount) OVER (PARTITION BY user_id
        ORDER BY event_date ROWS BETWEEN 29 PRECEDING AND CURRENT ROW)
        AS rolling_30d_total_spend,

    COUNT(*) OVER (PARTITION BY user_id
        ORDER BY event_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
        AS rolling_7d_purchase_count

FROM user_purchases
WINDOW w AS (PARTITION BY user_id ORDER BY event_date)
ORDER BY user_id, event_date;
```

### User Behaviour Features

```sql
WITH user_stats AS (
    SELECT
        user_id,
        COUNT(*)                                    AS total_orders,
        SUM(amount)                                 AS total_spend,
        AVG(amount)                                 AS avg_order_value,
        MAX(amount)                                 AS max_order_value,
        MIN(order_date)                             AS first_order_date,
        MAX(order_date)                             AS last_order_date,
        CURRENT_DATE - MAX(order_date)::DATE        AS days_since_last_order,
        COUNT(DISTINCT DATE_TRUNC('month', order_date)) AS active_months,
        STDDEV(amount)                              AS spend_stddev
    FROM orders
    GROUP BY user_id
),
recency_bucket AS (
    SELECT *,
        NTILE(5) OVER (ORDER BY days_since_last_order) AS recency_quintile,
        NTILE(5) OVER (ORDER BY total_spend DESC)       AS value_quintile
    FROM user_stats
)
SELECT * FROM recency_bucket;
-- Classic RFM (Recency-Frequency-Monetary) feature table for churn/LTV models
```

### Normalisation / Scaling in SQL

```sql
-- Min-Max normalisation → [0, 1]
WITH stats AS (
    SELECT MIN(salary) AS min_s, MAX(salary) AS max_s FROM employees
)
SELECT employee_id,
       (salary - stats.min_s) / NULLIF(stats.max_s - stats.min_s, 0) AS salary_scaled
FROM employees, stats;

-- Z-score normalisation
WITH stats AS (
    SELECT AVG(salary) AS mean_s, STDDEV(salary) AS std_s FROM employees
)
SELECT employee_id,
       (salary - stats.mean_s) / NULLIF(stats.std_s, 0) AS salary_zscore
FROM employees, stats;

-- Quantile bins (deciles)
SELECT employee_id,
       NTILE(10) OVER (ORDER BY salary) AS salary_decile
FROM employees;
```

### Encoding Categorical Variables

```sql
-- One-hot encoding via CASE WHEN
SELECT
    user_id,
    CASE WHEN country = 'IN' THEN 1 ELSE 0 END AS is_india,
    CASE WHEN country = 'US' THEN 1 ELSE 0 END AS is_us,
    CASE WHEN country = 'UK' THEN 1 ELSE 0 END AS is_uk,
    CASE WHEN device = 'mobile' THEN 1 ELSE 0 END AS is_mobile
FROM users;

-- Target encoding: replace category with mean of target
SELECT
    u.user_id,
    u.country,
    AVG(o.converted) OVER (PARTITION BY u.country) AS country_conversion_rate
FROM users u
JOIN orders o ON u.user_id = o.user_id;
```

### Cohort Analysis

```sql
-- User cohort by signup month, track retention
WITH cohorts AS (
    SELECT user_id,
           DATE_TRUNC('month', signup_date) AS cohort_month
    FROM users
),
activity AS (
    SELECT user_id,
           DATE_TRUNC('month', activity_date) AS activity_month
    FROM user_activity
)
SELECT
    c.cohort_month,
    DATE_PART('month', AGE(a.activity_month, c.cohort_month)) AS months_since_signup,
    COUNT(DISTINCT a.user_id) AS retained_users
FROM cohorts c
JOIN activity a USING (user_id)
GROUP BY 1, 2
ORDER BY 1, 2;
```

### Missing Value Handling

```sql
-- Fill NULLs with a constant
COALESCE(salary, 0)

-- Fill with column average (imputation)
SELECT user_id,
       COALESCE(age, AVG(age) OVER ()) AS age_imputed
FROM users;

-- Forward-fill (carry last known value forward)
SELECT user_id,
       event_date,
       COALESCE(score,
           LAST_VALUE(score IGNORE NULLS) OVER (
               PARTITION BY user_id
               ORDER BY event_date
               ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
           )
       ) AS score_filled
FROM user_scores;
```

### SQL for Model Evaluation Metrics

```sql
-- Compute accuracy, precision, recall from predictions table
WITH confusion AS (
    SELECT
        SUM(CASE WHEN predicted = 1 AND actual = 1 THEN 1 ELSE 0 END) AS tp,
        SUM(CASE WHEN predicted = 1 AND actual = 0 THEN 1 ELSE 0 END) AS fp,
        SUM(CASE WHEN predicted = 0 AND actual = 1 THEN 1 ELSE 0 END) AS fn,
        SUM(CASE WHEN predicted = 0 AND actual = 0 THEN 1 ELSE 0 END) AS tn
    FROM predictions
)
SELECT
    (tp + tn)::FLOAT / (tp + fp + fn + tn) AS accuracy,
    tp::FLOAT / NULLIF(tp + fp, 0)          AS precision,
    tp::FLOAT / NULLIF(tp + fn, 0)          AS recall,
    2.0 * tp / NULLIF(2*tp + fp + fn, 0)    AS f1_score
FROM confusion;
```

---

## 14. Classic Interview Problems — Solved

These patterns appear constantly in SQL rounds. Know them cold.

### 1. Find the Nth Highest Salary

```sql
-- Method 1: ROW_NUMBER (cleanest)
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) ranked
WHERE rnk = 2;

-- Method 2: Subquery (works without window functions)
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

### 2. Top N Records per Group

```sql
-- Top 3 highest-paid employees per department
SELECT department, name, salary
FROM (
    SELECT department, name, salary,
           ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rn
    FROM employees
) ranked
WHERE rn <= 3;
```

### 3. Running Total / Cumulative Sum

```sql
SELECT order_date, amount,
       SUM(amount) OVER (ORDER BY order_date
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM orders;
```

### 4. Year-over-Year Growth Rate

```sql
WITH yearly AS (
    SELECT
        product_id,
        EXTRACT(YEAR FROM order_date) AS yr,
        SUM(amount) AS total
    FROM orders
    GROUP BY 1, 2
)
SELECT
    product_id,
    yr,
    total AS current_year,
    LAG(total) OVER (PARTITION BY product_id ORDER BY yr) AS prev_year,
    ROUND(
        100.0 * (total - LAG(total) OVER (PARTITION BY product_id ORDER BY yr))
              / NULLIF(LAG(total) OVER (PARTITION BY product_id ORDER BY yr), 0),
        2
    ) AS yoy_growth_pct
FROM yearly;
```

### 5. Deduplicate — Keep Latest Record per User

```sql
-- Keep only the most recent record per user
WITH ranked AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY updated_at DESC) AS rn
    FROM user_records
)
SELECT * FROM ranked WHERE rn = 1;
```

### 6. Find Users Active for N Consecutive Days

```sql
-- Users active 4+ consecutive days
WITH dated AS (
    SELECT DISTINCT user_id, activity_date,
           activity_date - ROW_NUMBER() OVER (
               PARTITION BY user_id ORDER BY activity_date
           )::INTEGER * INTERVAL '1 day' AS grp
    FROM activity
),
streaks AS (
    SELECT user_id, grp, COUNT(*) AS streak_len
    FROM dated
    GROUP BY user_id, grp
)
SELECT DISTINCT user_id
FROM streaks
WHERE streak_len >= 4;
```

### 7. Sessions from Events (Gap-and-Island)

```sql
-- Find user sessions: new session if gap > 30 minutes
WITH gaps AS (
    SELECT user_id, event_time,
           CASE WHEN event_time - LAG(event_time) OVER (
               PARTITION BY user_id ORDER BY event_time
           ) > INTERVAL '30 minutes' OR
                LAG(event_time) OVER (PARTITION BY user_id ORDER BY event_time) IS NULL
           THEN 1 ELSE 0 END AS session_start
    FROM events
),
sessions AS (
    SELECT user_id, event_time,
           SUM(session_start) OVER (PARTITION BY user_id ORDER BY event_time) AS session_id
    FROM gaps
)
SELECT user_id, session_id,
       MIN(event_time) AS session_start,
       MAX(event_time) AS session_end,
       COUNT(*) AS events_in_session
FROM sessions
GROUP BY user_id, session_id;
```

### 8. Median (no built-in in all databases)

```sql
-- PostgreSQL
SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median
FROM employees;

-- Generic SQL using ROW_NUMBER
WITH ordered AS (
    SELECT salary,
           ROW_NUMBER() OVER (ORDER BY salary) AS rn,
           COUNT(*) OVER () AS total
    FROM employees
)
SELECT AVG(salary) AS median
FROM ordered
WHERE rn IN (FLOOR((total+1)/2.0), CEIL((total+1)/2.0));
```

### 9. Find Duplicate Records

```sql
-- Find duplicate emails
SELECT email, COUNT(*) AS cnt
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Show full rows for duplicates
SELECT *
FROM users
WHERE email IN (
    SELECT email FROM users GROUP BY email HAVING COUNT(*) > 1
)
ORDER BY email;

-- Delete duplicates keeping latest
DELETE FROM users
WHERE id NOT IN (
    SELECT MAX(id) FROM users GROUP BY email
);
```

### 10. Pivot — Monthly Revenue by Product

```sql
SELECT
    product_id,
    SUM(CASE WHEN EXTRACT(MONTH FROM order_date) = 1 THEN revenue ELSE 0 END) AS jan,
    SUM(CASE WHEN EXTRACT(MONTH FROM order_date) = 2 THEN revenue ELSE 0 END) AS feb,
    SUM(CASE WHEN EXTRACT(MONTH FROM order_date) = 3 THEN revenue ELSE 0 END) AS mar,
    SUM(revenue) AS total
FROM orders
WHERE EXTRACT(YEAR FROM order_date) = 2025
GROUP BY product_id;
```

### 11. Self-Join — Employee and Manager Salary Comparison

```sql
SELECT
    e.name AS employee,
    e.salary AS emp_salary,
    m.name AS manager,
    m.salary AS mgr_salary,
    CASE WHEN e.salary > m.salary THEN 'Earns more than manager' ELSE 'OK' END AS flag
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;
```

### 12. Retention Rate

```sql
-- Month-1 retention: users who returned the month after signup
WITH signups AS (
    SELECT user_id, DATE_TRUNC('month', signup_date) AS signup_month
    FROM users
),
next_month_active AS (
    SELECT DISTINCT user_id, DATE_TRUNC('month', activity_date) AS active_month
    FROM activity
)
SELECT
    s.signup_month,
    COUNT(DISTINCT s.user_id) AS new_users,
    COUNT(DISTINCT a.user_id) AS retained,
    ROUND(100.0 * COUNT(DISTINCT a.user_id) / COUNT(DISTINCT s.user_id), 1) AS retention_rate
FROM signups s
LEFT JOIN next_month_active a
    ON s.user_id = a.user_id
    AND a.active_month = s.signup_month + INTERVAL '1 month'
GROUP BY s.signup_month
ORDER BY s.signup_month;
```

---

## 15. Interview Q&A — Fundamentals

**Q: What is the difference between WHERE and HAVING?**
> WHERE filters individual rows **before** grouping. HAVING filters groups **after** aggregation. You can use aggregate functions (COUNT, SUM, AVG) in HAVING but not in WHERE. Example: `WHERE salary > 50000` (per-row filter before grouping); `HAVING COUNT(*) > 5` (filter departments with more than 5 employees).

**Q: What is a NULL value and how does SQL handle it?**
> NULL represents the absence of a value — it's not zero, not an empty string, not false. Any arithmetic or comparison with NULL returns NULL: `NULL = NULL` → NULL (not TRUE). Use `IS NULL` / `IS NOT NULL` to check for NULLs. Use `COALESCE(col, default)` to replace NULLs. Be careful with `NOT IN` — if the subquery returns any NULL, the entire NOT IN condition returns no rows.

**Q: What is the difference between a primary key and a unique key?**
> Both enforce uniqueness. A PRIMARY KEY also enforces NOT NULL — no NULLs allowed, exactly one per table. A UNIQUE key allows NULLs (and in most databases, multiple NULLs are allowed since NULL ≠ NULL) and a table can have multiple unique keys.

**Q: What is referential integrity and how is it enforced?**
> Referential integrity ensures that a foreign key value in one table always corresponds to an existing primary key in the referenced table — you can't have an order referencing a customer_id that doesn't exist. Enforced via FOREIGN KEY constraints. If you try to delete a parent row with children, the DB either rejects it (RESTRICT), deletes the children too (CASCADE), or sets children to NULL (SET NULL).

**Q: What is a view and what are its limitations?**
> A view is a saved SQL query presented as a virtual table. It doesn't store data (unless materialised). Benefits: simplify complex queries, hide sensitive columns, present consistent interface. Limitations: (1) Performance — standard views rerun the query each time; (2) No direct INSERT/UPDATE through complex views; (3) Materialised views need explicit refresh to see new data.

**Q: What is the difference between a view and a materialised view?**
> A regular view reruns its query each time it's accessed — always current but can be slow. A materialised view stores the query result physically on disk — fast to query but stale until refreshed (`REFRESH MATERIALISED VIEW`). Use materialised views for expensive aggregations that don't need real-time freshness (daily summary tables, BI dashboards).

**Q: What does COALESCE do?**
> `COALESCE(a, b, c, ...)` returns the first non-NULL value from its arguments. `COALESCE(salary, 0)` returns salary if not NULL, else 0. It's the standard way to handle NULLs in computations. `NULLIF(a, b)` is the inverse — returns NULL if a equals b, else returns a. Useful to avoid division-by-zero: `a / NULLIF(b, 0)`.

---

## 16. Interview Q&A — Window Functions & CTEs

**Q: What is the difference between ROW_NUMBER, RANK, and DENSE_RANK?**
> All three assign sequential numbers to rows within a partition. For tied values: `ROW_NUMBER` assigns unique numbers (arbitrary tie-breaking). `RANK` assigns the same rank to ties but skips the next ranks (1,1,3). `DENSE_RANK` assigns the same rank to ties without skipping (1,1,2). Use ROW_NUMBER for deduplication (exactly 1 per group); use DENSE_RANK when ties should share a rank and you don't want gaps.

**Q: How would you get the top 3 products by sales per category?**
> Use `ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC)` to rank products within each category, then filter on `rn <= 3` in an outer query or CTE. If ties should be included, use `DENSE_RANK` and filter `rnk <= 3` — this may return more than 3 rows per category when there are ties at rank 3.

**Q: Can a window function be used in a WHERE clause?**
> No. Window functions execute in the SELECT phase, which comes after WHERE in the logical execution order. You cannot filter on a window function result in WHERE. Solution: wrap the SELECT with window function in a CTE or subquery, then filter in the outer query's WHERE.

**Q: What is the difference between PARTITION BY and GROUP BY?**
> `GROUP BY` collapses rows into one row per group — you lose the individual row detail. `PARTITION BY` in a window function defines groups for the calculation but **preserves all original rows**. Use GROUP BY when you only need group-level aggregates; use PARTITION BY (window function) when you need both row-level detail and group-level aggregates in the same result.

**Q: What is a recursive CTE and when do you use it?**
> A recursive CTE references itself to iteratively process hierarchical or graph data. It has two parts: an anchor (base case) and a recursive member (references the CTE itself) joined with UNION ALL. The recursion stops when the recursive member returns no rows. Use cases: org charts, folder hierarchies, bill of materials, pathfinding in graphs, generating date sequences.

**Q: What does LAG/LEAD do and give a use case?**
> `LAG(col, n)` returns the value of `col` from `n` rows before the current row within the partition. `LEAD(col, n)` returns the value from `n` rows after. Classic uses: computing day-over-day change (`revenue - LAG(revenue, 1)`), comparing an employee's salary to their previous job, detecting consecutive status changes, computing time between events.

---

## 17. Interview Q&A — Performance & Design

**Q: How do you optimise a slow SQL query?**
> (1) Run `EXPLAIN ANALYZE` to see the execution plan — look for sequential scans on large tables. (2) Check if the right indexes exist on columns in WHERE, JOIN, and ORDER BY clauses. (3) Ensure the query is SARGable — no functions applied to indexed columns in WHERE. (4) Avoid `SELECT *` — only select needed columns, enabling covering indexes. (5) Rewrite correlated subqueries as JOINs. (6) Break complex queries into CTEs for readability and incremental debugging. (7) Check for implicit type conversions that prevent index use. (8) Consider partitioning for very large tables.

**Q: What is a covering index?**
> A covering index contains all the columns needed by a query, so the database can answer the query entirely from the index without accessing the main table. Example: query `SELECT email, status FROM users WHERE department = 'Eng'` — a composite index on `(department, email, status)` covers it completely. Covering indexes eliminate the expensive "heap fetch" step and dramatically speed up read-heavy queries.

**Q: What is the N+1 query problem?**
> Executing N additional queries to fetch related data for N parent rows — extremely common in ORMs. Example: fetch 100 users (1 query), then for each user fetch their orders (100 more queries) = 101 total. Solution: use a JOIN or IN clause to fetch all data in 1–2 queries. In ML pipelines this matters — lazy loading in ORM-based data loaders creates N+1 problems at scale.

**Q: Explain the difference between clustered and non-clustered indexes.**
> A clustered index determines the physical order of data storage in the table — the table IS sorted by this index. Each table can have only one. Retrieval for range queries on the clustered key is fastest because related rows are physically adjacent. A non-clustered index is a separate structure with sorted key values and pointers to the actual row locations. A table can have many. Every non-clustered index lookup requires an extra pointer hop to get the actual row.

**Q: When should you NOT add an index?**
> (1) Low-selectivity columns (boolean, status with 2–5 values) — the planner may ignore the index anyway. (2) Heavily written tables — each index adds overhead to every INSERT/UPDATE/DELETE. (3) Small tables — full scans are often faster than index overhead. (4) Columns rarely used in WHERE/JOIN/ORDER BY. Too many indexes can confuse the query planner and slow down writes significantly.

**Q: What are the different normal forms and why do they matter?**
> 1NF: atomic values, no repeating groups. 2NF: no partial dependencies (all non-key cols depend on the full PK). 3NF: no transitive dependencies (non-key cols don't depend on other non-key cols). Higher normal forms reduce data redundancy, prevent update anomalies, and ensure data integrity. In practice, most OLTP systems target 3NF. Data warehouses intentionally denormalise (star schema) for query performance.

**Q: How is SQL used differently in OLTP vs OLAP systems?**
> OLTP (Online Transaction Processing): many small transactions, high write throughput, normalised schema, optimised for row-level operations (insert/update/delete single records). Examples: MySQL, PostgreSQL. OLAP (Online Analytical Processing): large analytical queries, primarily reads, denormalised star/snowflake schema, columnar storage, full table scans on specific columns. Examples: BigQuery, Redshift, Snowflake. As an ML engineer you extract features from OLAP (warehouse) and write predictions back to OLTP (serving DB).

**Q: What is a deadlock and how do you handle it?**
> A deadlock occurs when two transactions each hold a lock the other needs, creating a circular wait. The database detects it and automatically kills one transaction (the victim). Handle in application code: catch the deadlock error and retry. Prevent by: always acquiring locks in the same order across transactions, keeping transactions short, using lower isolation levels where appropriate.

**Q: What is query plan and how do you read EXPLAIN output?**
> The query plan describes the steps the database engine will take to execute your query. Key elements in EXPLAIN ANALYZE output: `Seq Scan` (full table scan — bad on large tables without filter), `Index Scan` (using an index — good), `Hash Join` / `Nested Loop` / `Merge Join` (join strategies), actual rows vs estimated rows (large discrepancy = stale statistics → run ANALYZE), cost numbers (not milliseconds, relative units). The goal is to avoid Seq Scans on large tables and ensure join strategies are appropriate for table sizes.

---

## 18. Quick Reference Cheat Sheet

### SQL Execution Order
```
FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

### Window Function Syntax
```sql
function() OVER (
    PARTITION BY col      -- group (like GROUP BY, but keeps all rows)
    ORDER BY col DESC     -- ordering within window
    ROWS BETWEEN          -- frame definition
        UNBOUNDED PRECEDING AND CURRENT ROW   -- cumulative
        6 PRECEDING AND CURRENT ROW           -- rolling 7
        UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING  -- entire partition
)
```

### Ranking Functions Reference
```
ROW_NUMBER()  → always unique: 1, 2, 3, 4
RANK()        → ties share rank, skip next: 1, 1, 3, 4
DENSE_RANK()  → ties share rank, no skip: 1, 1, 2, 3
NTILE(n)      → divide into n buckets: 1, 1, 2, 2 (for n=2)
PERCENT_RANK()→ 0.0 to 1.0 relative ranking
```

### JOIN Quick Reference
```
INNER JOIN      → matching rows only
LEFT JOIN       → all left + matched right (NULL for misses)
RIGHT JOIN      → all right + matched left
FULL OUTER JOIN → all rows from both sides
CROSS JOIN      → cartesian product (every combo)
SELF JOIN       → table joined to itself
LEFT ANTI-JOIN  → LEFT JOIN + WHERE right.key IS NULL
```

### NULL Handling
```sql
IS NULL / IS NOT NULL     -- test for NULL
COALESCE(a, b, c)         -- first non-NULL value
NULLIF(a, b)              -- return NULL if a = b
a / NULLIF(b, 0)          -- safe division
```

### Index Types
```
B-Tree      → default; equality, range, ORDER BY
Hash        → equality only (faster for = checks)
GIN         → arrays, JSON, full-text
BRIN        → large ordered tables (timestamps)
Partial     → WHERE condition subset
Expression  → LOWER(email), YEAR(date)
Covering    → includes all query columns → zero heap fetch
```

### Transaction Template
```sql
BEGIN;
SAVEPOINT sp1;
-- operations
ROLLBACK TO SAVEPOINT sp1;  -- undo to checkpoint
COMMIT;                      -- or ROLLBACK to undo all
```

### ACID Quick Reference
```
A = Atomicity   → all or nothing
C = Consistency → valid state before and after
I = Isolation   → concurrent txns don't interfere
D = Durability  → committed data survives crashes
```

### Normal Forms Quick Reference
```
1NF → atomic values, no repeating groups
2NF → no partial dependency (full PK determines all cols)
3NF → no transitive dependency (non-key cols independent)
BCNF → every determinant is a candidate key
```

### Common Patterns Cheat Sheet
```sql
-- Top N per group
ROW_NUMBER() OVER (PARTITION BY grp ORDER BY val DESC) → filter rn <= N

-- YoY growth
LAG(val) OVER (PARTITION BY id ORDER BY yr) → prev year

-- Running total
SUM(val) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)

-- 7-day rolling average
AVG(val) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)

-- Deduplication (keep latest)
ROW_NUMBER() OVER (PARTITION BY key ORDER BY updated_at DESC) → filter rn = 1

-- Upsert
INSERT INTO t (...) VALUES (...) ON CONFLICT (key) DO UPDATE SET ...

-- Safe division
val / NULLIF(denominator, 0)

-- Null-safe NOT IN alternative
WHERE NOT EXISTS (SELECT 1 FROM t2 WHERE t2.id = t1.id)
```

### Key Differences Quick Reference
```
DELETE vs TRUNCATE vs DROP → rows / all rows / whole table
WHERE vs HAVING            → before GROUP BY / after GROUP BY
UNION vs UNION ALL         → deduplicate / keep all (faster)
COUNT(*) vs COUNT(col)     → all rows / non-NULL only
VIEW vs MATERIALISED VIEW  → virtual/live / stored/stale
RANK vs DENSE_RANK         → gaps / no gaps on ties
CHAR vs VARCHAR            → fixed-length / variable-length
FLOAT vs DECIMAL           → approximate / exact (use DECIMAL for money)
OLTP vs OLAP               → row operations / analytical scans
Clustered vs Non-clustered → physical ordering / pointer lookup
```

---

*Sources: DataQuest, DataCamp, Exponent, InterviewKickstart, StrataScratch, Devinterview.io SQL-ML repo, Udacity, DataLemur, Analytics Vidhya — question banks from 2024–2026 technical interviews at FAANG, data/ML engineering roles.*
