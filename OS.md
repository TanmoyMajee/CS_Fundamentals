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
A *critical section* is a part of the code where a shared resource (like shared data, variable, or file) is accessed by multiple threads or processes.

- Only **one thread** should execute inside the critical section at a time.
- If two threads enter at the same time → **race condition** happens.

---

## What is Thread Synchronization?
When multiple threads access shared data, synchronization prevents problems like **race conditions**.

Common synchronization tools:
- **Mutex (Mutual Exclusion)**
- **Semaphore**
- **Lock/Unlock mechanisms**
- **Monitors**

---

## What is Deadlock in Multithreading?
Deadlock happens when two or more threads wait forever for each other’s resources.

Conditions for deadlock:
1. Mutual exclusion  
2. Hold and wait  
3. No preemption  
4. Circular wait  

All four must be present.

---

## Advantages of Multithreading
- Faster execution  
- Less memory usage than multiple processes  
- Improved responsiveness  
- Better CPU utilization  

## Disadvantages of Multithreading
- Difficulty debugging  
- Risk of race conditions  
- Deadlocks possible  
- More complex program design  



