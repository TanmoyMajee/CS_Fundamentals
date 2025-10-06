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

**How I Implemented the Retrieval Chain:**

*Initialization:*
>I began by initializing the LLM model and setting up a ChatPromptTemplate (a LangChain class) that defines how the model should respond — for example, to be concise and mention when there isn’t enough information in the context.

*Document Chain:*
>Then I used the createStuffDocumentsChain() function from LangChain. This function combines the LLM and the prompt into a single chain that processes the retrieved documents and generates a context-aware response.

*Retrieval Chain:*
>After that, I used the createRetrievalChain() function, which connects the document chain with a retriever (from the vector store). The retriever is responsible for fetching the most relevant document chunks from the vector database.

*Execution:*
>Finally, I invoked the retrievalChain by passing the user’s query as input. The chain first retrieves the relevant context using the retriever, then passes it through the document chain to generate a final answer.

**Challanges to  INtegrate AI Model **
>In my project, I utilized two primary AI models. First, I used the Hugging Face Sentence Transformer to generate embeddings, which helped in converting text into meaningful vector representations. Secondly, I leveraged the DeepSeek LLM to generate the final responses.

>In my project, I first used LangChain’s default HuggingFaceInference class. Later, Hugging Face updated their system to use something called “Inference Providers,” where you have to mention the provider name (like novita or together) in the API request. The default class didn’t allow adding a custom provider name, so it stopped working properly with the new setup.

>To solve this, I built my own custom LLM class where I could set the provider, endpoint, headers, and retry logic manually. This made the integration work smoothly with Hugging Face’s updated policy.


**What is Prisma & Why did u use it **
>Prisma is an open-source ORM (Object Relational Mapping tool) that lets us write database queries in JavaScript or TypeScript instead of raw SQL, making them more readable and type-safe. It also simplifies migrations, helping us manage schema changes effortlessly and reducing errors. 



**15.	If I upload a very large PDF, how does your system handle it?**
>If a very large document is uploaded, the first step is file handling.
I use Multer with memoryStorage in my Node.js backend to temporarily store the uploaded PDF in memory.
Then LangChain’s PDFLoader extracts the text, and a RecursiveCharacterTextSplitter breaks it into smaller overlapping chunks.
Each chunk gets its own embedding vector and is stored in Qdrant.
Because we only retrieve a few most relevant chunks when answering, the size of the PDF doesn’t slow down the query or overload the model.
This chunking and retrieval strategy lets the system handle very large documents efficiently within the LLM’s context limits.

24.	How do you avoid LLM hallucination — ensuring answers come from the PDF?
>To avoid LLM hallucination and ensure answers come only from the PDF, I use a retrieval-augmented generation (RAG) setup. First, I convert the PDF into vector embeddings and store them in a vector database (Qdrant). Then, when a user asks a question, a retriever fetches only the most relevant chunks from the PDF. The LLM receives both the query and these retrieved chunks as context. I also use a prompt template that instructs the LLM to answer only using the provided context and to respond with “I don’t have enough information” if the context is insufficient. This way, the LLM is grounded in the PDF content and doesn’t make up information.

** future,  improvement:**

*Multi-Document Support:*
>Right now, the system works on a single PDF per conversation. In the future, it could handle multiple PDFs or documents at once, allowing the retriever to fetch relevant information from all sources. This would make the system more flexible and useful for larger datasets.

*Caching Frequent Queries:*
>For repeated or common questions, we could cache the answers so the system doesn’t have to query the vector store and LLM every time. This would reduce response time and save API usage, improving performance.

*Streaming Responses:*
>Currently, the LLM generates the full answer before sending it back. By streaming the response, users could see the answer as it’s being generated, making the system feel faster and more interactive.

17.	Can your app handle multiple PDFs at the same time? If not, how would you extend it?
19.	Why k=3 chunks in retrieval? How did you decide?
21.	How do you chunk PDF text? (chunk size and overlap)
22.	How do you create embeddings and which model did you use?
23.	Why Qdrant and how many chunks do you retrieve?
25.	How do you scale the system for large documents or many users?
26.	How do you store and secure user PDFs?
27.	What are the main limitations 
28.	•  What is HuggingFace and why did you use it?
29.	•  What is LangChain and why did you use it?
30.	•  What is a retriever chain? How does it work?
31.	•  How do you create embeddings for documents and queries?
32.	•  How do you split documents into chunks? Why is it needed?
33.	•  What is Qdrant and why did you choose it as a vector database?
34.	•  How do you manage multiple sessions in your app?
35.	•  How do you ensure the model only answers from the given context?
36.	•  How do you handle chat history storage?
37.	•  What are alternatives to HuggingFace, LangChain, or Qdrant?
38.	•  How do you perform similarity search in the vector DB?
39.	•  How do you handle scalability if many users upload documents?



***************************************************************

________________________________________
🔹 Project 2: Chat App (Real-time Messaging)
** 1.	Can you explain your Chat App project? **

>I built a real-time chat application that allows users to chat both one-to-one and in groups. For authentication, I used JWT along with Firebase, which also supports Google login, to keep sessions secure. I implemented protected routes so that only authenticated users can access the chat features. The app also shows an online and offline indicator for users, and with Socket.IO, messages are delivered instantly. All the chat history is stored in MongoDB so that users can continue conversations where they left off.”

>What I Learned:
“Through this project, I gained hands-on experience with real-time communication using Socket.IO, and improved my understanding of integrating authentication and managing protected routes.”

>Tech Stack:
“For the tech stack, I used React with Tailwind CSS for the frontend, Node.js with Express for the backend, MongoDB with Mongoose for the database, and Socket.IO for real-time messaging.”

**3.	Socket.IO **
*✅ What is Socket.IO?*
>Socket.IO is a JavaScript library that enables real-time, bidirectional communication between the client and server. It builds on top of WebSockets and includes features like automatic reconnections and fallback mechanisms.

*✅ Why did you use Socket.IO instead of normal API calls?*
>In my chat app, real-time updates are crucial. With normal API calls, the client would need to constantly poll the server, which is inefficient and introduces latency. Socket.IO, on the other hand, keeps a persistent connection, allowing the server to instantly push messages to the client. This results in a smoother and more responsive chat experience.

>Short Polling: The client repeatedly sends requests to the server at regular intervals, even if there’s no new data. This can cause unnecessary traffic and load.

>Long Polling: The client sends a request, and the server holds that request open until new data is available. Once the server responds, the client immediately sends another request. This reduces unnecessary traffic but still keeps the connection open for new data.

>WebSockets: The client and server establish a continuous, two-way connection. Once established, either side can send messages at any time without the need for repeated requests. This is the most efficient and real-time method.

5.	How do you handle real-time updates in React?
6.	How do you differentiate between one-to-one chat and group chat?
7.	How do you store chats in MongoDB? What is the schema design? how did u retrive  chat history?
8.	How do you make sure only logged-in users can send/receive messages?
9.	How does JWT authentication work in your app?
10.	Why did you use Firebase in addition to JWT?
11.	If many users are online at the same time, how does your app scale?
12.	What challenges did you face while building this app?
13.	How do you show the online/offline indicator?
14.	How would you scale your chat app if thousands of users are online?
15.	How would you add features like typing indicators or read receipts?
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

** What is the role in this project **
>I was the sole developer for this project,handling everything from development to deployment.

1.	Which project are you most proud of, and why?
2.	What’s the difference between WebSockets and HTTP?
3.	why u use react not angular ? Why did you choose MongoDB? or chose mern stack 
5.	Which project was the most challenging?
6.	How did you debug issues when something wasn’t working?
7.	How do you ensure your projects are scalable and maintainable?
8.	If you had more time, what improvements would you add to your projects?

