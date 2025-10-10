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
