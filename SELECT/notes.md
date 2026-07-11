`SELECT` is the **most important SQL statement** in interviews. Nearly every SQL interview starts with it and then builds into filtering, sorting, grouping, joins, and subqueries.

---

# SELECT Statement

### Syntax

```sql
SELECT column1, column2
FROM table_name;
```

or

```sql
SELECT *
FROM table_name;
```

`*` means **all columns**.

Example:

Employee

| id | name  | salary |
| -- | ----- | ------ |
| 1  | Alice | 50000  |
| 2  | Bob   | 60000  |

Query

```sql
SELECT name, salary
FROM Employee;
```

Output

| name  | salary |
| ----- | ------ |
| Alice | 50000  |
| Bob   | 60000  |

---

# SELECT *

```sql
SELECT *
FROM Employee;
```

Returns every column.

**Interview Question**

**Q:** Is `SELECT *` good practice?

**Answer:**

* Good for exploration.
* Avoid in production.
* Fetches unnecessary data.
* Slower on large tables.
* Makes code harder to maintain.

---

# DISTINCT

Removes duplicate values.

```sql
SELECT DISTINCT department
FROM Employee;
```

Output

```
HR
IT
Sales
```

---

# Column Alias

Rename output column.

```sql
SELECT salary AS EmployeeSalary
FROM Employee;
```

or

```sql
SELECT salary EmployeeSalary
FROM Employee;
```

---

# Arithmetic Operations

```sql
SELECT salary,
salary*12 AS AnnualSalary
FROM Employee;
```

---

# WHERE Clause

Filters rows.

```sql
SELECT *
FROM Employee
WHERE salary > 50000;
```

Execution order:

```
FROM
↓

WHERE
↓

SELECT
```

---

# Comparison Operators

| Operator | Meaning          |
| -------- | ---------------- |
| =        | Equal            |
| >        | Greater          |
| <        | Less             |
| >=       | Greater or equal |
| <=       | Less or equal    |
| <> or != | Not equal        |

Example

```sql
SELECT *
FROM Employee
WHERE salary >= 50000;
```

---

# Logical Operators

## AND

```sql
SELECT *
FROM Employee
WHERE department='IT'
AND salary>50000;
```

---

## OR

```sql
SELECT *
FROM Employee
WHERE department='IT'
OR department='HR';
```

---

## NOT

```sql
SELECT *
FROM Employee
WHERE NOT department='HR';
```

---

# BETWEEN

Inclusive.

```sql
SELECT *
FROM Employee
WHERE salary BETWEEN 40000 AND 70000;
```

Equivalent to

```sql
salary>=40000
AND salary<=70000
```

---

# IN

Instead of multiple ORs.

```sql
SELECT *
FROM Employee
WHERE department IN ('IT','HR');
```

---

# LIKE

Pattern matching.

## Starts with A

```sql
SELECT *
FROM Employee
WHERE name LIKE 'A%';
```

---

## Ends with a

```sql
WHERE name LIKE '%a';
```

---

## Contains "mit"

```sql
WHERE name LIKE '%mit%';
```

---

## Exactly 5 letters

```sql
WHERE name LIKE '_____';
```

Each `_` represents one character.

---

# IS NULL

```sql
SELECT *
FROM Employee
WHERE manager_id IS NULL;
```

Never write

```sql
manager_id = NULL
```

Wrong!

---

# ORDER BY

Ascending

```sql
SELECT *
FROM Employee
ORDER BY salary;
```

Descending

```sql
SELECT *
FROM Employee
ORDER BY salary DESC;
```

Multiple columns

```sql
ORDER BY department, salary DESC;
```

---

# LIMIT

Top rows.

```sql
SELECT *
FROM Employee
LIMIT 5;
```

Second highest

```sql
SELECT *
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

---

# Interview Execution Order

SQL is written as

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

Executed as

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

↓

LIMIT
```

This is a **very common interview question**.

---

# Frequently Asked Interview Questions

### 1. Difference between WHERE and HAVING?

* `WHERE` filters rows **before grouping**.
* `HAVING` filters groups **after grouping**.

---

### 2. Why is `SELECT *` discouraged?

* Retrieves unnecessary columns.
* Uses more memory.
* Slower.
* Can prevent use of covering indexes.
* Breaks if schema changes.

---

### 3. Difference between DISTINCT and GROUP BY?

`DISTINCT`

```sql
SELECT DISTINCT department
FROM Employee;
```

`GROUP BY`

```sql
SELECT department
FROM Employee
GROUP BY department;
```

Both can remove duplicates, but `GROUP BY` is mainly used with aggregate functions (`COUNT`, `SUM`, `AVG`, etc.).

---

### 4. Why can't we write `= NULL`?

Because `NULL` represents an unknown value. Use:

```sql
IS NULL
```

or

```sql
IS NOT NULL
```

---

### 5. Difference between `%` and `_` in `LIKE`?

* `%` → zero or more characters.
* `_` → exactly one character.

---

### 6. How do you find duplicate values?

```sql
SELECT email, COUNT(*)
FROM Users
GROUP BY email
HAVING COUNT(*) > 1;
```

---

### 7. How do you get the top 5 highest salaries?

```sql
SELECT *
FROM Employee
ORDER BY salary DESC
LIMIT 5;
```

---

# LeetCode: Recyclable and Low Fat Products (Easy)

Table: `Products`

| product_id | low_fats | recyclable |
| ---------- | -------- | ---------- |
| 0          | Y        | N          |
| 1          | Y        | Y          |
| 2          | N        | Y          |
| 3          | Y        | Y          |

Return products that are **both low fat and recyclable**.

### SQL Solution

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
  AND recyclable = 'Y';
```

### Explanation

* `SELECT product_id` → Return only the `product_id` column.
* `FROM Products` → Read rows from the `Products` table.
* `WHERE low_fats = 'Y'` → Keep only low-fat products.
* `AND recyclable = 'Y'` → Of those, keep only recyclable products.

Only rows satisfying **both** conditions are returned.

### Time Complexity

* **O(n)**, where `n` is the number of rows, because the database scans the table once (assuming no relevant indexes).
* With appropriate indexes on `low_fats` and `recyclable`, the database may reduce the amount of data scanned.

---

