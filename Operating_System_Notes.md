# Operating Systems — BUET MSc Exam Notes
### Single Source of Truth | MCQ + Written | Based on Past Questions

> **Textbook Reference:** *Operating System Concepts* — Silberschatz, Galvin & Gagne (Dinosaur Book)
> *Modern Operating Systems* — Tanenbaum

---

## 📋 Table of Contents

1. [OS Fundamentals & Concepts](#1-os-fundamentals--concepts)
2. [Processes & Threads](#2-processes--threads)
3. [Process Scheduling (CPU Scheduling)](#3-process-scheduling-cpu-scheduling)
4. [Process Synchronization](#4-process-synchronization)
5. [Deadlocks](#5-deadlocks)
6. [Memory Management](#6-memory-management)
7. [Paging & Segmentation](#7-paging--segmentation)
8. [Virtual Memory & Page Replacement](#8-virtual-memory--page-replacement)
9. [Linux Commands & Shell](#9-linux-commands--shell)
10. [System Throughput & Multithreading Problems](#10-system-throughput--multithreading-problems)
11. [Quick MCQ Reference Card](#11-quick-mcq-reference-card)

---

## 1. OS Fundamentals & Concepts

### What is an Operating System?
An OS is system software that acts as an **intermediary between hardware and user programs**. It manages resources (CPU, memory, I/O) and provides a convenient, efficient environment.

### Core Functions of an OS
| Function | Description |
|---|---|
| Process Management | Creates, schedules, terminates processes |
| Memory Management | Allocates/deallocates memory, handles virtual memory |
| File System Management | Organizes files, directories, permissions |
| I/O Management | Controls devices via drivers |
| Security & Protection | Controls access to resources |

### Types of OS

| Type | Description | Example |
|---|---|---|
| **Batch OS** | Jobs collected and executed in batches, no user interaction | Early IBM systems |
| **Time-Sharing OS** | Multiple users share CPU using time slices | UNIX |
| **Real-Time OS (RTOS)** | Strict timing guarantees | VxWorks, FreeRTOS |
| **Distributed OS** | Coordinates multiple computers | Amoeba, Plan 9 |
| **Network OS** | Provides network services | Windows Server |
| **Embedded OS** | For embedded systems | Android, iOS |

### Key Terminology
- **Kernel**: Core of the OS; runs in privileged mode
- **Shell**: User interface to the OS (CLI or GUI)
- **System Call**: Interface between user program and OS kernel
- **Interrupt**: Signal to CPU that an event needs attention
- **Context Switch**: Saving state of one process and loading another

---

## 2. Processes & Threads

### Process vs Thread — [BUET MSC ICT 15, 17]

**Process**: An instance of a program in execution. Has its own:
- Address space (code, data, heap, stack)
- PCB (Process Control Block)
- Resources (file handles, I/O)

**Thread**: Lightweight process; a unit of execution within a process. Threads **share** the process's:
- Code segment
- Data segment (global variables)
- Open files and signals

But each thread has its own:
- Program Counter (PC)
- Registers
- Stack

```
PROCESS                         THREAD
+---------------------+         +---------------------+
|  Code  |  Data      |         |  Code  |  Data      |  <- Shared
|  Heap  |  Files     |         |  Heap  |  Files     |  <- Shared
+--------+------------+         +--------+------------+
|Stack 1 |            |         |Stack T1| Stack T2   |  <- Per-Thread
+--------+------------+         +--------+------------+
```

### Why Threads are More Efficient than Processes
1. **Faster creation**: Thread creation takes ~10x less time (no new address space)
2. **Faster context switch**: Lighter state to save/restore
3. **Shared memory**: No need for IPC mechanisms for communication
4. **Responsiveness**: One thread can block while others continue
5. **Resource sharing**: Threads share code & data naturally

### Multitasking vs Multiprogramming vs Multithreading — [BUET MSC 17]

| Term | Definition |
|---|---|
| **Multiprogramming** | Multiple programs loaded in memory simultaneously; CPU switches when one waits for I/O. Goal: maximize CPU utilization. |
| **Multitasking (Time-sharing)** | Extension of multiprogramming where CPU switches so fast users feel simultaneous execution. Uses time slices (quanta). |
| **Multithreading** | A single process has multiple threads of execution running concurrently. |

> **Memory Trick:** Multiprogramming -> Memory focus; Multitasking -> Time focus; Multithreading -> Thread focus within one process.

### Process Control Block (PCB)
The OS maintains a PCB for every process containing:
- Process ID (PID)
- Process state (New, Ready, Running, Waiting, Terminated)
- Program Counter
- CPU registers
- Memory management info (page tables)
- I/O status info
- Accounting info (CPU time used)

### Process State Diagram — [BPDB-AE 11, BUET MSC 15]

```
                     +--------------+
               admit |              | interrupt / time quantum expires
         +---------->|    READY     |<---------------------------------+
         |           |              |                                  |
    +----+---+       +------+-------+                         +--------+------+
    |  NEW   |              |                                 |   RUNNING     |
    +--------+              | scheduler dispatch              |               |
                            +-------------------------------->|               |
                                                              +-------+-------+
                                                                      |
                                         I/O or event wait            |  exit
                                        +----------------------------++      |
                                        v                             |      v
                                 +----------+                  +-----------+
                                 | WAITING  |                  | TERMINATED|
                                 | (BLOCKED)|                  +-----------+
                                 +-----+----+
                                       | I/O or event completion
                                       +----> READY
```

**Transitions:**
- **New -> Ready**: Process admitted by OS
- **Ready -> Running**: CPU scheduler dispatches it
- **Running -> Ready**: Interrupted (time quantum expires, higher priority process arrives)
- **Running -> Waiting**: Process requests I/O or waits for event
- **Waiting -> Ready**: I/O completes
- **Running -> Terminated**: Process finishes

---

## 3. Process Scheduling (CPU Scheduling)

### Key Metrics — [BUET MSC ICT 15]

| Metric | Formula | Meaning |
|---|---|---|
| **Turnaround Time (TAT)** | Completion Time - Arrival Time | Total time from arrival to completion |
| **Waiting Time (WT)** | TAT - Burst Time | Time spent waiting in ready queue |
| **Response Time** | First CPU run - Arrival Time | Time until first response |
| **Throughput** | Processes / Unit Time | Number of processes completed per unit time |

### Scheduling Algorithms

#### 1. FCFS — First Come First Served (Non-Preemptive)
- Simple queue; process arriving first gets CPU first
- **Convoy Effect**: Short processes wait behind long ones
- **Not optimal** for average waiting time

**Example:**
FCFS with A(8), B(4), C(4), D(4) — order A, B, C, D:

```
+--------+----+----+----+
|   A    | B  | C  | D  |
| 0    8 |8 12|12 16|16 20|
+--------+----+----+----+
TAT: A=8, B=12, C=16, D=20  =>  Avg TAT = 14
```

#### 2. SJF — Shortest Job First (Non-Preemptive) — [BUET MSC ICT 15]
- Process with **shortest burst time** runs next
- **Optimal** for minimizing average waiting time (when all arrive simultaneously)
- **Problem**: Requires knowing future burst times; can cause **starvation** for long processes

**Worked Example [BUET MSC ICT 15]:**
A(8), B(4), C(4), D(4) — all arrive at time 0 => SJF order = B, C, D, A

```
+----+----+----+--------+
| B  | C  | D  |   A    |
|0  4|4  8|8 12| 12   20|
+----+----+----+--------+

TAT:  B = 4-0 = 4
      C = 8-0 = 8
      D = 12-0 = 12
      A = 20-0 = 20
Avg TAT = (4+8+12+20)/4 = 44/4 = 11 minutes
```

#### 3. SRTF — Shortest Remaining Time First (Preemptive SJF)
- Preempts current process if new process has shorter remaining burst time
- **Optimal** for average waiting time among all algorithms

#### 4. Round Robin (RR) — [BUET MSC 16]
- Each process gets a fixed **time quantum (q)**
- After quantum expires, process goes back to end of ready queue
- **No starvation** — everyone gets a turn
- **Best for time-sharing** systems
- Large q -> degenerates to FCFS; Small q -> too many context switches

**Example — Round Robin with q=3:**
Processes: P1(6), P2(4), P3(2) — all arrive at t=0

```
+----+----+----+----+----+----+
| P1 | P2 | P3 | P1 | P2 | P1 |
|0  3|3  6|6  8|8 11|11 13|13 15|
+----+----+----+----+----+----+

Wait: P1 = (8-3)+(13-11) = 7
      P2 = (3)+(8-6) = 5
      P3 = 6
TAT:  P1=15, P2=13, P3=8
```

#### 5. Priority Scheduling
- Each process assigned a priority number
- CPU allocated to highest priority process
- **Problem**: **Starvation** — low priority processes may never run
- **Solution**: **Aging** — gradually increase priority of waiting processes

#### Summary Comparison Table

| Algorithm | Preemptive | Starvation | Optimal for |
|---|---|---|---|
| FCFS | No | No | Simple FIFO systems |
| SJF | No | Yes (long jobs) | Min avg WT (non-preemptive) |
| SRTF | Yes | Yes (long jobs) | Min avg WT (globally) |
| Round Robin | Yes | No | Time-sharing, response time |
| Priority | Both | Yes (low priority) | Priority-sensitive systems |

### Types of Schedulers

| Scheduler | Also Called | Role |
|---|---|---|
| **Long-term** | Job Scheduler | Selects which processes to bring into memory (controls degree of multiprogramming) |
| **Short-term** | CPU Scheduler | Selects which ready process gets CPU next (runs very frequently, ~ms) |
| **Medium-term** | Swapper | Handles swapping of processes in/out of memory |

---

## 4. Process Synchronization

### The Critical Section Problem — [BUET MSC ICT 15]

**Critical Section**: Part of code accessing shared resources (shared variables, files, I/O).

**Problem**: When multiple processes access shared data concurrently, it can lead to **race conditions** where the outcome depends on execution order.

**Three Requirements for any valid solution:**
1. **Mutual Exclusion**: Only one process in critical section at a time
2. **Progress**: If no process is in critical section, a waiting process must eventually enter (no deadlock among waiters)
3. **Bounded Waiting**: A limit on how many times other processes can enter CS before a waiting process gets in (no starvation)

### Analysis of the Given Code — [BUET MSC ICT 15]

```c
// Shared Variables: C1 = 1, C2 = 1
// Process 1:
while(True) {
    C1 = 0;
    while(C2 == 0);   // busy wait
    // Critical Section 1
    C1 = 1;
}

// Process 2:
while(True) {
    C2 = 0;
    while(C1 == 0);   // busy wait
    // Critical Section 2
    C2 = 1;
}
```

**Is mutual exclusion ensured?**
- If P1 sets C1=0, and then P2 sets C2=0 **before** P1 checks `while(C2==0)`, then both processes will be stuck waiting for each other -> **DEADLOCK**.
- **Mutual exclusion is NOT reliably ensured.**

**Other problem:** **Deadlock** — Both processes can simultaneously set their flags to 0 and then both spin-wait forever, neither proceeding.

### Semaphores — The Classic Solution

A **semaphore** `S` is an integer variable accessed only through two atomic operations:

```
wait(S):    // also called P() or down()
    while S <= 0 do nothing;  // busy wait
    S = S - 1;

signal(S):  // also called V() or up()
    S = S + 1;
```

**Binary Semaphore (Mutex)**: S = 0 or 1; used for mutual exclusion
**Counting Semaphore**: S = any non-negative integer; used to control access to resources with multiple instances

**Mutual Exclusion using Semaphore:**
```c
semaphore mutex = 1;

Process P_i:
    wait(mutex);
    // Critical Section
    signal(mutex);
    // Remainder Section
```

### Classic Synchronization Problems

#### Producer-Consumer (Bounded Buffer) Problem
```
Shared buffer of size N.
Producer: adds items to buffer
Consumer: removes items from buffer

Semaphores:
  mutex = 1      (mutual exclusion on buffer)
  empty = N      (count of empty slots)
  full  = 0      (count of full slots)

Producer:                    Consumer:
  wait(empty)                  wait(full)
  wait(mutex)                  wait(mutex)
  add item to buffer           remove item from buffer
  signal(mutex)                signal(mutex)
  signal(full)                 signal(empty)
```

#### Readers-Writers Problem
- Multiple readers can read simultaneously
- Writers need exclusive access
- First variant: No reader waits unless writer holds lock

#### Dining Philosophers Problem
```
5 philosophers sit at a table.
Each needs 2 forks (left and right) to eat.
Only 5 forks available.

Risk: Deadlock if all pick up left fork simultaneously.
Solutions:
  1. Allow at most 4 philosophers at table at once
  2. Pick both forks atomically
  3. Odd philosophers pick left first; even pick right first
```

### Mutex vs Semaphore

| Feature | Mutex | Semaphore |
|---|---|---|
| Value | Binary (0/1) | Integer (0 to N) |
| Ownership | Owned by locking thread | No ownership |
| Purpose | Mutual exclusion only | Signaling + mutual exclusion |
| Usage | Protecting critical sections | Resource counting, signaling |

---

## 5. Deadlocks

### What is a Deadlock? — [GTCL AE 16, BUET MSC ICT 15, 17, 18, 19]

A **deadlock** is a situation where a set of processes are **permanently blocked**, each waiting for a resource held by another process in the set.

**Classic Example:**
```
Process A holds Resource 1, needs Resource 2
Process B holds Resource 2, needs Resource 1

A --wants--> R2 --held by--> B
B --wants--> R1 --held by--> A
             ^_____circular_____^
```

### Four Necessary Conditions (Coffman Conditions) — [BUET MSC 17, 18, 19]

All four must hold simultaneously for deadlock to occur:

| # | Condition | Description |
|---|---|---|
| 1 | **Mutual Exclusion** | At least one resource must be held in non-shareable mode (only one process at a time) |
| 2 | **Hold and Wait** | A process holds at least one resource while waiting to acquire additional resources held by others |
| 3 | **No Preemption** | Resources cannot be forcibly taken from a process; must be released voluntarily |
| 4 | **Circular Wait** | A set {P0, P1,...,Pn} of waiting processes exists such that P0 waits for P1, P1 for P2,..., Pn waits for P0 |

> **Memory Trick:** **M**y **H**orse **N**ever **C**ircles = Mutual exclusion, Hold & wait, No preemption, Circular wait

### Resource-Allocation Graph (RAG) — [BUET MSC ICT 16]

Nodes:
- **Circle** = Process (P)
- **Rectangle** = Resource type (R); dots inside = instances

Edges:
- **Request edge**: P -> R (process requests resource)
- **Assignment edge**: R -> P (resource assigned to process)

```
Deadlock Example:

    (P1) ---request---> [R2] ---assigned---> (P2)
     ^                                        |
     |                                        |
  assigned                                 request
     |                                        |
    [R1] <------------------------------------------+

Cycle: P1->R2->P2->R1->P1  ==>  DEADLOCK
```

**Key Rule:**
- If graph has **no cycle** -> **No deadlock**
- If graph has a **cycle** with **single-instance resources** -> **Deadlock**
- If graph has a **cycle** with **multiple-instance resources** -> **Deadlock possible** but not certain

### Deadlock Handling Strategies

#### 1. Deadlock Prevention
Negate one of the four conditions:

| Condition to Negate | Method |
|---|---|
| Mutual Exclusion | Make resources shareable (not always possible) |
| Hold and Wait | Process must request ALL resources at once before starting; or release all before requesting new |
| No Preemption | If a process can't get new resource, release all held resources |
| Circular Wait | Impose a **total ordering** on resource types; processes request resources only in increasing order |

#### 2. Deadlock Avoidance — Banker's Algorithm

**Idea**: System dynamically checks whether granting a resource request leads to a **safe state**.

**Safe State**: A state where there exists a **safe sequence** in which all processes can complete.

**Banker's Algorithm Data Structures (for n processes, m resources):**

```
Available[m]       : Resources currently available
Max[n][m]          : Maximum demand of each process
Allocation[n][m]   : Resources currently allocated
Need[n][m]         : Remaining need = Max - Allocation
```

**Safety Algorithm:**
```
1. Work = Available;  Finish[i] = false for all i
2. Find i such that: Finish[i] = false AND Need[i] <= Work
3. Work = Work + Allocation[i];  Finish[i] = true
4. Repeat step 2
5. If all Finish[i] = true -> SAFE STATE
```

**Example:**

Processes: P0-P4, Resources: A(10), B(5), C(7)

| Process | Allocation (A,B,C) | Max (A,B,C) | Need (A,B,C) |
|---|---|---|---|
| P0 | 0,1,0 | 7,5,3 | 7,4,3 |
| P1 | 2,0,0 | 3,2,2 | 1,2,2 |
| P2 | 3,0,2 | 9,0,2 | 6,0,0 |
| P3 | 2,1,1 | 2,2,2 | 0,1,1 |
| P4 | 0,0,2 | 4,3,3 | 4,3,1 |

Available = (3,3,2)

**Finding safe sequence:**
- Work=(3,3,2): P1 needs(1,2,2)<=(3,3,2) -> grant. Work=(5,3,2), P1 done
- Work=(5,3,2): P3 needs(0,1,1)<=(5,3,2) -> grant. Work=(7,4,3), P3 done
- Work=(7,4,3): P4 needs(4,3,1)<=(7,4,3) -> grant. Work=(7,4,5), P4 done
- Work=(7,4,5): P0 needs(7,4,3)<=(7,4,5) -> grant. Work=(7,5,5), P0 done
- Work=(7,5,5): P2 needs(6,0,0)<=(7,5,5) -> grant. P2 done

**Safe sequence: P1 -> P3 -> P4 -> P0 -> P2  =>  SAFE STATE**

#### 3. Deadlock Detection
- Allow deadlocks to occur, detect them using **wait-for graph** (for single instance resources)
- For multiple instances, use a detection algorithm similar to Banker's

#### 4. Deadlock Recovery
- **Process termination**: Kill one or all deadlocked processes
- **Resource preemption**: Take resources from processes; rollback and restart them

### Deadlock-Free Resource Request — [BUET MSC CSE 21]

**Question**: A and B each need R1, R2, R3. A requests in order R2, R3, R1. Which order for B is deadlock free?

**Answer**: **R1, R2, R3** (or R1, R3, R2)

**Reason**: Using the total ordering technique, ensure both processes request resources in a **compatible order** so no circular wait can form. If A requests R2 first and B requests R1 first, they will never both be waiting on each other's held resource -> **No circular wait -> No deadlock**.

---

## 6. Memory Management

### Memory Allocation Techniques

#### Contiguous Allocation
Memory divided into fixed or variable partitions.

**Fixed Partitioning:**
- Memory divided into fixed-size partitions
- **Internal fragmentation**: Allocated partition larger than needed

**Variable (Dynamic) Partitioning:**
- Partitions created dynamically to fit process size
- **External fragmentation**: Free memory scattered in small chunks

**Allocation Strategies:**
| Strategy | Description | Fragmentation |
|---|---|---|
| **First Fit** | Allocate first hole big enough | Moderate |
| **Best Fit** | Allocate smallest hole that fits | Worst external frag |
| **Worst Fit** | Allocate largest hole | Poor utilization |

**Compaction**: Moving all processes together to consolidate free space. Expensive operation.

### Fragmentation Types

| Type | Cause | Where it occurs |
|---|---|---|
| **Internal Fragmentation** | Allocated memory > needed | Fixed partitioning, paging |
| **External Fragmentation** | Total free memory sufficient but not contiguous | Variable partitioning, segmentation |

### Swapping
- Process temporarily moved from main memory to disk (backing store)
- Allows more processes than memory can hold
- **Swap out**: Move process to disk
- **Swap in**: Move process back to memory

---

## 7. Paging & Segmentation

### Paging — [BUET MSC ICT 15, 16, 17]

**Concept:**
- Physical memory divided into fixed-size blocks called **frames**
- Logical memory divided into same-size blocks called **pages**
- OS maintains a **page table** for each process to map pages->frames

```
LOGICAL ADDRESS SPACE          PHYSICAL MEMORY
+------------------+           +------------------+
|   Page 0         | ------->  |    Frame 3       |
|   Page 1         | ------->  |    Frame 7       |
|   Page 2         | ------->  |    Frame 0       |
|   Page 3         | ------->  |    Frame 1       |
+------------------+           +------------------+
                                Frame 2 is free
```

**Address Translation:**
```
Logical Address  = [Page Number (p)] [Page Offset (d)]
                          |
                     Page Table
                          |
Physical Address = [Frame Number (f)] [Page Offset (d)]
```

**Formula:**
```
Page number  p = Logical Address / Page Size
Page offset  d = Logical Address mod Page Size
Physical Address = frame[p] x Page Size + d
```

### Worked Example — [BUET MSC ICT 16]

**Question**: Logical address space of 16 pages of 2048 words each, mapped onto physical memory of 64 frames.

**(a) Bits in logical address:**
- Pages = 16 = 2^4  =>  Page number bits = 4
- Page size = 2048 words = 2^11  =>  Page offset bits = 11
- **Total logical address bits = 4 + 11 = 15 bits**

**(b) Bits in physical address:**
- Frames = 64 = 2^6  =>  Frame number bits = 6
- Page offset bits = 11 (same as page size)
- **Total physical address bits = 6 + 11 = 17 bits**

### Another Worked Example — [BUET MSC 17]

**Question**: 32-bit logical address space, page size = 4 KB, each page table entry = 4 bytes. Find page table size.

```
Page size = 4 KB = 2^12 bytes
Number of pages = 2^32 / 2^12 = 2^20 pages
Page table entries = 2^20
Each entry = 4 bytes
Page table size = 2^20 x 4 = 4 MB
```
**Answer: Page table size = 4 MB**

### Advantages & Disadvantages of Small Page Size — [BUET MSC ICT 15, 17]

| Aspect | Small Page Size | Large Page Size |
|---|---|---|
| **Internal Fragmentation** | Less (GOOD) | More (BAD) |
| **Page Table Size** | Larger (BAD) | Smaller (GOOD) |
| **I/O Efficiency** | Slower (BAD) — more page faults, smaller transfers | Faster (GOOD) |
| **Locality Exploitation** | Harder (BAD) | Better (GOOD) |
| **Memory Utilization** | Better, fine-grained (GOOD) | Wastes space (BAD) |

**Summary for exam**: Small page size -> Less internal fragmentation BUT larger page table and worse I/O performance.

### Segmentation

**Concept:**
- Logical address space divided into variable-size segments (code, data, stack, heap)
- Each segment has a **base** address and **limit**

```
Logical Address  = [Segment Number (s)] [Offset (d)]
                          |
                   Segment Table
                          |
Physical Address = Base[s] + d   (if d < Limit[s])
```

**Paging vs Segmentation:**

| Feature | Paging | Segmentation |
|---|---|---|
| Size | Fixed-size pages | Variable-size segments |
| Fragmentation | Internal fragmentation | External fragmentation |
| User visibility | Transparent to user | User-visible (logical) |
| Sharing | Harder | Easier (share a segment) |
| Protection | Less natural | Natural (per segment) |

### Translation Lookaside Buffer (TLB)

Problem: Every memory access requires TWO memory accesses (page table + actual data).

Solution: **TLB** — a fast associative cache that stores recent page-to-frame mappings.

```
CPU generates address
        |
        v
   TLB lookup
   +-----+------+
   |             |
TLB Hit      TLB Miss
   |             |
   |        Access Page Table in memory
   |             |
   +------+-------+
          |
    Physical Address
```

**Effective Access Time (EAT):**
```
EAT = hit_ratio x (TLB time + memory time)
    + (1 - hit_ratio) x (TLB time + 2 x memory time)
```

---

## 8. Virtual Memory & Page Replacement

### Virtual Memory Concepts

**Virtual Memory**: Technique that allows execution of processes that are not completely in memory. Logical address space can be larger than physical memory.

**Demand Paging**: Load pages into memory **only when needed** (on demand).
- If page is in memory: access proceeds normally
- If page NOT in memory: **Page Fault** occurs

### Page Fault Handling — [BUET MSC ICT 16, 19]

**When does a page fault occur?**
When a process accesses a page that is **not currently in physical memory** (page is on disk).

**Steps taken by OS on page fault:**
```
1. Check internal table (in PCB) to determine if reference is valid/invalid
2. If invalid reference -> terminate process (segmentation fault)
3. If valid but not in memory:
   a. Find a free frame (or use page replacement algorithm)
   b. Read the desired page from disk into the free frame
   c. Update page table to reflect new frame
   d. Set valid bit = 1
   e. Restart the instruction that caused the fault
```

```
DEMAND PAGING FLOW:

    CPU Access
   ----------->  Is page in memory?
                      |        |
                     YES       NO (Page Fault)
                      |        |
              Access   v       v
              Memory  OS handles page fault
                               |
                     Is free frame available?
                          |          |
                         YES         NO
                          |          |
                          |    Page Replacement Algorithm
                          |    (find victim page, evict it)
                          |          |
                          +----------+
                               |
                         Load page from disk
                         Update page table
                         Restart instruction
```

### Page Replacement Algorithms — [BUET MSC ICT 16, 19]

**Reference String**: 7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2
**Number of Frames**: 3

#### FIFO (First-In First-Out)
Replace the page that has been in memory the **longest**.

```
Ref:   7   0   1   2   0   3   0   4   2   3   0   3   2
      ---  ---  ---  ---  ---  ---  ---  ---  ---  ---  ---  ---  ---
F1:   [7]   7    7    2    2    2    2    4    4    4    0    0    0
F2:        [0]   0    0    0    3    3    3    2    2    2    2    2
F3:             [1]   1    1    1    0    0    0    3    3    3    3
PF:    x    x    x    x    -    x    x    x    x    x    x    -    -
                                                    Total PF = 9
```

> **Belady's Anomaly**: In FIFO, more frames can sometimes cause MORE page faults! (counterintuitive)

#### LRU (Least Recently Used)
Replace the page that has **not been used for the longest time**.

```
Ref:   7   0   1   2   0   3   0   4   2   3   0   3   2
      ---  ---  ---  ---  ---  ---  ---  ---  ---  ---  ---  ---  ---
F1:   [7]   7    7    2    2    2    2    4    4    4    0    0    0
F2:        [0]   0    0    0    0    0    0    2    2    2    2    2
F3:             [1]   1    1    3    3    3    3    3    3    3    3
PF:    x    x    x    x    -    x    -    x    x    x    x    -    -
                                                    Total PF = 8
```

#### Optimal Algorithm (OPT / Belady's Optimal)
Replace the page that **will not be used for the longest time** in the future.
- Theoretical — impossible to implement in practice (need to know future)
- Serves as a benchmark; gives **minimum possible page faults**

```
For the same reference string with 3 frames:
Total PF = 5  (optimal - minimum)
```

#### Comparison Table

| Algorithm | Page Faults | Implementable | Notes |
|---|---|---|---|
| **FIFO** | Highest | Yes | Simple; suffers Belady's anomaly |
| **LRU** | Medium | Yes (costly) | Good approximation of optimal; no Belady's anomaly |
| **Optimal** | Lowest | No (future unknown) | Theoretical benchmark only |

### Thrashing — [BUET MSC ICT 16]

**Definition**: A process is thrashing when it spends more time **paging** than **executing**.

**Cause**: If a process doesn't have enough frames, it will constantly page fault. High paging activity -> Low CPU utilization -> OS thinks CPU is idle -> Loads more processes -> Even less frames per process -> More thrashing.

```
CPU Utilization vs. Degree of Multiprogramming:

  CPU
Utilization
  100%|           * optimal point
     |         /  \
   50%|       /     \  <- THRASHING starts here
     |     /         \
    0%|---/            \__________
     +---------------------------------> Degree of Multiprogramming
```

**Solution — Working Set Model:**
- **Locality of Reference**: Processes tend to access a small set of pages repeatedly (locality)
- **Working Set**: Set of pages a process is actively using in a time window delta
- OS ensures each process has enough frames for its working set

### Locality of Reference — [BUET MSC ICT 16]

**Locality of Reference**: The tendency of programs to access the **same memory locations** repeatedly over a short period.

**Types:**
- **Temporal Locality**: Recently accessed location likely to be accessed again soon (loops, function calls)
- **Spatial Locality**: Locations near recently accessed ones likely to be accessed soon (arrays, sequential code)

**Why it matters for virtual memory performance:**
Without locality of reference, demand paging would result in a page fault for nearly every memory reference, making virtual memory impossibly slow. With locality, the working set is small and fits in memory, making page faults rare. This is the fundamental reason why virtual memory works in practice.

---

## 9. Linux Commands & Shell

### Common Commands — [BUET MSC 17, 18]

#### Directory Operations
```bash
mkdir dirname          # Create directory
mkdir -p a/b/c         # Create directory tree (parent dirs too)
rmdir dirname          # Remove empty directory
rm -r dirname          # Delete directory with all contents (recursive)
rm -rf dirname         # Force delete (no confirmation)
ls -la                 # List all files with details
cd /path/to/dir        # Change directory
pwd                    # Print working directory
```

#### File Operations
```bash
touch file.txt         # Create empty file
cp src dest            # Copy file
mv src dest            # Move/rename file
cat file.txt           # Display file contents
less file.txt          # Page through file
head -n 10 file.txt    # First 10 lines
tail -n 10 file.txt    # Last 10 lines
```

#### Search Operations
```bash
grep "pattern" file          # Search pattern in file
grep -w phoenix *             # Search word 'phoenix' in all files
grep -r "pattern" /path/      # Recursive search
grep -i "pattern" file        # Case-insensitive search
find /path -name "*.txt"      # Find files by name
```

### File Permissions — [BUET MSC 17]

Linux file permissions: `rwxrwxrwx` (Owner | Group | Others)

| Permission | Symbol | Number |
|---|---|---|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |

**Question [BUET MSC 17]**: "Anyone can execute the file `sample`, only the owner can read, no other users can read."

```
Owner: read + execute = r-x = 4+1 = 5
Group: execute only  = --x = 0+1 = 1
Other: execute only  = --x = 0+1 = 1

Command: chmod 511 sample
```

**Common chmod examples:**
```bash
chmod 755 file    # rwxr-xr-x (owner all, group/others r+x)
chmod 644 file    # rw-r--r-- (owner r+w, others r only)
chmod 777 file    # rwxrwxrwx (everyone all permissions)
chmod +x file     # Add execute permission for all
chmod u+x file    # Add execute for owner only
```

### Shell Scripting — Quick Sort — [BUET MSC CSE 24]

```bash
#!/bin/bash
quicksort() {
    local arr=("$@")
    local len=${#arr[@]}
    if [ $len -le 1 ]; then
        echo "${arr[@]}"
        return
    fi
    
    local pivot=${arr[0]}
    local left=()
    local right=()
    
    for ((i=1; i<len; i++)); do
        if [ ${arr[i]} -le $pivot ]; then
            left+=("${arr[i]}")
        else
            right+=("${arr[i]}")
        fi
    done
    
    left_sorted=($(quicksort "${left[@]}"))
    right_sorted=($(quicksort "${right[@]}"))
    echo "${left_sorted[@]}" "$pivot" "${right_sorted[@]}"
}

arr=(3 6 8 10 1 2 1)
sorted=($(quicksort "${arr[@]}"))
echo "Sorted: ${sorted[@]}"
# Output: Sorted: 1 1 2 3 6 8 10
```

---

## 10. System Throughput & Multithreading Problems

### Throughput Calculation — [BUET MSC CSE 21]

**Question**: In a multithreaded server, it takes **25 ms** to get a request, dispatch it, and do the rest of the processing (when data is in block cache). Otherwise, disk I/O is needed requiring an **additional 75 ms** during which the thread sleeps. **One-third** of the time the data is NOT in block cache. How many requests/sec can the server handle on average?

**Solution:**

```
Two cases:
Case 1 (data in cache, probability 2/3):
  - Processing time = 25 ms
  - Thread ACTIVE (using CPU) for 25 ms

Case 2 (data not in cache, probability 1/3):
  - Processing time = 25 + 75 = 100 ms total
  - Thread SLEEPS for 75 ms (CPU serves other threads during this time)
  - Thread ACTIVE for 25 ms

KEY INSIGHT: When thread sleeps (disk I/O wait), another thread runs.
The CPU is blocked only during ACTIVE processing, not during sleep.

CPU active time per request = 25 ms (SAME in both cases)

Because another thread runs during the 75ms sleep, from the SERVER perspective:
  Time to process = 25 ms active per request

Requests per second = 1000 ms / 25 ms = 40 requests/second
```

**Answer: 40 requests/second**

---

## 11. Quick MCQ Reference Card

### Process & Threads
- PCB stores: PID, state, PC, registers, memory info, I/O info
- **New->Ready**: admitted by OS (long-term scheduler)
- **Running->Waiting**: I/O request or wait for event
- **Running->Ready**: interrupt (preemption/time quantum)
- Threads share: code, data, heap, open files (NOT stack, PC, registers)
- Thread creation is faster than process creation (no new address space)

### Scheduling
- **FCFS**: Non-preemptive, convoy effect, fair order but poor avg WT
- **SJF**: Optimal for avg WT (non-preemptive), starvation possible
- **SRTF**: Preemptive SJF, globally optimal, starvation possible
- **RR**: Preemptive, no starvation, time quantum is key parameter
- **Priority**: Starvation solved by aging
- Large time quantum in RR -> degenerates to FCFS
- Aging: Gradually increase priority of waiting processes to prevent starvation

### Synchronization
- **Mutual Exclusion**: Only 1 process in CS at a time
- **Progress**: Cannot be blocked by processes outside CS
- **Bounded Waiting**: Cannot wait indefinitely
- Semaphore **wait()** = P() = down() -> decrement, block if <= 0
- Semaphore **signal()** = V() = up() -> increment, wake if waiting
- **Mutex** = binary semaphore (value 0 or 1) with ownership
- Race condition: concurrent access to shared data without sync

### Deadlock
- 4 Coffman conditions: Mutual Exclusion, Hold & Wait, No Preemption, Circular Wait
- **ALL 4 must hold** simultaneously for deadlock
- RAG cycle with single-instance resources -> deadlock
- RAG cycle with multi-instance resources -> deadlock possible (not guaranteed)
- Banker's Algorithm = deadlock **avoidance**
- Wait-for graph = deadlock **detection** (single instance)
- Prevention: negate one condition; Recovery: kill process or preempt resource

### Memory & Paging
- **Internal fragmentation**: in paging (allocated > needed)
- **External fragmentation**: in segmentation/variable partitioning
- Small page size -> less internal frag, larger page table
- **TLB**: Hardware cache for page table entries; speeds up address translation
- TLB hit -> 1 memory access; TLB miss -> 2 memory accesses
- Page number = logical address div page size
- Page offset = logical address mod page size
- Physical address = frame_number x page_size + offset

### Virtual Memory
- **Page Fault**: Page not in memory; OS must load from disk
- After page fault: check validity -> find frame -> load page -> update table -> restart instruction
- **FIFO**: Oldest page replaced; susceptible to Belady's anomaly
- **LRU**: Least recently used; approximation of optimal; no Belady's anomaly
- **Optimal**: Replace page needed farthest in future; theoretical minimum page faults
- **Thrashing**: Process spends more time paging than executing
- **Working Set**: Pages actively used in current time window
- **Temporal locality**: Same address accessed repeatedly (loops)
- **Spatial locality**: Nearby addresses accessed together (arrays)
- More frames -> fewer page faults (except FIFO with Belady's anomaly)

### Linux Permissions
- `chmod 777` = rwxrwxrwx (all permissions for all)
- `chmod 644` = rw-r--r-- (standard file)
- `chmod 755` = rwxr-xr-x (standard executable)
- `r=4, w=2, x=1` -> sum for each of [owner, group, others]

---

## Past Question Summary & Answers

### [BUET MSC 17] — Define multitasking, multi-programming, multi-threading
> See Section 2 — detailed table comparison above.

### [BUET MSC ICT 15] — Distinguish processes and threads; thread efficiency
> See Section 2 — full comparison table and 5 efficiency reasons.

### [BPDB-AE 11, BUET MSC 15] — 3-state process state diagram
> See Section 2 — full ASCII state diagram with all transitions labeled.

### [BUET MSC ICT 15] — Mutual exclusion code analysis
> See Section 4 — the code does NOT reliably ensure mutual exclusion and can deadlock.

### [GTCL AE 16, BUET MSC ICT 15] — What is deadlock?
> See Section 5 — definition, example, and all 4 conditions.

### [BUET MSC ICT 16] — Resource-allocation graph and deadlock
> See Section 5 — RAG explanation with ASCII diagram.

### [BUET MSC 17, 18, 19] — 4 necessary conditions of deadlock
> **Mutual Exclusion, Hold and Wait, No Preemption, Circular Wait**

### [BUET MSC CSE 21] — Deadlock-free request order for B
> If A requests R2, R3, R1 -> B should request **R1, R2, R3** (avoid circular wait).

### [BUET MSC ICT 15, 17] — Advantages of small page size
> See Section 7 — detailed table comparing small vs large page sizes.

### [BUET MSC ICT 16] — 16 pages x 2048 words, 64 frames: logical/physical address bits
> Logical = 15 bits; Physical = 17 bits. (See Section 7 worked example)

### [BUET MSC 17] — Page table size for 32-bit address, 4KB pages, 4-byte entries
> Page table size = 4 MB. (See Section 7 worked example)

### [BUET MSC ICT 16] — Locality of reference and virtual memory
> See Section 8 — full explanation of temporal and spatial locality.

### [BUET MSC ICT 16, 19] — Page fault causes and OS actions
> See Section 8 — complete 5-step fault handling procedure.

### [BUET MSC 18] — Linux commands for directory, delete, find-word
> `mkdir -p`, `rm -r`, `grep -w phoenix *` — See Section 9.

### [BUET MSC 17] — File permission shell command for 'sample'
> `chmod 511 sample` — See Section 9.

### [BUET MSC ICT 15] — Turnaround time with SJF
> A(8),B(4),C(4),D(4) all at t=0 -> SJF order B,C,D,A -> Avg TAT = 11 minutes.

### [BUET MSC 16] — Round Robin scheduling
> See Section 3 — RR example with Gantt chart, TAT and WT calculations.

### [BUET MSC CSE 21] — Multithreaded server throughput
> 40 requests/second. (See Section 10 — full worked solution)

---

*End of Operating System Notes*
*Reference: Silberschatz et al., "Operating System Concepts", 10th Edition*
