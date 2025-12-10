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

## 3.1 Batch Operating System
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
A system call is a **request** made by a user-space program to the kernel.

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

