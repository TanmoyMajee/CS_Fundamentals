# Dfii betw Alter Updat , | delete drop tuncate #
# whats rool back #

# 🧩 SQL: DELETE vs TRUNCATE vs DROP — Comparison Table

| Feature | DELETE | TRUNCATE | DROP |
|---------|--------|----------|------|
| **Command Type** | DML (Data Manipulation Language) | DDL (Data Definition Language) | DDL (Data Definition Language) |
| **Purpose** | Removes **specific rows** from a table | Removes **all rows** from a table | Removes **entire table** or **database** |
| **Syntax** | `DELETE FROM table_name WHERE condition;` | `TRUNCATE TABLE table_name;` | `DROP TABLE table_name;` <br> `DROP DATABASE db_name;` |
| **WHERE Clause** | ✅ Yes | ❌ No | ❌ No |
| **Rollback** | ✅ Yes, if using transactions | ❌ Often cannot rollback (depends on DBMS) | ❌ No rollback, permanent deletion |
| **Speed** | Slower (logs each row deletion) | Fast (does not log each row) | Fast (drops structure and data) |
| **Resets Identity / Auto-increment** | ❌ No | ✅ Yes | ✅ Table gone |
| **Triggers Fired** | ✅ Yes | ❌ Usually no | ❌ No |
| **Effect on Table Structure** | Table **remains intact** | Table **remains intact** | Table or DB **completely removed** |
| **Use Case** | Remove some or all rows while keeping structure | Quickly clear all data but keep table for reuse | Completely remove table or database permanently |
| **Example** | `DELETE FROM Students WHERE Age < 18;` | `TRUNCATE TABLE Students;` | `DROP TABLE Students;` |

---

💡 **Quick Interview Tip:**  
- **DELETE:** Specific rows, rollback possible, slower.  
- **TRUNCATE:** All rows, fast, resets counters, cannot filter.  
- **DROP:** Removes table completely, irreversible.



# 🧩 SQL: ALTER vs UPDATE — Comparison Table

| Feature | UPDATE | ALTER |
|---------|--------|-------|
| **Command Type** | DML (Data Manipulation Language) | DDL (Data Definition Language) |
| **Purpose** | Modifies **existing data** in one or more rows of a table | Modifies the **structure** of a table (add, drop, or change columns) |
| **Syntax** | `UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;` | `ALTER TABLE table_name ADD column_name datatype;` <br> `ALTER TABLE table_name DROP COLUMN column_name;` <br> `ALTER TABLE table_name MODIFY column_name datatype;` |
| **WHERE Clause** | ✅ Yes, can update specific rows | ❌ No, affects table structure |
| **Rollback** | ✅ Yes, if using transactions | ❌ Usually cannot rollback (depends on DBMS) |
| **Speed** | Depends on number of rows affected | Usually fast for structure changes |
| **Effect on Table** | Only **data values** are modified | **Table structure** is changed (columns added, dropped, or modified) |
| **Triggers Fired** | ✅ Yes (if row-level triggers exist) | ❌ No (structure changes do not fire row triggers) |
| **Use Case** | Update values of specific records or all records | Change table schema without dropping the table |
| **Example** | `UPDATE Students SET Age = 20 WHERE StudentID = 1;` | `ALTER TABLE Students ADD Email VARCHAR(50);` <br> `ALTER TABLE Students DROP COLUMN Age;` |
| **Additional Notes** | Can affect multiple rows at once | Commonly used during database design or schema updates |


# 🔄 Transaction Control in SQL: COMMIT & ROLLBACK

**COMMIT** and **ROLLBACK** are **TCL (Transaction Control Language) commands** used to manage database transactions. They help ensure **data integrity** by controlling when changes become permanent or are undone.

---

## 🔹 Definition & Purpose
- **COMMIT:** Permanently saves all changes made in the current transaction to the database.  
- **ROLLBACK:** Undoes all changes in the current transaction, restoring the database to its last committed state.  

**Purpose:**  
- Maintain **ACID properties** (Atomicity & Durability).  
- Prevent partial updates and ensure consistency in multi-step operations.

---

## 🔹 Single Example
Imagine a **banking system** transferring ₹1000 from Account A to Account B:

1. Deduct ₹1000 from Account A  
2. Add ₹1000 to Account B  

- If **both steps succeed**, we use **COMMIT** to save the changes permanently.  
- If **any step fails** (e.g., system crash while adding to B), we use **ROLLBACK** to undo the deduction from A, keeping balances consistent.

---

# 🔹 SQL Constraints


**Definition:**  
Constraints are **rules applied to a table’s columns** to control what kind of data can be entered.  
They help ensure the database **stays accurate and consistent**. Constraints can be defined **while creating a table** (`CREATE TABLE`) or **afterwards** (`ALTER TABLE`)

---

## 🔹 Types of Constraints

| Constraint | Purpose / Use |
|------------|---------------|
| **PRIMARY KEY** | Uniquely identifies each row in a table; cannot be NULL. |
| **FOREIGN KEY** | Maintains referential integrity between two tables. |
| **UNIQUE** | Ensures all values in a column are unique; allows one NULL. |
| **NOT NULL** | Ensures that a column cannot have NULL values. |
| **CHECK** | Ensures values meet a specific condition (e.g., Age > 18). |
| **DEFAULT** | Assigns a default value to a column if none is specified. |

---

## Add Remove con
```sql

CREATE TABLE Orders (
  order_id INT,
  product_id INT,
  PRIMARY KEY (order_id, product_id)   -- composite primary key
);

CREATE TABLE OrderDetails (
  order_id INT,
  product_id INT,
  FOREIGN KEY (order_id, product_id)
    REFERENCES Orders(order_id, product_id)  --(More than one column together → composite key) composite foreign key
);


-- Step A: Remove the existing foreign key constraint
ALTER TABLE Student 
DROP FOREIGN KEY fk_branch; 

-- Step B: Add the foreign key back with the CASCADE property
ALTER TABLE Student 
ADD CONSTRAINT fk_branch 
FOREIGN KEY (branch_id) 
REFERENCES Branch(branch_id) 
ON DELETE CASCADE 
ON UPDATE CASCADE;


-- Normal way
CREATE TABLE Branch (
    branch_id INT PRIMARY KEY,         -- Primary Key
    branch_name VARCHAR(50) NOT NULL
);

-- 2. Create the Child Table with CASCADE
CREATE TABLE Student (
    student_id INT PRIMARY KEY,        -- Primary Key
    student_name VARCHAR(50),
    branch_id INT,                     -- Foreign Key Column
    
    -- Defining the Foreign Key Relationship
    CONSTRAINT fk_branch 
    FOREIGN KEY (branch_id) 
    REFERENCES Branch(branch_id) 
    ON DELETE CASCADE                  -- The Magic Line
    ON UPDATE CASCADE                  -- Also updates if branch_id changes
);
```



## 🔹 Syntax
```sql
-- Start a transaction
START TRANSACTION;

-- Perform operations
UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 'A';
UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountID = 'B';

-- Commit if successful
COMMIT;

-- Or rollback in case of error
ROLLBACK;

START TRANSACTION;

-- 1. Deduct Money
UPDATE Wallet SET Balance = Balance - 500 WHERE UserID = 'User1';

-- Create a Checkpoint after payment is successful
SAVEPOINT payment_done;

-- 2. Try to Apply a Promo Code (This might fail)
UPDATE Orders SET Discount = 50 WHERE OrderID = 'ORD123';

-- Logic Check: If the promo code is invalid, we go back to 'payment_done'
ROLLBACK TO SAVEPOINT payment_done;

-- 3. Update Inventory (This happens even if promo failed)
UPDATE Inventory SET Stock = Stock - 1 WHERE ProductID = 'P1';

-- Make everything permanent
COMMIT;


````
"A Savepoint is a way to break a large transaction into smaller, manageable sections. It allows for a Partial Rollback.

For example, in an e-commerce system, if a customer’s payment is successful but the 'Email Notification' service fails, I don't want to roll back the entire payment. I can set a savepoint right after the payment. If the email fails, I roll back to that specific savepoint and continue with the rest of the transaction. This ensures that the core business operation (the sale) is completed even if a non-critical step fails."


# SQL Functions and Clauses — Quick Reference Notes

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
SELECT UPPER(SUBSTR('hello world', 7)) FROM dual; -- LOWER()
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

UPDATE Employees
SET Email = REPLACE(Email, '.net', '.com')
WHERE Email LIKE '%.net';

UPDATE Employees
SET Name = TRIM(Name);

--TRANSLATE(string, from_set, to_set)
-- How it works: It replaces the 1st character of from_set with the 1st character of to_set, the 2nd with the 2nd, and so on.
-- Result: ABC-DEF Explanation: 1 → A, 2 → B, 3 → C, and so on.
SELECT TRANSLATE('123-456', '123456', 'ABCDEF') FROM dual;


-- Translates '.', '-', and '/' all into '_'
UPDATE Products
SET Product_Code = TRANSLATE(Product_Code, '.-/', '___');

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


```

** Nth Highest Salary — Short Notes **

## 1. `LIMIT` (MySQL)

**One-line:** Skip `N-1` distinct salaries and fetch 1.

```sql
-- standard (count + OFFSET)
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;

-- MySQL alternative (offset, count)
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT N-1, 1;

> Note: use `DISTINCT` so duplicates don't change the rank.

---

## 2. `DENSE_RANK()` (best for ties)

**One-line:** Rank distinct salaries without gaps; pick rank = N.

```sql
SELECT salary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employees
) t
WHERE rnk = N;
```

---

## 3. `COUNT(DISTINCT ...)` subquery (works everywhere)

**One-line:** Count how many *distinct* salaries are greater than current salary.

```sql
SELECT salary
FROM employees e1
WHERE (
  SELECT COUNT(DISTINCT e2.salary) 
  FROM employees e2 
  WHERE e2.salary > e1.salary
) = N-1;
```

---

## 4. `ROW_NUMBER()` fixed for duplicates

**One-line:** Apply `ROW_NUMBER()` to `DISTINCT` salaries, then pick rn = N.

```sql
SELECT salary
FROM (
  SELECT salary, ROW_NUMBER() OVER (ORDER BY salary DESC) AS rn
  FROM (SELECT DISTINCT salary FROM employees) s
) t
WHERE rn = N;
```

---

## Quick recommendation

* Use **`DENSE_RANK()`** for clarity and correct handling of ties.
* Use **`LIMIT`** for simple MySQL solutions (remember `DISTINCT`).
* `COUNT(DISTINCT)` is portable to DBs without window functions.


# 📘 Triggers in SQL — Interview Notes

---

## 🔹 Definition of a Trigger

A **trigger** is a special kind of stored procedure in a SQL database that **automatically executes** or **fires** when a specific event occurs on a table or view. Triggers help in **automating tasks**, maintaining **data integrity**, and enforcing **business rules** without manual intervention.

---

## 🔹 Purpose of Triggers

- **Automate tasks:** Triggers can perform automatic actions in response to data changes.
- **Enforce business rules:** Ensure data meets certain conditions before it is updated or inserted.
- **Maintain data integrity:** Automatically validate or modify data to keep it consistent.
- **Audit and logging:** Track changes to data for security and compliance.

---

## 🔹 Types of Triggers

1. **BEFORE Trigger:**  
   Executes **before** the triggering event (e.g., before an insert, update, or delete).  
   **Purpose:** Validate or modify data before it is saved to the database.

2. **AFTER Trigger:**  
   Executes **after** the triggering event (e.g., after an insert, update, or delete).  
   **Purpose:** Perform actions such as logging, auditing, or updating related tables after the change has been made.

3. **INSTEAD OF Trigger:**  
   Replaces the standard action of the triggering event (often used with views).  
   **Purpose:** Customize how data is inserted, updated, or deleted.

---

## 🔹 Examples

### 🟢 BEFORE Trigger Example

```sql
CREATE TRIGGER trg_before_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    -- Ensure the employee’s age is at least 18
    IF :NEW.age < 18 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Age must be at least 18');
    END IF;
END;
```
### 🟢 AFTER Trigger Example

```sql
CREATE TRIGGER trg_after_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    -- Log the update action into the employee_log table
    INSERT INTO employee_log (employee_id, action, action_time)
    VALUES (:NEW.id, 'UPDATE', SYSDATE);
END;
```

## 1. SQL Query Execution Order (VERY IMPORTANT)
```sql
SELECT column_name, AGG_FUNCTION(column)
FROM table_name
JOIN table_name ON condition
WHERE condition
GROUP BY column_name
HAVING condition_on_aggregate
ORDER BY column_name ASC | DESC;
```

# SQL Interview Question: Find Employees in at least one Department

### 1. Problem Statement
**Question:** Given two tables, `emp` and `dept`, write a query to fetch the details of all employees who work in **at least one** department.

---

### 2. Table Structure

**Table: `emp` (Employee Master)**
| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| **eid** | Integer | Primary Key (Unique ID) |
| **nm** | Varchar | Employee Name |

**Table: `dept` (Department Mapping)**
| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| **did** | Varchar | Department ID |
| **eid** | Integer | Foreign Key (Link to emp table) |
| **dnm** | Varchar | Department Name |

---

### 3. Different Approaches



#### Approach A: Using INNER JOIN (Most Common)
This approach joins both tables. We use `DISTINCT` to avoid duplicate employee names if they belong to multiple departments.

```sql
SELECT DISTINCT e.eid, e.nm
FROM emp e
INNER JOIN dept d ON e.eid = d.eid;

SELECT eid, nm
FROM emp e
WHERE EXISTS (
    SELECT 1 
    FROM dept d 
    WHERE d.eid = e.eid
);

SELECT eid, nm
FROM emp
WHERE eid IN (SELECT eid FROM dept);
````

# SQL Interview Guide: Joins, Grouping, and Filtering

## 1. Sample Data (for Context)
Assume the following data for all queries below.

### **Employee Table** (`employee`)
| emp_id | emp_name | dept_id | salary |
| :--- | :--- | :--- | :--- |
| 101 | Alice | 1 | 60000 |
| 102 | Bob | 1 | 80000 |
| 103 | Charlie | 2 | 55000 |
| 104 | David | 3 | 90000 |
| 105 | Eve | 1 | 45000 |

### **Department Table** (`department`)
| dept_id | dept_name |
| :--- | :--- |
| 1 | IT |
| 2 | HR |
| 3 | Sales |
| 4 | Marketing |

### **Project Table** (`project`)
| project_id | emp_id | project_name |
| :--- | :--- | :--- |
| P100 | 101 | Cloud Migration |
| P200 | 102 | Security Audit |
| P300 | 102 | Backend Refactor |
| P400 | 103 | Recruitment Drive|

---

## 2. JOIN + GROUP BY (Basic Aggregations)

### Q1. Find total salary of each department
*Goal: Join tables to get department names and sum salaries.*
```sql
SELECT d.dept_name, SUM(e.salary) AS total_salary
FROM employee e
JOIN department d ON e.dept_id = d.dept_id
GROUP BY d.dept_name;
```
### Q2. Find departments whose total salary is greater than 100,000
*Goal: Filter based on an aggregated value (sum of salary).*
```sql
SELECT d.dept_name, SUM(e.salary) AS total_salary
FROM employee e
JOIN department d ON e.dept_id = d.dept_id
GROUP BY d.dept_name
HAVING SUM(e.salary) > 100000;
```
### Q3. Count number of employees in each department

*Goal: Use LEFT JOIN to ensure departments with 0 employees (like 'Marketing') are still listed with a count of 0.

```sql
SELECT d.dept_name, COUNT(e.emp_id) AS emp_count
FROM department d
LEFT JOIN employee e ON d.dept_id = e.dept_id
GROUP BY d.dept_name;
```

### Q4. Find departments having more than 2 employees earning above 50,000

*Goal: Filter rows first (WHERE), then group, then filter groups (HAVING).
```sql
SELECT dept_id, COUNT(*) AS emp_count
FROM employee
WHERE salary > 50000  -- 1. Filters individual rows (Before Grouping)
GROUP BY dept_id      -- 2. Groups the remaining rows
HAVING COUNT(*) > 2;  -- 3. Filters the groups (After Grouping)
```

Key Differences
Clause	Used For	Execution Order
WHERE	Filtering Rows	Before Grouping
HAVING	Filtering Groups (Aggregates)	After Grouping


### Q5. List departments ordered by total salary (highest first)
```sql
SELECT dept_id, SUM(salary) AS total_salary
FROM employee
GROUP BY dept_id
ORDER BY total_salary DESC;
```


### Q6. Find the department with the highest total salary

Strategy: Order by sum descending and take the top 1.

```sql
SELECT dept_id
FROM employee
GROUP BY dept_id
ORDER BY SUM(salary) DESC
LIMIT 1;
```

### Q7.  Find employees who are not assigned to any project

Strategy: Use LEFT JOIN and check for NULLs in the right table (Project).
```sql

SELECT e.emp_name
FROM employee e
LEFT JOIN project p ON e.emp_id = p.emp_id
WHERE p.emp_id IS NULL;
```

### Q8. Find employees working on more than one project

Strategy: Group by employee and count the number of project_ids.

```sql
SELECT emp_id
FROM project
GROUP BY emp_id
HAVING COUNT(project_id) > 1;
```

### Q9. Find departments with no employees

Strategy: Similar to Q7, using LEFT JOIN from Department to Employee.
```sql
SELECT d.dept_name
FROM department d
LEFT JOIN employee e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

### 6. Common Interview Mistakes
❌ The "WHERE Aggregate" Mistake

Wrong:
```sql
SELECT dept_id FROM employee WHERE SUM(salary) > 50000
```

Why: You cannot use aggregate functions (SUM, COUNT, AVG) in a WHERE clause because the groups haven't been formed yet.
✅ The Correct Way

Correct:
```sql
SELECT dept_id FROM employee GROUP BY dept_id HAVING SUM(salary) > 50000
```


