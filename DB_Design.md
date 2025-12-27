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
| id | userId | conversationId | originalName | filetype | downloadUrl |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 50 | 1 | 101 | financials_q3.pdf | application/pdf | https://storage.../fin_q3.pdf |
| 51 | 1 | 101 | revenue_data.csv | text/csv | https://storage.../rev_data.csv |
| 52 | 2 | 103 | gym_routine.docx | application/docx | https://storage.../gym.docx |

### 4. Messages Table (`messages`)
| id | conversationId | sender | content | modelUsed | tokenCount | contextChunks |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 5001 | 101 | HUMAN | What is the total net profit? | NULL | 15 | NULL |
| 5002 | 101 | AI | The net profit is $50,000. | gpt-4 | 45 | `["ck_88", "ck_92"]` |
| 5003 | 102 | HUMAN | Explain useEffect hook. | NULL | 12 | NULL |
| 5004 | 102 | AI | useEffect handles side effects... | gpt-3.5 | 120 | `["ck_12"]` |

```pgsql
-- 1. Create Enums (PostgreSQL requires explicit types)
CREATE TYPE "Sender" AS ENUM ('HUMAN', 'AI');
CREATE TYPE "FileStatus" AS ENUM ('PROCESSING', 'COMPLETED', 'FAILED', 'DELETED');

-- 2. Create Users Table
CREATE TABLE "users" (
    "id" SERIAL PRIMARY KEY,  -- Uses SERIAL for auto-increment
    "name" TEXT NOT NULL,
    "email" TEXT NOT NULL UNIQUE,
    "password" TEXT NOT NULL,
    "createdAt" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" TIMESTAMP NOT NULL,
    "image" TEXT,
    "plan" TEXT NOT NULL DEFAULT 'FREE',
    "planExpiry" TIMESTAMP,
    "sessionCount" INTEGER NOT NULL DEFAULT 0,
    "lastSessionDate" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- 3. Create Conversations Table
CREATE TABLE "conversations" (
    "id" SERIAL PRIMARY KEY,
    "title" TEXT,
    "createdAt" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" TIMESTAMP NOT NULL,
    "queryCount" INTEGER NOT NULL DEFAULT 0,
    "userId" INTEGER NOT NULL,
    CONSTRAINT "conversations_userId_fkey" FOREIGN KEY ("userId") REFERENCES "users"("id") ON DELETE CASCADE
);

-- 4. Create Files Table
CREATE TABLE "files" (
    "id" SERIAL PRIMARY KEY,
    "filename" TEXT NOT NULL,
    "originalName" TEXT NOT NULL,
    "filetype" TEXT NOT NULL,
    "fileSize" INTEGER,
    "downloadUrl" TEXT NOT NULL,
    "imageUrl" TEXT,
    "createdAt" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" TIMESTAMP NOT NULL,
    "userId" INTEGER NOT NULL,
    "conversationId" INTEGER NOT NULL,
    CONSTRAINT "files_userId_fkey" FOREIGN KEY ("userId") REFERENCES "users"("id") ON DELETE CASCADE,
    CONSTRAINT "files_conversationId_fkey" FOREIGN KEY ("conversationId") REFERENCES "conversations"("id") ON DELETE CASCADE
);

-- 5. Create Messages Table
CREATE TABLE "messages" (
    "id" SERIAL PRIMARY KEY,
    "content" TEXT NOT NULL,
    "sender" "Sender" NOT NULL,
    "tokenCount" INTEGER,
    "responseTime" INTEGER,
    "contextChunks" TEXT[],  -- PostgreSQL supports native Arrays
    "modelUsed" TEXT,
    "createdAt" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "conversationId" INTEGER NOT NULL,
    CONSTRAINT "messages_conversationId_fkey" FOREIGN KEY ("conversationId") REFERENCES "conversations"("id") ON DELETE CASCADE
);
````
```mysql
-- 1. Users Table
CREATE TABLE `users` (
    `id` INT AUTO_INCREMENT PRIMARY KEY, -- CHANGE: Use AUTO_INCREMENT instead of SERIAL
    `name` VARCHAR(255) NOT NULL,
    `email` VARCHAR(255) NOT NULL UNIQUE,
    `password` VARCHAR(255) NOT NULL,
    `createdAt` DATETIME DEFAULT CURRENT_TIMESTAMP,
    `updatedAt` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- CHANGE: Auto-update trigger
    `image` VARCHAR(255),
    `plan` VARCHAR(50) NOT NULL DEFAULT 'FREE',
    `planExpiry` DATETIME,
    `sessionCount` INT NOT NULL DEFAULT 0,
    `lastSessionDate` DATETIME DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;

-- 2. Conversations Table
CREATE TABLE `conversations` (
    `id` INT AUTO_INCREMENT PRIMARY KEY,
    `title` VARCHAR(255),
    `createdAt` DATETIME DEFAULT CURRENT_TIMESTAMP,
    `updatedAt` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    `queryCount` INT NOT NULL DEFAULT 0,
    `userId` INT NOT NULL,
    CONSTRAINT `conversations_userId_fkey` FOREIGN KEY (`userId`) 
        REFERENCES `users`(`id`) ON DELETE CASCADE
) ENGINE=InnoDB;

-- 3. Files Table
CREATE TABLE `files` (
    `id` INT AUTO_INCREMENT PRIMARY KEY,
    `filename` VARCHAR(255) NOT NULL,
    `originalName` VARCHAR(255) NOT NULL,
    `filetype` VARCHAR(100) NOT NULL,
    `fileSize` INT,
    `downloadUrl` TEXT NOT NULL,
    `imageUrl` TEXT,
    `createdAt` DATETIME DEFAULT CURRENT_TIMESTAMP,
    `updatedAt` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    `userId` INT NOT NULL,
    `conversationId` INT NOT NULL,
    CONSTRAINT `files_userId_fkey` FOREIGN KEY (`userId`) 
        REFERENCES `users`(`id`) ON DELETE CASCADE,
    CONSTRAINT `files_conversationId_fkey` FOREIGN KEY (`conversationId`) 
        REFERENCES `conversations`(`id`) ON DELETE CASCADE
) ENGINE=InnoDB;

-- 4. Messages Table
CREATE TABLE `messages` (
    `id` INT AUTO_INCREMENT PRIMARY KEY,
    `content` TEXT NOT NULL,
    `sender` ENUM('HUMAN', 'AI') NOT NULL, -- CHANGE: Enums are defined inline
    `tokenCount` INT,
    `responseTime` INT,
    `contextChunks` JSON, -- CHANGE: Arrays replaced with JSON type
    `modelUsed` VARCHAR(100),
    `createdAt` DATETIME DEFAULT CURRENT_TIMESTAMP,
    `conversationId` INT NOT NULL,
    CONSTRAINT `messages_conversationId_fkey` FOREIGN KEY (`conversationId`) 
        REFERENCES `conversations`(`id`) ON DELETE CASCADE
) ENGINE=InnoDB;
````

