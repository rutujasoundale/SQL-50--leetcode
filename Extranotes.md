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
