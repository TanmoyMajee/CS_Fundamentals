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

## 🧱 Step-by-Step Normalization Example

Let’s start with a single **Student table** and normalize it step by step.

---

### 🧩 Unnormalized Table (UNF)

| StudentID | StudentName | Course | Instructor | InstructorPhone |
|------------|--------------|---------|-------------|------------------|
| 1 | Rahul | DBMS, OS | Dr. Sen, Dr. Roy | 9999, 8888 |
| 2 | Priya | DBMS | Dr. Sen | 9999 |
| 3 | Aman | CN, OS | Dr. Das, Dr. Roy | 7777, 8888 |

🔸 **Problems:**  
- Multiple values in one cell (non-atomic).  
- Repetition of instructor data.  
- Hard to update or delete cleanly.

---

### 🔹 1NF — First Normal Form

**Rule:**  
- Each column should have atomic (single) values.  
- No repeating groups or arrays.

**👉 Convert to 1NF:**

| StudentID | StudentName | Course | Instructor | InstructorPhone |
|------------|--------------|---------|-------------|------------------|
| 1 | Rahul | DBMS | Dr. Sen | 9999 |
| 1 | Rahul | OS | Dr. Roy | 8888 |
| 2 | Priya | DBMS | Dr. Sen | 9999 |
| 3 | Aman | CN | Dr. Das | 7777 |
| 3 | Aman | OS | Dr. Roy | 8888 |

✅ **Now all cells are atomic.**

---

### 🔹 2NF — Second Normal Form

**Rule:**  
- Must be in 1NF.  
- No **partial dependency** — non-key attributes should depend on the whole primary key, not part of it.

If we consider `(StudentID, Course)` as the **composite primary key**,  
then attributes like `StudentName` depend only on `StudentID`, not the full key.

**👉 Convert to 2NF:**

**Student Table:**

| StudentID | StudentName |
|------------|--------------|
| 1 | Rahul |
| 2 | Priya |
| 3 | Aman |

**Enrollment Table:**

| StudentID | Course | Instructor | InstructorPhone |
|------------|---------|-------------|------------------|
| 1 | DBMS | Dr. Sen | 9999 |
| 1 | OS | Dr. Roy | 8888 |
| 2 | DBMS | Dr. Sen | 9999 |
| 3 | CN | Dr. Das | 7777 |
| 3 | OS | Dr. Roy | 8888 |

✅ Partial dependency removed.

---

### 🔹 3NF — Third Normal Form

**Rule:**  
- Must be in 2NF.  
- No **transitive dependency** (non-key attribute depends on another non-key attribute).

Here, `InstructorPhone` depends on `Instructor`, not directly on `Course`.

**👉 Convert to 3NF:**

**Instructor Table:**

| Instructor | InstructorPhone |
|-------------|------------------|
| Dr. Sen | 9999 |
| Dr. Roy | 8888 |
| Dr. Das | 7777 |

**Course Table:**

| Course | Instructor |
|---------|-------------|
| DBMS | Dr. Sen |
| OS | Dr. Roy |
| CN | Dr. Das |

**Enrollment Table:**

| StudentID | Course |
|------------|---------|
| 1 | DBMS |
| 1 | OS |
| 2 | DBMS |
| 3 | CN |
| 3 | OS |

✅ Transitive dependency removed (InstructorPhone moved out).

---

### 🔹 BCNF — Boyce-Codd Normal Form

**Rule:**  
- Must be in 3NF.  
- For every functional dependency **X → Y**, X should be a **super key**.

Suppose one instructor can teach multiple courses,  
and each course can also be handled by multiple instructors (many-to-many).

**CourseInstructor Table (before BCNF):**

| Course | Instructor |
|---------|-------------|
| DBMS | Dr. Sen |
| DBMS | Dr. Roy |
| CN | Dr. Das |

Here, neither `Course` nor `Instructor` alone is a super key.  
So, this violates BCNF.

**👉 Convert to BCNF:**

**Course Table:**

| CourseID | CourseName |
|-----------|-------------|
| C1 | DBMS |
| C2 | OS |
| C3 | CN |

**Instructor Table:**

| InstructorID | InstructorName | Phone |
|---------------|----------------|--------|
| I1 | Dr. Sen | 9999 |
| I2 | Dr. Roy | 8888 |
| I3 | Dr. Das | 7777 |

**CourseInstructor Table:**

| CourseID | InstructorID |
|-----------|---------------|
| C1 | I1 |
| C1 | I2 |
| C3 | I3 |

**Enrollment Table:**

| StudentID | CourseID |
|------------|-----------|
| 1 | C1 |
| 1 | C2 |
| 2 | C1 |
| 3 | C3 |
| 3 | C2 |

✅ Now every dependency has a super key → satisfies BCNF.

---

## 🧮 Summary Table

| **Normal Form** | **Condition** | **Problem Solved** |
|------------------|----------------|--------------------|
| **1NF** | Atomic values only | Removes repeating groups |
| **2NF** | No partial dependency | Removes partial dependency |
| **3NF** | No transitive dependency | Removes transitive dependency |
| **BCNF** | Every determinant is a super key | Removes advanced anomalies |

---

## 💬 Real-Life Analogy

Think of a **college database**:
- 1NF: One student-course per row.  
- 2NF: Separate student details.  
- 3NF: Move instructor and phone separately.  
- BCNF: Use unique IDs for every instructor and course.

✅ This ensures minimal redundancy, easy updates, and data consistency.

---

## 📘 Interview Tip

While answering in interviews:
1. Start by defining **Normalization** and why it’s needed.  
2. Mention **anomalies** (Insertion, Deletion, Update).  
3. Explain each **Normal Form** briefly.  
4. Use one **consistent example** (like Student–Course–Instructor).  
5. End by saying BCNF ensures every dependency is based on a **super key**.

---



