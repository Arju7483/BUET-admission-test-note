# Operating System — MCQ Quick Reference Sheet
### BUET MSc Exam | Dense Facts | No Fluff

---

## 1. OS Basics

| Fact | Answer |
|---|---|
| OS acts as | Intermediary between hardware and user programs |
| Kernel runs in | Privileged (supervisor) mode |
| User programs run in | User mode |
| System call switches mode from | User mode → Kernel mode |
| Spooling stands for | Simultaneous Peripheral Operations On-Line |
| Monolithic kernel example | Linux, Unix |
| Microkernel example | Mach, Minix, QNX |
| Shell is | User interface to the OS (CLI/GUI) |

### OS Types — MCQ Traps
- **Batch OS** → No user interaction during execution
- **Time-sharing OS** → Multiple users, CPU time-sliced (UNIX)
- **Real-time OS** → Hard (zero tolerance) vs Soft (tolerable delay)
- **Distributed OS** → Transparent to user, looks like single system
- **Multiprogramming** → Multiple programs in memory; CPU never idle
- **Multitasking** → Fast CPU switching; illusion of parallelism

---

## 2. Process & Thread

### Process States
```
NEW → READY → RUNNING → TERMINATED
              ↕
           WAITING (BLOCKED)
```

| Transition | Cause |
|---|---|
| New → Ready | Admitted by OS (long-term scheduler) |
| Ready → Running | Dispatched by CPU scheduler |
| Running → Ready | **Interrupt** / time quantum expired |
| Running → Waiting | **I/O request** or event wait |
| Waiting → Ready | I/O completion |
| Running → Terminated | Process exits |

### Process vs Thread

| Feature | Process | Thread |
|---|---|---|
| Address space | Own (separate) | Shared with parent process |
| Resources | Own file handles, I/O | Shared with other threads |
| Stack | Own | Own (per thread) |
| PC & Registers | Own | Own (per thread) |
| Creation overhead | High | Low (~10x faster) |
| Context switch | Slow (save whole state) | Fast |
| Communication | IPC needed (expensive) | Shared memory (direct) |

### Threads SHARE: code, data, heap, open files, signals
### Threads OWN: stack, program counter, registers

### Key Definitions
- **Multitasking** = fast CPU switching between processes (time-sharing)
- **Multiprogramming** = multiple programs in RAM; CPU switches on I/O wait
- **Multithreading** = multiple threads within a single process
- **PCB** = Process Control Block (stores PID, state, PC, registers, memory info)
- **Context Switch** = save state of current process, load state of next
- **Dispatcher** = gives CPU control to selected process (does actual context switch)
- **Dispatch Latency** = time to stop one process and start another

---

## 3. CPU Scheduling

### Metrics Formulas
```
Turnaround Time (TAT) = Completion Time - Arrival Time
Waiting Time (WT)     = TAT - Burst Time
Response Time         = First CPU Time - Arrival Time
Throughput            = Number of processes / Total time
```

### Algorithm Cheat Sheet

| Algorithm | Type | Starvation | Convoy Effect | Special |
|---|---|---|---|---|
| FCFS | Non-preemptive | No | YES | Simplest |
| SJF | Non-preemptive | Yes (long jobs) | No | Optimal avg WT* |
| SRTF | Preemptive | Yes (long jobs) | No | Global optimal WT |
| Round Robin | Preemptive | **No** | No | Best for time-sharing |
| Priority | Both | Yes (low priority) | No | Aging fixes starvation |
| HRRN | Non-preemptive | **No** | No | Response ratio = (W+B)/B |

*SJF is optimal only when all processes arrive at the same time

### Critical MCQ Facts
- **Convoy effect** → Short jobs stuck behind long jobs → **FCFS problem**
- **Starvation** → Process waits indefinitely → fixed by **Aging**
- **Aging** → Gradually increase priority of waiting processes
- **Large time quantum in RR** → degenerates into **FCFS**
- **Small time quantum in RR** → too many context switches, overhead increases
- **SRTF** = preemptive version of SJF
- SJF requires knowing burst times in advance (impractical in reality)
- Round Robin → **no starvation** (everyone gets a turn)
- **Long-term scheduler** → decides which jobs enter memory (controls multiprogramming degree)
- **Short-term scheduler** → picks which ready process runs next (~ms frequency)
- **Medium-term scheduler** → swaps processes in/out of memory

### Scheduler Types Summary
| Scheduler | Frequency | Role |
|---|---|---|
| Long-term (Job) | Low (seconds/minutes) | Job queue → Ready queue |
| Short-term (CPU) | High (milliseconds) | Ready queue → CPU |
| Medium-term (Swapper) | Medium | Memory ↔ Disk swap |

---

## 4. Process Synchronization

### Critical Section Requirements (ALL THREE must hold)
1. **Mutual Exclusion** — Only 1 process in CS at a time
2. **Progress** — If CS is free, a waiting process must eventually enter (no indefinite blocking by outsiders)
3. **Bounded Waiting** — No process waits forever (limited number of other entries before it)

### Semaphore Operations
```
wait(S) / P(S) / down(S):   S = S - 1; block if S < 0
signal(S) / V(S) / up(S):   S = S + 1; wake one if sleeping
```

| Type | Initial Value | Use |
|---|---|---|
| Binary Semaphore | 1 | Mutual exclusion (like mutex) |
| Counting Semaphore | N | Resource pool of size N |
| Mutex | 1 | Like binary semaphore but with **ownership** |

### Key Differences — Mutex vs Semaphore
| | Mutex | Semaphore |
|---|---|---|
| Value | 0 or 1 only | Any non-negative integer |
| Ownership | Yes (only owner can unlock) | No ownership |
| Use | CS protection | Signaling + resource counting |

### Classic Problems Summary
| Problem | Key Semaphores | Key Rule |
|---|---|---|
| Producer-Consumer | mutex=1, empty=N, full=0 | Producer waits on empty; consumer on full |
| Readers-Writers | mutex, read_count | Multiple readers OK; writers exclusive |
| Dining Philosophers | 5 forks as semaphores | Risk: deadlock if all pick left fork |

### Race Condition
- Occurs when result depends on **order of execution** of concurrent processes
- Prevented by synchronization (semaphores, mutex, monitors)

---

## 5. Deadlock

### 4 Coffman Conditions (ALL must hold simultaneously)

| # | Condition | One-line definition |
|---|---|---|
| 1 | **Mutual Exclusion** | Resource held in non-shareable mode |
| 2 | **Hold and Wait** | Process holds resource while waiting for another |
| 3 | **No Preemption** | Resources can't be forcibly taken |
| 4 | **Circular Wait** | P0 waits for P1 waits for ... waits for P0 |

> **Trick**: **M**y **H**orse **N**ever **C**ircles

### Deadlock Strategies

| Strategy | What it does | Example |
|---|---|---|
| **Prevention** | Negate one of 4 conditions | Total ordering of resources → no circular wait |
| **Avoidance** | Check safe state before granting | **Banker's Algorithm** |
| **Detection** | Let it happen, find it | Wait-for graph, detection algorithm |
| **Recovery** | Resolve after detection | Kill process / preempt resource |
| **Ignorance** | Do nothing | Windows, Linux (Ostrich Algorithm) |

### Resource Allocation Graph (RAG) Rules
- **Request edge**: Process → Resource (arrow toward resource)
- **Assignment edge**: Resource → Process (arrow toward process)
- **No cycle** → No deadlock (guaranteed)
- **Cycle + single-instance resources** → **Deadlock** (guaranteed)
- **Cycle + multi-instance resources** → Deadlock **possible** (not certain)

### Banker's Algorithm Key Points
- Used for deadlock **avoidance** (not prevention or detection)
- Need = Max - Allocation
- System is in **safe state** if a safe sequence exists
- A safe sequence = order in which all processes can complete
- If no safe state → **unsafe** (deadlock possible but not certain)

### Deadlock-Free Request Order
- If processes request resources in a **consistent total order** → Circular wait impossible → No deadlock
- Example: A requests R2,R3,R1; B should request starting with R1 (below R2 in ordering)

---

## 6. Memory Management

### Fragmentation

| Type | Cause | Found in |
|---|---|---|
| **Internal** | Allocated > needed | Paging, fixed partitioning |
| **External** | Free space scattered | Variable partitioning, segmentation |

### Allocation Strategies
- **First Fit** → Allocate first hole large enough → **Fastest**
- **Best Fit** → Allocate smallest sufficient hole → **Worst external fragmentation**
- **Worst Fit** → Allocate largest hole → **Poor utilization**
- **Compaction** → Move all processes together to merge free space → expensive

### Swapping
- Move process from RAM to disk → **Swap out**
- Move from disk back to RAM → **Swap in**
- Done by **medium-term scheduler**

---

## 7. Paging

### Key Formulas
```
Page Number (p) = Logical Address ÷ Page Size
Page Offset (d) = Logical Address mod Page Size
Physical Address = Frame[p] × Page Size + d

Number of pages = Logical address space ÷ Page size
Page table size = Number of pages × Entry size

Bits for page number = log2(number of pages)
Bits for offset      = log2(page size)
Logical addr bits    = page bits + offset bits
Physical addr bits   = frame bits + offset bits
```

### Quick Calculations Reference
| Given | Formula |
|---|---|
| Page size = 4KB = 2¹² | Offset bits = 12 |
| 32-bit logical space | 2³² total addresses |
| Number of pages in 32-bit, 4KB pages | 2³²/2¹² = 2²⁰ = 1M pages |
| Page table size (1M pages × 4B entry) | = 4 MB |

### Small vs Large Page Size

| | Small Page | Large Page |
|---|---|---|
| Internal Fragmentation | Less ✅ | More ❌ |
| Page Table Size | Bigger ❌ | Smaller ✅ |
| I/O Efficiency | Worse ❌ | Better ✅ |
| Locality exploitation | Harder ❌ | Easier ✅ |

### TLB (Translation Lookaside Buffer)
- Hardware cache for page table entries
- **TLB Hit** → 1 memory access (fast)
- **TLB Miss** → 2 memory accesses (page table + data)
- **Hit ratio** = fraction of accesses found in TLB
- EAT = h(t_TLB + t_mem) + (1-h)(t_TLB + 2×t_mem)

### Paging vs Segmentation

| Feature | Paging | Segmentation |
|---|---|---|
| Unit size | Fixed (pages) | Variable (segments) |
| Fragmentation | Internal | External |
| User awareness | Transparent | User-visible |
| Sharing/Protection | Less natural | More natural |

---

## 8. Virtual Memory

### Demand Paging
- Pages loaded **only when needed** (on first access)
- **Valid bit** in page table: 1 = in memory, 0 = on disk
- **Page Fault** = accessing page not in memory

### Page Fault Steps (OS Actions)
1. Check if reference is valid (in PCB table)
2. Invalid → Terminate (segmentation fault)
3. Valid → Find free frame
4. Load page from disk into free frame
5. Update page table (valid bit = 1)
6. Restart the faulting instruction

### Page Replacement Algorithms

| Algorithm | Replace | Belady's Anomaly | Implementable |
|---|---|---|---|
| **FIFO** | Oldest loaded page | **YES** (more frames can = more faults) | Yes |
| **LRU** | Least recently used | No | Yes (costly) |
| **Optimal (OPT)** | Farthest future use | No | **No** (needs future knowledge) |
| **Clock (Second Chance)** | FIFO with reference bit | No | Yes (practical LRU approx) |

- OPT gives **minimum** page faults → theoretical benchmark only
- LRU is the best **practical** algorithm
- FIFO is **simplest** but suffers Belady's anomaly

### Thrashing
- **Definition**: Process spends more time paging than executing
- **Cause**: Too many processes, not enough frames per process
- **Detection**: CPU utilization drops while paging increases
- **Solution**: Working Set Model — give each process frames for its working set

### Locality of Reference
- **Temporal Locality**: Access same address again soon (loops, function calls)
- **Spatial Locality**: Access nearby addresses (arrays, sequential code)
- WHY IT MATTERS: Makes virtual memory practical — working set stays small

### Working Set
- Set of pages a process actively uses in last Δ references
- If sum of all working sets > physical memory → **Thrashing**
- Solution: Suspend some processes to free frames

---

## 9. Linux Commands — Quick Facts

### Essential Commands
```bash
mkdir -p a/b/c          # Create directory tree (with parents)
rm -r dirname           # Delete folder recursively
rm -rf dirname          # Force delete (no prompt)
grep -w word *          # Find exact word in all files
grep -r pattern /path   # Recursive grep
chmod 755 file          # rwxr-xr-x
chmod 644 file          # rw-r--r--
chmod 511 file          # r-x--x--x  (owner r+x, others x only)
```

### Permission Number System
```
r = 4,  w = 2,  x = 1

Owner | Group | Others
 rwx  |  r-x  |  r-x   =  7  5  5  →  chmod 755
 rw-  |  r--  |  r--   =  6  4  4  →  chmod 644
 r-x  |  --x  |  --x   =  5  1  1  →  chmod 511
 rwx  |  rwx  |  rwx   =  7  7  7  →  chmod 777
```

### BUET Specific — [BUET MSC 17]
- "Anyone executes, only owner reads" → `chmod 511 sample`
- Owner: r+x = 4+1 = 5 | Group: x = 1 | Others: x = 1

---

## 10. Key Numbers & Tricky MCQ Facts

### Must-Know Numbers
| Fact | Value |
|---|---|
| 1 KB | 2¹⁰ = 1024 bytes |
| 1 MB | 2²⁰ bytes |
| 1 GB | 2³⁰ bytes |
| 32-bit address space | 2³² = 4 GB |
| 4KB page size offset bits | 12 bits |

### Tricky MCQ Traps
- ❗ SRTF is the **preemptive** version of SJF (not FCFS)
- ❗ SJF is optimal only when **all arrive at the same time**
- ❗ Round Robin has **no starvation** (not FCFS, not priority)
- ❗ Belady's anomaly is specific to **FIFO**, not LRU or OPT
- ❗ Banker's algorithm is **avoidance**, not prevention or detection
- ❗ Deadlock needs **ALL 4** conditions, not just one
- ❗ A cycle in RAG with **multi-instance** resources → possible deadlock, NOT certain
- ❗ **Internal** fragmentation → paging; **External** → segmentation
- ❗ TLB stores **page-to-frame** mappings (not data itself)
- ❗ Mutex has **ownership** — only the locker can unlock it; semaphore doesn't
- ❗ wait() = P() = down(); signal() = V() = up()
- ❗ **Virtual memory** allows logical space > physical memory
- ❗ **Demand paging** loads page only on first access (lazy loading)
- ❗ Page fault restarts the **same instruction** that caused it
- ❗ **Thrashing** fix = reduce degree of multiprogramming
- ❗ Context switch itself is **pure overhead** (no useful work done during it)
- ❗ **Long-term scheduler** controls degree of multiprogramming
- ❗ **Dispatcher** does the actual context switch (not scheduler)

### Process Synchronization Traps
- ❗ **Busy waiting** = spinning loop (wastes CPU); used in spinlocks
- ❗ **Deadlock** among waiters = **Progress** condition violated
- ❗ Starvation = **Bounded Waiting** condition violated
- ❗ In Producer-Consumer: Producer does `wait(empty)` first, then `wait(mutex)`
  - Reversing order → **Deadlock**

---

## 11. Worked Problem Shortcuts

### Scheduling — TAT & WT Formula
```
Given: Processes with Arrival Time (AT) and Burst Time (BT)

SJF (all arrive at t=0), processes: A(8), B(4), C(4), D(4)
Order: B→C→D→A
TAT:  B=4, C=8, D=12, A=20
Avg TAT = (4+8+12+20)/4 = 11
Avg WT  = Avg TAT - Avg BT = 11 - 5 = 6
```

### Paging Address Calculation
```
Given: 16 pages, page size = 2048 words, 64 frames
Pages = 16 = 2⁴  →  page number needs 4 bits
Page size = 2048 = 2¹¹  →  offset needs 11 bits
Logical address = 4+11 = 15 bits
Frames = 64 = 2⁶  →  frame number needs 6 bits
Physical address = 6+11 = 17 bits
```

### Page Table Size
```
32-bit logical address, page size = 4KB (2¹²), entry = 4 bytes
Pages = 2³² / 2¹² = 2²⁰
Page table = 2²⁰ × 4 bytes = 4 MB
```

### Multithreaded Server Throughput
```
25ms active, 75ms disk sleep (1/3 of requests need disk)
Active CPU time per request = 25ms (always)
During 75ms sleep → another thread runs (no CPU waste)
Throughput = 1000ms / 25ms = 40 requests/second
```

---

*OS MCQ Sheet | Silberschatz & Tanenbaum | BUET MSc Prep*
