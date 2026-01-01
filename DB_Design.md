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

```js
export async function uploadPdfHandler(req: Request, res: Response) {
  const connection = await pool.getConnection();
  let firebaseUrl: string | null = null;

  try {
    await connection.beginTransaction();

    if (!req.file) {
      return res.status(400).json({ msg: "No file uploaded" });
    }

    const userId = req.user.id;
    const { buffer, originalname, mimetype } = req.file;

    // get user plan
    const [userRows]: any = await connection.query(
      `SELECT plan FROM users WHERE id = ?`,
      [userId]
    );

    const plan = userRows[0].plan; // FREE or PREMIUM

    let conversationId = req.body.conversationId;

    // if conversation not provided → create
    if (!conversationId) {
      const [convResult]: any = await connection.query(
        `INSERT INTO conversations (title, userId) VALUES (?, ?)`,
        [originalname, userId]
      );
      conversationId = convResult.insertId;
    }

    // check how many files already uploaded in this conversation
    const [countRows]: any = await connection.query(
      `SELECT COUNT(*) AS fileCount FROM files WHERE conversationId = ?`,
      [conversationId]
    );

    const fileCount = countRows[0].fileCount;

    // enforce limits
    const limit = plan === "PREMIUM" ? 10 : 3;

    if (fileCount >= limit) {
      return res.status(403).json({
        msg: `File upload limit reached for your plan`,
        plan,
        limit,
      });
    }

    // upload file to Firebase
    firebaseUrl = await FirebaseFileUpload(
      buffer,
      originalname,
      mimetype,
      userId
    );

    // extract pdf text
    const parsed = await pdfParse(buffer);
    const extractedText = parsed.text;

    // save file row
    await connection.query(
      `
      INSERT INTO files 
      (userId, conversationId, originalName, filetype, downloadUrl, extractedText)
      VALUES (?, ?, ?, ?, ?, ?)
      `,
      [
        userId,
        conversationId,
        originalname,
        mimetype,
        firebaseUrl,
        extractedText,
      ]
    );

    await connection.commit();

    return res.status(200).json({
      msg: "File uploaded successfully",
      conversationId,
      plan,
      filesUsed: fileCount + 1,
      limit,
    });
  } catch (err) {
    await connection.rollback();
    console.error(err);
    return res.status(500).json({ msg: "Upload failed" });
  } finally {
    connection.release();
  }
}



export async function getAnswerHandler(req: Request, res: Response) {
  const connection = await pool.getConnection();

  try {
    await connection.beginTransaction();

    const { query, conversationId } = req.body;
    const userId = req.user.id;

    // ownership validation
    const [convRows]: any = await connection.query(
      `SELECT userId FROM conversations WHERE id = ?`,
      [conversationId]
    );

    if (!convRows.length || convRows[0].userId !== userId) {
      return res.status(403).json({ msg: "Not authorized" });
    }

    // get ALL file texts in this conversation
    const [fileRows]: any = await connection.query(
      `SELECT extractedText FROM files WHERE conversationId = ? ORDER BY id ASC`,
      [conversationId]
    );

    if (!fileRows.length) {
      return res
        .status(400)
        .json({ msg: "No PDF uploaded for this conversation" });
    }

    // merge all text
    const mergedText = fileRows
      .map((row: any) => row.extractedText || "")
      .join("\n\n");

    const model = genAI.getGenerativeModel({
      model: "gemini-2.5-flash",
    });

    const prompt = `
Answer ONLY using the following document content.
If not present, reply exactly: "Information not found in document."

Document:
${mergedText}

Question:
${query}
`;

    const result = await model.generateContent(prompt);
    const answer = result.response.text();

    // save messages
    await connection.query(
      `INSERT INTO messages (conversationId, sender, content)
       VALUES (?, 'HUMAN', ?)`,
      [conversationId, query]
    );

    await connection.query(
      `INSERT INTO messages (conversationId, sender, content)
       VALUES (?, 'AI', ?)`,
      [conversationId, answer]
    );

    await connection.query(
      `UPDATE conversations SET queryCount = queryCount + 1 WHERE id = ?`,
      [conversationId]
    );

    await connection.commit();

    res.status(200).json({ answer });
  } catch (err) {
    await connection.rollback();
    console.error(err);
    res.status(500).json({ msg: "Query failed" });
  } finally {
    connection.release();
  }
}



In my project, when the user uploads a PDF, I extract the text using pdf-parse and store it in MySQL.

When they ask a question, I fetch the stored text and send both the user question and the document text in a single prompt to Gemini API.

The model is instructed to answer only from the document text.
I store both human and AI messages in a messages table.

If asked about large docs:

For large documents, the limitation is model context size.
In future I would move to RAG with vector search, but for now this approach keeps the system simple and easy to maintain.

````
File upload flow

When the user uploads a PDF, the frontend sends it to the backend.
The backend first checks the user’s plan and the conversation.
If the conversation already has files, I count how many are uploaded.
Based on the plan, I enforce limits:
– free users can upload up to 3 PDFs per conversation
– premium users up to 10 PDFs

After validation, I upload the file to Firebase, extract the PDF text, and store that text in the database inside the files table along with the conversationId. This allows one conversation to have multiple files.

💬 Query / answer flow

When the user asks a question, I receive the query and the conversationId.
First, I check authorization to ensure the conversation belongs to the user.
Then I fetch all PDFs’ extracted text linked to that conversation.
I merge all that text together and send it to Gemini along with the query using a custom prompt that forces it to answer only from the document content.
The generated answer is stored in the messages table and returned to the user.

🎯 Key ideas you can say confidently

conversations now support multiple files

plan controls how many files

AI answers from all files together

extracted text stored in DB avoids reprocessing

LIMIT problem solved, not ignored

you’re aware of token limits & tradeoffs


