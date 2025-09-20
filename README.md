# Project_Q-A

🔹 **Project 1: RagIt (PDF Chat Application)**

**1. Can you explain your project RagIt in simple words?**

> “Sure, I’ll explain. RagIt is an **AI-powered Document Q&A platform** that allows users to upload a document and ask natural language questions to get context-aware answers.”

**Transition to Motivation**

- 👉 _“The idea behind building this was…”_
- 👉 _“…to help users quickly get answers from research papers or make notes from long e-books without reading every page”_

**Transition to Technical Highlights**

- 👉 “From the technical side, I implemented a RAG pipeline…”
- 👉 “…where the document is split into chunks, converted into embeddings, stored in Qdrant for vector similarity search, and then LangChain with a HuggingFace LLM generates accurate, context-aware responses.
- The platform also keeps chat history and even supports free and premium usage plans.”

**Transition to Tech Stack**

- 👉 “For the tech stack, I used React on the frontend, Node.js with Express on the backend, Prisma ORM for database management, Qdrant as the vector database, and Firebase for storage and authentication.
- Both the frontend and backend are built with TypeScript instead of plain JavaScript, which gave me better type safety and fewer runtime errors.”



**2.	Why did you choose this project idea? // Motivation**

>“I chose this idea because going through long documents is time-consuming. This project makes it easier to quickly extract answers — for example, from research papers or e-books — without reading every page.”

**3. Can you explain how your RAG application works? Flow**

**How does the system work step by step when a user uploads a PDF and asks a question?**

> When a user uploads a document, the backend extracts the text and splits it into chunks. Each chunk is converted into embeddings using a HuggingFace sentence transformer model and saved inside Qdrant, which is a vector database. For every new conversation, I create a separate collection in Qdrant based on the conversation ID, so that the data from one session doesn’t get mixed up with another. This way each chat stays independent and organized.

> When the user asks a question, we create an embedding for that query and use Qdrant to fetch the most relevant chunks. Then I built a retrieval chain in LangChain, where those chunks are combined with a custom prompt. The prompt makes sure the model only answers from the given context. Finally, a HuggingFace LLM generates the response, and both the user’s question and the AI’s answer are stored in the database to maintain chat history.




**4.	What is an embedding? Why do you need it?**

>   “Embeddings are numerical vector representations of text that capture the meaning of the text. In my project, each chunk of the document gets its own embedding vector based on its content. Then, when the user asks a question, that question is also turned into an embedding. By comparing these vectors, the system can find the most similar chunks in the document and pass them to the model. That’s how semantic search works here.”
  	
**6.	Why did you use Qdrant instead of MongoDB or MySQL?**
>   I used Qdrant because it’s designed for vector similarity search, which is essential for retrieving relevant chunks from embeddings. Relational databases like MySQL or Postgres aren’t built for that use case. In my project, Qdrant handled the embeddings, while Prisma with Postgres managed structured data like users, sessions, and messages

**8.	What is the role of LangChain in your project?**
> “LangChain is an open-source framework that helps build applications powered by LLMs. In my project, its role was to connect different components of the RAG pipeline.
For example, I used classes like PDFLoader to read documents, RecursiveCharacterTextSplitter to break them into smaller chunks, and HuggingFaceTransformersEmbeddings to generate embeddings. On the other side, LangChain provides functions like createRetrievalChain and createStuffDocumentsChain, which help me connect the retriever with my LLM.
So overall, LangChain made it easy to orchestrate the whole flow — from loading the document, splitting it, generating embeddings, storing them, and finally retrieving the right chunks to pass into the LLM for answering.”
  	
**10.	what is HuggingFace , what is the role in yr project?**
>    “HuggingFace is a platform that provides pre-trained NLP models. In my project, I used their embedding model Xenova/all-MiniLM-L6-v2 (a sentence transformer) to convert document chunks and user queries into vectors, and their LLM (DeepSeek) to generate natural language answers. So, HuggingFace powered both the embedding generation and the answer generation parts of my RAG pipeline
   	
**13.	What challenges did you face while building RagIt?**
>    One major challenge was setting up the retrieval chain, which is the main augmentation step. Fetching the right chunks, combining them with a custom prompt, and then passing everything to the LLM was tricky to understand at first. To tackle this, I first built a small demo project to test the retrieval chain separately before integrating it into the main app.

>Another challenge was keeping every conversation’s data isolated. To prevent embeddings from different sessions from mixing, I created session-based collections in Qdrant, so each conversation stores its own vectors and can be retrieved independently.

>Finally, while implementing a plan-based payment system with Stripe, I had to build middleware to track how many sessions a user can create and how many queries are allowed per plan. Integrating these checks with Prisma and Stripe’s APIs required careful backend logic

15.	If I upload a very large PDF, how does your system handle it?
16.	Can your app handle multiple PDFs at the same time? If not, how would you extend it?
17.	How would you improve RagIt in the future?
18.	Why k=3 chunks in retrieval? How did you decide?
19.	How do you ensure answers don’t hallucinate outside the document? (→ your prompt already addresses this)
20.	How do you chunk PDF text? (chunk size and overlap)
21.	How do you create embeddings and which model did you use?
22.	Why Qdrant and how many chunks do you retrieve?
23.	How do you avoid LLM hallucination — ensuring answers come from the PDF?
24.	How do you scale the system for large documents or many users?
25.	How do you store and secure user PDFs?
26.	What are the main limitations and future improvements?
27.	•  What is HuggingFace and why did you use it?
28.	•  What is LangChain and why did you use it?
29.	•  What is a retriever chain? How does it work?
30.	•  How do you create embeddings for documents and queries?
31.	•  How do you split documents into chunks? Why is it needed?
32.	•  What is Qdrant and why did you choose it as a vector database?
33.	•  How do you manage multiple sessions in your app?
34.	•  How do you ensure the model only answers from the given context?
35.	•  How do you handle chat history storage?
36.	•  What are alternatives to HuggingFace, LangChain, or Qdrant?
37.	•  How do you perform similarity search in the vector DB?
38.	•  How do you handle scalability if many users upload documents?



***************************************************************

________________________________________
🔹 Project 2: Chat App (Real-time Messaging)
1.	Can you explain your Chat App project?
2.	Why did you use Socket.IO instead of normal API calls?
3.	How do you handle real-time updates in React?
4.	How do you differentiate between one-to-one chat and group chat?
5.	How do you store chats in MongoDB? What is the schema design?
6.	How do you make sure only logged-in users can send/receive messages?
7.	How does JWT authentication work in your app?
8.	If many users are online at the same time, how does your app scale?
9.	What challenges did you face while building this app?
10.	How would you add features like typing indicators or read receipts?
________________________________________
🔹 Project 3: Blog Application (MERN Blog Platform)
1.	Can you explain your Blog App project?
2.	How did you implement CRUD operations for posts?
3.	How does your JWT authentication work in this app?
4.	How do you handle image uploads?
5.	How did you build the comment section?
6.	How do you ensure only the post owner can edit or delete a post?
7.	What challenges did you face while building the blog?
8.	If multiple users update the same post at the same time, what happens?
9.	Why did you choose the MERN stack for this project?
10.	How would you improve this blog app in the future?
________________________________________
🔹 General Project Questions (for any project)
1.	Which project are you most proud of, and why?
2.	Which project was the most challenging?
3.	How did you debug issues when something wasn’t working?
4.	How do you ensure your projects are scalable and maintainable?
5.	If you had more time, what improvements would you add to your projects?

