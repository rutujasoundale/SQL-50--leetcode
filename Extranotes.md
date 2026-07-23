1.GROUP BY
`GROUP BY` is one of the **most important SQL interview topics**. It is used to **group rows that have the same value in one or more columns**, usually so you can perform aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MIN()`, or `MAX()`.

---

# Syntax

```sql
SELECT column_name, AGGREGATE_FUNCTION(column)
FROM table_name
GROUP BY column_name;
```

---

# Example Table: Employees

| emp_id | name    | department | salary |
| ------ | ------- | ---------- | -----: |
| 1      | Alice   | HR         |  40000 |
| 2      | Bob     | IT         |  60000 |
| 3      | Charlie | IT         |  70000 |
| 4      | David   | HR         |  50000 |
| 5      | Eva     | Sales      |  55000 |

---

# Example 1: Count employees in each department

```sql
SELECT department, COUNT(*)
FROM Employees
GROUP BY department;
```

Output

| department | COUNT(*) |
| ---------- | -------- |
| HR         | 2        |
| IT         | 2        |
| Sales      | 1        |

### What happens?

Without `GROUP BY`

```
HR
HR
IT
IT
Sales
```

After `GROUP BY`

```
HR
IT
Sales
```

Then `COUNT(*)` counts the rows in each group.

---

# Example 2: Total salary department-wise

```sql
SELECT department, SUM(salary)
FROM Employees
GROUP BY department;
```

Output

| department |    SUM |
| ---------- | -----: |
| HR         |  90000 |
| IT         | 130000 |
| Sales      |  55000 |

---

# Example 3: Average salary

```sql
SELECT department, AVG(salary)
FROM Employees
GROUP BY department;
```

Output

| department |   AVG |
| ---------- | ----: |
| HR         | 45000 |
| IT         | 65000 |
| Sales      | 55000 |

---

# Example 4: Maximum salary

```sql
SELECT department, MAX(salary)
FROM Employees
GROUP BY department;
```

Output

| department |   MAX |
| ---------- | ----: |
| HR         | 50000 |
| IT         | 70000 |
| Sales      | 55000 |

---

# Example 5: Minimum salary

```sql
SELECT department, MIN(salary)
FROM Employees
GROUP BY department;
```

Output

| department |   MIN |
| ---------- | ----: |
| HR         | 40000 |
| IT         | 60000 |
| Sales      | 55000 |

---

# Group by Multiple Columns

Table

| name | department | city   |
| ---- | ---------- | ------ |
| A    | IT         | Pune   |
| B    | IT         | Pune   |
| C    | IT         | Mumbai |
| D    | HR         | Pune   |

Query

```sql
SELECT department, city, COUNT(*)
FROM Employees
GROUP BY department, city;
```

Output

| department | city   | COUNT |
| ---------- | ------ | ----- |
| IT         | Pune   | 2     |
| IT         | Mumbai | 1     |
| HR         | Pune   | 1     |

The rows are grouped by the **combination** of department and city.

---

# Why GROUP BY is Needed

Suppose you write

```sql
SELECT department, salary
FROM Employees;
```

Output

```
HR     40000
IT     60000
IT     70000
HR     50000
Sales  55000
```

If you write

```sql
SELECT department, SUM(salary)
FROM Employees;
```

❌ This is invalid because SQL doesn't know **which department** should be shown for the single total salary.

You must tell SQL to group the rows first:

```sql
SELECT department, SUM(salary)
FROM Employees
GROUP BY department;
```

---

# GROUP BY + HAVING

`WHERE` filters rows **before** grouping.

`HAVING` filters groups **after** grouping.

Example

```sql
SELECT department, COUNT(*)
FROM Employees
GROUP BY department
HAVING COUNT(*) > 1;
```

Output

| department | COUNT |
| ---------- | ----- |
| HR         | 2     |
| IT         | 2     |

Sales is not shown because it has only one employee.

---

# WHERE vs HAVING

Table

| department | salary |
| ---------- | -----: |
| HR         |  40000 |
| HR         |  50000 |
| IT         |  60000 |
| IT         |  70000 |

### WHERE

```sql
SELECT department, COUNT(*)
FROM Employees
WHERE salary > 45000
GROUP BY department;
```

SQL first removes salaries ≤ 45000, then groups the remaining rows.

### HAVING

```sql
SELECT department, COUNT(*)
FROM Employees
GROUP BY department
HAVING COUNT(*) >= 2;
```

SQL first creates groups, then filters the groups.

---

# Execution Order

SQL processes the query in this order:

```
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
SELECT
↓
ORDER BY
```

---

# Common Interview Questions

### 1. Count employees in each department

```sql
SELECT department, COUNT(*)
FROM Employees
GROUP BY department;
```

---

### 2. Find average salary of each department

```sql
SELECT department, AVG(salary)
FROM Employees
GROUP BY department;
```

---

### 3. Departments having more than 5 employees

```sql
SELECT department, COUNT(*)
FROM Employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

### 4. Highest salary in each department

```sql
SELECT department, MAX(salary)
FROM Employees
GROUP BY department;
```

---

### 5. Sort departments by total salary

```sql
SELECT department, SUM(salary) AS total_salary
FROM Employees
GROUP BY department
ORDER BY total_salary DESC;
```

---

# Important Interview Rules

* Every column in the `SELECT` list must either:

  * be included in the `GROUP BY`, or
  * be wrapped in an aggregate function (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`).

  ✔️ Correct:

  ```sql
  SELECT department, COUNT(*)
  FROM Employees
  GROUP BY department;
  ```

  ❌ Incorrect:

  ```sql
  SELECT department, salary
  FROM Employees
  GROUP BY department;
  ```

  `salary` is neither grouped nor aggregated, so this is invalid in standard SQL.

---

## Quick Revision

* `GROUP BY` groups rows with the same values.
* Used with aggregate functions (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`).
* `WHERE` filters rows **before** grouping.
* `HAVING` filters groups **after** grouping.
* `ORDER BY` sorts the final result.
* You can group by one or multiple columns.

For placement interviews (Infosys, TCS, Accenture, Capgemini, Cognizant, etc.), `GROUP BY` along with `HAVING` is one of the most frequently asked SQL topics, often in combination with joins and aggregate functions.
### 2.Time and space complexity in sql
This is a very common interview question. Unlike DSA, **SQL doesn't have a fixed Time Complexity (TC) and Space Complexity (SC)** because SQL is a **declarative language**—you tell the database *what* you want, and the database decides *how* to execute it.

Instead of analyzing loops, you think about:

* Number of rows scanned
* Index usage
* Joins
* Sorting
* Grouping

---

# 1. Full Table Scan

```sql
SELECT * FROM Employees;
```

Suppose there are **N rows**.

The database reads every row.

**Time Complexity:** **O(N)**

**Space Complexity:** **O(1)** (ignoring the result set)

---

# 2. WHERE without Index

```sql
SELECT *
FROM Employees
WHERE salary = 50000;
```

No index exists.

Database checks every row.

```
10000
20000
50000 ← found
40000
...
```

**TC = O(N)**

---

# 3. WHERE with Index

Suppose salary is indexed.

```sql
SELECT *
FROM Employees
WHERE salary = 50000;
```

Database uses a B-tree index.

Searching in a B-tree is approximately:

**TC = O(log N)**

This is why indexes improve performance.

---

# 4. ORDER BY

```sql
SELECT *
FROM Employees
ORDER BY salary;
```

Sorting N rows.

Most databases use efficient sorting algorithms.

**TC = O(N log N)**

---

# 5. GROUP BY

```sql
SELECT department, COUNT(*)
FROM Employees
GROUP BY department;
```

The database scans all rows.

Then groups them (often using hashing or sorting).

Typical complexity:

* Scan → O(N)
* Grouping → around O(N) with hashing, or O(N log N) if sorting is required.

Interview answer:

**Usually O(N)** (hash aggregation) or **O(N log N)** (sort aggregation).

---

# 6. JOIN

Suppose

Employees

```
1000 rows
```

Departments

```
100 rows
```

## Nested Loop Join

For every employee

check every department

```
Employee1 → 100 rows

Employee2 → 100 rows

...
```

TC

```
O(M × N)
```

where

M = Employees

N = Departments

---

## Indexed Join

If department_id is indexed

Search becomes

```
Employee
↓

Binary Search
```

Complexity

```
O(M log N)
```

---

## Hash Join

Many databases use Hash Join.

Complexity

```
O(M + N)
```

This is much faster.

---

# 7. COUNT(*)

```sql
SELECT COUNT(*)
FROM Employees;
```

Reads every row.

```
O(N)
```

---

# 8. MAX()

```sql
SELECT MAX(salary)
FROM Employees;
```

Without index

```
O(N)
```

With descending index

Database can often return the first index entry.

```
O(log N)
```

(or even close to constant time depending on the database implementation).

---

# 9. LIMIT

```sql
SELECT *
FROM Employees
LIMIT 10;
```

If no sorting

Database stops after reading 10 rows.

```
O(10)
```

≈

```
O(1)
```

If

```sql
ORDER BY salary
LIMIT 10;
```

Sorting may still dominate.

---

# 10. DISTINCT

```sql
SELECT DISTINCT department
FROM Employees;
```

Needs duplicate removal.

Usually

```
O(N log N)
```

or

```
O(N)
```

using hashing.

---

# SQL Interview Cheat Sheet

| Query            | Time Complexity    |
| ---------------- | ------------------ |
| SELECT *         | O(N)               |
| WHERE (no index) | O(N)               |
| WHERE (indexed)  | O(log N)           |
| COUNT()          | O(N)               |
| SUM()            | O(N)               |
| AVG()            | O(N)               |
| MAX()            | O(N)               |
| MIN()            | O(N)               |
| ORDER BY         | O(N log N)         |
| GROUP BY         | O(N) or O(N log N) |
| DISTINCT         | O(N) or O(N log N) |
| Nested Loop JOIN | O(M × N)           |
| Indexed JOIN     | O(M log N)         |
| Hash JOIN        | O(M + N)           |

---

## How to Think About SQL Complexity

When you see a SQL query, ask yourself these questions:

1. **How many rows does the database need to read?**

   * All rows → O(N)
   * Few rows using an index → O(log N)

2. **Does it need to sort?**

   * Yes → usually O(N log N)

3. **Does it need to group or remove duplicates?**

   * Hashing → around O(N)
   * Sorting → O(N log N)

4. **Does it join multiple tables?**

   * No index → potentially O(M × N)
   * Good indexes/hash join → much faster

### Interview Tip

In interviews, it's generally acceptable to answer with **expected complexity assuming common database implementations** (like B-tree indexes and hash joins), while mentioning that the **actual execution depends on the database's query optimizer, indexes, table statistics, and execution plan**. This shows a practical understanding of how SQL performance works.

