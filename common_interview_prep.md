# Fresher Technical Interview Prep: SQL, OOP and Software Engineering

This file covers three areas:

1. [SQL Interview Questions](#part-1-sql-interview-questions)
2. [OOP Interview Questions](#part-2-oop-interview-questions)
3. [Software Engineering Interview Questions](#part-3-software-engineering-interview-questions)

---

# Part 1: SQL Interview Questions

The core SQL areas for interviews are joins, filtering, aggregation, keys and constraints, normalization, subqueries and CTEs, window functions, indexes, transactions, and classic query problems.

## 1. SQL Fundamentals

### Q1. What is SQL?

SQL stands for Structured Query Language. It is used to interact with relational databases: to create, retrieve, update and delete data, as well as define database structures and control access.

Common operations:

- `SELECT` → retrieve data
- `INSERT` → add data
- `UPDATE` → modify data
- `DELETE` → remove data
- `CREATE` → create database objects
- `ALTER` → modify objects
- `DROP` → remove objects

### Q2. What is the difference between SQL and MySQL?

SQL is a language used to communicate with relational databases. MySQL is a relational database management system (RDBMS) that implements SQL.

Other RDBMSs include PostgreSQL, Oracle, SQL Server and SQLite.

> **Analogy:** SQL is the language; MySQL is the software that understands that language.

### Q3. What is DBMS?

DBMS stands for Database Management System. It is software used to store, manage, retrieve and manipulate data in databases.

Examples: MySQL, PostgreSQL, Oracle, SQL Server.

### Q4. What is RDBMS?

RDBMS stands for Relational Database Management System. It stores data in tables made of rows and columns and allows relationships between tables using keys.

The important word is **relational**.

```text
Employee
---------
employee_id
name
department_id

Department
----------
department_id
department_name
```

Here `department_id` establishes the relationship.

### Q5. DBMS vs RDBMS?

| DBMS | RDBMS |
|---|---|
| May not use tables | Uses tables |
| Relationships aren't necessarily enforced | Relationships supported using keys |
| Can be less structured | Highly structured |
| Example: some NoSQL-style systems | MySQL, PostgreSQL, Oracle |

For interviews, the important distinction is **relational tables plus relationships**.

---

## 2. Database Schema

### Q6. What is a schema?

A schema is the logical structure or blueprint of a database. It defines tables, columns, data types, relationships, constraints, views and other database objects.

```text
Company Database

Employee
  employee_id INT
  name VARCHAR
  salary DECIMAL
  department_id INT

Department
  department_id INT
  department_name VARCHAR
```

The schema describes how the database is organized, not the actual rows stored inside it.

### Q7. What is the difference between schema and data?

Schema (structure):

```text
Employee(
    id INT,
    name VARCHAR,
    salary DECIMAL
)
```

Data (values):

```text
1 | Rahul | 50000
2 | Amit  | 60000
3 | Neha  | 70000
```

Schema = structure. Data = actual values.

### Q8. What is a database schema vs database instance?

- **Schema:** the structure or design of the database.
- **Instance:** the actual data stored in the database at a particular point in time.

Example:

```text
Schema: Employee(id, name, salary)

At 10 AM:
1 Amit 50000
2 Rahul 60000

At 5 PM:
1 Amit 50000
2 Rahul 60000
3 Neha 70000
```

The schema stayed the same, but the instance changed.

### Q9. What are the types/levels of database schema?

This is a DBMS theory question. The three-level architecture is:

1. **External schema:** what a particular user or application sees.
   - Example: HR sees `employee_id`, `name`, `salary`.
2. **Conceptual schema:** the overall logical structure of the database.
   - Example: `Employee`, `Department`, `Project`, `Salary` and their relationships.
3. **Internal schema:** how data is physically stored.
   - Example: indexes, storage structures, files, physical organization.

Summary: **External** → what users see. **Conceptual** → logical structure. **Internal** → physical storage.

### Q10. What is a schema in MySQL?

In MySQL, *database* and *schema* are essentially synonyms:

```sql
CREATE DATABASE company;
-- and
CREATE SCHEMA company;
```

Both refer to the same concept in MySQL.

In systems such as PostgreSQL, a database can contain multiple schemas. So don't blindly say "a schema is always a separate database." That's wrong.

---

## 3. SQL JOINs

Joins are one of the highest-priority interview topics.

### Q11. What is a JOIN?

A JOIN combines rows from two or more tables based on a related column or condition.

```text
Employee
+----+------+---------+
| id | name | dept_id |
+----+------+---------+

Department
+---------+-----------+
| dept_id | dept_name |
+---------+-----------+
```

```sql
SELECT e.name, d.dept_name
FROM Employee e
JOIN Department d
  ON e.dept_id = d.dept_id;
```

### Q12. How many types of JOINs are there?

The commonly discussed SQL joins are:

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

This is the safest answer. Some sources treat SELF JOIN as a technique rather than a separate join operator, since it is an inner or outer join against the same table.

### Q13. What is INNER JOIN?

Returns only rows that have matching values in both tables.

```sql
SELECT *
FROM Employee e
INNER JOIN Department d
  ON e.dept_id = d.dept_id;
```

If:

```text
Employee:   1 → 10,  2 → 20,  3 → 30
Department: 10, 20, 40
```

Result: `1 → 10` and `2 → 20`. Rows 30 and 40 disappear.

### Q14. What is LEFT JOIN?

Returns every row from the left table and matching rows from the right table. If no match exists, right-side columns become `NULL`.

```sql
SELECT *
FROM Employee e
LEFT JOIN Department d
  ON e.dept_id = d.dept_id;
```

The **left table** is the table written before `JOIN`.

### Q15. What is RIGHT JOIN?

Returns every row from the right table and matching rows from the left table.

```sql
SELECT *
FROM Employee e
RIGHT JOIN Department d
  ON e.dept_id = d.dept_id;
```

In practice, a RIGHT JOIN can usually be rewritten as a LEFT JOIN by swapping the tables.

### Q16. What is FULL OUTER JOIN?

Returns all rows from both tables. Matching rows are combined; unmatched rows contain `NULL`s for the missing side.

```sql
SELECT *
FROM Employee e
FULL OUTER JOIN Department d
  ON e.dept_id = d.dept_id;
```

**Note:** MySQL does not directly support FULL OUTER JOIN. It is commonly emulated with `LEFT JOIN ... UNION ... RIGHT JOIN`.

### Q17. What is CROSS JOIN?

CROSS JOIN produces the Cartesian product: every row from the first table is paired with every row from the second.

If A has 3 rows and B has 4 rows, the result has 3 × 4 = 12 rows.

```sql
SELECT *
FROM A
CROSS JOIN B;
```

No `ON` condition is required.

### Q18. What is SELF JOIN?

A self join joins a table with itself, usually to represent relationships within the same table.

```text
Employee
---------
id
name
manager_id
```

Find each employee and their manager:

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM Employee e
LEFT JOIN Employee m
  ON e.manager_id = m.id;
```

This is very commonly asked.

---

## 4. JOIN vs UNION

### Q19. What is the difference between JOIN and UNION?

**JOIN** combines columns horizontally.

```text
Table A         Table B
id name         id salary
1  Amit         1  50000
2  Rahul        2  60000
```

JOIN result:

```text
id name  salary
1  Amit  50000
2  Rahul 60000
```

**UNION** combines rows vertically.

```sql
SELECT name FROM Employee_India
UNION
SELECT name FROM Employee_US;
```

Simplest interview answer: **JOIN adds columns by combining related rows; UNION adds rows by combining result sets.**

### Q20. UNION vs UNION ALL?

- **UNION** removes duplicates.
- **UNION ALL** keeps duplicates.

```text
A: Delhi, Mumbai
B: Mumbai, Pune

UNION:     Delhi, Mumbai, Pune
UNION ALL: Delhi, Mumbai, Mumbai, Pune
```

`UNION ALL` is generally faster because it skips duplicate elimination.

---

## 5. Keys

### Q21. What is a Primary Key?

A primary key uniquely identifies every row in a table. It cannot contain `NULL` values. A table can have only one primary-key constraint, though that key can span multiple columns.

```sql
CREATE TABLE Employee (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

### Q22. What is a Foreign Key?

A foreign key is a column that references a key in another table and maintains referential integrity.

```text
Employee
---------
id
dept_id   ← foreign key

Department
----------
id        ← primary key
```

```sql
FOREIGN KEY (dept_id) REFERENCES Department(id)
```

### Q23. Primary Key vs Foreign Key

| Primary Key | Foreign Key |
|---|---|
| Uniquely identifies a row | Establishes a relationship |
| Cannot be NULL | Can generally be NULL |
| Unique | Can contain duplicates |
| One PK constraint per table | Multiple FKs possible |
| Identifies its own record | References another table |

### Q24. Primary Key vs UNIQUE Key

- **Primary key:** uniquely identifies rows, cannot be NULL, one per table.
- **UNIQUE:** prevents duplicate values, NULL behavior is database-dependent, multiple UNIQUE constraints can exist.

Don't say "UNIQUE and PRIMARY KEY are exactly the same." They aren't.

### Q25. What is a composite key?

A composite key consists of two or more columns that together uniquely identify a row.

```text
Student_Course
student_id
course_id
```

One student can take many courses and one course can have many students, so `(student_id, course_id)` together uniquely identifies each enrollment.

### Q26. What is a candidate key?

A candidate key is a column or combination of columns that can uniquely identify a row and could be chosen as the primary key.

```text
Employee
---------
employee_id
email
name
```

If `employee_id` and `email` are both unique, both are candidate keys. One is selected as the primary key.

---

## 6. Constraints

### Q27. What are SQL constraints?

Constraints enforce rules on data. Common ones:

- `PRIMARY KEY`
- `FOREIGN KEY`
- `UNIQUE`
- `NOT NULL`
- `CHECK`
- `DEFAULT`

```sql
CREATE TABLE Employee (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    salary INT CHECK (salary > 0),
    department_id INT,
    FOREIGN KEY (department_id)
        REFERENCES Department(id)
);
```

---

## 7. NULL

### Q28. What is NULL?

`NULL` represents a missing, unknown or unavailable value. It is not the same as zero or an empty string.

```sql
-- Wrong
WHERE salary = NULL

-- Correct
WHERE salary IS NULL
WHERE salary IS NOT NULL
```

This distinction is commonly tested.

### Q29. NULL vs 0 vs empty string

```text
NULL → unknown or missing
0    → known numeric value zero
''   → known empty string
```

They are not equivalent.

### Q30. What is COALESCE?

`COALESCE` returns the first non-NULL value from a list of expressions.

```sql
SELECT COALESCE(phone, 'Not Available')
FROM Employee;
```

If `phone` is NULL, the result is `Not Available`.

```text
COALESCE(A, B, C) → first non-NULL value
```

---

## 8. SELECT, WHERE, GROUP BY

### Q31. WHERE vs HAVING?

**WHERE** filters individual rows before grouping.

```sql
SELECT *
FROM Employee
WHERE salary > 50000;
```

**HAVING** filters groups after `GROUP BY`.

```sql
SELECT department_id, AVG(salary)
FROM Employee
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

Interview answer: **WHERE filters rows before aggregation; HAVING filters groups after aggregation.**

### Q32. WHERE vs ON in a JOIN?

Consider:

```sql
-- Version A
SELECT *
FROM Employee e
LEFT JOIN Department d
  ON e.dept_id = d.id
 AND d.name = 'IT';

-- Version B
SELECT *
FROM Employee e
LEFT JOIN Department d
  ON e.dept_id = d.id
WHERE d.name = 'IT';
```

They can produce different results. In a LEFT JOIN, putting the condition in `WHERE` eliminates the NULL-extended rows, effectively turning the query into something closer to an INNER JOIN. This is a very common interview trap.

### Q33. What is DISTINCT?

`DISTINCT` removes duplicate rows from the result.

```sql
SELECT DISTINCT department_id
FROM Employee;
```

Input `10, 10, 20, 20, 30` produces `10, 20, 30`.

### Q34. What is ORDER BY?

Sorts the result.

```sql
SELECT * FROM Employee ORDER BY salary ASC;
SELECT * FROM Employee ORDER BY salary DESC;
```

### Q35. What is GROUP BY?

`GROUP BY` groups rows with the same values, usually so aggregate functions can be applied to each group.

```sql
SELECT department_id, AVG(salary)
FROM Employee
GROUP BY department_id;
```

---

## 9. Aggregate Functions

### Q36. What are aggregate functions?

They calculate a value across multiple rows. Common ones: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`.

```sql
SELECT
    department_id,
    COUNT(*) AS employees,
    AVG(salary) AS avg_salary
FROM Employee
GROUP BY department_id;
```

### Q37. COUNT(*) vs COUNT(column)?

- `COUNT(*)` counts rows.
- `COUNT(column)` counts non-NULL values in that column.

```text
id | phone
1  | 999
2  | NULL
3  | 888

COUNT(*)     → 3
COUNT(phone) → 2
```

---

## 10. DELETE, TRUNCATE, DROP

### Q38. DELETE vs TRUNCATE vs DROP

- **DELETE** removes rows and can use `WHERE`.

  ```sql
  DELETE FROM Employee WHERE id = 5;
  ```

- **TRUNCATE** removes all rows and does not use `WHERE`.

  ```sql
  TRUNCATE TABLE Employee;
  ```

- **DROP** removes the table itself.

  ```sql
  DROP TABLE Employee;
  ```

Summary:

```text
DELETE   → remove selected or all rows
TRUNCATE → remove all rows
DROP     → remove table structure and data
```

Transactional and rollback behavior can vary by database engine, so avoid an absolute claim like "TRUNCATE can never be rolled back" unless you know the DBMS being discussed.

---

## 11. Normalization

### Q39. What is normalization?

Normalization organizes data into related tables to reduce unnecessary duplication and prevent data anomalies.

Main goals:

- Reduce redundancy
- Avoid update anomalies
- Improve data integrity

### Q40. What is 1NF?

A table is in First Normal Form when each column holds atomic values and there are no repeating groups.

Bad:

```text
Student | Courses
Amit    | SQL, Python, Java
```

Better:

```text
Student | Course
Amit    | SQL
Amit    | Python
Amit    | Java
```

### Q41. What is 2NF?

A table is in 2NF if it is in 1NF and every non-key attribute depends on the *entire* primary key, not just part of a composite key. This matters mainly when the primary key is composite.

### Q42. What is 3NF?

A table is in 3NF if it is in 2NF and non-key attributes do not depend on other non-key attributes.

```text
Employee: employee_id, department_id, department_name

employee_id   → department_id
department_id → department_name
employee_id   → department_name   (transitive dependency)
```

Split into:

```text
Employee:   employee_id, department_id
Department: department_id, department_name
```

---

## 12. Subqueries and CTEs

### Q43. What is a subquery?

A query nested inside another query.

```sql
SELECT *
FROM Employee
WHERE salary > (
    SELECT AVG(salary)
    FROM Employee
);
```

### Q44. What is a correlated subquery?

A correlated subquery depends on the outer query and is logically evaluated for each relevant outer row.

```sql
SELECT e1.*
FROM Employee e1
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM Employee e2
    WHERE e2.department_id = e1.department_id
);
```

This finds employees earning more than their department's average.

### Q45. What is a CTE?

CTE stands for Common Table Expression. It is a temporary named result set defined with `WITH`, which makes complex queries easier to read and structure.

```sql
WITH avg_salary AS (
    SELECT AVG(salary) AS avg_sal
    FROM Employee
)
SELECT *
FROM Employee
WHERE salary > (SELECT avg_sal FROM avg_salary);
```

### Q46. CTE vs subquery?

Both can express similar logic. CTEs generally make complex queries more readable and can be referenced multiple times within a statement. Recursive CTEs also handle hierarchical problems.

Do not claim CTEs are automatically faster. They aren't.

---

## 13. Window Functions

### Q47. What is a window function?

A window function performs a calculation across related rows without collapsing them into one row the way `GROUP BY` does.

```sql
SELECT
    name,
    salary,
    AVG(salary) OVER() AS company_avg
FROM Employee;
```

Every employee remains in the result.

### Q48. GROUP BY vs window function?

**GROUP BY** collapses rows: one row per department.

```sql
SELECT department_id, AVG(salary)
FROM Employee
GROUP BY department_id;
```

**Window function** keeps individual rows: every employee stays visible.

```sql
SELECT
    name,
    department_id,
    salary,
    AVG(salary) OVER (PARTITION BY department_id)
FROM Employee;
```

### Q49. ROW_NUMBER vs RANK vs DENSE_RANK?

Given salaries `100, 100, 90, 80`:

| Function | Result |
|---|---|
| `ROW_NUMBER` | 1, 2, 3, 4 |
| `RANK` | 1, 1, 3, 4 (gap after ties) |
| `DENSE_RANK` | 1, 1, 2, 3 (no gap) |

Memorize:

- `ROW_NUMBER` → unique numbering
- `RANK` → gaps after ties
- `DENSE_RANK` → no gaps after ties

### Q50. What is LAG? What is LEAD?

- **LAG** accesses a previous row without a self join.
- **LEAD** accesses a subsequent row.

```sql
SELECT
    employee_id,
    salary,
    LAG(salary)  OVER (ORDER BY employee_id) AS previous_salary,
    LEAD(salary) OVER (ORDER BY employee_id) AS next_salary
FROM Employee;
```

Memory trick: **LAG looks backward; LEAD looks forward.**

### Q51. PARTITION BY vs ORDER BY (in window functions)?

- **PARTITION BY** divides rows into groups (windows).

  ```sql
  AVG(salary) OVER (PARTITION BY department_id)
  ```

- **ORDER BY** defines the order of rows within the window.

  ```sql
  ROW_NUMBER() OVER (
      PARTITION BY department_id
      ORDER BY salary DESC
  )
  ```

This means: divide employees by department, then rank them by salary within each department.

---

## 14. Indexes

### Q52. What is an index?

An index is a data structure that lets the database find rows more efficiently, reducing how much data it must scan.

```sql
CREATE INDEX idx_employee_name ON Employee(name);
```

Queries such as `WHERE name = 'Amit'` may benefit from this index.

### Q53. Why don't we index every column?

Indexes aren't free. They:

- consume storage
- increase INSERT, UPDATE and DELETE cost
- require maintenance

Good answer: **Indexes improve read performance but add storage and write overhead, so they should be created based on query patterns, not on every column.**

### Q54. What is a composite index?

An index on multiple columns.

```sql
CREATE INDEX idx_emp_dept_salary
ON Employee(department_id, salary);
```

Column order matters. A common follow-up: "If I have an index on `(department_id, salary)`, will a query filtering only by `salary` use it efficiently?" Don't automatically say yes. It depends on the DBMS, the optimizer and the query. Composite indexes work best when queries can use the leading columns.

### Q55. Clustered vs non-clustered index?

This is DBMS-specific, so don't give a universal answer.

A simplified SQL Server-style answer: a clustered index determines the physical or logical ordering of table data around the clustered key, while a non-clustered index is a separate structure containing keys and references to the underlying rows. SQL Server allows at most one clustered index per table.

MySQL/InnoDB behaves differently: the primary key is clustered.

Strong interview move: ask, "Which database are we talking about: SQL Server, MySQL or PostgreSQL?"

---

## 15. Transactions and ACID

### Q56. What is a transaction?

A transaction is a logical unit of work consisting of one or more operations that must be treated as a single unit.

```text
Transfer ₹1000:
  Debit Account A
  Credit Account B
```

Both operations should succeed, or the whole transaction should be rolled back.

### Q57. What is ACID?

- **A: Atomicity.** All operations succeed or none do.
- **C: Consistency.** The database moves from one valid state to another.
- **I: Isolation.** Concurrent transactions don't improperly interfere with each other.
- **D: Durability.** Committed changes survive failures.

### Q58. What is a deadlock?

A deadlock occurs when two or more transactions wait for each other indefinitely, each holding a resource the other needs.

```text
Transaction A: locks Row 1, waits for Row 2
Transaction B: locks Row 2, waits for Row 1
```

Neither can proceed.

### Q59. What are isolation levels?

Common levels:

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE

The general trade-off is **higher concurrency vs stronger isolation**. Defaults and behavior vary by DBMS, so specify the database if asked for implementation details.

### Q60. What is a dirty read?

A dirty read occurs when one transaction reads data written by another transaction before that transaction commits.

```text
Transaction A: salary 100000 → 150000
Transaction B: reads 150000
Transaction A: ROLLBACK
```

B read a value that was never committed.

### Q61. What is a phantom read?

A phantom read occurs when a transaction runs the same query twice and sees additional or missing rows because another transaction inserted or deleted matching rows in between.

This differs from a non-repeatable read, where an existing row's value changes.

---

## 16. Views, Procedures, Triggers, Other Concepts

### Q62. What is a view?

A view is a virtual table based on a SQL query. It usually stores the query definition rather than a separate copy of the data.

```sql
CREATE VIEW high_salary_employees AS
SELECT *
FROM Employee
WHERE salary > 100000;

SELECT * FROM high_salary_employees;
```

### Q63. View vs table?

| Table | View |
|---|---|
| Stores actual data | Usually stores a query definition |
| Physical database object | Virtual or logical representation |
| Can be modified directly, depending on DB rules | Updatability depends on the view |
| Takes storage for data | Usually little storage for data itself |

### Q64. What is a stored procedure?

A named collection of SQL statements stored in the database and executed when needed.

```sql
CALL get_employee_details();
```

Exact syntax differs across MySQL, SQL Server, PostgreSQL and others.

### Q65. What is a trigger?

A database object that automatically executes in response to events such as INSERT, UPDATE or DELETE.

```text
INSERT into employee → Trigger → Insert audit record
```

### Q66. What is a self-referencing foreign key?

A foreign key that references the same table.

```text
Employee: id, name, manager_id
manager_id → Employee.id
```

Useful for employee-manager hierarchies, category trees and organizational structures.

### Q67. What is referential integrity?

Referential integrity ensures that a foreign-key value refers to a valid record in the referenced table.

If `Department.id = 10` exists, then `Employee.department_id = 10` is valid. `Employee.department_id = 999` violates the foreign-key constraint if department 999 doesn't exist.

### Q68. What is denormalization?

Denormalization intentionally introduces some redundancy to improve read performance or simplify queries.

```text
Normalized:   Employee → Department (separate tables)
Denormalized: Employee (id, name, department_id, department_name)
```

Trade-off: **normalization gives less redundancy and better consistency; denormalization can give faster reads but more redundancy.**

### Q69. What is query optimization and EXPLAIN?

Query optimization finds an efficient execution strategy for a query, considering indexes, joins, filtering, statistics and execution plans.

`EXPLAIN` shows how the database plans to execute a query: access paths, indexes, joins and estimated costs.

```sql
EXPLAIN
SELECT * FROM Employee WHERE department_id = 10;
```

It is useful for diagnosing slow queries.

### Q70. What is CASE?

`CASE` performs conditional logic, like `if / elif / else`.

```sql
SELECT
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'High'
        WHEN salary >= 50000  THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM Employee;
```

### Q71. What is a scalar subquery?

A subquery that returns a single value.

```sql
SELECT
    name,
    salary,
    (SELECT AVG(salary) FROM Employee) AS avg_salary
FROM Employee;
```

### Q72. IN vs EXISTS vs JOIN?

- **JOIN:** combine information from tables.
- **EXISTS:** check whether a matching row exists.
- **IN:** test membership in a set of values.

```sql
-- IN
SELECT *
FROM Employee
WHERE department_id IN (SELECT department_id FROM Department);

-- EXISTS
SELECT *
FROM Employee e
WHERE EXISTS (
    SELECT 1
    FROM Department d
    WHERE d.department_id = e.department_id
);
```

Don't memorize "EXISTS is always faster." Optimizers transform these queries, and performance depends on the DBMS and data distribution.

### Q73. What is an anti-join?

An anti-join returns rows from one table that have no match in another.

```sql
-- Using LEFT JOIN
SELECT e.*
FROM Employee e
LEFT JOIN Department d
  ON e.department_id = d.department_id
WHERE d.department_id IS NULL;

-- Using NOT EXISTS
SELECT *
FROM Employee e
WHERE NOT EXISTS (
    SELECT 1
    FROM Department d
    WHERE d.department_id = e.department_id
);
```

### Q74. What is a semi-join?

A semi-join returns rows from one table when a match exists, without returning columns from the matching table. It is usually expressed with `EXISTS`.

### Q75. What happens if you join without a proper condition?

You can accidentally produce a Cartesian product.

```sql
SELECT *
FROM Employee e
JOIN Department d;
```

Depending on the dialect, this may be invalid or produce a Cartesian-style combination. With 100 employees and 10 departments, you get 100 × 10 = 1000 rows.

### Q76. Why can a JOIN unexpectedly create duplicate rows?

Because the relationship is one-to-many or many-to-many.

```text
Customer: 1 Amit
Orders:   Order A, Order B, Order C (all for Amit)

Customer JOIN Orders:
Amit → Order A
Amit → Order B
Amit → Order C
```

Amit appears three times. This isn't a bug; the join correctly represents the relationship.

### Q77. COUNT(*) vs COUNT(1) vs COUNT(column)?

```text
COUNT(*)      → counts rows
COUNT(1)      → commonly also counts rows
COUNT(column) → counts non-NULL values
```

Don't argue that `COUNT(1)` is faster than `COUNT(*)`. Modern optimizers generally treat them the same.

### Q78. CHAR vs VARCHAR?

- `CHAR(n)`: fixed-length string.
- `VARCHAR(n)`: variable-length string up to `n` characters.

Exact storage behavior varies by DBMS.

### Q79. What is pagination?

Retrieving data in chunks rather than all at once.

```sql
SELECT *
FROM Employee
ORDER BY id
LIMIT 10 OFFSET 20;
```

This skips 20 rows and returns the next 10. For very large datasets, keyset (seek) pagination is often preferable to large offsets.

### Q80. What is a surrogate key?

An artificial, generated identifier used as a primary key instead of a business attribute.

```text
employee_id = 101   (instead of using email as the primary key)
```

Common forms: auto-increment integer, UUID.

---

## 17. Practical SQL Query Problems

These are the queries you should be able to write without looking anything up.

### Q81. Find the second-highest salary

**Method 1: MAX with a subquery**

```sql
SELECT MAX(salary)
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

**Method 2: DENSE_RANK**

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employee
) x
WHERE rnk = 2;
```

`DENSE_RANK` is especially useful when duplicate salaries exist.

### Q82. Second-highest salary in each department

```sql
SELECT *
FROM (
    SELECT
        e.*,
        DENSE_RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM Employee e
) x
WHERE rnk = 2;
```

Tests window functions, `PARTITION BY`, ranking and duplicate handling.

### Q83. Find employees earning more than their department average

```sql
SELECT *
FROM Employee e
WHERE salary > (
    SELECT AVG(e2.salary)
    FROM Employee e2
    WHERE e2.department_id = e.department_id
);
```

Tests correlated subqueries.

### Q84. Find duplicate records

```sql
SELECT email, COUNT(*)
FROM Employee
GROUP BY email
HAVING COUNT(*) > 1;
```

### Q85. Find employees who don't have a department

```sql
SELECT e.*
FROM Employee e
LEFT JOIN Department d
  ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

This is the anti-join pattern.

### Q86. Highest salary in each department

```sql
SELECT *
FROM (
    SELECT
        e.*,
        RANK() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rnk
    FROM Employee e
) x
WHERE rnk = 1;
```

Use `RANK()` if you want to keep ties.

### Q87. Top 3 employees in each department

```sql
SELECT *
FROM (
    SELECT
        e.*,
        ROW_NUMBER() OVER (
            PARTITION BY department_id
            ORDER BY salary DESC
        ) AS rn
    FROM Employee e
) x
WHERE rn <= 3;
```

If ties should all be included, use `RANK()` instead.

### Q88. Employees earning above the overall average

```sql
SELECT *
FROM Employee
WHERE salary > (SELECT AVG(salary) FROM Employee);
```

### Q89. Departments with more than 5 employees

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM Employee
GROUP BY department_id
HAVING COUNT(*) > 5;
```

### Q90. Employees with the same salary

```sql
SELECT salary, COUNT(*)
FROM Employee
GROUP BY salary
HAVING COUNT(*) > 1;
```

### Q91. Find the Nth highest salary

```sql
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employee
) x
WHERE rnk = N;
```

### Q92. Why can't we use an aggregate directly in WHERE?

```sql
-- Invalid
SELECT department_id
FROM Employee
WHERE AVG(salary) > 50000;
```

`WHERE` runs before grouping and aggregation. Use:

```sql
SELECT department_id
FROM Employee
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

### Q93. What is the logical order of SQL query execution?

Conceptually:

```text
FROM
JOIN / ON
WHERE
GROUP BY
HAVING
SELECT
DISTINCT
ORDER BY
LIMIT
```

Even though we write `SELECT` first, the database logically processes the clauses in a different order.

---

## 18. Quick Comparison Table

| Operation | Main idea |
|---|---|
| JOIN | Combines related rows horizontally (adds columns) |
| UNION | Combines compatible result sets vertically (adds rows) |
| CROSS JOIN | Produces every possible row combination (multiplies rows) |

---

## 19. Priority Guide

### Tier 1: Must know

- What is SQL?
- DBMS vs RDBMS
- What is a schema?
- Schema vs instance
- Types of JOIN
- INNER vs LEFT JOIN
- JOIN vs UNION
- UNION vs UNION ALL
- Primary key vs foreign key
- Primary key vs UNIQUE
- WHERE vs HAVING
- GROUP BY
- COUNT(*) vs COUNT(column)
- NULL and IS NULL
- DELETE vs TRUNCATE vs DROP
- Normalization: 1NF, 2NF, 3NF
- Subquery vs CTE
- ROW_NUMBER vs RANK vs DENSE_RANK
- LAG vs LEAD

### Tier 2: Very likely follow-ups

- PARTITION BY vs GROUP BY
- PARTITION BY vs ORDER BY
- Self JOIN
- CROSS JOIN
- Correlated subquery
- EXISTS vs IN
- View
- Index
- Composite index
- ACID
- Transaction
- Deadlock
- WHERE vs ON
- CASE
- COALESCE

### Tier 3: Write these without looking anything up

- Second-highest salary
- Second-highest salary per department
- Nth-highest salary
- Highest salary per department
- Top 3 salaries per department
- Employees above company average
- Employees above their department average
- Find duplicate records
- Delete duplicates while keeping one record
- Employees with no department
- Departments with more than N employees
- Customers with no orders
- Running total
- Month-over-month difference
- Consecutive-day problems
- Latest record per user
- First and last transaction per customer
- Rank employees by salary
- Employees whose salary increased

### Interview tip

Interviewers often chain questions:

```text
"What are joins?"
  ↓
"Difference between INNER and LEFT?"
  ↓
"Find employees without departments."
  ↓
"Can you do it without JOIN?"
  ↓
"Use NOT EXISTS."
  ↓
"Find the second-highest salary per department."
  ↓
"What if there are duplicate salaries?"
  ↓
"Use RANK or DENSE_RANK. What's the difference?"
```

Understand the reasoning behind the SQL, not just the query templates.

---

# Part 2: OOP Interview Questions

This section focuses on questions asked after the basic definitions: differences between concepts, code-output questions, tricky follow-ups, Java-specific behavior and design decisions.

The examples use Java because its OOP rules are frequently tested in fresher interviews. The concepts apply to C++ and Python too, but some language-specific rules differ.

## 1. The Four Pillars

### Q1. What is the difference between encapsulation and abstraction?

Encapsulation combines data and methods within a class and controls access to internal state. Abstraction hides implementation details and exposes only the essential operations.

Example: a banking application.

- **Encapsulation:** the account balance is private and users cannot modify it directly.
- **Abstraction:** users call `withdraw(amount)` without knowing how the bank validates and processes the transaction.

```java
class BankAccount {
    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

`private` enforces encapsulation. The public methods provide a simplified interface.

**Likely follow-up:** Can encapsulation exist without abstraction? Yes. They are distinct concepts, although a well-designed class often uses both.

### Q2. Is data hiding the same as encapsulation?

No. Data hiding restricts access to internal data. Encapsulation groups data and the methods operating on it into a single unit. Data hiding is one technique used to achieve encapsulation.

Making a field `private` is data hiding. Organizing behavior around that field with controlled operations is encapsulation.

### Q3. What is inheritance, and when should you avoid it?

Inheritance lets a subclass reuse or specialize the accessible behavior of a parent class. It is appropriate when the subclass genuinely represents a subtype of the parent, satisfying an **IS-A** relationship.

```java
class Dog extends Animal {}
```

Avoid inheritance when the relationship isn't genuinely IS-A, or when the subclass would inherit behavior it cannot correctly support. For example, making `Car` inherit from `Engine` is poor design: a car *has an* engine; it *is not* an engine. Composition fits better.

### Q4. What is polymorphism, and why is it useful?

Polymorphism allows the same interface or method call to produce different behavior depending on the implementation. It lets code work with a common type without depending on every concrete implementation.

```java
interface Payment {
    void pay(double amount);
}

class UpiPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paying through UPI");
    }
}

class CardPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paying through card");
    }
}

Payment p = new UpiPayment();
p.pay(500);

p = new CardPayment();
p.pay(500);
```

The same call, `p.pay(500)`, invokes different implementations depending on the actual object.

**Why interviewers care:** you can add a new payment method without rewriting the code that calls `Payment.pay()`.

---

## 2. Overloading vs Overriding

### Q5. What is the difference between method overloading and method overriding?

| Overloading | Overriding |
|---|---|
| Same method name, different parameter list | Subclass provides a compatible implementation of an inherited instance method |
| Usually resolved at compile time | Instance-method dispatch occurs at runtime |
| Can occur in the same class | Requires inheritance or an equivalent subtype relationship |
| Return type alone cannot distinguish overloads | Return type must be compatible with the overridden method |
| Example: `add(int, int)` and `add(double, double)` | Example: `Animal.sound()` and `Dog.sound()` |

Overloading:

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

Overriding:

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Remember: **overloading changes the parameter list; overriding specializes inherited behavior.**

### Q6. Can we overload a method by changing only its return type?

No. Java does not allow two methods in the same class to differ only by return type, because the compiler cannot select the overload from the return type alone.

```java
// Invalid
int getValue() { return 10; }
double getValue() { return 10.5; }

// Valid
int getValue() { return 10; }
double getValue(double multiplier) { return 10 * multiplier; }
```

### Q7. Can we override a static method?

No. Static methods belong to the class rather than being dynamically dispatched through an object. A subclass can define a static method with the same signature, which is called **method hiding**, not overriding.

```java
class Parent {
    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void show() {
        System.out.println("Child");
    }
}

Parent p = new Child();
p.show();   // Output: Parent
```

The call is resolved using the reference's declared type.

### Q8. Can we override a private method?

No. A private method is not inherited as an accessible method, so a same-named method in the subclass is a separate method, not an override. This is a common trick question because the names look identical.

### Q9. Can we override a final method?

No. `final` prevents a method from being overridden. A `final` class also cannot be subclassed.

```java
class Parent {
    final void display() {
        System.out.println("Hello");
    }
}
// A subclass cannot override display().
```

### Q10. What is dynamic method dispatch?

Dynamic method dispatch is the runtime mechanism by which Java selects the overridden instance method based on the actual object's class rather than the reference type.

```java
Animal a = new Dog();
a.sound();   // Output: Bark
```

The reference type is `Animal`, but the object is a `Dog`, so `Dog.sound()` runs. This is the foundation of runtime polymorphism.

---

## 3. Abstract Classes vs Interfaces

### Q11. What is the difference between an abstract class and an interface?

| Abstract class | Interface |
|---|---|
| Declared using `abstract class` | Declared using `interface` |
| Can maintain instance state | Cannot have instance fields; fields are implicitly `public static final` |
| Can have constructors | Cannot have constructors |
| Can contain abstract and concrete methods | Can have abstract, `default`, `static` and (in modern Java) `private` methods |
| A class can extend only one class | A class can implement multiple interfaces |
| Useful for shared state and behavior | Useful for defining a contract or capability |

```java
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void sound();

    void sleep() {
        System.out.println("Sleeping");
    }
}

interface Flyable {
    void fly();
}
```

`Animal` shares state and behavior with its subclasses. `Flyable` defines a capability that unrelated classes can implement.

### Q12. Can we create an object of an abstract class?

No. You cannot instantiate an abstract class directly, but you can hold a reference of its type pointing to a concrete subclass object.

```java
abstract class Shape {
    abstract void draw();
}

class Circle extends Shape {
    void draw() {
        System.out.println("Drawing circle");
    }
}

Shape s = new Circle();
s.draw();   // Output: Drawing circle
```

### Q13. Can an abstract class have a constructor?

Yes. Its constructor initializes the common state of subclass objects. It runs as part of constructing the subclass object.

### Q14. Can an interface have implemented methods?

Yes. Modern Java interfaces can contain:

- `default` methods with implementations
- `static` methods with implementations
- `private` helper methods

So "all interface methods are abstract" is outdated.

### Q15. Why doesn't Java support multiple inheritance of classes?

Java allows a class to extend only one class, to avoid ambiguity and complexity when multiple parents provide conflicting implementations or state. Java supports multiple inheritance of *type* through interfaces.

```java
class A { void show() {} }
class B { void show() {} }

// Invalid Java:
// class C extends A, B {}
```

A class can instead implement multiple interfaces.

### Q16. What is the diamond problem?

The diamond problem arises when a class inherits the same method through multiple paths and it becomes ambiguous which implementation to use.

Java avoids it for classes. With interfaces that provide conflicting `default` methods, the implementing class must explicitly override the conflicting method and resolve the choice.

---

## 4. Inheritance and Object Relationships

### Q17. What are the types of inheritance?

Conceptual types:

- **Single:** A → B
- **Multilevel:** A → B → C
- **Hierarchical:** A → B and A → C
- **Multiple:** a class inherits from multiple parents
- **Hybrid:** a combination of the above

Java distinction: classes support single, multilevel and hierarchical inheritance. Multiple inheritance of classes is not supported, although a class can implement multiple interfaces.

### Q18. What is the difference between IS-A and HAS-A?

- **IS-A** represents inheritance or subtype relationships: `class Dog extends Animal {}` means a dog is an animal.
- **HAS-A** represents containment or reference: `class Car { Engine engine; }` means a car has an engine.

### Q19. What is composition, and how is it different from aggregation?

Both represent HAS-A relationships.

- **Composition:** strong ownership. The contained object is an integral part of the owner.
  - Example: a `House` and its `Room` objects.
- **Aggregation:** weaker association. The contained object can exist independently.
  - Example: a `Department` associated with `Teacher` objects who can exist without it.

These are conceptual distinctions; Java does not enforce them with special keywords.

### Q20. Inheritance vs composition: which is better?

Neither is universally better. Use inheritance when there is a valid IS-A relationship and subclasses can honor the parent's contract. Prefer composition when you want to combine behavior, reduce coupling or change dependencies independently.

Instead of `Car extends Engine`, give `Car` an `Engine` field. Composition is more flexible because the implementation can be replaced without changing the class hierarchy.

### Q21. What is the Liskov Substitution Principle?

An instance of a subtype should be usable wherever its supertype is expected without breaking the program's correctness.

If a subclass overrides a withdrawal operation but silently refuses valid withdrawals that the parent contract permits, it violates the parent's expected behavior.

The point is not just that a subclass compiles. It must preserve the promises and behavioral expectations of its parent type.

### Q22. Association vs aggregation vs composition?

| Relationship | Meaning | Example |
|---|---|---|
| Association | Objects are related | Doctor and patient |
| Aggregation | Weak whole-part relationship | Team and players |
| Composition | Strong whole-part ownership | Order and its line items |

Useful for class diagrams and OOP design interviews.

---

## 5. Constructors, `this`, `super`, Object Creation

### Q23. What is constructor overloading?

Defining multiple constructors with different parameter lists so an object can be initialized in different ways.

```java
class Student {
    String name;
    int age;

    Student(String name) {
        this.name = name;
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

Student s1 = new Student("Amit");
Student s2 = new Student("Rahul", 21);
```

### Q24. What is the difference between `this` and `super`?

- `this` refers to the current object.
- `super` accesses superclass members and can invoke the superclass constructor.

```java
class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20;

    void show() {
        System.out.println(this.x);   // 20
        System.out.println(super.x);  // 10
    }
}
```

`this(...)` calls another constructor in the same class. `super(...)` calls a superclass constructor. Constructor invocations must follow Java's ordering rules.

### Q25. In what order do constructors execute during inheritance?

```java
class Parent {
    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child");
    }
}

new Child();
// Output:
// Parent
// Child
```

The superclass constructor runs before the subclass constructor body.

### Q26. Can constructors be overridden?

No. Constructors are not inherited as ordinary methods and cannot be overridden. They can be overloaded.

### Q27. Can a constructor be private?

Yes. A private constructor restricts construction from outside the class. It is useful for patterns such as singleton, static factory methods and utility classes.

A private constructor alone does not make a class a correct singleton.

---

## 6. `static`, `final` and Access Modifiers

### Q28. Static vs instance variables?

| Static variable | Instance variable |
|---|---|
| Belongs to the class | Belongs to each object |
| One shared field per class (and class loader) | Each object has its own copy |
| Accessed via the class name by convention | Accessed via an object |
| Useful for shared state | Useful for object-specific state |

```java
class Employee {
    static String company = "ABC";
    String name;
}
```

All employees share `company`, but each has a separate `name`.

### Q29. Can a static method access a non-static variable directly?

No. A static method has no implicit current object (`this`), so it cannot directly access instance variables. It can access them through an explicitly supplied object reference.

### Q30. `final` vs `finally` vs `finalize()`?

- **`final`:** a keyword restricting reassignment, overriding or inheritance, depending on where it is applied.
- **`finally`:** a block in exception handling that normally runs whether or not an exception occurs.
- **`finalize()`:** a deprecated Java mechanism tied to garbage collection. Don't use it for resource management.

```java
final int MAX = 100;   // cannot be reassigned
```

For a reference variable, `final` prevents changing which object the reference points to. It does not make the referenced object immutable.

### Q31. Explain Java access modifiers.

| Modifier | Same class | Same package | Subclass in another package | Unrelated class in another package |
|---|---|---|---|---|
| `public` | Yes | Yes | Yes | Yes |
| `protected` | Yes | Yes | Yes, subject to protected-access rules | No |
| package-private (no modifier) | Yes | Yes | No | No |
| `private` | Yes | No | No | No |

A common mistake is assuming `protected` means "accessible to every class everywhere." It does not.

### Q32. What is an immutable class?

A class whose observable state cannot change after construction.

Typical techniques:

- Make the class `final`, where appropriate.
- Make fields `private final`.
- Initialize state in the constructor.
- Don't expose setters.
- Make defensive copies of mutable inputs and outputs.

`String` is immutable in Java. Immutability makes objects easier to reason about, especially when shared across threads.

---

## 7. Tricky OOP Output Questions

These separate memorized definitions from real understanding.

### Q33. What is the output?

```java
class Parent {
    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    @Override
    void show() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show();
    }
}
```

**Answer:** `Child`. The reference type is `Parent`, but the object is `Child`. Overridden instance methods use runtime dispatch.

### Q34. What is the output?

```java
class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20;
}

Parent p = new Child();
System.out.println(p.x);
```

**Answer:** `10`. Fields are *hidden*, not overridden. Field access is resolved using the reference's declared type. This differs from instance-method overriding.

### Q35. What is the output?

```java
class Test {
    void display(int x) {
        System.out.println("int");
    }

    void display(double x) {
        System.out.println("double");
    }

    public static void main(String[] args) {
        Test t = new Test();
        t.display(10);
        t.display(10.5);
    }
}
```

**Answer:**

```text
int
double
```

The compiler selects the applicable overload from the argument types.

### Q36. Can an overridden method reduce its visibility?

No. If the superclass method is `public`, the override cannot be `protected` or `private`. An override may preserve or widen visibility, but not narrow it.

### Q37. Can an overriding method throw a broader checked exception?

Generally, no. It cannot declare a broader checked exception than the overridden method. It can declare narrower checked exceptions or omit them. Unchecked exceptions are not restricted by this rule.

### Q38. What happens when a superclass constructor calls an overridden method?

A classic trap:

```java
class Parent {
    Parent() {
        show();
    }

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    int x = 10;

    @Override
    void show() {
        System.out.println(x);
    }
}

new Child();   // Output: 0
```

The subclass override is dispatched, but the superclass constructor runs before the subclass's instance field initializers. At that point `x` still has its default value, `0`.

This is why calling overridable methods from constructors is generally discouraged.

---

## 8. `Object` Class, Equality and Copying

### Q39. What is the difference between `==` and `.equals()`?

For reference types, `==` compares whether two references point to the same object. `.equals()` checks logical equality according to the class's implementation. The default `equals()` inherited from `Object` uses reference identity.

```java
String a = new String("hello");
String b = new String("hello");

System.out.println(a == b);        // false
System.out.println(a.equals(b));   // true
```

### Q40. Why should `equals()` and `hashCode()` be overridden together?

If two objects are equal according to `equals()`, they must return the same `hashCode()`. Otherwise hash-based collections such as `HashMap` and `HashSet` can behave incorrectly.

The reverse isn't required: unequal objects may share a hash code.

### Q41. Shallow copy vs deep copy?

- **Shallow copy:** copies the outer object but may share references to nested mutable objects.
- **Deep copy:** also copies the relevant nested mutable objects, so the copy is independent to the intended depth.

Example: copying a `Student` that holds an `Address` reference. With a shallow copy, changing the shared `Address` affects both students. With a proper deep copy, they are separate.

### Q42. What is garbage collection in Java?

Garbage collection automatically reclaims memory occupied by objects that are no longer reachable from live references and GC roots.

**Follow-up: Can we force garbage collection?** No. `System.gc()` is only a request; the JVM is not guaranteed to run the collector immediately.

---

## 9. SOLID Principles and Design

### Q43. What are the SOLID principles?

- **S: Single Responsibility Principle.** A class should have one coherent responsibility and one main reason to change.
  - Example: separate invoice calculation from invoice printing.
- **O: Open/Closed Principle.** Software should be open to extension but closed to unnecessary modification.
  - Example: add a new payment implementation instead of editing a large conditional.
- **L: Liskov Substitution Principle.** Subtypes must preserve the behavioral expectations of their parent type.
- **I: Interface Segregation Principle.** Clients should not be forced to depend on methods they don't use.
  - Example: separate `Printable` and `Scannable` interfaces rather than forcing every device to implement both.
- **D: Dependency Inversion Principle.** High-level logic should depend on abstractions rather than concrete implementations.

### Q44. Tight coupling vs loose coupling?

Tight coupling means one class depends heavily on another's specific implementation. Loose coupling reduces that dependency, often through interfaces and dependency injection.

```java
// Tightly coupled
class OrderService {
    private StripePayment payment = new StripePayment();
}
```

```java
// Loosely coupled
interface PaymentService {
    void pay(double amount);
}

class OrderService {
    private final PaymentService payment;

    OrderService(PaymentService payment) {
        this.payment = payment;
    }
}
```

`OrderService` can now work with any `PaymentService` implementation.

### Q45. What is dependency injection?

Supplying an object with its dependencies from outside, rather than making it create them itself.

The second `OrderService` example uses constructor injection. Benefits include easier testing, replaceable implementations and reduced coupling.

### Q46. What is cohesion?

Cohesion measures how closely related the responsibilities of a class or module are. High cohesion means a focused set of related responsibilities.

A class handling payment validation, email delivery, report generation and database backups has poor cohesion. A class focused on payment processing has better cohesion.

### Q47. Interface vs abstract class in real-world design?

Choose an **interface** when you need a contract that multiple, potentially unrelated classes can implement. Choose an **abstract class** when related subclasses need shared state, common implementation or protected helper methods.

Don't say interfaces are always better. The choice depends on the relationship and design requirements.

### Q48. What is the Open/Closed Principle in practice?

Suppose payment logic looks like this:

```java
if (type.equals("UPI")) {
    // UPI logic
} else if (type.equals("CARD")) {
    // Card logic
}
```

Every new payment type requires editing the same growing conditional. Instead, define a `PaymentService` interface with separate implementations. A factory or dependency-injection mechanism selects the right one.

The goal is to localize change, not to eliminate every conditional from every program.

---

## 10. Design Patterns

### Q49. What is a design pattern?

A reusable approach to a recurring software design problem. It describes a solution structure, not a complete ready-made implementation.

### Q50. What is the Singleton pattern?

A creational pattern that provides one shared instance of a class within a defined scope, with a controlled way to access it.

```java
public class AppConfig {
    private static final AppConfig INSTANCE = new AppConfig();

    private AppConfig() {}

    public static AppConfig getInstance() {
        return INSTANCE;
    }
}
```

This is eager initialization. It is thread-safe under normal Java class-initialization rules, but reflection, serialization and class-loader boundaries can introduce additional considerations.

### Q51. What is the Factory pattern?

The Factory pattern encapsulates object creation so client code doesn't need to know which concrete class to instantiate.

For example, a `PaymentFactory` might return a `UPIPayment` or `CardPayment` based on the requested payment type.

### Q52. What is the Observer pattern?

A one-to-many relationship where observers are notified when a subject's state or events change.

Example: when a stock price changes, all subscribers receive an update. Useful in event-driven systems, notification systems and UI frameworks.

### Q53. What is the Strategy pattern?

It encapsulates interchangeable algorithms behind a common interface, so the algorithm can be chosen or changed independently of the client.

Examples: different payment methods, route-planning algorithms or discount calculations.

---

## 11. Rapid-Fire Follow-ups

| Question | Correct answer |
|---|---|
| Can an abstract class have no abstract methods? | Yes. It still prevents direct instantiation and provides a shared base. |
| Can an interface extend another interface? | Yes, and it can extend multiple interfaces. |
| Can a class implement multiple interfaces? | Yes. |
| Can a class extend multiple classes in Java? | No. |
| Can a final variable be reassigned? | No, once initialized according to the variable's rules. |
| Does `final` make an object immutable? | No. It prevents reassigning the reference, not mutating the referenced object. |
| Can a static method use `this`? | No. |
| Can a subclass access a private parent field directly? | No, not through ordinary direct access. |
| Is every overloaded method an example of runtime polymorphism? | No. Overload selection is resolved at compile time. |
| Can an overriding method return a subtype of the original return type? | Yes, through covariant return types for reference types. |
| Does Java support operator overloading? | Not user-defined operator overloading for ordinary classes. Certain built-in behaviors exist, such as `+` for strings. |
| Is a Java `String` mutable? | No, it is immutable. |
| Can we instantiate an interface directly? | No. |
| Is a constructor inherited? | No. |
| Can an object have multiple references? | Yes. Multiple references can point to the same object. |

---

## 12. OOP Priority Checklist

Core concepts

- [ ] Encapsulation vs abstraction
- [ ] Overloading vs overriding
- [ ] Compile-time vs runtime polymorphism
- [ ] Abstract class vs interface
- [ ] Why Java doesn't support multiple inheritance of classes

Inheritance and object behavior

- [ ] IS-A vs HAS-A
- [ ] Composition vs aggregation
- [ ] Inheritance vs composition
- [ ] Liskov Substitution Principle
- [ ] Static method hiding vs overriding

Java-specific traps

- [ ] `this` vs `super`
- [ ] Constructor execution order
- [ ] Can constructors be overridden?
- [ ] `==` vs `equals()`
- [ ] `equals()` and `hashCode()` contract

Design and practical use

- [ ] SOLID principles
- [ ] Tight vs loose coupling
- [ ] Dependency injection
- [ ] Shallow vs deep copy
- [ ] Singleton and Factory patterns

### How to prepare

For each important concept, be able to:

1. Define it in two or three sentences.
2. Demonstrate it with a short code example.
3. Explain the trade-off or a situation where you'd use it.

For example, if asked about polymorphism, don't stop at the definition. Predict the output of `Parent p = new Child()`, distinguish instance methods from fields and static methods, and explain why the distinction matters.

Recommended order: master the four pillars, overloading vs overriding, abstract class vs interface, constructor behavior and the tricky output questions. Then move to SOLID and design patterns.

---

# Part 3: Software Engineering Interview Questions

This section covers SDLC, Agile, requirements, testing, debugging, code quality, design principles, version control, deployment and practical scenarios.

It differs from OOP: OOP focuses on object-oriented programming concepts, while software engineering focuses on how software is specified, designed, built, tested, maintained and delivered reliably.

## 1. Software Engineering Fundamentals

### Q1. What is software engineering?

The systematic application of engineering principles to software development. It covers requirements, design, implementation, testing, deployment and maintenance to produce reliable, maintainable software.

Unlike simply writing code, software engineering also considers scalability, cost, quality, security and long-term maintenance.

### Q2. What are the characteristics of good software?

- **Correctness:** meets its requirements.
- **Reliability:** works consistently under expected conditions.
- **Maintainability:** can be modified without excessive effort.
- **Efficiency:** uses CPU, memory and network resources effectively.
- **Usability:** easy for intended users to operate.
- **Security:** protects data and prevents unauthorized actions.
- **Scalability:** handles growth in users, requests or data.

**Follow-up:** Is software that works correctly necessarily good? No. It may still be slow, insecure, difficult to maintain or unsuitable for users.

### Q3. Bug, defect and failure: what's the difference?

- **Error:** a human mistake, such as misunderstanding a requirement.
- **Defect/bug:** a flaw in software or its artifacts.
- **Failure:** the software behaves incorrectly when executed.

Example: a developer writes code that applies the tax rate incorrectly. The coding mistake introduces a defect; when an invoice shows the wrong total, the user experiences a failure.

Terminology varies between organizations, so explain the distinction rather than arguing over labels.

### Q4. What is technical debt?

The future cost of maintaining or changing software because of shortcuts, design compromises or accumulated implementation decisions.

Example: duplicating validation logic in five places saves time today, but every future change must be applied consistently in all five.

Technical debt can be acceptable when the trade-off is deliberate, documented and managed. Untracked debt compounds and slows future development.

### Q5. Functional vs non-functional requirements?

| Functional requirements | Non-functional requirements |
|---|---|
| Describe what the system must do | Describe qualities or constraints of the system |
| Login, registration, payment | Security, performance, availability |
| Usually tested through functional behavior | Often measured through performance, security and reliability testing |

- **Functional:** users can upload a PDF.
- **Non-functional:** a 10 MB PDF must upload within a specified time under defined network and load conditions.

Non-functional requirements should be measurable whenever possible.

---

## 2. SDLC and Development Models

### Q6. What is SDLC?

Software Development Life Cycle: the process followed to build and maintain software.

| Phase | Purpose |
|---|---|
| 1. Planning | Define objectives, feasibility, budget and risks |
| 2. Requirements | Gather and document what users need |
| 3. Design | Define architecture, database, APIs and components |
| 4. Implementation | Write and integrate the code |
| 5. Testing | Check functionality, performance, security and defects |
| 6. Deployment | Release the software to its target environment |
| 7. Maintenance | Fix bugs, monitor operation and improve features |

Real teams may overlap these phases or repeat them iteratively.

### Q7. What are the different SDLC models?

- Waterfall
- Agile
- V-Model
- Spiral
- Iterative
- Incremental
- Prototype

Interviewers expect you to explain the differences and when each model fits.

### Q8. What is the Waterfall model?

A sequential model in which phases are generally completed in order, each providing inputs to the next.

```text
Requirements → Design → Development → Testing → Deployment → Maintenance
```

**Advantages:** clear documentation and milestones; easier planning when requirements are stable; suits projects needing formal approvals.

**Disadvantages:** changes are expensive late in the process; feedback arrives late; working software is typically delivered later than in incremental approaches.

### Q9. What is Agile?

An approach to software development that emphasizes iterative delivery, collaboration, customer feedback and responding to change.

Instead of waiting for the entire application to be finished, a team delivers working increments, collects feedback and adjusts its plans.

Agile is not a single model. Scrum and Kanban are approaches used by Agile teams.

### Q10. Agile vs Waterfall?

| Agile | Waterfall |
|---|---|
| Iterative and incremental | Generally sequential |
| Requirements can evolve during development | Best suited to relatively stable requirements |
| Frequent feedback and delivery | Feedback may arrive later |
| Planning is revised regularly | More upfront planning |
| Testing happens continuously | Testing often follows implementation |

**Interview trap:** Agile doesn't mean "no documentation" or "no planning." It means adapting the level of planning and documentation to the project's needs.

### Q11. What is the V-Model?

The V-Model pairs each development phase with a corresponding testing activity and emphasizes planning tests alongside requirements and design.

| Development activity | Corresponding testing activity |
|---|---|
| Requirements analysis | Acceptance testing |
| System design | System testing |
| Architecture design | Integration testing |
| Module design | Unit testing |
| Coding | Implementation |

The exact mapping varies. The key idea is early test planning and traceability between requirements and tests.

### Q12. What is the Spiral model?

A model that develops software through repeated cycles emphasizing risk identification and reduction. Each cycle typically includes planning, risk analysis, engineering and evaluation.

It suits large, complex or high-risk projects. Its risk-analysis overhead may be unnecessary for a small, straightforward application.

### Q13. Iterative vs incremental development?

- **Iterative:** improve an existing solution through repeated cycles.
- **Incremental:** add new functional pieces in stages.

Example: first release basic search, then add filtering, then improve ranking. That process is both incremental and iterative.

---

## 3. Agile, Scrum and Project Management

### Q14. What is Scrum?

An Agile framework in which a team works in short, fixed-length Sprints to create and improve a usable product increment.

Scrum defines three accountabilities:

- **Product Owner:** maximizes product value and orders the Product Backlog.
- **Scrum Master:** helps establish Scrum and improve team effectiveness.
- **Developers:** build the usable increment during the Sprint.

Events include Sprint Planning, Daily Scrum, Sprint Review and Sprint Retrospective.

### Q15. What is a Sprint?

A fixed-length period of one month or less during which the team works toward a Sprint Goal and produces a usable increment. Many teams use one- or two-week Sprints.

A Sprint is not just a deadline for a list of unrelated tasks; it should have a clear objective.

### Q16. Sprint Review vs Sprint Retrospective?

| Sprint Review | Sprint Retrospective |
|---|---|
| Focuses on the product and its outcome | Focuses on improving how the team works |
| Reviews the increment with stakeholders | Examines collaboration, process and quality |
| May result in Product Backlog changes | Produces improvement actions |

Memory trick: **review the product; retrospect the process.**

### Q17. What is a user story?

A description of a capability from the perspective of the person who needs it.

```text
As a [type of user], I want [capability], so that [benefit].
```

Example: *As a customer, I want to reset my password so that I can regain access to my account.*

Each story should have acceptance criteria defining when it is complete.

### Q18. What is a Product Backlog?

An ordered list of work that may improve the product: features, defects, technical improvements and research. It evolves as the team learns about user needs and business priorities.

### Q19. Agile vs Scrum?

Agile is a set of values and principles for adaptive development. Scrum is a specific framework that implements those principles through defined roles, events and artifacts.

A team can use Agile principles without Scrum, for example with Kanban.

### Q20. What is a burndown chart?

A chart that tracks remaining work over time against a target. It helps a team see whether progress is on plan, but it doesn't prove product quality or business value.

---

## 4. Software Testing and Quality Assurance

Developers are expected to test their own code, not just hand it to QA. Interviews emphasize test levels, verification and validation, regression testing and risk-based test design.

### Q21. What is software testing?

Evaluating software to find defects, check whether requirements are met and assess whether the system behaves as users need.

Testing may be manual or automated, and it can include static analysis as well as executing the software.

### Q22. Verification vs validation?

| Verification | Validation |
|---|---|
| Are we building the product right? | Are we building the right product? |
| Checks conformance to specifications | Checks whether user needs are met |
| Includes reviews, inspections and static analysis | Includes execution-based testing and user acceptance testing |
| Can detect problems before execution | Evaluates actual behavior |

Example: a shopping application follows its written specification perfectly, but the specification omitted a required refund workflow. It may pass verification against that specification but fail validation against real customer needs.

### Q23. What are the levels of testing?

1. **Unit testing:** tests a small unit, such as a function or class, in isolation where practical. Example: a discount-calculation function.
2. **Integration testing:** checks interactions between components. Example: an order service correctly writes an order to the database.
3. **System testing:** tests the integrated system against its requirements. Example: an entire checkout workflow.
4. **Acceptance testing:** evaluates whether the system satisfies acceptance criteria and business or user needs.

### Q24. Unit vs integration vs system testing?

Consider a login application:

- **Unit test:** does the password-validation function reject an invalid password?
- **Integration test:** does the login service correctly communicate with the user database?
- **System test:** can a user log in, navigate the application and log out correctly?

The difference is mainly the scope and interactions being tested.

### Q25. What is regression testing?

Checking whether changes have unintentionally broken existing functionality.

Example: changing the payment module should not break order creation or order history. Automated regression tests are especially valuable in frequently changing projects.

### Q26. Smoke testing vs sanity testing?

- **Smoke testing:** a broad, shallow check that a new build is stable enough for detailed testing.
- **Sanity testing:** a narrower check of a particular change or affected area.

Teams use these terms somewhat differently, so explain the purpose rather than relying only on the labels.

### Q27. Black-box vs white-box testing?

- **Black-box:** tests externally observable behavior without knowledge of the implementation. Example: submit valid and invalid credentials and check the responses.
- **White-box:** uses knowledge of internal code, branches and logic. Example: testing each branch of a password-validation function.
- **Gray-box:** uses partial knowledge of the implementation.

### Q28. Positive vs negative testing?

- **Positive:** checks valid inputs and expected workflows.
- **Negative:** checks invalid inputs, failure conditions and unexpected situations.

For a registration form: positive tests use valid email and password combinations. Negative tests use malformed emails, missing fields, oversized input and duplicate registrations.

### Q29. What is boundary-value analysis?

Testing values at or around the edges of an allowed range, where defects commonly occur.

For an age field accepting 18 through 60, test: `17, 18, 19, 59, 60, 61`. Testing only a typical value like 30 misses many boundary defects.

### Q30. What is equivalence partitioning?

Dividing inputs into groups expected to behave similarly, so representative values can be tested instead of every possible value.

For an age field accepting 18–60:

- Below 18: invalid group
- 18–60: valid group
- Above 60: invalid group

Combine this with boundary-value analysis for stronger coverage.

### Q31. Severity vs priority in bug tracking?

| Severity | Priority |
|---|---|
| How badly the defect affects the system | How urgently the defect should be fixed |
| Primarily technical or user impact | Primarily scheduling and business urgency |

Example: a minor spelling error on a high-visibility launch page may have low severity but high priority. A severe crash in a rarely used internal feature may have high severity but lower immediate priority, depending on business impact and workarounds.

### Q32. Test case, test scenario and test plan?

- **Test scenario:** a high-level behavior to test, such as password reset.
- **Test case:** specific preconditions, inputs, actions and expected results.
- **Test plan:** the overall scope, approach, resources, responsibilities, schedule and risks of testing.

Example test case: enter a registered email, request a password reset, and verify the confirmation message and reset workflow.

---

## 5. Requirements and Software Design

### Q33. What is a functional requirement?

A behavior or capability the system must provide.

Example: users must be able to download their invoices as PDFs.

### Q34. What is a non-functional requirement?

A quality attribute or constraint.

Example: the invoice-download endpoint must respond within 500 ms at the specified percentile under a defined workload.

Avoid vague statements like "the application should be fast." Define the conditions and metric.

### Q35. What is feasibility analysis?

Determining whether a proposed project is practical and worthwhile.

Typical dimensions:

- Technical
- Economic
- Operational
- Legal and regulatory
- Schedule

A project may be technically possible but economically unjustified if its maintenance cost exceeds its expected benefit.

### Q36. What is modularity?

Dividing a system into components with clear responsibilities and interfaces, so each can be developed, tested and maintained more independently.

Example: an e-commerce application might separate authentication, catalog management, order processing and payment processing.

Good modularity reduces unnecessary dependencies, but excessive fragmentation adds complexity too.

### Q37. Cohesion vs coupling?

| Cohesion | Coupling |
|---|---|
| How closely related a module's responsibilities are | How strongly modules depend on one another |
| Prefer high cohesion | Generally prefer low coupling |

A payment module that handles only payment-related behavior has high cohesion. If it directly depends on the internal database structures of every other module, it has high coupling.

The goal is not zero coupling, since components must communicate. The goal is to make dependencies explicit and manageable.

### Q38. What is a software design document?

A document describing how a system will meet its requirements. Depending on the project, it may include:

- Architecture and component diagrams
- Database schemas
- API contracts
- Data flows
- Security considerations
- Error-handling strategy
- Performance requirements
- Deployment and operational considerations

### Q39. What is a design pattern?

A reusable approach to a recurring design problem. Examples:

- **Factory:** encapsulates object creation.
- **Strategy:** allows interchangeable algorithms.
- **Observer:** notifies interested objects of events or state changes.
- **Singleton:** provides a controlled shared instance within a defined scope.

A pattern is not automatically an improvement. Use it when it simplifies the design, not when it adds unnecessary abstraction.

### Q40. What is refactoring?

Changing the internal structure of code without intentionally changing its externally observable behavior.

Examples: extracting a large method into smaller ones, removing duplication, simplifying dependencies. Tests help verify that behavior is preserved.

Refactoring improves internal structure; a feature changes or extends externally observable functionality.

---

## 6. Debugging, Code Review and Version Control

### Q41. How would you debug a software issue?

A repeatable process:

1. Reproduce the issue reliably.
2. Collect error messages, logs and relevant inputs.
3. Narrow down the failing component.
4. Form a hypothesis about the cause.
5. Test the hypothesis with a debugger, logs or a minimal reproduction.
6. Fix the root cause.
7. Add a regression test.
8. Verify the fix and monitor for recurrence.

Avoid random code changes. A reproducible failure and a test for the root cause are much stronger than a fix that merely appears to work.

### Q42. What is a code review?

Examining code changes before integration to identify defects, design problems, security risks and maintainability issues.

A good review checks:

- Correctness and edge cases
- Readability and maintainability
- Test coverage
- Security and error handling
- Consistency with project conventions

Reviews shouldn't become debates about formatting when automated tools can handle it.

### Q43. What is version control?

A system that tracks changes to files over time, enabling collaboration, comparison of versions and recovery of earlier states. Git is the most common distributed version-control system.

### Q44. Git vs GitHub?

- **Git:** a version-control tool.
- **GitHub:** a hosting and collaboration platform for Git repositories, pull requests, issues and automation.

Git can be used without GitHub.

### Q45. Merge vs rebase?

- **Merge** combines histories, usually preserving branch structure through a merge commit.
- **Rebase** replays commits onto a new base, creating a more linear history but changing the commit IDs of the replayed commits.

Use merge when preserving branch history matters. Rebase is useful for cleaning up local work, but rewriting shared history can disrupt collaborators.

### Q46. CI vs Continuous Delivery vs Continuous Deployment?

- **Continuous Integration:** developers frequently integrate changes, with automated builds and tests.
- **Continuous Delivery:** changes are kept in a releasable state; production release may require approval.
- **Continuous Deployment:** every change that passes the pipeline is automatically deployed to production.

A typical pipeline:

```text
Code commit
  ↓
Build
  ↓
Automated tests
  ↓
Security and quality checks
  ↓
Package artifact
  ↓
Deploy to staging
  ↓
Release to production
  ↓
Monitor
```

The exact pipeline depends on the project. CI/CD reduces manual effort and catches defects earlier, but it doesn't guarantee bug-free software.

---

## 7. Scenario-Based Questions

These test engineering judgment rather than memorized definitions.

### Q47. A bug appears immediately after deployment. What would you do?

First, assess impact and severity. If users or critical data are at risk, follow the incident procedure and consider rolling back or disabling the affected feature. Then inspect logs, metrics and recent changes, reproduce the issue, identify the root cause, test the fix and deploy it safely. Finally, add regression coverage and document preventive actions.

The priority is restoring safe service, not proving you can fix it alone.

### Q48. A feature works on your machine but fails in production. Why?

Possible causes:

- Different environment variables or configuration
- Dependency or runtime-version differences
- Database schema or migration differences
- Permissions and credentials
- Network restrictions
- Different input data or load
- Race conditions or concurrency problems

Compare the environments, inspect logs and reproduce production conditions rather than immediately rewriting code.

### Q49. Your manager wants the feature shipped tomorrow, but testing is incomplete. What do you do?

Identify which tests remain and the risks they cover, then communicate the trade-offs clearly. Prioritize critical workflows, security and data-integrity checks. If the remaining risk is unacceptable, recommend delaying the release or using a controlled rollout or feature flag. Don't silently claim the feature is fully tested.

This shows that deadlines matter, but so do risk and transparency.

### Q50. Requirements change halfway through development. What do you do?

Clarify the new requirement and why it changed. Assess its effect on scope, design, testing and delivery, and discuss trade-offs with stakeholders. Then update the backlog, acceptance criteria and plan.

In Agile, change can be accommodated, but not every change can be absorbed without affecting scope or delivery.

### Q51. How do you handle a design disagreement with another developer?

Compare alternatives against requirements, correctness, maintainability, performance and project constraints. If needed, build a small prototype or benchmark. Document the reasoning and follow the team's decision-making process. Treat it as a technical question, not a personal one.

### Q52. How do you decide what to test first when time is limited?

Prioritize by risk:

- Critical business workflows
- Security and authorization
- Data integrity and irreversible operations
- Common user paths
- Recent changes and areas with a history of defects
- Edge cases and lower-impact features

The goal is to maximize defect detection and reduce risk, not to maximize the number of test cases executed.

### Q53. What is a rollback, and when would you use it?

A rollback restores a previously working version or reverses a problematic change. It's useful when a release causes severe failures and a safe rollback exists.

Database migrations and irreversible data changes can make rollback difficult. Plan releases with recovery paths, backward compatibility and data-integrity in mind.

---

## 8. Additional Concepts Worth Knowing

| Question | Key answer |
|---|---|
| What is maintainability? | How easily software can be understood, modified and tested. |
| What is scalability? | Ability to handle increasing workload while meeting defined requirements. |
| What is reliability? | Ability to perform required functions consistently under specified conditions. |
| What is availability? | Proportion of time a system is operational and accessible as defined. |
| What is a prototype? | An early model used to explore or validate requirements and design ideas. |
| What is a proof of concept (PoC)? | A small experiment to test whether an idea or technology is feasible. |
| What is an MVP? | A minimal product version that enables meaningful learning from intended users. |
| What is root-cause analysis? | Identifying the underlying reason a problem occurred, not just treating symptoms. |
| What is a flaky test? | A test that gives inconsistent results without a meaningful change to the software. |
| What is a release candidate? | A build considered potentially ready for release after validation. |
| What is a feature flag? | A mechanism to enable or disable functionality through configuration instead of a new deployment. |
| What is observability? | Inferring a system's internal state from outputs such as logs, metrics and traces. |
| What is a postmortem? | A structured review of an incident to understand causes and identify improvements. |
| What is code coverage? | A measure of which code was exercised by tests. It does not prove correctness. |
| What is configuration management? | Managing and tracking software configuration, environments and changes consistently. |

---

## 9. Software Engineering Priority Checklist

- [ ] SDLC and its phases
- [ ] Waterfall vs Agile
- [ ] Scrum, Sprints and user stories
- [ ] Functional vs non-functional requirements
- [ ] Verification vs validation
- [ ] Unit vs integration vs system testing
- [ ] Regression vs smoke testing
- [ ] Black-box vs white-box testing
- [ ] Boundary-value analysis
- [ ] Severity vs priority
- [ ] Technical debt
- [ ] Cohesion vs coupling
- [ ] Modularity and maintainability
- [ ] Refactoring vs adding a feature
- [ ] Debugging methodology
- [ ] Code review
- [ ] Git vs GitHub
- [ ] Merge vs rebase
- [ ] CI vs continuous delivery vs continuous deployment
- [ ] Handling a production bug

### Final advice

Don't stop at definitions. Be able to explain when to use a technique, its limitations, and how you'd handle a realistic failure. A candidate who can reason through a production bug or choose appropriate tests shows more engineering judgment than someone who can recite every SDLC model from memory.

---

# Final Notes

- **SQL:** Practice writing queries by hand, especially window functions, anti-joins and "Nth highest" patterns. Explain the reasoning behind each query.
- **OOP:** Be able to predict output, explain overloading vs overriding, and justify inheritance vs composition with examples.
- **Software Engineering:** Focus on reasoning through scenarios: production bugs, deadlines, requirement changes and testing trade-offs.

Across all three areas, interviewers reward explanations that show trade-offs, not memorized definitions alone.
