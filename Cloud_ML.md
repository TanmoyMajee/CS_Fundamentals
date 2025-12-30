# AI / ML / Generative AI – BASIC Interview Notes (TCS Digital)

## 1. What is Artificial Intelligence (AI)?
Artificial Intelligence is a field of computer science where we build systems that can perform tasks that normally require human intelligence.

These tasks include:
- Learning from data
- Recognizing patterns
- Making decisions
- Understanding language

**Example:**
- Face recognition
- Voice assistants
- Recommendation systems

---

## 2. What is Machine Learning (ML)?
Machine Learning is a subset of AI where machines learn from data without being explicitly programmed.

The model is trained using data to recognize patterns and make predictions.

---

## 3. Types of Machine Learning

### a) Supervised Learning
- Data is **labeled**
- Model learns input → output mapping

**Examples of algorithms:**
- Linear Regression
- Logistic Regression
- Decision Tree

**Use cases:**
- Email spam detection
- House price prediction

---

### b) Unsupervised Learning
- Data is **not labeled**
- Model finds hidden patterns

**Examples of algorithms:**
- K-Means Clustering
- Hierarchical Clustering
- PCA (Dimensionality Reduction)

**Use cases:**
- Customer segmentation
- Grouping similar data

---

## 4. Linear Regression
Linear Regression is a supervised learning algorithm used to predict continuous values.

**Example:**
- Predicting house price based on size
- 
Key Points:

It can handle one or multiple input features.

The algorithm tries to find the best-fitting linear line that minimizes the difference between predicted and actual values.

Equation:
The relationship is represented by the equation y = mx + b, where:
y is the predicted value.
m is the slope (coefficient) that shows the impact of the input feature.
x is the input feature.
b is the intercept, the value of y when x is zero.

Goal:
The aim is to draw a line that best fits the data, minimizing the difference between the predicted and actual values.
---

## 5. Logistic Regression
Logistic Regression is a supervised learning algorithm used for classification problems.

**Example:**
- Spam vs Not Spam email detection

Sigmoid Function: The algorithm uses the sigmoid function to map any input value to a probability between 0 and 1. The sigmoid function is defined as σ(z) = 1 / (1 + e^(-z)), where z is a linear combination of the input features.
Decision Boundary: A common threshold is 0.5. If the predicted probability is greater than 0.5, the class is 1; otherwise, it is 0.
Model Training: The algorithm learns the coefficients (weights) that best fit the data by minimizing the difference between the predicted probabilities and the actual class labels.
---

## 6. Decision Tree
A Decision Tree is a supervised learning algorithm that makes decisions using a tree-like structure.Each internal node represents a decision based on an input feature, and each leaf node represents an outcome or class label.e final nodes (leaves) represent the predicted class or outcome

** challanges  overfit **

**Example:**
- Loan approval decision
- Student pass/fail prediction
  
## Random Forest Classifier:
Random Forest is an ensemble learning method that combines multiple decision trees to improve predictive performance and reduce overfitting. Each tree is trained on a random subset of the data and features, and the final prediction is made by aggregating the predictions of all the trees, typically by majority vote.

## Ensemble Learning
Ensemble learning is a technique that combines multiple machine learning models to produce a better overall performance.

## Overfitting:
Overfitting occurs when a model learns the training data too well, capturing noise and details that don’t generalize to new data. This results in high accuracy on training data but poor performance on unseen data.

## Underfitting:
Underfitting happens when a model is too simple to capture the underlying patterns in the data, leading to poor performance on both the training data and new data

## Regularization: A technique to reduce overfitting by penalizing complex models.

---

## 7. What is Generative AI?
Generative AI is a type of AI that can generate new content such as text, images, code, or music based on learned data. 

**Examples:**
- ChatGPT (text generation)
- DALL·E (image generation)

---

## 8. What is Agentic AI?
Agentic AI refers to AI systems that can act autonomously as an agent to achieve a goal.

"In Agentic AI, we often have multiple specialized agents that work together to achieve a common goal. For example, in a self-driving car, there can be a perception agent that processes camera and sensor data, a planning agent that determines the best route, and a control agent that executes the driving decisions. Each of these agents operates autonomously, and together, they ensure the car can drive safely and efficiently.

These systems:
- Take decisions on their own
- Perform tasks without continuous human input

**Real-life examples:**
- Self-driving cars
- Smart assistants that schedule tasks automatically

---

## 9. What is NLP (Natural Language Processing)?
NLP is a branch of AI that enables machines to understand, interpret, and respond to human language.

**Examples:**
- Chatbots
- Voice assistants
- Language translation

---

## 10. What is Reinforcement Learning (RL)?
Reinforcement Learning is a type of ML where an agent learns by interacting with an environment and receiving rewards or penalties.

**Example:**
- Game-playing AI
- Robot navigation
- Training an AI to play chess
---

## TCS INTERVIEW PERFECT 1-LINERS

- **AI:** Machines that mimic human intelligence  
- **ML:** Machines learn from data  
- **Generative AI:** AI that creates new content  
- **Agentic AI:** AI that acts autonomously  
- **NLP:** AI that understands human language  
- **RL:** Learning through rewards and punishment  

---

## FINAL INTERVIEW TIP
If you are **not confident**, always say:
> *“At a basic level…”*

This keeps the answer safe and correct ✔️





# Cloud Computing – Basic Interview Notes (TCS Digital / NQT)

## 1. What is Cloud Computing?
Cloud computing is the delivery of computing resources such as storage, servers, databases, networking, and software over the internet instead of using local machines or physical hardware.

In simple words:
Instead of buying and maintaining physical hardware, we use these resources from the internet.

---

## 2. Why Cloud Computing?
- No need to buy physical hardware
- Easy scalability (increase or decrease resources)
- Cost-effective (pay only for what we use)
- Accessible from anywhere (remote access)
- High availability and backup support

---

## 3. Cloud Service Models
These describe **what level of service the cloud provider offers**.

### a) IaaS – Infrastructure as a Service
- Provides virtual hardware resources
- Includes servers, storage, and networking
- User manages OS, applications, and data

**Examples:**
- AWS EC2 (virtual server)
- AWS S3 (storage)

**Use case:**  
When we want full control over infrastructure.

---

### b) PaaS – Platform as a Service
- Provides a platform to build and deploy applications
- Developer does NOT worry about servers, OS, or database configuration
- Focus is only on writing code

**Examples:**
- Google App Engine
- Microsoft Azure App Service
- Heroku

**Use case:**  
When developers want to focus only on development.

---

### c) SaaS – Software as a Service
- Fully ready-to-use software
- No need to manage hardware or platform
- Accessed via browser or internet

**Examples:**
- Gmail
- Google Drive
- Google Maps
- Zoom

**Use case:**  
When we just want to use software directly.

---

## 4. Cloud Deployment Models
These describe **how the cloud is deployed**.

### a) Public Cloud
- Resources shared among multiple users
- Managed by third-party providers

**Examples:**
- AWS
- Google Cloud
- Microsoft Azure

---

### b) Private Cloud
- Dedicated to a single organization
- More secure and controlled

**Example:**
- Bank using its own private cloud for sensitive data

---

### c) Hybrid Cloud
- Combination of public and private cloud

**Example:**
- Sensitive data in private cloud
- Website hosted on public cloud

---

## 5. Advantages of Cloud Computing
- Scalability
- Cost efficiency
- Remote accessibility
- Automatic updates
- Disaster recovery

---

## 6. Challenges of Cloud Computing
- Security concerns (data stored on internet)
- Internet dependency
- Downtime or service outage
- Latency (speed issues)
- Compliance and data privacy issues

---

## 7. Common Interview Question Examples

### Q1. What is EC2?
Amazon EC2 allows users to rent virtual servers to run applications in the cloud.

---

### Q2. What is S3?
Amazon S3 is an object storage service used to store files, images, backups, etc.

---

### Q3. Difference between IaaS, PaaS, SaaS?
- IaaS → Infrastructure only
- PaaS → Platform for development
- SaaS → Ready-made software

---

# Cloud Computing – MOST IMPORTANT Extra Questions (TCS Digital)

## Virtualization:

Virtualization is a technology that allows multiple virtual machines (VMs) to run on a single physical machine. Each VM operates independently, with its own operating system, storage, and resources. This isolation means that each VM is protected from the others, and they can run different operating systems or applications simultaneously.

Example:
In cloud computing, AWS EC2 uses virtualization to provide virtual servers, allowing multiple VMs on one physical server.

## Virtual Machine (VM):

A virtual machine is a software-based computer that behaves like a physical computer. It has its own CPU, memory, disk storage, and network interface. VMs can run different operating systems and applications on the same physical hardware, which helps in maximizing resource utilization.

Example:
An AWS EC2 instance is a virtual machine that runs on top of Amazon’s physical servers.

## Hypervisor:

A hypervisor is the software layer that enables virtualization. It sits between the hardware and the virtual machines, managing the creation, execution, and resources of each VM.

Types of Hypervisors:

Type 1 (Bare-metal): Runs directly on the physical hardware.
Example: VMware ESXi, Microsoft Hyper-V.

Type 2 (Hosted): Runs on top of an existing operating system.
Example: Oracle VirtualBox, VMware Workstation.

Example of Hypervisor in Cloud:
AWS uses a Type 1 hypervisor to create and manage EC2 instances.

---

## 3. What is Cloud Security?
Cloud security refers to protecting data, applications, and services stored in the cloud from unauthorized access.

**Example measures:**
- Authentication
- Encryption
- Access control

---

## 4. What is Load Balancing?
Load balancing distributes incoming traffic across multiple servers so that no single server gets overloaded.

**Benefit:**
- Better performance
- High availability

**Example:**  
A website using multiple servers to handle many users.

---

## 5. What is Auto Scaling?
Auto scaling automatically increases or decreases resources based on demand.

**Example:**  
During high traffic, more servers are added automatically.

---

## 6. What is Pay-as-you-go Model?
You pay only for the resources you use.

**Example:**  
Paying only for hours your EC2 instance runs.

---

## 7. Difference between On-Premise and Cloud
| On-Premise | Cloud |
|-----------|------|
| Physical hardware required | No physical hardware |
| High upfront cost | Pay-as-you-go |
| Limited scalability | Easy scalability |

---

## 8. What is Disaster Recovery?
Disaster recovery is the process of restoring data and services after a failure or disaster.

**Example:**  
Cloud backup helps recover data after server failure.

---

## 9. Popular Cloud Service Providers
- AWS (Amazon Web Services)
- Microsoft Azure
- Google Cloud Platform (GCP)

---

## TCS Interview GOLDEN TIP
If interviewer asks:
> “Do you know about virtualization?”

Your **perfect answer**:
> *Yes, virtualization allows multiple virtual machines to run on a single physical machine, which helps in better resource utilization and is widely used in cloud computing.*

✔️ Clear  
✔️ Correct  
✔️ Enough for TCS

---

## IMPORTANT NOTE FOR YOU
You **do NOT need more cloud topics** than this for TCS Digital.  
This is **100% sufficient**.

Next best step 👉 **AI / ML + Generative AI rapid revision**

Just say **“Next AI notes”** 👍

