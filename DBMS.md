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




