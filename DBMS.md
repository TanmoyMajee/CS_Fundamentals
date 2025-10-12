### 🧠 Denormalization

**Definition:**  
Denormalization is the process of combining related tables into one to improve **query performance**, especially in **read-heavy** systems.  
It increases **redundancy** to reduce the need for expensive joins.

---

### ⚖️ Difference from Normalization

| Aspect | Normalization | Denormalization |
|--------|----------------|-----------------|
| Purpose | Reduce redundancy, improve integrity | Improve performance (speed) |
| Data Redundancy | Minimal | Intentional redundancy |
| Best for | Write-heavy systems | Read-heavy systems |

---
### ⚙️ Real-Life Examples of Write-Heavy vs Read-Heavy Systems

#### 📝 Write-Heavy Systems
These systems perform frequent **insert**, **update**, or **delete** operations.

**Examples:**
- **Banking applications:** Transactions are continuously inserted and updated (balance, transfers, etc.).
- **E-commerce order systems:** New orders, stock updates, and payments are recorded every second.
- **Social media posting system:** Every post, like, or comment is a new write operation.

➡️ Such systems prefer **Normalization** to maintain data accuracy and avoid redundancy.

---

#### 📖 Read-Heavy Systems
These systems perform frequent **select (read)** operations and fewer writes.

**Examples:**
- **Reporting or analytics dashboards:** Data is read repeatedly to generate reports or charts.
- **News or blog websites:** Many users read articles, but only admins write/update content.
- **Recommendation systems (e.g., Netflix, Amazon):** Data is read often to suggest products or movies.

➡️ Such systems often use **Denormalization** to speed up reads and reduce join operations.


## 👁️ SQL VIEW — Interview Notes

### 📘 Definition
A **View** is a **virtual table** that is created from the result of an SQL query.  
It does **not store data physically** — instead, it **stores the SQL query** itself.  
Whenever the view is accessed, the **underlying SQL query is executed again** to fetch the latest data from the base tables.

**Syntax:**
```sql
CREATE VIEW view_name AS
SELECT column1, column2
FROM table_name
WHERE condition;
```

---

### 🧩 Why Use a View?

Views are mainly used for:

- Simplifying complex queries — reusing a query by saving it as a view.  
- Enhancing security — users can access specific columns only.  
- Data abstraction — hides table structure from end users.  
- Reusability — makes code cleaner and consistent.  

---

### 🔍 Example (Real-Life Scenario)
**Example:** University Database  

**Tables:**
- `Students(student_id, name, dept, marks)`  
- `Departments(dept_id, dept_name)`  

**View:**
```sql
CREATE VIEW TopStudents AS
SELECT name, dept, marks
FROM Students
WHERE marks > 80;
```

**Usage:**
```sql
SELECT * FROM TopStudents;
```

✅ *Real-life use:* In a university portal, teachers can view only top students through this view without accessing the entire student table.

---

### 🚫 Limitations of Views

- Cannot always perform DML operations on complex views.  
- Performance can slow down for large joins.  
- Views do **not store data** (unless materialized).  

---

### 💡 Real-Life Examples of Views
| Use Case | Description |
|-----------|--------------|
| **Banking System** | View to show customer transactions without exposing account details. |
| **E-commerce Dashboard** | View for showing total sales by region or category. |
| **HR Portal** | View to show only employee names and departments (not salaries). |

---

### 🧠 Common Interview Question
**Q:** What’s the difference between a View and a Table?  
**A:** A table stores data physically, while a view is a **virtual representation** of data.  
It **stores the SQL query**, and each time it’s accessed, the **query is re-executed** to fetch updated data.


# 🧠 Database Normalization — Step-by-Step with One Example

---

## 📌 What is Normalization?

Normalization is the process of organizing data in an RDBMS to reduce redundancy and improve data integrity.  
It is done by dividing a large table into two or more smaller related tables and creating relationships between them using **primary keys** and **foreign keys**.

➡️ It works by breaking large tables into smaller related tables and defining relationships among them.  
When a **view** or **query** accesses the data, the **underlying SQL query** runs to fetch it dynamically from the related tables.

---

## ⚙️ Why Do We Need Normalization?

Without normalization:
- The same data might appear in multiple places (redundancy).
- Data updates can cause inconsistencies.
- Insertion or deletion of data might affect unrelated information.

Normalization helps to:
✅ Reduce redundancy  
✅ Improve data integrity  
✅ Make the database flexible and efficient

---

### 🧩 Data Integrity — Simple Definition with Example

**Data Integrity** means keeping the data **correct, consistent, and reliable** inside the database.

It ensures that the data in related tables always makes sense and stays valid.

🧠 **Example:**
Suppose we have two tables — **Student** and **Branch**.

- `Branch` table: stores all branch details.  
- `Student` table: has a foreign key `branch_id` referring to the `Branch` table.

Now, **if a branch is deleted from the Branch table while students from that branch still exist**,  
it will **break the relationship** — this is a **data integrity issue**.

✅ **To maintain data integrity**, the database won’t allow deleting that branch  
until all related students are removed or updated — ensuring data remains consistent.


## 💥 Problems Normalization Removes (Anomalies)

| **Type** | **Problem** | **Example** |
|-----------|--------------|--------------|
| **Insertion Anomaly** | Can’t insert data without another. | Can’t add a new course unless a student enrolls. |
| **Deletion Anomaly** | Deleting one record removes others. | Deleting a student deletes course data too. |
| **Updation Anomaly** | Changing data in one place doesn’t update all. | Updating a course name in one row leaves others old. |

---

### 🧱 Types of Normal Forms — Summary

| **Normal Form** | **Requirement / Rule** | **What It Fixes / Ensures** |
|------------------|------------------------|------------------------------|
| **1NF (First Normal Form)** | Each table cell must contain a single, atomic value (no repeating groups). | Removes repeating groups or multivalued attributes. |
| **2NF (Second Normal Form)** | Must be in 1NF, and every non-key attribute must depend on the whole primary key (not just part of it). | Removes partial dependency (for composite keys). |
| **3NF (Third Normal Form)** | Must be in 2NF, and no transitive dependency(non prime attr is depeding on anth no prim attribut) is allowed for non-key attributes. | Removes transitive dependencies. |
| **BCNF (Boyce-Codd Normal Form)** | A stronger version of 3NF — for every functional dependency X → Y, X must be a super key. | Solves anomalies not handled by 3NF. |
---

# 🧠 Normalization Example — Student & Branch Table

---

## 📋 Initial Table (Unnormalized Form)

| id | name | age | branch_code | branch_name | branch_HOD |
|----|------|-----|--------------|--------------|-------------|
| 1 | A | 18 | 1 | CS | X |
| 2 | B | 19 | 1 | CS | X |
| 3 | C | 18 | 1 | CS | X |
| 4 | D | 19 | 2 | ECE | Y |
| 5 | E | 20 | 2 | ECE | Y |
| 6 | F | 19 | 3 | ME | Z |

---

## ⚠️ Anomalies in This Table

### 🟡 1. Insertion Anomaly
If a new **branch (IT)** is introduced but **no student** has enrolled yet,  
we **cannot insert** that branch data without leaving student columns empty.

➡️ **Example:**  
To add branch "IT", we must insert a student with null or dummy values.  
This causes incomplete (invalid) data.

---

### 🔴 2. Deletion Anomaly
If the **last student** of a branch is deleted,  
the **branch information** is also lost.

➡️ **Example:**  
If all students of **ME** branch are deleted,  
then branch "ME" and its HOD "Z" are also removed from the database.

---

### 🟢 3. Update Anomaly
If the **HOD of a branch changes**,  
we need to update it in **multiple rows**.

➡️ **Example:**  
If HOD of **ECE** changes from "Y" to "P",  
we must update it in every student row with branch_code = 2.  
Missing one will lead to inconsistent data.

---

## 🧩 Step-by-Step Normalization

---

### 🔹 1NF (First Normal Form)
- All data is atomic (no repeating groups).  
- Our table already satisfies 1NF.

---

### 🔹 2NF (Second Normal Form)
- Remove **partial dependency**.  
- Attributes like `branch_name` and `branch_HOD` depend only on `branch_code`,  
  not on the entire primary key `(id, branch_code)`.

✅ **Split the table:**

**Student Table**

| id | name | age | branch_code |
|----|------|-----|--------------|
| 1 | A | 18 | 1 |
| 2 | B | 19 | 1 |
| 3 | C | 18 | 1 |
| 4 | D | 19 | 2 |
| 5 | E | 20 | 2 |
| 6 | F | 19 | 3 |

**Branch Table**

| branch_code | branch_name | branch_HOD |
|--------------|--------------|-------------|
| 1 | CS | X |
| 2 | ECE | Y |
| 3 | ME | Z |

---

### 🔹 3NF (Third Normal Form)
- Remove **transitive dependency**.  
- Suppose `branch_HOD` depends on `branch_name` instead of `branch_code`.  
  Then we separate the HOD information.

✅ **Split into another table:**

**Branch Table**

| branch_code | branch_name |
|--------------|-------------|
| 1 | CS |
| 2 | ECE |
| 3 | ME |

**HOD Table**

| branch_name | branch_HOD |
|--------------|-------------|
| CS | X |
| ECE | Y |
| ME | Z |

---

# 🧩 Boyce–Codd Normal Form (BCNF) — DBMS Notes

## 🔹 Definition
A relation (table) is in **BCNF** if for every **Functional Dependency (A → B)**,  
**A must be a Super Key**.

👉 It is a **stronger version of 3NF**, removing all redundancy caused by non-key determinants.

---

## 🔹 Example: STUDENT_SUBJECT Table

| student_id | subject   | project_guide |
|-------------|-----------|---------------|
| S1          | DBMS      | Prof. A       |
| S2          | DBMS      | Prof. A       |
| S3          | Networks  | Prof. B       |
| S4          | Networks  | Prof. B       |

### Functional Dependencies
1. `student_id → subject`  
2. `subject → project_guide`

### Candidate Key
- `student_id` is the **Primary Key**.

---

## 🔹 BCNF Check
- `student_id → subject` ✅ (student_id is a key)  
- `subject → project_guide` ❌ (subject is **not** a key → violates BCNF)

Hence, the table is **not in BCNF**.

---

## 🔹 Decomposition

### Table 1: SUBJECT_GUIDE
| subject | project_guide |
|----------|----------------|
| DBMS     | Prof. A        |
| Networks | Prof. B        |

### Table 2: STUDENT_SUBJECTS
| student_id | subject |
|-------------|----------|
| S1          | DBMS     |
| S2          | DBMS     |
| S3          | Networks |
| S4          | Networks |

✅ Now both tables satisfy BCNF.

---

## 🔹 Key Points
- BCNF Rule: For every FD **A → B**, **A must be a super key**.  
- Removes **redundancy** and **update anomalies**.  
- **Stricter than 3NF** — every BCNF table is in 3NF, but not vice versa.

# ⚙️ Functional Dependency (FD) — DBMS Notes

## 🔹 Definition
A **Functional Dependency** exists when **one attribute uniquely determines another attribute** in a relation (table).
---

## 🔹 Example
If in a STUDENT table:
`roll_no → name, course, branch`

This means the value of `roll_no` uniquely determines `name`, `course`, and `branch`.

🧠 **Interview Tip:**  
When explaining, say —  
> “We start with one Student table that has redundancy.  
Normalization step by step removes insertion, deletion, and update anomalies  
until every dependency is based on a superkey in BCNF.”

# 🔑 Keys in DBMS

Keys are attributes (or a set of attributes) used to **uniquely identify a record** in a table.

---

## 1️⃣ Super Key
- A **set of one or more attributes** that can uniquely identify a row.
- Can include extra, unnecessary attributes.
- Every table has at least one super key (all attributes combined).

📝 *Example*: `{id}`, `{id, email}`, `{id, phone}`

---

## 2️⃣ Candidate Key
- A **minimal super key** — no attribute can be removed without losing uniqueness.
- Ensures **uniqueness** and **minimality**.
- Every candidate key is a super key, but not every super key is a candidate key.

📝 *Example*: `{id}`, `{email}`

---

## 3️⃣ Primary Key
- The **chosen candidate key** to uniquely identify rows.
- **Unique** and **cannot be NULL**.
- Only **one** primary key per table.

---

## 4️⃣ Foreign Key
- A field in one table that **refers to the primary key** of another.
- Ensures **referential integrity** between tables.

---

## 5️⃣ Alternate Key
- **Candidate keys** that are **not chosen** as the primary key.

---

## 6️⃣ Composite Key
- A **combination of two or more attributes** used together for unique identification.

---

## 7️⃣ Unique Key
- Similar to primary key but **allows one NULL value**.
- Ensures all non-null values are **unique**.

---

# 💰 Transaction & ACID Properties — DBMS Notes

## 🔹 What is a Transaction?
A **transaction** is a single logical unit of work in a database that consists of one or more operations (like `INSERT`, `UPDATE`, `DELETE`) executed together.  

It ensures that the database remains **consistent and reliable**, even in case of failures.

🧠 Example:  
Transferring ₹1000 from **Account A** to **Account B** involves two operations:
1. Deduct ₹1000 from A’s balance.  
2. Add ₹1000 to B’s balance.  

Both must happen together — or not at all.

---

## 🔹 What is ACID?
**ACID** stands for the four key properties that guarantee reliable processing of transactions.
ACID ensures that transactions are **safe, accurate, and dependable** even with errors, crashes, or concurrent users.

---

## ⚙️ ACID Properties Explained (with Banking Example)

### 1️⃣ Atomicity — “All or Nothing”
- Ensures that **either the entire transaction is completed or none of it is**.  
- Example:  
  - When ₹1000 is transferred from A → B  
  - If ₹1000 is deducted from A **but not added to B** (due to a crash),  
  - Then **the whole transaction is rolled back** — both balances remain unchanged.  
- ❗ Prevents partial transactions.
- 
  Steps involved:
  1. **Read(A)** – Get A’s current balance.  
  2. **A = A - 1000** – Deduct ₹1000 from A.  
  3. **Write(A)** – Save A’s new balance.  
  4. **Read(B)** – Get B’s current balance.  
  5. **B = B + 1000** – Add ₹1000 to B.  
  6. **Write(B)** – Save B’s new balance.  

---

### 2️⃣ Consistency — “Valid State to Valid State”
- Ensures the database remains in a **consistent state** before and after the transaction.  
- Example:  
  - Before transfer: A = ₹5000, B = ₹2000 → Total = ₹7000  
  - After transfer: A = ₹4000, B = ₹3000 → Total still = ₹7000  
- No rule or constraint is broken (e.g., no negative balance, totals remain correct).

---

### 3️⃣ Isolation — “No Interference”
- Ensures that **multiple transactions can run at the same time** without affecting each other.  
- Example:  
  - You are transferring ₹1000 from A → B  
  - At the same time, another transaction checks A’s balance.  
  - Isolation ensures that the second transaction **won’t see intermediate results** (like ₹1000 already deducted but not yet added).  
- ❗ Prevents inconsistent or dirty reads.

---

### 4️⃣ Durability — “Permanent Once Done”
- Once a transaction is **committed**, its results are **permanently saved** — even after system failure.  
- Example:  
  - After the transfer completes, and the database commits,  
  - Even if the system crashes right after, the ₹1000 transfer remains valid when it restarts.  
- ❗ Ensures reliability after commit.

---

## 🧠 Why ACID is Important
- Maintains **data integrity** and **reliability**.  
- Prevents **data loss**, **duplication**, or **inconsistent updates**.  
- Essential for **banking, ticket booking, e-commerce**, and any system involving concurrent transactions.

---




# 📘 Indexing in DBMS

## 🔹 What is Indexing?
Indexing is a technique used in databases to improve the **speed of data retrieval** by minimizing disk scans. Instead of searching through all rows, the DBMS uses index structures to quickly locate data using key values..  
It works similar to an index in a book — instead of scanning every page, you can quickly jump to the required section.

In a database, an **index** is a separate data structure (usually stored as a B-Tree or Hash table) that keeps track of **key values** and their corresponding **record locations** in the table.

## 🔹 Index vs Indexing
The term **Index** refers to the actual data structure (like a table of key-address pairs),  
while **Indexing** refers to the **process or technique** of creating and managing such structures.

### 🧱 Dense Index
A **Dense Index** has an entry for **every record** in the table.  
Each key value is mapped directly to the address of the record.  
This makes searching very fast but increases storage size.

**Example:**  
If a STUDENT table has Roll_Nos 1, 2, and 3,  
the index will contain entries for all three.  
So searching for Roll_No = 3 goes directly to that record.  

✅ Fast search  
❌ Requires more space  
👉 Common for **secondary indexes**

---

### 🌿 Sparse Index
A **Sparse Index** keeps entries for **only some records**, usually the first record of each data block.  
The data must be **sorted** on the key.  
To find a record, DBMS finds the nearest smaller key in the index and scans forward in that block.

**Example:**  
If Block 1 has Roll_Nos 1–3, Block 2 has 4–6,  
the index stores only (1 → Block1) and (4 → Block2).

✅ Less space  
⚠️ Slightly slower  
👉 Common for **primary indexes**

---

## 🔹 Types of Indexing Techniques

### 1️⃣ Primary Index
- Built on the **primary key** of the table.  
- Data is **physically sorted** on that key.  
- Contains one index entry per data block → **Sparse Index**.  
- Efficient for range and key-based queries.

---

### 2️⃣ Secondary Index
- Built on a **non-primary attribute**.  
- Data file is **not sorted** on this field.  
- Contains entries for **every record** → **Dense Index**.  
- Used to speed up queries on non-key columns (like name or department).

---

### 3️⃣ Clustering Index
- Used when **multiple records share the same key value**.  
- Records are **grouped physically** in the data file.  
- Index stores one entry per key value, pointing to the block of all related records.  
- Useful for grouped attributes like `Department` or `Category`.

---

### 4️⃣ Multi-level Index
- When the index itself becomes large, another index is created **on the index**.  
- Forms a hierarchy (like a **B+ tree**) for faster searching.  
- Reduces disk I/O and improves efficiency in large databases.

---

## 🔹 When to Use
| Type | Used For | Data Sorted? | Example |
|------|-----------|--------------|----------|
| **Primary Index** | Primary key | ✅ Yes | Roll_No |
| **Secondary Index** | Non-key field | ❌ No | Name |
| **Clustering Index** | Grouped records | ✅ Yes | Dept |
| **Dense Index** | Small data, fast lookup | Doesn’t matter | Email |
| **Sparse Index** | Large sorted data | ✅ Yes | Roll_No |

---

## 🔹 Summary
- **Indexing** improves data retrieval by using key–address mappings.  
- **Dense Index** → Fast but large.  
- **Sparse Index** → Space-efficient but slower.  
- **Primary, Secondary, and Clustering** are indexing strategies based on key type and data order.  
- Large databases often combine these methods using **multi-level indexing** for better performance.





