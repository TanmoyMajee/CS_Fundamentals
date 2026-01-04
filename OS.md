# Operating System Notes

## 1. What is an Operating System?
An operating system (OS) is system software that acts as an intermediary between the hardware and user applications. It manages hardware resources like CPU, memory, storage, and provides a stable environment for applications to run.

---

## 2. Features of Operating System
- **User Interface:** Provides GUI or CLI for user interaction.  
- **Resource Allocation:** Manages CPU, memory, and I/O device distribution among processes.  
- **Memory Management:** Tracks memory usage and allocates/deallocates memory to processes.  
- **File Management:** Manages files and directories, including creation, deletion, reading, and writing.  
- **Process Management:** Handles process creation, scheduling, and termination.

---

# 3. Types of Operating Systems
---
## 3.1 Batch Operating System?
In a batch operating system, users do not interact with the system directly. Instead, they create jobs, which consist of the program, the input and output data, and control statements needed for execution. These jobs are submitted to an operator, often using punch cards or files. The operator groups similar jobs into batches. Once a batch is created, it is sent to the CPU for execution

**Advantage:** Efficient for processing large volumes of similar data.  
**Disadvantage:** No user interaction during execution; CPU may stay idle during batch loading/unloading.(CPU  underutilized)
**Example:** payroll management system. In such systems, payroll data for all employees is collected, processed, and paid out in batches, typically at regular intervals (like monthly or bi-weekly)

---

## 3.2 Multiprogramming Operating System
Multiple programs are kept in main memory at the same time. The CPU switches to another program (context switch) whenever the current program is waiting for I/O, improving CPU utilization.

**Advantage:** Better CPU utilization than batch systems.  
**Disadvantage:** Risk of starvation if some processes wait too long. 

---

## 3.3 Multitasking / Time-Sharing Operating System
The CPU switches rapidly between multiple tasks, giving each one a fixed **time quantum** using the **round-robin** scheduling method. This makes it appear as if all programs run simultaneously.

**Advantage:** Very responsive; supports interactive applications and multiple users.  
**Disadvantage:** Too many tasks can slow the system due to context switching overhead.

---

## 3.4 Multiprocessing Operating System
The OS uses **multiple CPUs or processor cores**, allowing true parallel execution of processes.

**Advantage:** High performance; faster processing due to parallelism.  
**Disadvantage:** More complex and costly to implement.  
**Examples:** Windows, Linux, macOS, Android (multi-core support).

---

## 3.5 Distributed Operating System
Multiple independent computers work together and appear as a single system to the user. Tasks are distributed across machines.

**Advantage:** Load balancing—workload can be shared across systems.  
**Disadvantage:** Complex to set up and manage due to communication and synchronization issues.
**Examples:** Google cloud , aws

---

## 3.6 Real-Time Operating System (RTOS)
Designed to respond to events within strict time constraints. Timing is predictable and guaranteed.
real-time operating systems are used when we need immediate or time-critical responses, like in air traffic control or missile systems.

**Used in:**  
- Missile control systems  
- Air Traffic Control (ATC)  
- Medical devices  

**Advantage:** Guaranteed and predictable response time.  
**Disadvantage:** Limited flexibility; focuses only on time-critical tasks.

---

# Program vs Process – Key Differences (Short & Important)

| Aspect | Program | Process |
|--------|---------|---------|
| **Definition** | A program is a set of instructions written to perform a specific task. | A process is the running instance of a program in execution. |
| **Nature** | Static — stored on disk, not executing. | Dynamic — active, executing in main memory. |
| **Resource Usage** | Does not use system resources until executed. | Uses CPU, memory, files, and I/O resources while running. |
| **Example** | `chrome.exe` stored on disk. | When Chrome is opened, each running window/tab is a process. |

# 4. User Space and Kernel Space


## 1. Kernel Space
Kernel space is the part of the operating system where the **main OS code runs**.  
You can think of it as the **control room of the computer**.

- The kernel can directly access hardware like CPU, memory, disk, keyboard, etc.  
- It manages processes, memory, security, and all system resources.  
- User programs cannot enter kernel space because it may crash or damage the system.

---

## 2. User Space
User space is where **normal applications run**, such as browsers, games, editors, etc.  
It is like a **safe area for apps**.

- Applications cannot access hardware directly.  
- They cannot freely modify system resources.  
- If a program crashes in user space, the whole system remains safe.

---

## 3. System Call
A system call is a **request** made by a user-space program/Application to the kernel whenever it needs to access certain resources.

Since applications cannot directly access hardware, they ask the kernel to do it.

Examples of system calls:
- Reading a file  
- Writing a file  
- Allocating memory  
- Creating a process  

So a **system call = a message from user space to kernel → “Please do this for me.”**

---

## Summary Table

| Concept | Simple Meaning | Why It Is Needed? |
|--------|----------------|-------------------|
| **Kernel Space** | Area where the OS itself runs. | Controls hardware and manages system resources. |
| **User Space** | Area where applications run. | Keeps the system safe even if apps crash. |
| **System Call** | Request sent from user space to kernel. | Allows apps to use hardware safely through the OS. |


## Process States
- **New**: The process is being created.
- **Ready**: The process is loaded into memory and waiting to be assigned to the CPU.
- **Running**: The process is currently being executed by the CPU.
- **Waiting (Blocked)**: The process is waiting for some event, like I/O completion.
- **Terminated**: The process has finished execution.

## What is Interrept ? 

## Process Control Block (PCB)
- The PCB is a data structure that stores all the information about a process.
- It includes the process ID, state, CPU registers, memory limits, and pointers to resources.

## Context Switching
- Context switching is the process of saving the state of the current process in pcb and loading the state of the next process from pcb.
- The OS uses the PCB to store and restore process states during a switch.

## Process Scheduling Algorithms
Process scheduling is the method used by the operating system to decide which process gets the CPU next.
- **FCFS (First-Come, First-Served)**: Executes processes in the order they arrive.
- **SJF (Shortest Job First)**: Executes the process with the shortest execution time first.
- **Round Robin**: Each process gets a fixed time slice in a rotating order.
- **Priority Scheduling**: Executes the process with the highest priority first.

# Threads and Multithreading – OS Notes

## What is a Thread?
A thread is the smallest unit of execution inside a process.  
A process can have one thread (single-threaded) or multiple threads (multi-threaded).

- Threads share the same memory and resources of the process.
- Each thread has its own program counter, register set, and stack.

---

## What is Multithreading?
Multithreading is the ability of a CPU or operating system to run multiple threads **within the same process** simultaneously or concurrently.

- It improves performance.
- Useful for tasks that can run in parallel.
- Makes better use of CPU time.

---

## Benefits of Multithreading
- **Faster execution** for parallel tasks.
- **Efficient CPU utilization**.
- **Responsiveness**: UI applications stay responsive even while doing background work.
- **Resource sharing**: Threads share memory → less overhead than multiple processes.

---

## Disadvantages of Multithreading
- Risk of race conditions  
- Deadlocks possible  
- More complex program design  

---

## Thread vs Process (Short Interview Difference)

| Aspect | Process | Thread |
|--------|---------|---------|
| Definition | Program in execution | Smallest unit of execution inside a process |
| Memory | Has its own memory | Shares memory of the process |
| Creation cost | Heavy | Lightweight |
| Communication | Slow (IPC required) | Fast (shared memory) |
| Dependency | Independent | Threads of same process depend on each other |

---
### What is IPC?
Inter-Process Communication (IPC) is a mechanism that allows **processes to communicate with each other** and **share data**.  
Since processes run in separate memory spaces, they need IPC to exchange information safely.

---

## Types of Threads
### 1. User-Level Threads (ULT)
- Managed by user-level libraries (not the OS).
- Faster to create and manage.
- OS is not aware of them.

### 2. Kernel-Level Threads (KLT)
- Managed by the operating system.
- Slower but more powerful.
- OS schedules threads individually.

---

## What is Context Switching Between Threads?
Context switching is when the CPU switches from one thread to another.

- Thread context switch is **faster** than process context switch.
- Because threads share memory → less information needs to be saved/loaded.

---

## What is a Race Condition?
A race condition occurs when two or more threads access shared data at the same time, and the final result depends on the order of execution.

Example:
Two threads updating the same bank balance → inconsistent output.

**Synchronization** fixes race conditions.

---
### What is a Critical Section?
A *critical section* is a part of the code where a critical section / shared resource (like shared data, variable, or file) is accessed by multiple threads or processes.

- Only **one thread** should execute inside the critical section at a time.
- If two threads enter at the same time → **race condition** happens.

---

## What is Thread Synchronization?
When multiple threads access shared data, synchronization prevents problems like **race conditions**.

Common synchronization tools:

---

### 1. Semaphore
A semaphore is an integer-based synchronization mechanism used to control access to a shared resource. It uses two main operations: `wait` (P) and `signal` (V).

### 🔹 Wait and Signal Functions

- **Wait (P)**: Decrements the semaphore value. If the value is less than or equal to zero, the thread will wait (block) until it becomes positive again.
  
  ```pseudo
  function wait(semaphore s) {
      while (s.value <= 0) {
          // Wait in a loop (block) if no resources are available
      }
      s.value = s.value - 1; // Decrement the semaphore value
  }

    Signal (V): Increments the semaphore value and wakes up a waiting thread if any.

    function signal(semaphore s) {
        s.value = s.value + 1; // Increment the semaphore value
    }
  
🔹 Types of Semaphores

    Binary Semaphore: Acts like a mutex with a value of 0 or 1, allowing only one thread into the critical section.

    Counting Semaphore: Allows multiple threads to access a limited number of resources, with a value greater than 1.

🔹 One-liner Examples

    Binary Semaphore: Used when only one thread should access the critical section (like a mutex).

    Counting Semaphore: Used when multiple threads can access a limited resource (e.g., a pool of connections).
---

### 2 . Mutex
mutex (mutual exclusion) is a locking mechanism that ensures only one thread can access a critical section at a time.

Lock (Acquire): A thread locks the mutex before entering the critical section.

Unlock (Release): The thread releases the mutex after leaving the critical section.

---

##🔹 Difference Between Mutex and Binary Semaphore

Ownership: A mutex has ownership; the thread that locks it must unlock it. A binary semaphore does not have ownership; any thread can signal it.

Use Case: A mutex is typically used for locking a critical section to one thread, while a binary semaphore can be used for signaling without strict ownership rules.

In mUtex only one tred can acces the cirical setion , But in semaphore(counting) Multiple thread can acces


### 3. Monitor
- **Purpose**: A higher-level synchronization construct that combines locks and condition variables.
- **How it works**: A monitor is basically an object or module that has both the data (shared resources) and the procedures (methods) that synchronize access to the data.
- **Key point**: Monitors make it easier to write thread-safe code because they handle locking and unlocking automatically when you enter or leave the monitor's methods.


---

### 4. Lock and Unlock Mechanism (not imp)
- **Purpose**: A generic term often used for any mechanism that locks a resource and then unlocks it.
- **How it works**: A thread acquires a lock before entering the critical section and releases the lock when done.
- **Key point**: Very similar concept to a mutex, often used interchangeably in casual conversation.

---



### 🍽️ Dining Philosophers Problem

Problem: Five philosophers sit at a round table with one fork between each pair of philosophers. Each philosopher needs two forks to eat. The problem is how to design a protocol so that they can all eat without causing a deadlock or starvation.

Interview Script:
“The dining philosophers problem is a classic synchronization problem where multiple philosophers need to share limited resources (forks) without conflicts. In simple terms, we need to make sure that no philosopher is left waiting forever and that they don’t all end up waiting on each other indefinitely, causing a deadlock.”

### 📦 Producer-Consumer Problem

Problem: Producers generate data and place it into a buffer, and consumers take data from the buffer. The challenge is to ensure that producers don’t add to a full buffer and consumers don’t remove from an empty buffer.

Interview Script:
“The producer-consumer problem is about coordinating producers and consumers sharing a common buffer. The main goal is to prevent the producer from adding items when the buffer is full and the consumer from taking items when the buffer is empty. We use synchronization techniques to handle this smoothly.”

### 📖 Readers-Writers Problem

Problem: Multiple threads (readers) can read a shared resource simultaneously, but if a writer wants to write to the resource, it needs exclusive access. The challenge is to avoid conflicts and starvation of readers or writers.

Interview Script:
“The readers-writers problem is about managing access to a shared resource where many threads can read at the same time, but writing must be exclusive. The main goal is to ensure that readers don’t starve writers or vice versa, and to allow multiple readers to read concurrently.”

## Starvation 
Starvation happens when a process keeps waiting indefinitely because it never gets the required resource, often due to unfair scheduling (e.g., continuous arrival of higher-priority processes). The process remains stuck in the waiting state and makes no progress. It can be prevented using fair scheduling techniques like aging.

---

## Livelock 
 Livelock occurs when two or more processes continuously change states in response to each other, preventing any progress. The processes are running, not blocked, but stuck in a loop. It can be prevented by adding randomness to retries or using stronger synchronization methods.

---

## Livelock vs. Deadlock
- **Deadlock:** In deadlock, processes are stuck because each one holds a resource and waits for another resource held by another process, forming a cycle of dependency. Resources are locked and not released.
- **Livelock:** In livelock, processes keep changing states and attempting to avoid blocking each other, but they remain stuck in a loop and never make progress.

---


# Deadlock – Important Interview Notes

## 1. What is a Deadlock?
A deadlock is a situation where two or more processes are stuck forever because each process is waiting for a resource held by another process, and none of them can continue.

---

## 2. Four Necessary Conditions for Deadlock
Deadlock can occur **only if all four conditions hold simultaneously**:

1. **Mutual Exclusion** – Only one process can use a resource at a time.  
2. **Hold and Wait** – A process is holding one resource and waiting for another.  
3. **No Preemption** – Resources cannot be forcibly taken away from a process.  
4. **Circular Wait** – A circular chain exists where each process waits for a resource held by the next process.

If even one condition is removed, deadlock cannot occur.

---

## 3. Deadlock Prevention (Breaking one of the four conditions)
Deadlocks can be prevented by ensuring at least one of the necessary conditions never holds:

- **Avoid Hold and Wait** → Request all resources at once.  
- **Allow Preemption** → If a process is waiting, take back resources and give to others.  
- **Break Circular Wait** → Impose a fixed order on resource allocation.

---

## 4. Deadlock Avoidance
Deadlock avoidance ensures the system never enters an unsafe state.

- **Banker's Algorithm**  
  - Used to check if allocating a resource keeps the system in a safe state.  
  - Name only is enough for interviews.

---

## 5. Deadlock Detection
Deadlocks can be detected by:

- **Using a Resource Allocation Graph**  
  - A cycle in the graph indicates deadlock.  
- **Deadlock Detection Algorithms**  
  - The OS periodically checks for circular wait.

Once detected, the OS must recover.

---

## 6. Deadlock Recovery
After detecting a deadlock, the system removes it by:

- **Killing a process** (terminate one or more processes involved in the deadlock)  
- **Resource Preemption** (take resources away and give to other processes)

Interviewers usually expect only these two points.

---
## Safe State (Important for Deadlock Avoidance)

A system is in a **safe state** when there exists at least one sequence of process execution in which *every process can get the resources it needs*, complete its execution, and release the resources safely.

- In a safe state → **No deadlock will occur**.
- In an unsafe state → Deadlock *may* occur (not guaranteed, but possible).

  ---# Virtual Memory – Simple Notes

## What is Virtual Memory?
Virtual Memory is a memory management technique that creates an **illusion of very large main memory (RAM)** to the user.  
It uses a part of **secondary storage (disk)** as an extension of RAM, allowing very large programs to run even if physical RAM is small.

The OS achieves this by **swapping required parts of a process (pages) between Disk and RAM when needed.**

---

## Why Do We Need Virtual Memory?
- Programs may be larger than physical RAM
- Allows multiple programs to run at the same time
- Better memory utilization
- Supports multiprogramming

---

## How Virtual Memory is Implemented?
Virtual memory is mainly implemented using:

- **Paging**
- **Segmentation**
- or a combination of both

---

## Paging
In paging:
- A process is divided into **fixed-size blocks** called **Pages**
- RAM is divided into equal-size blocks called **Frames**
- A page is loaded into any available frame

### Fragmentation in Paging
- Solves **external fragmentation**
- Suffers from **internal fragmentation**
  - Last page may not fully fill its frame → small memory waste inside the frame

---

## Segmentation
In segmentation:
- Memory is divided into **variable-sized logical units**
  - like functions, array, stack, code, data section etc.
- Each segment has different size

### Fragmentation in Segmentation
- Suffers from **external fragmentation**
  - free spaces scattered in between segments

---

## One-Line Summary
Virtual Memory makes the system **act like it has more RAM than it actually has** by using disk space and loading only required parts of programs into RAM when needed.

  
# Memory Management – Important Interview Topics

## 🧩 Fragmentation

### 1. Internal Fragmentation
Internal fragmentation occurs when **fixed-size memory blocks are allocated** and some part of the block remains unused.

- Happens in **paging / fixed partitions**
- Wastage is **inside** allocated block

Example:  
If frame = 4 KB and page uses 3.6 KB → 0.4 KB wasted.

---

### 2. External Fragmentation
External fragmentation occurs when **free memory is available but in small non-contiguous pieces**, so large processes cannot be allocated.

- Happens in **segmentation / variable partitions**
- Wastage is **between blocks**

---

### Internal vs External Fragmentation (Short Table)

| Internal Fragmentation | External Fragmentation |
|------------------------|------------------------|
| Wastage inside block   | Wastage between blocks |
| Fixed-size allocation  | Variable-size allocation |
| Happens in paging      | Happens in segmentation |

---
## 📦 Paging vs Segmentation (Very Important)

| Paging | Segmentation |
|--------|--------------|
| Divides process into **fixed-size pages** | Divides process into **variable-size segments** |
| Based on **physical memory** | Based on **logical divisions** (code, stack, data) |
| Suffers from **internal fragmentation** | Suffers from **external fragmentation** |
| Size is fixed | Size varies |

## ⚠️ Page Fault

A page fault occurs when a process tries to access a page that is **not present in RAM**.

### Steps
1. Trap to OS
2. Page loaded from disk to RAM
3. Page table updated
4. Process resumes

Too many page faults → thrashing.

## 🔁 Page Replacement Algorithms (Short Notes)

### FIFO (First-In-First-Out)
- Oldest page removed first
- Simple but may cause **Belady's anomaly**

### LRU (Least Recently Used)
- Removes page **not used for longest time**
- Good performance

### Optimal
- Removes page **not needed in future**
- Best but **not practical**


## 🚨 Belady’s Anomaly

In FIFO page replacement, increasing number of frames **increases page faults** sometimes.  
This unexpected condition is called **Belady’s Anomaly**.


## 🔁 Swapping

Swapping is a technique where **entire processes are moved between RAM and disk** to free memory.

- Process swapped out → disk
- Brought back → RAM when needed

## 🔥 Thrashing

Thrashing happens when:

- Too many page faults occur
- CPU spends most time **swapping pages**
- Very little real execution happens

### Causes
- Too many processes
- Too small RAM
- Poor memory allocation

### Solution
- Reduce degree of multiprogramming
- Increase RAM




