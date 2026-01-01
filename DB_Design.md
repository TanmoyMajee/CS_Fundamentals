### 1. Users Table (`users`)
| id | name | email | plan | sessionCount | createdAt |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | John Doe | john@example.com | PREMIUM | 12 | 2023-10-01 09:00:00 |
| 2 | Jane Smith | jane@test.com | FREE | 3 | 2023-10-05 14:30:00 |
| 3 | Mike Dev | mike@code.com | FREE | 0 | 2023-11-12 10:15:00 |

### 2. Conversations Table (`conversations`)
| id | userId | title | queryCount | createdAt | updatedAt |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 101 | 1 | Q3 Financial Analysis | 5 | 2023-10-27 09:00:00 | 2023-10-27 09:45:00 |
| 102 | 1 | React Hooks Learning | 10 | 2023-10-28 11:15:00 | 2023-10-28 12:00:00 |
| 103 | 2 | Workout Plan 2024 | 2 | 2023-11-01 18:45:00 | 2023-11-01 18:50:00 |


### 3. Files Table (`files`)

| id | userId | conversationId | originalName | filetype | downloadUrl | extractedText |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 50 | 1 | 101 | financials_q3.pdf | application/pdf | https://s3.amazonaws.com/bucket/fin_q3.pdf | Q3 Revenue: $200k. Net Profit: $50k. Expenses: $150k... |
| 51 | 1 | 101 | revenue_data.csv | text/csv | https://s3.amazonaws.com/bucket/rev_data.csv | Month,Revenue\nJuly,60000\nAug,70000\nSept,70000... |
| 52 | 2 | 103 | gym_routine.pdf | application/pdf | https://s3.amazonaws.com/bucket/gym.pdf | Monday: Chest and Triceps. 3 sets of Bench Press... |
| 53 | 3 | 105 | resume_draft.docx | application/msword | https://s3.amazonaws.com/bucket/res.docx | John Doe - Software Engineer. Skills: React, Node, MySQL... |

### 4. Messages Table (`messages`)
| id | conversationId | sender | content | modelUsed | tokenCount | contextChunks |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 5001 | 101 | HUMAN | What is the total net profit? | NULL | 15 | NULL |
| 5002 | 101 | AI | The net profit is $50,000. | gpt-4 | 45 | `["ck_88", "ck_92"]` |
| 5003 | 102 | HUMAN | Explain useEffect hook. | NULL | 12 | NULL |
| 5004 | 102 | AI | useEffect handles side effects... | gpt-3.5 | 120 | `["ck_12"]` |

```mysql

) MySQL Database Tables (Updated Design)
Users Table
CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  plan ENUM('FREE','PREMIUM') DEFAULT 'FREE',
  sessionCount INT DEFAULT 0,
  lastSessionDate DATE,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

Conversations Table
CREATE TABLE IF NOT EXISTS conversations (
  id INT AUTO_INCREMENT PRIMARY KEY,
  userId INT NOT NULL,
  title VARCHAR(255),
  queryCount INT DEFAULT 0,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (userId) REFERENCES users(id)
);

Files Table (important change 👉 stores extractedText)
CREATE TABLE IF NOT EXISTS files (
  id INT AUTO_INCREMENT PRIMARY KEY,
  userId INT NOT NULL,
  conversationId INT NOT NULL,
  originalName VARCHAR(255),
  filetype VARCHAR(100),
  downloadUrl TEXT,
  extractedText LONGTEXT,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (userId) REFERENCES users(id),
  FOREIGN KEY (conversationId) REFERENCES conversations(id)
);

Messages Table
CREATE TABLE IF NOT EXISTS messages (
  id INT AUTO_INCREMENT PRIMARY KEY,
  conversationId INT NOT NULL,
  sender ENUM('HUMAN','AI'),
  content LONGTEXT,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (conversationId) REFERENCES conversations(id)
);

````
