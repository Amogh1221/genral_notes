# 🗄️ DBMS — Database Management Systems

> **Theory + Interview Q&A | Placement Focused | AI/ML Engineer**
> *Questions sourced from Google, Amazon, Stripe, Snowflake, and campus placement rounds 2024–2026*

---

## Table of Contents

1. [Core Concepts](#1-core-concepts)
2. [ACID Properties](#2-acid-properties)
3. [Transactions & Concurrency Control](#3-transactions--concurrency-control)
4. [Keys & Constraints](#4-keys--constraints)
5. [Normalisation](#5-normalisation)
6. [Indexing](#6-indexing)
7. [Joins](#7-joins)
8. [NoSQL vs SQL](#8-nosql-vs-sql)
9. [Query Optimisation](#9-query-optimisation)
10. [Interview Q&A — Rapid Fire](#10-interview-qa--rapid-fire)

---

## 1. Core Concepts

### What is a DBMS?

A **Database Management System** manages structured data — handles storage, retrieval, security, concurrency, and integrity. Examples: PostgreSQL, MySQL, SQLite, MongoDB.

```
Application
    ↓
  DBMS  ← handles queries, transactions, concurrency, security
    ↓
Physical Storage (disk)
```

### DBMS vs RDBMS vs File System

| | File System | DBMS | RDBMS |
|---|---|---|---|
| Data structure | Files | Any model | Tables (relations) |
| Relationships | None | Depends | FK relationships |
| Query language | None | Varies | SQL |
| ACID | ❌ | Partial | ✓ Full |
| Examples | OS files | MongoDB | PostgreSQL, MySQL |

### Three-Level Architecture

```
External Level  → What each user/app sees (views, access control)
Conceptual Level → Full logical schema (tables, relationships, constraints)
Internal Level  → Physical storage layout (files, indexes, pages)

Logical Independence:  Change schema without changing user views.
Physical Independence: Change storage without changing schema.
```

### Data Models

```
Relational Model (most common):
  Data in tables (relations). Rows = tuples. Columns = attributes.
  Relationships via foreign keys.

Hierarchical Model:
  Tree structure. Parent-child. Used in early IBM systems, XML.

Network Model:
  Graph structure. Many-to-many natively.

Object-Oriented Model:
  Objects with methods stored in DB. Used in some specialised DBs.

Document Model (NoSQL):
  JSON/BSON documents. MongoDB, CouchDB.
```

---

## 2. ACID Properties

> **Interview Q:** *What are ACID properties? Explain each with a real example.*

**ACID = Atomicity + Consistency + Isolation + Durability**

These guarantee database transactions are processed reliably even in the presence of crashes, errors, and concurrent access.

### Atomicity — "All or Nothing"

```
A transaction either completes fully or has no effect at all.

Example: Bank transfer (A sends ₹1000 to B)
  Step 1: Debit A by ₹1000   ← succeeds
  Step 2: Credit B by ₹1000  ← system crashes ❌

Without Atomicity: A loses ₹1000, B gets nothing.
With Atomicity:    Both steps are rolled back. Money stays with A.

Implementation: Write-Ahead Logging (WAL)
  → Log the operation to disk BEFORE applying it.
  → On crash, replay log or undo incomplete transactions.
```

### Consistency — "Valid State to Valid State"

```
A transaction brings the database from one valid state to another.
All integrity constraints, rules, and triggers must hold after the transaction.

Example: Rule — account balance ≥ 0.
  Transaction tries to withdraw ₹5000 from account with ₹3000.
  → DBMS enforces constraint → transaction rejected → DB stays consistent.

Note: DBMS enforces structural constraints. Business logic consistency
      is the developer's responsibility.
```

### Isolation — "Concurrent = Serial"

```
Concurrent transactions execute as if they ran serially, one after another.
Intermediate states of a transaction are NOT visible to other transactions.

Example: Two users booking the LAST airline seat simultaneously.
  Without isolation: Both see 1 seat available → both book → overbooking.
  With isolation: One succeeds, other is blocked or gets "seat unavailable."

Full isolation = correctness but slow.
Weaker isolation = faster but allows some anomalies.
(See Section 3 for isolation levels.)
```

### Durability — "Committed = Permanent"

```
Once a transaction commits, it persists even if the system crashes immediately after.

Example: You click "Payment Successful" and your phone dies.
  With Durability: Payment record is preserved on disk.
  Without Durability: Might need to re-pay.

Implementation:
  → Write committed data to non-volatile storage (disk) before confirming.
  → Techniques: Write-Ahead Log (WAL), shadow paging, checksums.
```

### ACID in ML Context

```
Feature store writes: Atomicity ensures partial feature updates don't corrupt training data.
Experiment tracking: Durability ensures completed experiment results survive crashes.
Model registry: Isolation prevents two concurrent deployments from conflicting.
```

---

## 3. Transactions & Concurrency Control

> **Interview Q:** *What are the isolation levels? What problems does each solve?*

### What is a Transaction?

```
A transaction is a unit of work — a sequence of operations treated as one.

BEGIN;                          -- start
  UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
  UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT;                         -- make permanent
-- OR ROLLBACK;                 -- undo everything
```

### Concurrency Problems (Without Isolation)

**1. Dirty Read** — Reading uncommitted data from another transaction.
```
T1: Updates balance to ₹500 (not yet committed)
T2: Reads balance → sees ₹500
T1: ROLLBACK (balance goes back to ₹1000)
T2: Made decision based on ₹500 which never really existed.
```

**2. Non-Repeatable Read** — Same row read twice gives different results.
```
T1: Reads balance → ₹1000
T2: Updates balance to ₹500 and commits
T1: Reads balance again → ₹500 (changed within same transaction!)
```

**3. Phantom Read** — Re-running a query returns different set of rows.
```
T1: SELECT * WHERE salary > 50000 → returns 10 rows
T2: INSERT a new employee with salary 60000 and commits
T1: Re-runs same query → returns 11 rows (phantom appeared!)
```

**4. Lost Update** — Two transactions read same value and both update it.
```
T1: Read balance = ₹1000, plan to add ₹500
T2: Read balance = ₹1000, plan to add ₹200
T1: Write ₹1500
T2: Write ₹1200 (overwrites T1's update — ₹500 lost!)
```

### Isolation Levels

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read | Speed |
|---|---|---|---|---|
| READ UNCOMMITTED | ✓ Possible | ✓ Possible | ✓ Possible | Fastest |
| READ COMMITTED | ❌ Prevented | ✓ Possible | ✓ Possible | Fast |
| REPEATABLE READ | ❌ Prevented | ❌ Prevented | ✓ Possible | Medium |
| SERIALIZABLE | ❌ Prevented | ❌ Prevented | ❌ Prevented | Slowest |

```sql
-- Set isolation level in PostgreSQL
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN;
  -- your queries here
COMMIT;
```

**Default isolation levels:**
- PostgreSQL: READ COMMITTED
- MySQL InnoDB: REPEATABLE READ
- Oracle: READ COMMITTED
- SQL Server: READ COMMITTED

### Deadlocks

```
T1 holds lock on Table A, waiting for Table B.
T2 holds lock on Table B, waiting for Table A.
→ Neither can proceed → Deadlock.

Detection: DBMS detects cycles in the wait-for graph.
Resolution: One transaction is chosen as victim and rolled back.

Prevention strategies:
  1. Lock ordering: always acquire locks in same order.
  2. Timeout: rollback if a transaction waits too long.
  3. Optimistic concurrency: no locks, check for conflicts at commit time.
```

### Locks

```
Shared Lock (S):    Multiple transactions can read. No writes allowed.
Exclusive Lock (X): Only one transaction can hold. No reads or writes by others.
Intent Lock:        Signals intent to lock at a finer granularity.

Two-Phase Locking (2PL):
  Growing phase:  Acquire locks, release none.
  Shrinking phase: Release locks, acquire none.
  Guarantees serializability but can cause deadlocks.
```

---

## 4. Keys & Constraints

> **Interview Q:** *What is the difference between primary key, candidate key, and foreign key?*

### Types of Keys

```
Super Key:
  Any set of attributes that uniquely identifies a row.
  Example: {StudentID}, {StudentID, Name}, {Email} — all are super keys.

Candidate Key:
  Minimal super key — no subset of it can uniquely identify rows.
  Example: {StudentID} and {Email} are candidate keys (minimal).

Primary Key (PK):
  The chosen candidate key for the table. Only ONE per table.
  Cannot be NULL. Creates a clustered index automatically.

Alternate Key:
  Candidate keys not chosen as PK. Can be enforced with UNIQUE constraint.

Foreign Key (FK):
  Attribute in one table that references the PK of another table.
  Enforces referential integrity — you can't have an FK value with no matching PK.

Composite Key:
  Primary key made of multiple columns together.
  Example: (StudentID, CourseID) in an Enrollment table.

Surrogate Key:
  Artificially generated key (auto-increment ID). Has no business meaning.
  vs Natural Key: a key that has business meaning (like SSN, email).
```

### Constraints

```sql
CREATE TABLE Students (
    id       INT PRIMARY KEY,          -- PK: unique + NOT NULL
    email    VARCHAR(100) UNIQUE,      -- UNIQUE: no duplicates, NULLs allowed
    name     VARCHAR(50) NOT NULL,     -- NOT NULL: must have a value
    age      INT CHECK (age >= 18),    -- CHECK: custom condition
    dept_id  INT REFERENCES Dept(id)  -- FK: referential integrity
);

-- ON DELETE / ON UPDATE behaviour for FK:
-- CASCADE:    delete/update propagates to child rows
-- SET NULL:   child FK set to NULL
-- RESTRICT:   prevent deletion if child rows exist (default)
-- NO ACTION:  similar to RESTRICT but deferred
```

---

## 5. Normalisation

> **Interview Q:** *What is normalisation? Explain 1NF, 2NF, 3NF, and BCNF.*

### Why Normalise?

```
Eliminate data redundancy → saves space, prevents update anomalies.

Anomalies caused by redundancy:
  Insertion anomaly:  Can't insert data without unrelated data.
  Update anomaly:     Must update same info in multiple places.
  Deletion anomaly:   Deleting one thing accidentally removes another.
```

### 1NF — First Normal Form

```
Rule: Every column must have atomic (indivisible) values.
      No repeating groups. Each row must be uniquely identifiable.

Violation:
  | StudentID | Courses              |
  |-----------|----------------------|
  | 1         | Math, Physics, Chem  |  ← multi-valued, not atomic

Fix:
  | StudentID | Course  |
  |-----------|---------|
  | 1         | Math    |
  | 1         | Physics |
  | 1         | Chem    |
```

### 2NF — Second Normal Form

```
Rule: Must be in 1NF AND every non-key attribute must be
      FULLY functionally dependent on the ENTIRE primary key.
      (No partial dependencies — only matters with composite PKs)

Violation (PK = {StudentID, CourseID}):
  | StudentID | CourseID | CourseName | StudentName |
  StudentName depends only on StudentID (partial dependency!)
  CourseName depends only on CourseID (partial dependency!)

Fix: Split into separate tables:
  Students(StudentID, StudentName)
  Courses(CourseID, CourseName)
  Enrollment(StudentID, CourseID)
```

### 3NF — Third Normal Form

```
Rule: Must be in 2NF AND no transitive dependencies.
      Non-key attributes must depend ONLY on the primary key,
      not on other non-key attributes.

Violation (PK = EmployeeID):
  | EmpID | DeptID | DeptName |
  DeptName depends on DeptID (which depends on EmpID) → transitive!

Fix:
  Employee(EmpID, DeptID)
  Department(DeptID, DeptName)
```

### BCNF — Boyce-Codd Normal Form

```
Rule: Stricter than 3NF. For every functional dependency X → Y,
      X must be a superkey.

Violation: A situation where a non-candidate-key determines
           part of a candidate key.

Example:
  Table: (Student, Course, Teacher) where one Teacher teaches one Course
  FD: Teacher → Course  (but Teacher is not a superkey)
  This violates BCNF even if it's in 3NF.

Fix: Decompose into:
  TeacherCourse(Teacher, Course)
  StudentTeacher(Student, Teacher)

Note: BCNF decomposition may not always preserve all FDs.
      Sometimes 3NF is preferred for this reason.
```

### Denormalisation

```
Intentionally introducing redundancy to improve READ performance.
Used in data warehouses, analytics, and ML feature stores.

Example: Pre-joining tables so queries don't need expensive JOINs.

Trade-off: Faster reads ↔ Slower writes, more storage, risk of inconsistency.

In ML: Feature stores often denormalise for fast feature retrieval during inference.
```

---

## 6. Indexing

> **Interview Q:** *What is an index? How does a B-tree index work? When would you NOT use an index?*

### What is an Index?

```
An index is a data structure that speeds up data retrieval at the cost of
extra storage and slower writes (insert/update/delete must update the index too).

Without index: Full table scan → O(n)
With index:    Direct lookup    → O(log n) for B-tree, O(1) avg for hash
```

### B-Tree Index (Most Common)

```
Default index type in PostgreSQL, MySQL, SQL Server.

Structure: Balanced tree. Leaf nodes contain the actual data pointers.

Operations: O(log n) for search, insert, delete.

Supports:
  ✓ Equality: WHERE id = 5
  ✓ Range:    WHERE id BETWEEN 5 AND 10
  ✓ Sorting:  ORDER BY id
  ✓ Prefix:   WHERE name LIKE 'John%'

Does NOT support:
  ❌ Suffix:  WHERE name LIKE '%John'  (use full-text index)
```

### Hash Index

```
Uses a hash table. O(1) average lookup.

Supports: Equality only: WHERE id = 5
Does NOT support: Range queries, sorting.

Used in: PostgreSQL hash indexes, MySQL MEMORY tables.
```

### Index Types

```sql
-- Single column index
CREATE INDEX idx_name ON employees(name);

-- Composite index (column ORDER matters!)
CREATE INDEX idx_dept_salary ON employees(dept_id, salary);
-- Can use: WHERE dept_id = 5
-- Can use: WHERE dept_id = 5 AND salary > 50000
-- CANNOT use efficiently: WHERE salary > 50000 alone (left-prefix rule)

-- Unique index (auto-created for PRIMARY KEY and UNIQUE constraints)
CREATE UNIQUE INDEX idx_email ON users(email);

-- Covering index: includes all columns needed by a query (no table lookup needed)
CREATE INDEX idx_covering ON orders(user_id, order_date, amount);

-- Partial index: index only a subset of rows
CREATE INDEX idx_active ON users(email) WHERE status = 'active';
```

### When NOT to Use an Index

```
❌ Low cardinality columns (e.g., gender — only M/F — full scan is faster)
❌ Small tables (full scan is faster than index lookup overhead)
❌ Columns rarely used in WHERE / JOIN / ORDER BY
❌ Very frequently updated columns (index maintenance overhead)
❌ Tables with very heavy write workloads (e-commerce orders)

Rule of thumb: Index columns used in WHERE, JOIN, ORDER BY, GROUP BY.
              Index foreign keys (often queried in joins).
```

### Clustered vs Non-Clustered Index

```
Clustered Index:
  The actual table data is physically sorted and stored according to the index.
  Only ONE clustered index per table.
  In MySQL InnoDB: the PK is always the clustered index.
  Range queries very fast (data is contiguous on disk).

Non-Clustered Index:
  Separate structure with pointers to actual data rows.
  Multiple non-clustered indexes allowed.
  Extra lookup needed: index → row pointer → actual row (unless covering index).
```

---

## 7. Joins

> **Interview Q:** *Explain the different types of JOINs. When would you use each?*

```
Table A (Employees):          Table B (Departments):
EmpID | Name | DeptID         DeptID | DeptName
1     | Alice | 10            10     | Engineering
2     | Bob   | 20            20     | Marketing
3     | Carol | 30            30     | (exists but no employees)
4     | Dave  | NULL          (40 exists in some versions)
```

### INNER JOIN

```sql
-- Only rows that have a match in BOTH tables
SELECT e.Name, d.DeptName
FROM Employees e INNER JOIN Departments d ON e.DeptID = d.DeptID;
-- Result: Alice-Engineering, Bob-Marketing
-- Carol excluded (DeptID 30 not in Departments), Dave excluded (NULL)
```

### LEFT JOIN (LEFT OUTER JOIN)

```sql
-- All rows from LEFT table, matched rows from RIGHT (NULL if no match)
SELECT e.Name, d.DeptName
FROM Employees e LEFT JOIN Departments d ON e.DeptID = d.DeptID;
-- Result: Alice-Engineering, Bob-Marketing, Carol-NULL, Dave-NULL
-- All employees included, NULL for those with no department match
```

### RIGHT JOIN (RIGHT OUTER JOIN)

```sql
-- All rows from RIGHT table, matched rows from LEFT (NULL if no match)
SELECT e.Name, d.DeptName
FROM Employees e RIGHT JOIN Departments d ON e.DeptID = d.DeptID;
-- Result: Alice-Engineering, Bob-Marketing, NULL-HR (dept with no employees)
```

### FULL OUTER JOIN

```sql
-- All rows from BOTH tables, NULL where no match
SELECT e.Name, d.DeptName
FROM Employees e FULL OUTER JOIN Departments d ON e.DeptID = d.DeptID;
-- Result: All employees + all departments, NULLs where no match on either side
```

### CROSS JOIN

```sql
-- Cartesian product: every row in A paired with every row in B
-- m rows × n rows = m*n rows. Use carefully!
SELECT e.Name, d.DeptName
FROM Employees e CROSS JOIN Departments d;
-- 4 employees × 3 departments = 12 rows
```

### SELF JOIN

```sql
-- Join a table to itself. Common for hierarchical data (employee-manager).
SELECT e.Name AS Employee, m.Name AS Manager
FROM Employees e
LEFT JOIN Employees m ON e.ManagerID = m.EmpID;
```

### Join Implementation Algorithms (How the DB does it)

```
Nested Loop Join:   For each row in A, scan all of B. O(n*m). Good for small tables.
Hash Join:          Build hash table on smaller table, probe with larger. O(n+m). Best for equality joins.
Merge Join:         Sort both tables on join key, then merge. O(n log n + m log m). Best for sorted data or range joins.

Query optimiser chooses based on table sizes, available indexes, and statistics.
```

---

## 8. NoSQL vs SQL

> **Interview Q:** *When would you choose NoSQL over SQL? What are the trade-offs?*

### CAP Theorem

```
In a distributed system, you can only guarantee TWO of three:

Consistency:   Every read gets the most recent write.
Availability:  Every request gets a response (not necessarily latest data).
Partition Tolerance: System works even if network partitions occur.

Since network partitions are unavoidable in distributed systems,
real choice is between Consistency and Availability:

CP systems (Consistent + Partition Tolerant): HBase, MongoDB (w/ majority read concern), Zookeeper
AP systems (Available + Partition Tolerant):  Cassandra, CouchDB, DynamoDB

SQL databases (single node): CA — consistent + available, not partition tolerant.
```

### SQL vs NoSQL Comparison

| | SQL (Relational) | NoSQL |
|---|---|---|
| Schema | Fixed schema, defined upfront | Flexible / schemaless |
| Scaling | Vertical (bigger machine) | Horizontal (more machines) |
| ACID | Full ACID | Usually BASE (Eventually Consistent) |
| Query | SQL — powerful, standardised | Varies by DB type |
| Joins | Easy, native | Hard, usually denormalised |
| Best for | Structured data, complex queries | Unstructured, high-volume, flexible schema |
| Examples | PostgreSQL, MySQL, Oracle | MongoDB, Cassandra, Redis, DynamoDB |

### NoSQL Types

```
Document Store (MongoDB):
  Stores JSON/BSON documents. No fixed schema.
  Best for: Content management, catalogs, user profiles.
  ML use: Storing experiment configs, model metadata.

Key-Value Store (Redis, DynamoDB):
  Simple key → value mapping. Extremely fast.
  Best for: Caching, session storage, feature flags.
  ML use: Feature caching for real-time inference, model output caching.

Column-Family Store (Cassandra, HBase):
  Data stored by columns not rows. Great for sparse data.
  Best for: Time-series, IoT, event logging.
  ML use: Storing time-series sensor data for training.

Graph Database (Neo4j):
  Nodes and edges. Best for relationship-heavy queries.
  Best for: Social networks, fraud detection, knowledge graphs.
  ML use: Graph neural networks, recommendation systems.
```

### BASE Properties (NoSQL alternative to ACID)

```
Basically Available: System guarantees availability.
Soft State:          State may change over time even without new input (eventual consistency propagating).
Eventually Consistent: System will become consistent given enough time.

SQL: ACID (strong consistency, immediate)
NoSQL: BASE (eventual consistency, high availability)
```

---

## 9. Query Optimisation

> **Interview Q:** *How does a database execute a SQL query? What is an execution plan?*

### Query Execution Pipeline

```
SQL Query (text)
    ↓ Parser        → checks syntax, builds parse tree
    ↓ Semantic Check → validates table/column names, permissions
    ↓ Query Rewriter → applies views, rewrites subqueries
    ↓ Optimiser     → generates & evaluates execution plans, picks best
    ↓ Executor      → executes chosen plan
    ↓ Results
```

### EXPLAIN / EXPLAIN ANALYZE

```sql
-- See the execution plan (PostgreSQL)
EXPLAIN SELECT * FROM orders WHERE user_id = 5;

-- Actually run it and show timing
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 5;

-- Look for:
--   Seq Scan   → full table scan (bad for large tables)
--   Index Scan → using an index (good)
--   Hash Join, Merge Join, Nested Loop → join strategies
--   Cost: startup_cost..total_cost
--   Rows: estimated rows returned
```

### Common Optimisation Techniques

```sql
-- 1. Use indexes on columns in WHERE, JOIN, ORDER BY
CREATE INDEX idx_user ON orders(user_id);

-- 2. Avoid SELECT * — only fetch needed columns
SELECT id, name FROM users WHERE active = true;
-- Not: SELECT * FROM users WHERE active = true;

-- 3. Avoid functions on indexed columns in WHERE (index can't be used)
-- Bad:
WHERE YEAR(created_at) = 2024
-- Good:
WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31'

-- 4. Use EXISTS instead of IN for large subqueries
-- Bad (executes inner query for all rows):
WHERE user_id IN (SELECT id FROM users WHERE premium = true)
-- Good (stops at first match):
WHERE EXISTS (SELECT 1 FROM users WHERE users.id = orders.user_id AND premium = true)

-- 5. Limit early, paginate properly
SELECT * FROM products ORDER BY created_at DESC LIMIT 20 OFFSET 1000;
-- Better for deep pagination: keyset pagination
WHERE created_at < '2024-01-15' ORDER BY created_at DESC LIMIT 20;

-- 6. Use connection pooling (PgBouncer for PostgreSQL) for many concurrent connections.
```

### Stored Procedures vs Views vs Triggers

```sql
-- VIEW: saved SELECT query, acts like a virtual table
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE status = 'active';
SELECT * FROM active_users;  -- clean, reusable

-- STORED PROCEDURE: saved executable logic with parameters
CREATE PROCEDURE transfer_funds(sender INT, receiver INT, amount DECIMAL)
BEGIN
    UPDATE accounts SET balance = balance - amount WHERE id = sender;
    UPDATE accounts SET balance = balance + amount WHERE id = receiver;
END;
CALL transfer_funds(1, 2, 1000);

-- TRIGGER: auto-executed on INSERT/UPDATE/DELETE
CREATE TRIGGER log_update
AFTER UPDATE ON employees
FOR EACH ROW
INSERT INTO audit_log(emp_id, changed_at) VALUES(NEW.id, NOW());
```

---

## 10. Interview Q&A — Rapid Fire

**Q: What is the difference between DELETE, TRUNCATE, and DROP?**
> `DELETE`: Removes specific rows based on WHERE clause. DML statement. Logged (can be rolled back). Triggers fire. Slower. `TRUNCATE`: Removes ALL rows. DDL statement. Minimal logging (faster). Cannot be rolled back in most DBs. Triggers don't fire. Resets auto-increment. `DROP`: Removes the entire table structure and data permanently. Cannot be rolled back.

**Q: What is a view? What are its advantages and limitations?**
> A view is a saved SELECT query that behaves like a virtual table. Advantages: simplifies complex queries, provides security (users see only view not base table), provides abstraction (underlying schema can change). Limitations: cannot always be updated (if view has joins, aggregates, DISTINCT), no performance benefit by itself unless it's a materialised view (which stores results on disk).

**Q: What is the difference between HAVING and WHERE?**
> `WHERE` filters individual rows **before** aggregation. `HAVING` filters groups **after** `GROUP BY` aggregation. You can't use aggregate functions in WHERE. `HAVING` is used with GROUP BY.
```sql
SELECT dept, AVG(salary) FROM employees
WHERE status = 'active'          -- filter rows before grouping
GROUP BY dept
HAVING AVG(salary) > 50000;     -- filter groups after aggregation
```

**Q: What is a composite index and how does the left-prefix rule work?**
> A composite index on (A, B, C) can be used for queries on (A), (A, B), or (A, B, C) but NOT for queries on (B) or (B, C) alone. The leftmost column must be included. This is the left-prefix rule. Always put the most selective column first, and columns used in WHERE before columns only used in ORDER BY.

**Q: What is the N+1 query problem?**
> When fetching N records, you make N additional queries (one per record). Common ORM issue. Example: fetch 100 users, then for each user make a separate query to fetch their orders = 101 queries. Fix: use JOIN or eager loading (`SELECT * FROM users JOIN orders ON ...`). Very relevant in ML data pipelines — batch your database reads.

**Q: What is sharding? How is it different from partitioning?**
> **Partitioning** splits one table into multiple segments within the same database server (range partition, hash partition). **Sharding** distributes data across multiple separate database servers. Sharding enables horizontal scaling (more servers = more capacity) at the cost of complexity (cross-shard queries, distributed transactions become hard).

**Q: What is a deadlock and how do databases handle it?**
> Deadlock = two transactions each holding a lock the other needs, waiting forever. Detection: DB builds a wait-for graph and detects cycles. Resolution: one transaction is chosen as a victim (usually the one with least work done) and is rolled back. Prevention: always acquire locks in consistent order, use timeouts, or use optimistic concurrency control.

**Q: What is an ORM and what are its pros/cons?**
> ORM (Object-Relational Mapper) maps database tables to programming language objects. SQLAlchemy (Python), Hibernate (Java), ActiveRecord (Ruby). Pros: no raw SQL needed, productivity boost, DB-agnostic code, built-in migrations. Cons: N+1 problems, performance overhead, complex queries are awkward, hides what SQL is actually being executed. For ML data pipelines: sometimes raw SQL or pandas read_sql is better than ORM for complex aggregations.

**Q: What is the difference between optimistic and pessimistic concurrency control?**
> **Pessimistic**: Assume conflicts will happen. Lock data before reading or writing. Blocks other transactions. Safe but slow. Use when conflicts are frequent. **Optimistic**: Assume conflicts are rare. No locks during read. At commit time, check if data was modified by another transaction (using version numbers or timestamps). If conflict detected → retry or rollback. Faster in low-contention scenarios. Common in web apps and distributed systems.

**Q: Explain the difference between a clustered and non-clustered index.**
> A **clustered index** dictates the physical storage order of rows — table data is sorted and stored according to the index key. Only one per table (it IS the table). In MySQL InnoDB, the primary key is always the clustered index. Very fast for range queries. A **non-clustered index** is a separate structure with index key values and pointers to the actual row locations. Multiple allowed per table. Requires an extra lookup to get the full row (unless it's a covering index).

---

*Sources: Google/Amazon/Stripe interview reports 2024–2026, GeeksforGeeks DBMS, DataCamp DBMS Interview Guide, Analytics Vidhya Top DBMS Questions.*
