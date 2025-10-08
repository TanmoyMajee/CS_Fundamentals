# SQL Functions and Clauses — Quick Reference Notes

This document summarizes the key SQL functions and clauses discussed.

---

## 🧩 TO_CHAR()

**Purpose:** Converts a number or date to a string (text).  
**Syntax:**
```sql
TO_CHAR(value [, format])
```

**Examples:**
```sql
SELECT TO_CHAR(12345) FROM dual;
SELECT TO_CHAR(SYSDATE, 'DD-Mon-YYYY HH24:MI:SS') FROM dual;
SELECT TO_CHAR(1234567.89, '9,999,999.99') FROM dual;
```

**Common Date Formats:**
| Format | Meaning | Example |
|:--|:--|:--|
| YYYY | 4-digit year | 2025 |
| MON | 3-letter month | OCT |
| MONTH | Full month name | OCTOBER |
| DD | Day of month | 07 |
| DY | 3-letter day | TUE |
| HH24 | 24-hour format | 14 |
| MI | Minutes | 35 |
| SS | Seconds | 45 |
| AM/PM | Meridian | PM |

---

## 🗃 DUAL Table

**Purpose:** Dummy table used in Oracle SQL for selecting expressions or functions.  
**Examples:**
```sql
SELECT SYSDATE FROM dual;
SELECT 5 + 3 FROM dual;
SELECT TO_CHAR(SYSDATE, 'DD-Mon-YYYY') FROM dual;
```

---

## ✂️ SUBSTR()

**Purpose:** Extracts a substring from a string.  
**Syntax:**
```sql
SUBSTR(string, start_position [, length])
```

**Examples:**
```sql
SELECT SUBSTR('HELLO WORLD', 1, 5) FROM dual;   -- HELLO
SELECT SUBSTR('HELLO WORLD', 7) FROM dual;      -- WORLD
SELECT SUBSTR('HELLO WORLD', -5, 3) FROM dual;  -- WOR
```

**Common Uses:**
- Extract part of an ID or code
- Get domain from email
- Extract last 4 digits of a phone number

---

## 🔁 REPLACE()

**Purpose:** Replaces all occurrences of a substring.  
**Syntax:**
```sql
REPLACE(string, search_string, replacement_string)
```

**Examples:**
```sql
SELECT REPLACE('Hello World', 'World', 'Tanmoy') FROM dual;
SELECT REPLACE('123-456-7890', '-', '') FROM dual;  -- 1234567890
```

## 🔁 Concat || Double pipe

**Syntax:**
```sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
```
**Syntax:**
```sql
SELECT CONCAT_WS(' - ', first_name, last_name, department) AS employee_info
FROM employees;
```

---

## 🔢 LIMIT / FETCH

**Purpose:** Restrict number of rows returned.  
**Usage differs by SQL dialect.**

### MySQL / PostgreSQL:
```sql
SELECT * FROM employees LIMIT 5;
SELECT * FROM employees LIMIT 5 OFFSET 10;
```

### Oracle / SQL Server:
```sql
SELECT * FROM employees OFFSET 0 ROWS FETCH NEXT 5 ROWS ONLY;
SELECT * FROM employees OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```

---

## 🎯 ROUND()

**Purpose:** Rounds numbers or dates.

**Syntax (Numbers):**
```sql
ROUND(number [, decimal_places])
```
**Syntax (Dates):**
```sql
ROUND(date [, format])
```

**Examples:**
```sql
SELECT ROUND(123.456, 2) FROM dual;  -- 123.46
SELECT ROUND(TO_DATE('15-OCT-2025', 'DD-MON-YYYY'), 'MONTH') FROM dual;  -- 01-NOV-2025
```

---

## 🗓 MONTHS_BETWEEN()

**Purpose:** Finds the number of months between two dates.  
**Syntax:**
```sql
MONTHS_BETWEEN(date1, date2)
```

**Examples:**
```sql
SELECT MONTHS_BETWEEN(TO_DATE('07-OCT-2025','DD-MON-YYYY'),
                      TO_DATE('07-JAN-2025','DD-MON-YYYY')) FROM dual;  -- 9
SELECT ROUND(MONTHS_BETWEEN(TO_DATE('15-FEB-2025','DD-MON-YYYY'),
                             TO_DATE('01-JAN-2025','DD-MON-YYYY'))) FROM dual;  -- 1
```

---

## ✅ Summary

| Function | Purpose | Example |
|-----------|----------|----------|
| TO_CHAR() | Convert number/date to string | `TO_CHAR(SYSDATE, 'DD-Mon-YYYY')` |
| DUAL | Dummy table for expressions | `SELECT SYSDATE FROM dual;` |
| SUBSTR() | Extract part of string | `SUBSTR('HELLO WORLD', 7)` |
| REPLACE() | Replace substring | `REPLACE('ABCDEF', 'ABC', 'XYZ')` |
| LIMIT/FETCH | Restrict rows | `LIMIT 5` / `FETCH NEXT 5 ROWS ONLY` |
| ROUND() | Round number or date | `ROUND(123.456, 2)` |
| MONTHS_BETWEEN() | Find month difference | `MONTHS_BETWEEN(date1, date2)` |

---
# SQL JOINs — Complete Interview Notes

## 🔹 What is a JOIN?
A **JOIN** in SQL is used to combine rows from two or more tables based on a related column between them.  
It helps retrieve data that is stored across multiple tables by linking them through a **common column** (usually a primary key and foreign key).

---

## 🔸 Condition to Perform a JOIN
1. Must Match the DATA TYPE of both colom **
1. There must be **at least one common column** between the tables.  
2. The relationship is usually defined using **ON** or **USING** keywords.  
3. The condition specifies **how rows from one table match rows in another**.


Example:
```sql
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d
ON e.dept_id = d.dept_id;

🔹 Types of JOINs in SQL
1. INNER JOIN
Returns only the rows that have matching values in both tables.

sql
Copy code
SELECT * FROM A INNER JOIN B ON A.id = B.id;
2. LEFT JOIN (LEFT OUTER JOIN)
Returns all rows from the left table and matching rows from the right table.
If there is no match, NULLs are returned for columns of the right table.

sql
Copy code
SELECT * FROM A LEFT JOIN B ON A.id = B.id;
3. RIGHT JOIN (RIGHT OUTER JOIN)
Returns all rows from the right table and matching rows from the left table.
If no match exists, NULLs appear for columns of the left table.

sql
Copy code
SELECT * FROM A RIGHT JOIN B ON A.id = B.id;
4. FULL JOIN (FULL OUTER JOIN)
Returns all rows from both tables, whether or not there is a match.
Non-matching rows from each side will have NULLs for missing values.

sql
Copy code
SELECT * FROM A FULL JOIN B ON A.id = B.id;
5. CROSS JOIN
Returns the Cartesian product of both tables — every row from the first table is combined with every row from the second table.

sql
Copy code
SELECT * FROM A CROSS JOIN B;
6. SELF JOIN
A table is joined with itself to compare rows within the same table (requires table aliases).

sql
Copy code
SELECT a.name, b.name
FROM employees a
JOIN employees b ON a.manager_id = b.id;
7. NATURAL JOIN
Automatically joins tables using all columns with the same names in both tables.
It eliminates duplicate columns from the result.

sql
Copy code
SELECT * FROM A NATURAL JOIN B;
⚠️ Note: Avoid using NATURAL JOIN in production because if a new column with the same name is added, it may produce unexpected results.

8. EQUI JOIN
A join that uses the equality operator (=) to match rows between tables (most common type).
Example: INNER JOIN is a type of equi join.

9. NON-EQUI JOIN
A join that uses operators other than = (like <, >, or BETWEEN).

sql
Copy code
SELECT e.name, s.grade
FROM employees e
JOIN salary s
ON e.salary BETWEEN s.min_salary AND s.max_salary;

💡 Summary Table
Type	Returns
INNER JOIN	Only matching rows from both tables
LEFT JOIN	All rows from left + matching from right
RIGHT JOIN	All rows from right + matching from left
FULL JOIN	All rows from both tables
CROSS JOIN	All possible row combinations
SELF JOIN	Table joined with itself
NATURAL JOIN	Joins automatically on columns with same names
EQUI JOIN	Join using = condition
NON-EQUI JOIN	Join using <, >, BETWEEN, etc.
