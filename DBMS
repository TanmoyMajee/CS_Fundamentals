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

### 📘 Example

#### ✅ Normalized Tables
**Students**
| student_id | name |
|-------------|------|
| 1 | Tanmoy |
| 2 | Rahul |

**Courses**
| course_id | student_id | course_name |
|------------|-------------|--------------|
| 101 | 1 | DBMS |
| 102 | 1 | OS |
| 103 | 2 | CN |

**Query:**
```sql
SELECT s.name, c.course_name
FROM Students s
JOIN Courses c ON s.student_id = c.student_id;
This needs a join — which can be slow on large datasets.

⚡ Denormalized Table
StudentCourses

student_id	name	courses
1	Tanmoy	DBMS, OS
2	Rahul	CN

Query:

sql
Copy code
SELECT name, courses FROM StudentCourses;
No join needed → Faster read performance.

🧩 When to Use Denormalization
System is read-heavy (e.g., reports, dashboards).

Join queries are slowing down performance.

You need faster query response and can tolerate redundancy.


