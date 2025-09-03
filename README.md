# Linux
# Process Management and Memory Segmentation in Operating Systems

## 1. Running Process Information

To view running processes and their details in Linux, the following command is used:

```bash
$ ps -ef

This displays:

Process IDs (PID)

Parent process IDs (PPID)

Users

TTY (terminal association)

Start time

Command executed


Processes are associated with memory segments and managed by the kernel using Process Control Blocks (PCBs).


---

2. Scheduler and Process Scheduling

The scheduler selects which process to run from the ready queue based on the scheduling policy.

flowchart LR
    subgraph ReadyQueue[Ready Queue]
        P1[Process 1] --> P2[Process 2]
        P2 --> P3[Process 3]
        P3 --> P4[Process 4]
    end
    ReadyQueue -->|Round Robin| GPOS[General Purpose OS]
    ReadyQueue -->|Priority Scheduling| RTOS[Real-Time OS]

Round Robin Scheduler (RR):

Used in General Purpose Operating Systems (GPOS).

Each process gets a fixed time slice in cyclic order.

Ensures fairness.


Preemptive Priority-Based Scheduler:

Used in Real-Time Operating Systems (RTOS).

Higher-priority processes preempt lower-priority ones.

Ensures predictability and deadlines.



---

3. Memory Layout of a Process

When a program (a.out) is executed, the process image is loaded into RAM.

graph TD
    A[a.out on Disk] --> B[Process Image in RAM]
    B --> T[Text Segment<br/>Program Code]
    B --> D[Data Segment<br/>Global/Static Variables]
    B --> BSS[BSS Segment<br/>Uninitialized Variables]
    B --> H[Heap<br/>Dynamic Allocation]
    B --> S[Stack<br/>Function Calls & Locals]

    subgraph Memory Layout
        T
        D
        BSS
        H
        S
    end
    Memory Layout --> US[User Space]
    Memory Layout --> KS[Kernel Space]

User Space contains application segments.

Kernel Space is reserved for OS management.



---

4. Process Control Block (PCB)

Each process is represented by a PCB in kernel space.
It stores all metadata required to manage a process.

classDiagram
    class PCB {
        +PID : Process Identifier
        +PPID : Parent Process Identifier
        +Process State
        +Program Counter
        +CPU Registers
        +File Descriptor Table
        +Signal Disposition Table
        +Page Table (Memory Mapping)
        +Scheduling Information
        +Accounting Information
    }


---

5. Process Creation and Execution

sequenceDiagram
    participant U as User (Terminal)
    participant OS as Operating System
    participant HDD as Disk
    participant RAM as RAM
    participant CPU as CPU

    U->>OS: $ ./a.out
    OS->>HDD: Fetch a.out binary
    HDD-->>OS: Binary Loaded
    OS->>RAM: Load Process Image (Text, Data, BSS, Heap, Stack)
    OS->>OS: Create PCB in Kernel Space
    OS->>CPU: Add Process to Ready Queue
    CPU->>CPU: Execute Scheduled Process


---

Summary

GPOS: Uses Round Robin Scheduling → fairness, multitasking.

RTOS: Uses Priority-Based Scheduling → deterministic, deadline-driven.

Process Memory: Divided into Text, Data, BSS, Heap, Stack.

PCB: Stores critical metadata (PID, PPID, FDs, signals, page tables).

Command $ ps -ef: Monitor running processes.



---
