# Operating Systems — Complete Notes
### Interview-Ready | Placement-Focused | Intuition + Theory + Q&A

> **How to use:** Every section opens with the interview question it answers.
> The explanation *is* the answer. Read once → answer any OS question.

---

## Table of Contents

**PART 1 — FOUNDATIONS**
1. [What is an OS?](#1-what-is-an-os)
2. [Kernel & System Calls](#2-kernel--system-calls)
3. [Types of OS](#3-types-of-os)

**PART 2 — PROCESSES & THREADS**
4. [Processes — PCB, States, Lifecycle](#4-processes)
5. [Threads — User vs Kernel, Multithreading](#5-threads)
6. [Process vs Thread](#6-process-vs-thread)
7. [Context Switching](#7-context-switching)
8. [Inter-Process Communication IPC](#8-inter-process-communication)

**PART 3 — CPU SCHEDULING**
9. [Scheduling Concepts & Criteria](#9-scheduling-concepts--criteria)
10. [Scheduling Algorithms](#10-scheduling-algorithms)

**PART 4 — SYNCHRONISATION & DEADLOCK**
11. [Race Conditions & Critical Section](#11-race-conditions--critical-section)
12. [Mutex, Semaphore, Monitor](#12-mutex-semaphore-monitor)
13. [Classical Synchronisation Problems](#13-classical-synchronisation-problems)
14. [Deadlock](#14-deadlock)

**PART 5 — MEMORY MANAGEMENT**
15. [Memory Layout of a Process](#15-memory-layout-of-a-process)
16. [Paging](#16-paging)
17. [Segmentation](#17-segmentation)
18. [Virtual Memory & Demand Paging](#18-virtual-memory--demand-paging)
19. [Page Replacement Algorithms](#19-page-replacement-algorithms)
20. [Thrashing](#20-thrashing)

**PART 6 — STORAGE & FILE SYSTEMS**
21. [File Systems](#21-file-systems)
22. [Disk Scheduling](#22-disk-scheduling)

**PART 7 — INTERVIEW Q&A**
23. [Interview Q&A — Foundations & Processes](#23-interview-qa--foundations--processes)
24. [Interview Q&A — Scheduling & Synchronisation](#24-interview-qa--scheduling--synchronisation)
25. [Interview Q&A — Memory Management](#25-interview-qa--memory-management)
26. [Interview Q&A — Advanced & Tricky](#26-interview-qa--advanced--tricky)
27. [Quick Reference Cheat Sheet](#27-quick-reference-cheat-sheet)

---

# PART 1 — FOUNDATIONS

---

## 1. What is an OS?

> **Interview Q:** *What is an Operating System? What are its main functions?*

An **Operating System** is system software that acts as an **intermediary between hardware and user programs**. It manages hardware resources and provides services to applications so programmers don't have to deal with raw hardware.

```
User Applications  (Chrome, VS Code, Python script)
        ↓
  Operating System  (Windows, Linux, macOS)
        ↓
    Hardware  (CPU, RAM, Disk, GPU, Network)
```

### Five Core Functions

| Function | What the OS does |
|---|---|
| **Process Management** | Create, schedule, terminate processes; allocate CPU |
| **Memory Management** | Allocate/deallocate RAM; virtual memory; paging |
| **File System Management** | Files, directories, permissions, storage abstraction |
| **Device Management** | Drivers, I/O scheduling, hardware abstraction |
| **Security & Protection** | Access control, user isolation, authentication |

### Analogy

> The OS is like a **government**. Citizens (programs) don't directly access land (hardware). The government (OS) manages land allocation (memory), assigns jobs (CPU scheduling), maintains roads (I/O), and enforces laws (security).

---

## 2. Kernel & System Calls

> **Interview Q:** *What is a kernel? What is a system call? What is the difference between user mode and kernel mode?*

### Kernel

The **kernel** is the core of the OS — the part that runs with full hardware privileges. It manages CPU, memory, and devices directly. Everything else (shell, GUI, apps) runs on top of it.

```
┌─────────────────────────────────┐
│       User Space                │  ← low privilege
│  (shell, apps, libraries)       │
├─────────────────────────────────┤
│       Kernel Space              │  ← full hardware access
│  (scheduler, memory mgr, VFS)  │
├─────────────────────────────────┤
│       Hardware                  │
└─────────────────────────────────┘
```

### User Mode vs Kernel Mode

| | User Mode | Kernel Mode |
|---|---|---|
| Privilege | Restricted | Full (ring 0) |
| Hardware access | No — goes through kernel | Direct |
| Crash impact | Only that process crashes | Entire system can crash |
| Examples | Your Python script, Chrome | OS scheduler, device drivers |

### System Calls

A **system call** is the mechanism a user-space program uses to request a service from the kernel — the only legal way to cross the user ↔ kernel boundary.

```
User program calls: read(fd, buf, n)
         ↓
  C library wraps it → software interrupt / syscall instruction
         ↓
  CPU switches to kernel mode
         ↓
  Kernel handles request (reads from disk/buffer)
         ↓
  CPU switches back to user mode
         ↓
  Return value back to program
```

**Common system calls:**

| Category | Examples |
|---|---|
| Process | `fork()`, `exec()`, `exit()`, `wait()` |
| File | `open()`, `read()`, `write()`, `close()` |
| Memory | `mmap()`, `brk()` |
| Network | `socket()`, `connect()`, `send()`, `recv()` |
| IPC | `pipe()`, `shmget()`, `msgget()` |

> **System call vs function call:** A regular function call stays in user mode — just a jump to another address. A system call crosses into kernel mode via a software interrupt — has significant overhead (~100s of ns). This is why we batch I/O operations instead of calling `write()` one byte at a time.

### Kernel Types

| Type | Design | Examples | Pro / Con |
|---|---|---|---|
| **Monolithic** | All OS services in one large kernel | Linux, Unix | Fast (no IPC between services) but large |
| **Microkernel** | Minimal kernel; services in user space | Minix, QNX, L4 | Stable, isolated but slower (IPC overhead) |
| **Hybrid** | Mix of both | Windows NT, macOS XNU | Balance of both |
| **Exokernel** | Minimal abstraction; apps control hardware | Research OS | Max performance |

---

## 3. Types of OS

> **Interview Q:** *What are the different types of operating systems?*

| Type | Description | Example |
|---|---|---|
| **Batch** | Jobs collected, run without interaction | Early IBM mainframes |
| **Time-Sharing** | Multiple users share CPU via time slices | Unix |
| **Real-Time (RTOS)** | Strict timing guarantees; hard/soft deadlines | VxWorks, FreeRTOS |
| **Distributed** | Multiple networked machines act as one | LOCUS, Amoeba |
| **Embedded** | Minimal OS for dedicated hardware | Android (phones), FreeRTOS (microcontrollers) |
| **Network OS** | Manages network resources | Novell NetWare |

**Hard vs Soft Real-Time:**
- **Hard RTOS:** Missing a deadline = system failure (airbag controller, pacemaker)
- **Soft RTOS:** Missing a deadline = degraded quality (video streaming, online gaming)

---

# PART 2 — PROCESSES & THREADS

---

## 4. Processes

> **Interview Q:** *What is a process? What is a PCB? Explain process states.*

### What is a Process?

A **process** is a **program in execution** — an active instance of a program. A program is just code sitting on disk; a process is that code loaded into memory with its own resources.

```
Program (disk)  +  Execution context (CPU state, memory, files)  =  Process
```

### Memory Layout of a Process

```
High address ┌──────────────┐
             │   Stack      │  ← grows downward (function calls, local vars)
             │      ↓       │
             │   (gap)      │
             │      ↑       │
             │   Heap       │  ← grows upward (malloc / new)
             ├──────────────┤
             │   BSS        │  ← uninitialised global/static vars
             ├──────────────┤
             │   Data       │  ← initialised global/static vars
             ├──────────────┤
             │   Text/Code  │  ← program instructions (read-only)
Low address  └──────────────┘
```

### Process Control Block (PCB)

The PCB is a **data structure maintained by the OS** for every process — the OS's complete record of a process.

```
PCB contains:
  ├── Process ID (PID)
  ├── Process State (running, ready, waiting...)
  ├── Program Counter (address of next instruction)
  ├── CPU Registers (all register values)
  ├── CPU Scheduling info (priority, queue pointers)
  ├── Memory Management info (page tables, segment tables)
  ├── I/O Status info (open files, devices allocated)
  └── Accounting info (CPU time used, time limits)
```

> **Analogy:** PCB is like a **bookmark + sticky note** for a process. When the OS pauses a process, it saves everything to the PCB. When the process resumes, it restores from the PCB — as if it never stopped.

### Process States (5-State Model)

```
              fork()
  NEW ──────────────────→ READY
                            │  ↑
               scheduler    │  │  preemption / time slice ends
               dispatches   ↓  │
                         RUNNING ──────→ TERMINATED (exit())
                            │
               I/O or       │
               event wait   ↓
                         WAITING / BLOCKED
                            │
               I/O done /   │
               event occurs ↓
                          READY
```

| State | Meaning |
|---|---|
| **New** | Process being created |
| **Ready** | Waiting for CPU; in ready queue |
| **Running** | Currently executing on CPU |
| **Waiting / Blocked** | Waiting for I/O, event, or resource |
| **Terminated** | Finished execution; OS cleaning up |

### `fork()` and `exec()`

```c
pid_t pid = fork();   // creates a COPY of the current process
                      // returns 0 to child, child's PID to parent
if (pid == 0) {
    // Child process
    exec("ls", args); // replaces child's memory image with new program
} else {
    // Parent process
    wait(NULL);       // waits for child to finish
}
```

- **`fork()`** — duplicates the calling process. Child gets a copy of parent's address space (copy-on-write in modern OSes).
- **`exec()`** — replaces the current process's memory with a new program. PID stays the same.
- **Zombie process** — child finishes but parent hasn't called `wait()` yet. PCB still exists.
- **Orphan process** — parent dies before child. Adopted by `init` (PID 1).

---

## 5. Threads

> **Interview Q:** *What is a thread? What is the difference between user-level and kernel-level threads?*

### What is a Thread?

A **thread** is the smallest unit of CPU execution — a **lightweight process** — a sequence of instructions within a process that can be scheduled independently.

All threads in a process **share:**
- Code segment, Data segment, Heap, Open file descriptors

Each thread has its **own:**
- Stack, Program Counter, CPU Registers, Thread ID

```
Process
├── Shared: code, heap, data, open files
├── Thread 1: stack, PC, registers
├── Thread 2: stack, PC, registers
└── Thread 3: stack, PC, registers
```

### Why Threads?

| Reason | Explanation |
|---|---|
| **Responsiveness** | UI thread stays responsive while worker thread does I/O |
| **Resource sharing** | Threads share memory — no IPC needed to communicate |
| **Economy** | Creating a thread is ~10–100× cheaper than a process |
| **Scalability** | Run threads on multiple CPUs in true parallel |

### User-Level vs Kernel-Level Threads

| | User-Level Threads (ULT) | Kernel-Level Threads (KLT) |
|---|---|---|
| Managed by | Thread library (user space) | Kernel |
| Kernel awareness | Kernel doesn't know they exist | Kernel knows and schedules each |
| Context switch | Fast (no system call) | Slower (system call needed) |
| Parallelism | No — kernel sees ONE process | Yes — scheduled on multiple CPUs |
| Blocking | If one blocks on I/O, all block | Others can run while one blocks |
| Examples | Green threads (old Java) | Linux pthreads, Windows threads |

### Threading Models

| Model | Description |
|---|---|
| **Many-to-One** | Many ULTs → 1 KLT. No parallelism. One blocks = all block |
| **One-to-One** | 1 ULT → 1 KLT. True parallelism. Linux/Windows default |
| **Many-to-Many** | Many ULTs → Many KLTs. Best flexibility. Complex to implement |

### Multithreading: Python vs C++

```python
# Python — GIL limits true parallelism for CPU-bound tasks
import threading
def task(): ...
t = threading.Thread(target=task)
t.start()
t.join()
# Use multiprocessing for CPU-bound, threading for I/O-bound
```

```cpp
// C++ — true parallelism, no GIL
#include <thread>
void task() { ... }
std::thread t(task);
t.join();
```

---

## 6. Process vs Thread

> **Interview Q:** *What is the difference between a process and a thread? Which is faster to create?*

| Dimension | Process | Thread |
|---|---|---|
| Definition | Independent program in execution | Execution unit within a process |
| Memory | Own address space | Shares process address space |
| Creation cost | Heavy (~ms, allocate memory + PCB) | Light (~µs, just stack + TCB) |
| Communication | IPC (pipes, sockets, shared memory) | Direct (shared heap / globals) |
| Context switch | Expensive (full address space switch + TLB flush) | Cheaper (same address space, no TLB flush) |
| Crash isolation | One process crash doesn't affect others | One thread crash can kill the process |
| Synchronisation | Rarely needed (isolated memory) | Always needed (shared memory) |
| Example use | Chrome tab (isolated for safety) | Chrome renderer threads within one tab |

> **Analogy:** Processes are like separate **houses** — each has its own rooms. Threads are like **people in the same house** — they share the kitchen (heap) and living room (globals) but have their own bedrooms (stack).

---

## 7. Context Switching

> **Interview Q:** *What is a context switch? What does it involve? Why is it expensive?*

A **context switch** is the OS saving the state of the currently running process/thread and restoring the state of the next one to run on the CPU.

### Steps

```
Running: Process A
    ↓
1. Timer interrupt / voluntary yield / I/O wait
    ↓
2. OS saves Process A's context into A's PCB:
   - Program Counter
   - All CPU registers (general, floating point, SIMD)
   - Stack pointer
    ↓
3. OS runs scheduler → selects Process B
    ↓
4. OS restores Process B's context from B's PCB
    ↓
Running: Process B
```

### Why It Is Expensive

| Cost | Reason |
|---|---|
| **Direct** | Save/restore dozens of registers |
| **TLB flush** | On process switch (different address space), TLB must be flushed → cold-start penalty |
| **Cache cold** | CPU cache filled with A's data; needs B's data → cache misses spike |
| **Typical cost** | Process context switch: ~1–10µs. Thread context switch: ~100ns–1µs |

> Thread context switch is faster because threads share the same address space — TLB doesn't need flushing.

---

## 8. Inter-Process Communication

> **Interview Q:** *What are the methods of IPC? Compare shared memory and message passing.*

Since processes have isolated address spaces, they need explicit mechanisms to communicate.

### IPC Methods

| Method | How | Speed | Use case |
|---|---|---|---|
| **Pipe** | Unidirectional byte stream; parent ↔ child | Fast | Shell pipes (`ls \| grep`) |
| **Named Pipe (FIFO)** | Like pipe but has a name; unrelated processes | Fast | Logger service |
| **Message Queue** | OS-managed queue of messages | Medium | Task queues |
| **Shared Memory** | Map same physical memory into both processes | Fastest | High-throughput data sharing |
| **Socket** | Network-style communication (same or different host) | Variable | Client-server, microservices |
| **Signal** | Async notification to a process | Instant | `SIGINT`, `SIGKILL`, `SIGSEGV` |
| **Semaphore** | Synchronisation primitive (not data transfer) | Fast | Coordinate access to shared memory |

### Shared Memory vs Message Passing

| | Shared Memory | Message Passing |
|---|---|---|
| Speed | Fastest (no copy) | Slower (data copied) |
| Synchronisation | Manual (need semaphores/mutexes) | Built-in (OS handles ordering) |
| Complexity | Higher (race conditions possible) | Lower |
| Example | `mmap()`, `shmget()` | `pipe()`, `send()`/`recv()` |

### Common Signals (Linux)

| Signal | Value | Meaning |
|---|---|---|
| `SIGINT` | 2 | Ctrl+C — interrupt |
| `SIGKILL` | 9 | Force kill — **cannot** be caught or ignored |
| `SIGTERM` | 15 | Polite termination — **can** be caught |
| `SIGSEGV` | 11 | Segmentation fault (invalid memory access) |
| `SIGCHLD` | 17 | Child process stopped or terminated |
| `SIGSTOP` | 19 | Pause process — **cannot** be caught |

---

# PART 3 — CPU SCHEDULING

---

## 9. Scheduling Concepts & Criteria

> **Interview Q:** *What is CPU scheduling? What are the criteria used to evaluate scheduling algorithms?*

**CPU Scheduling** is the OS deciding which process in the ready queue gets the CPU next. Essential for multiprogramming — keeping the CPU busy while other processes wait for I/O.

### Scheduling Queues

```
New → Job Queue (all processes)
         ↓
      Ready Queue (in RAM, ready to run) → CPU
         ↑ (after I/O done)
      Wait Queue (blocked on I/O / event)
         ↓
      I/O Device Queue (waiting for specific device)
```

### Types of Schedulers

| Scheduler | Role | Frequency |
|---|---|---|
| **Long-term (Job)** | Admits processes from disk to RAM | Infrequent (seconds–minutes) |
| **Short-term (CPU)** | Picks next process from ready queue | Very frequent (~100ms) |
| **Medium-term** | Swaps processes in/out of RAM (swapping) | Moderate |

### Scheduling Criteria

| Metric | Definition | Goal |
|---|---|---|
| **CPU Utilisation** | % of time CPU is busy | Maximise (target: 40–90%) |
| **Throughput** | Processes completed per unit time | Maximise |
| **Turnaround Time** | Total time from submission to completion | Minimise |
| **Waiting Time** | Time spent in ready queue | Minimise |
| **Response Time** | Time from request to first response | Minimise |

```
Turnaround Time = Completion Time − Arrival Time
Waiting Time    = Turnaround Time − Burst Time
Response Time   = Time of first CPU − Arrival Time
```

### Preemptive vs Non-Preemptive

| | Non-Preemptive | Preemptive |
|---|---|---|
| Once CPU given | Process runs until it finishes or blocks | OS can interrupt and take CPU back |
| Fairness | Poor | Better |
| Overhead | Low | Higher (more context switches) |
| Examples | FCFS, non-preemptive SJF | Round Robin, SRTF, Priority (preemptive) |

---

## 10. Scheduling Algorithms

> **Interview Q:** *Explain FCFS, SJF, Round Robin, and Priority scheduling. What are their trade-offs?*

### FCFS — First Come First Served

Non-preemptive. Process that arrives first runs first. Implemented with a FIFO queue.

```
Processes: P1(bt=24), P2(bt=3), P3(bt=3)  — all arrive at t=0

Gantt: |──────P1(0-24)──────|P2(24-27)|P3(27-30)|

Waiting: P1=0, P2=24, P3=27
Average waiting time = (0 + 24 + 27) / 3 = 17ms
```

**Problem — Convoy Effect:** Short processes get stuck behind one long process. Like a truck blocking all cars at a toll.

---

### SJF — Shortest Job First

Non-preemptive. Process with the smallest burst time runs next. **Optimal** — gives minimum average waiting time.

**Problem:** Requires knowing burst time in advance (often unknown). Causes **starvation** of long processes.

```
Processes: P1(bt=6), P2(bt=8), P3(bt=7), P4(bt=3)  — all arrive t=0

Gantt: |P4(0-3)|P1(3-9)|P3(9-16)|P2(16-24)|

Average waiting time = (3 + 16 + 9 + 0) / 4 = 7ms   (vs FCFS = 10.25ms)
```

---

### SRTF — Shortest Remaining Time First

Preemptive version of SJF. If a new process arrives with a shorter remaining time than the current process, preempt current.

**Optimal** for minimising average waiting time with arrivals. High context-switch overhead. Starvation possible.

---

### Round Robin (RR)

Preemptive. Each process gets a fixed **time quantum (q)**. If not done, preempted and placed at the back of the queue. Designed for time-sharing systems.

```
Time Quantum = 4ms
Processes: P1(bt=24), P2(bt=3), P3(bt=3)

Gantt: |P1(0-4)|P2(4-7)|P3(7-10)|P1(10-14)|P1(14-18)|P1(18-22)|P1(22-26)|P1(26-30)|

Average waiting time = (6 + 4 + 7) / 3 = 5.66ms
```

**Choosing time quantum:**
- Too small → too many context switches → overhead dominates
- Too large → degenerates to FCFS
- Rule of thumb: 80% of CPU bursts should be shorter than q (typically 10–100ms)

---

### Priority Scheduling

Each process has a priority. Highest priority runs first. Can be preemptive or non-preemptive.

**Problem: Starvation** — low-priority processes may never run.
**Solution: Aging** — gradually increase priority of waiting processes over time.

---

### Multilevel Queue Scheduling

Ready queue split into multiple queues (e.g., foreground/background), each with its own algorithm. Processes permanently assigned to a queue.

```
┌──────────────────────┐  ← Foreground (interactive): Round Robin
│  System processes    │
│  Interactive procs   │
├──────────────────────┤  ← Background (batch): FCFS
│  Batch processes     │
│  Student processes   │
└──────────────────────┘
```

---

### Multilevel Feedback Queue (MLFQ)

Like multilevel queue but processes **can move between queues**. New processes start at highest priority. If they use full quantum → move down (CPU-bound). If they do I/O → move up (interactive). **Most flexible and widely used** (Linux CFS, Windows).

```
Queue 1 (q=8ms,  highest priority)  — new / I/O-bound processes
Queue 2 (q=16ms)
Queue 3 (FCFS,   lowest priority)   — CPU-bound long jobs
```

---

### Scheduling Algorithm Summary

| Algorithm | Preemptive | Starvation | Avg Wait | Best for |
|---|---|---|---|---|
| FCFS | No | No | High | Batch, simple |
| SJF | No | Yes | Optimal | Known burst times |
| SRTF | Yes | Yes | Optimal | Known burst times |
| Round Robin | Yes | No | Medium | Time-sharing |
| Priority | Both | Yes | Low (high prio) | Real-time |
| MLFQ | Yes | No (aging) | Good | General purpose |

---

# PART 4 — SYNCHRONISATION & DEADLOCK

---

## 11. Race Conditions & Critical Section

> **Interview Q:** *What is a race condition? What is the critical section problem?*

### Race Condition

A **race condition** occurs when multiple threads/processes access shared data concurrently and the outcome depends on the **order of execution** — which is non-deterministic.

```c
// Global: int counter = 0;
// Thread 1 and Thread 2 both run simultaneously:
void increment() {
    counter++;   // NOT atomic! Expands to:
                 // 1. LOAD counter into register
                 // 2. ADD 1 to register
                 // 3. STORE register back to counter
}

// If threads interleave:
// T1: LOAD (reads 5)
// T2: LOAD (reads 5)
// T1: ADD → 6, STORE → counter = 6
// T2: ADD → 6, STORE → counter = 6   ← should be 7, lost update!
```

### Critical Section

A **critical section** is a segment of code that accesses shared resources and must not be executed by more than one process/thread simultaneously.

### Requirements for a Valid Solution — All three must hold

| Requirement | Meaning |
|---|---|
| **Mutual Exclusion** | Only ONE process in critical section at a time |
| **Progress** | If no process is in CS and some want to enter, selection can't be postponed indefinitely |
| **Bounded Waiting** | A process that wants to enter CS must eventually be allowed in — no starvation |

---

## 12. Mutex, Semaphore, Monitor

> **Interview Q:** *What is the difference between a mutex and a semaphore? What is a monitor?*

### Mutex (Mutual Exclusion Lock)

A **mutex** is a locking mechanism — only the thread that **locked** it can **unlock** it. Binary: locked or unlocked.

```c
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void increment() {
    pthread_mutex_lock(&lock);    // acquire lock (blocks if taken)
    counter++;                    // critical section
    pthread_mutex_unlock(&lock);  // release lock
}
```

```python
import threading
lock = threading.Lock()

def increment():
    with lock:       # acquire on enter, release on exit (even on exception)
        counter += 1
```

---

### Semaphore

A **semaphore** is a signalling mechanism — a non-negative integer with two atomic operations:
- **`wait()` / `P()`** — decrement; if result < 0, block
- **`signal()` / `V()`** — increment; wake one blocked process

```c
sem_t sem;
sem_init(&sem, 0, 1);   // initial value = 1 (binary semaphore)

sem_wait(&sem);   // P() — enter critical section
// critical section
sem_post(&sem);   // V() — leave critical section
```

**Binary semaphore (value 0 or 1):** works like a mutex, but the locker and unlocker can be different threads — used for signalling.

**Counting semaphore (value 0 to N):** controls access to a pool of N resources.

```c
sem_t db_pool;
sem_init(&db_pool, 0, 5);   // 5 DB connections available

sem_wait(&db_pool);   // blocks if all 5 are in use
// use connection
sem_post(&db_pool);   // release back to pool
```

---

### Mutex vs Semaphore

| | Mutex | Semaphore |
|---|---|---|
| Purpose | Mutual exclusion (ownership) | Signalling + resource counting |
| Value | Binary (locked / unlocked) | Integer (0 to N) |
| Ownership | Yes — only locker can unlock | No — any thread can signal |
| Use case | Protect a single shared resource | Limit concurrent access, producer-consumer |

> **Analogy:** Mutex = a **toilet key** — only one person has it, only they give it back. Semaphore = **parking lot slots** — multiple slots, anyone can leave (signal) freeing a slot.

---

### Monitor

A **monitor** is a high-level synchronisation construct combining a mutex (implicit) with condition variables (for threads to wait/signal each other). Used in Java (`synchronized`), Python (`threading.Condition`).

```python
import threading

class BoundedBuffer:
    def __init__(self, size):
        self.buffer = []
        self.size = size
        self.lock = threading.Lock()
        self.not_full  = threading.Condition(self.lock)
        self.not_empty = threading.Condition(self.lock)

    def produce(self, item):
        with self.not_full:
            while len(self.buffer) == self.size:
                self.not_full.wait()       # release lock + block
            self.buffer.append(item)
            self.not_empty.notify()        # wake a consumer

    def consume(self):
        with self.not_empty:
            while not self.buffer:
                self.not_empty.wait()
            item = self.buffer.pop(0)
            self.not_full.notify()         # wake a producer
            return item
```

---

### Spinlock

A lock where the waiting thread **busy-waits** (loops checking the lock) instead of blocking. Fast when the lock is held very briefly (no context switch overhead). Wastes CPU if held long. Used in OS kernel internals and interrupt handlers.

---

## 13. Classical Synchronisation Problems

> **Interview Q:** *Explain the Producer-Consumer / Dining Philosophers / Readers-Writers problem.*

### Producer-Consumer (Bounded Buffer)

Producer creates items and puts them in a shared buffer of size N. Consumer takes items from the buffer.

```
Semaphores:
  mutex = 1    — mutual exclusion for buffer access
  empty = N    — count of empty slots
  full  = 0    — count of filled slots

Producer:
  wait(empty)    ← block if no empty slot
  wait(mutex)    ← lock buffer
  add item to buffer
  signal(mutex)
  signal(full)   ← one more full slot available

Consumer:
  wait(full)     ← block if no item
  wait(mutex)
  remove item from buffer
  signal(mutex)
  signal(empty)  ← one more empty slot available
```

---

### Dining Philosophers

5 philosophers sit around a table. 5 chopsticks between them. Each needs 2 chopsticks (left + right) to eat.

**Problem:** If all pick up left chopstick simultaneously → circular wait → deadlock.

**Solutions:**
1. Allow at most 4 philosophers to sit at once
2. Asymmetric — odd philosophers pick left first, even pick right first
3. Use a semaphore to allow only one philosopher to pick up both chopsticks atomically

---

### Readers-Writers

Multiple readers can read simultaneously. Writers need exclusive access — no readers or writers allowed at the same time.

```
Semaphores:
  mutex      = 1   — protect read_count
  write_lock = 1   — exclusive write access
  read_count = 0

Reader:
  wait(mutex)
  read_count++
  if read_count == 1: wait(write_lock)   ← first reader blocks writers
  signal(mutex)
  ← read ←
  wait(mutex)
  read_count--
  if read_count == 0: signal(write_lock) ← last reader unblocks writers
  signal(mutex)

Writer:
  wait(write_lock)
  ← write ←
  signal(write_lock)
```

**Problem:** Writers can starve if readers keep arriving. Fix: give writers priority or use a fair queue.

---

## 14. Deadlock

> **Interview Q:** *What is a deadlock? What are the four necessary conditions? Explain Banker's algorithm.*

### What is a Deadlock?

A **deadlock** is a situation where a set of processes are **permanently blocked**, each waiting for a resource held by another process in the set.

```
Process A holds Resource 1, needs Resource 2
Process B holds Resource 2, needs Resource 1
→ Both wait forever. Neither can proceed.
```

### Four Necessary Conditions (Coffman) — ALL four must hold simultaneously

| Condition | Meaning |
|---|---|
| **Mutual Exclusion** | At least one resource must be non-shareable |
| **Hold and Wait** | Process holds ≥1 resource while waiting for more |
| **No Preemption** | Resources can't be forcibly taken — only voluntarily released |
| **Circular Wait** | Circular chain: P1 waits for P2, P2 waits for P3, … Pn waits for P1 |

### Dealing with Deadlock

| Approach | Strategy |
|---|---|
| **Prevention** | Ensure at least one Coffman condition never holds |
| **Avoidance** | Dynamically ensure system stays in safe state (Banker's algorithm) |
| **Detection & Recovery** | Allow deadlocks, detect them, then recover |
| **Ignorance** | Pretend it doesn't happen — used in most commercial OS (ostrich algorithm) |

### Deadlock Prevention — Breaking Each Condition

| Condition to Break | Method |
|---|---|
| Mutual Exclusion | Make resources shareable (often impossible) |
| Hold and Wait | Request ALL resources at once before starting; or release all before requesting more |
| No Preemption | If process can't get resource, release what it holds; restart later |
| Circular Wait | Impose total ordering on resources; always request in increasing order |

### Deadlock Avoidance — Banker's Algorithm

Before granting a resource, simulate the allocation. If the resulting state is **safe** → grant. If **unsafe** → deny.

**Safe State:** There exists a **safe sequence** — an ordering of all processes such that each can complete using currently available resources + resources released by earlier processes in the sequence.

```
Resources: A=10, B=5, C=7  (total)

         Allocation    Max Need    Need = Max−Alloc    Available
         A  B  C      A  B  C     A  B  C
P0:      0  1  0      7  5  3     7  4  3             A=3, B=3, C=2
P1:      2  0  0      3  2  2     1  2  2
P2:      3  0  2      9  0  2     6  0  0
P3:      2  1  1      2  2  2     0  1  1
P4:      0  0  2      4  3  3     4  3  1

Safe sequence: P1 → P3 → P4 → P0 → P2

Step 1: P1 needs (1,2,2). Available (3,3,2) ≥ (1,2,2) ✓
        P1 finishes. Available = (3,3,2) + (2,0,0) = (5,3,2)
Step 2: P3 needs (0,1,1). Available (5,3,2) ≥ (0,1,1) ✓
        ... and so on until all finish.
→ Safe state. System is deadlock-free.
```

**Limitation:** Requires knowing maximum resource needs in advance — impractical for most real systems.

---

# PART 5 — MEMORY MANAGEMENT

---

## 15. Memory Layout of a Process

> **Interview Q:** *How is a process's memory organised? What is the difference between stack and heap?*

```
Virtual Address Space:

0xFFFFFFFF ┌──────────────────┐
           │   Kernel Space   │  ← OS code, inaccessible from user space
0xC0000000 ├──────────────────┤
           │   Stack          │  ← grows downward
           │   ↓              │    local vars, return addresses, function frames
           │                  │
           │   (free space)   │
           │                  │
           │   ↑              │
           │   Heap           │  ← grows upward
           │                  │    malloc / new, dynamic allocation
           ├──────────────────┤
           │   BSS Segment    │  ← uninitialised global/static (zeroed by OS)
           ├──────────────────┤
           │   Data Segment   │  ← initialised globals (int x = 5;)
           ├──────────────────┤
           │   Text / Code    │  ← program instructions (read-only)
0x00000000 └──────────────────┘
```

| Region | Contents | Size | Lifetime |
|---|---|---|---|
| **Text** | Machine code | Fixed | Program lifetime |
| **Data** | Initialised globals | Fixed | Program lifetime |
| **BSS** | Uninitialised globals | Fixed | Program lifetime |
| **Heap** | Dynamic allocations | Grows at runtime | Until explicitly freed |
| **Stack** | Function frames, locals | Grows at runtime | Until function returns |

- **Stack overflow** — stack grows too deep (deep recursion)
- **Heap overflow** — no more memory for malloc

---

## 16. Paging

> **Interview Q:** *What is paging? What is a page table? What is internal fragmentation?*

### The Problem Paging Solves

Without paging, a process must be loaded into **contiguous** physical memory. External fragmentation makes this impossible as the system runs longer — free memory exists but in scattered pieces.

### Paging Solution

**Paging** divides both physical memory and virtual address space into fixed-size blocks:
- **Page** — fixed-size block of **virtual** memory
- **Frame** — fixed-size block of **physical** memory
- Page size = Frame size (typically 4KB)

Pages of a process can be placed in **any free frames** — no need for contiguous physical memory.

```
Virtual Address  →  [ Page Number  |  Page Offset ]
                          ↓
                    Page Table lookup
                          ↓
Physical Address →  [ Frame Number |  Page Offset ]
```

### Page Table

The OS maintains a **page table per process** mapping page numbers to frame numbers.

```
Page Table (for Process P):
  Page 0 → Frame 3
  Page 1 → Frame 7
  Page 2 → Frame 2
  Page 3 → Frame 5

Virtual address = Page 1, Offset = 0x234
→ Physical address = Frame 7 × page_size + 0x234
```

### TLB — Translation Lookaside Buffer

Page table is in RAM → every memory access needs TWO RAM accesses (page table + actual data). Too slow!

**TLB** is a small, fast hardware cache (~64–1024 entries) storing recent page→frame translations.

```
Virtual Address
      ↓
   TLB lookup
   ↙          ↘
TLB hit       TLB miss
(~1 cycle)    → page table walk (~100s of cycles)
   ↓                ↓
Physical Address   Update TLB, retry
```

TLB hit rate is typically >99% due to locality of reference. On a process context switch, the TLB is flushed (different address space) — contributing to context switch cost.

### Fragmentation with Paging

| Type | Cause | Where it occurs |
|---|---|---|
| **Internal Fragmentation** | Last page of a process is not fully used — wasted space inside an allocated page | Paging |
| **External Fragmentation** | Free memory scattered; no single large chunk | Variable-size allocation (not paging) |

Paging **eliminates external fragmentation** but introduces **internal fragmentation** (up to page_size − 1 bytes wasted per process).

### Multi-Level Paging

For 64-bit systems, a single page table would be enormous. Solution: hierarchical page tables.

```
Virtual Address = [ L1 index | L2 index | L3 index | Offset ]
                        ↓
                  L1 Page Table (in RAM)
                        ↓
                  L2 Page Table (only allocated if L1 entry present)
                        ↓
                  Physical Frame
```

Linux uses 4-level paging (PGD → PUD → PMD → PTE).

---

## 17. Segmentation

> **Interview Q:** *What is segmentation? How does it differ from paging?*

**Segmentation** divides memory into variable-size **logical** segments (code, stack, heap, data). Each segment has a **base address** and a **limit**.

```
Segment Table:
  Segment 0 (code):  base=1400, limit=1000
  Segment 1 (data):  base=6300, limit=400
  Segment 2 (stack): base=4300, limit=1100

Virtual Address = [ Segment Number | Offset ]
If offset > limit → Segmentation Fault!
Physical Address = base + offset
```

### Paging vs Segmentation

| | Paging | Segmentation |
|---|---|---|
| Division size | Fixed (e.g., 4KB) | Variable (logical units) |
| Fragmentation | Internal only | External only |
| User visibility | Transparent | Visible (programmer-defined) |
| Protection | Per-page | Per-segment (different permissions) |
| Used in practice | Yes (Linux, Windows) | Mostly historical (x86 used both) |

---

## 18. Virtual Memory & Demand Paging

> **Interview Q:** *What is virtual memory? What is demand paging? What is a page fault?*

### Virtual Memory

**Virtual memory** allows a process to use more memory than physically available RAM by storing some pages on disk (swap space). Each process gets an illusion of a large, contiguous private address space.

```
RAM (physical)  =  8GB
Disk (swap)     =  16GB
Process sees    =  Large virtual address space

Pages actively used   → in RAM  (fast, ~100ns)
Pages not recently used → on disk (slow, ~10ms)
```

### Demand Paging

**Demand paging** loads pages into RAM **only when accessed** — not at process startup. This reduces startup time and memory usage.

- Pages start **not present** (valid bit = 0 in page table)
- When process accesses a page not in RAM → **page fault**

### Page Fault Handling

```
Process accesses a virtual address
         ↓
  MMU checks page table — valid bit = 0 (not in RAM)
         ↓
  PAGE FAULT exception → control transferred to OS
         ↓
  OS finds a free frame (or evicts one via page replacement algorithm)
         ↓
  OS reads the page from disk into the free frame
         ↓
  OS updates page table: frame number + valid bit = 1
         ↓
  Restart the faulting instruction
         ↓
  Process continues transparently
```

### Effective Access Time (EAT)

```
EAT = (1 − p) × memory_access_time  +  p × page_fault_time

memory_access_time ≈ 100ns
page_fault_time    ≈ 10,000,000ns  (10ms disk I/O)

If p = 0.001  (1 page fault per 1000 accesses):
EAT = 0.999 × 100  +  0.001 × 10,000,000
    ≈ 10,100ns  — 100× slower than zero page faults
```

---

## 19. Page Replacement Algorithms

> **Interview Q:** *Explain FIFO, LRU, and Optimal page replacement. What is Belady's anomaly?*

When a page fault occurs and RAM is full, the OS must **evict** a page to disk. Which one to evict?

### FIFO — First In First Out

Evict the page that has been in memory the **longest**.

Simple but poor performance. Suffers from **Belady's Anomaly**.

**Belady's Anomaly:** With FIFO, adding more frames can **increase** page faults. Counter-intuitive — unique to FIFO.

```
Reference string: 1 2 3 4 1 2 5 1 2 3 4 5
  3 frames: 9 page faults
  4 frames: 10 page faults  ← MORE faults with more memory!
```

---

### Optimal (OPT)

Evict the page that **will not be used for the longest time in the future**. Gives the minimum possible page faults.

**Problem:** Requires future knowledge — impossible to implement in practice. Used only as a **theoretical benchmark**.

---

### LRU — Least Recently Used

Evict the page that was used **least recently** (farthest in the past). Best practical algorithm — approximates Optimal.

```
Reference string: 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1
  3 frames:
    FIFO:    15 page faults
    LRU:     12 page faults
    Optimal: ~9 page faults
```

**Implementation options:**

| Method | Idea | Complexity |
|---|---|---|
| Counter | Each page has a timestamp of last use; evict min | O(n) to find min |
| Stack | Move page to top on access; bottom = LRU | O(n) find |
| Doubly linked list + hash map | O(1) for both access and eviction | O(1) — used in practice |

> The doubly linked list + hash map implementation is the classic LRU Cache problem on LeetCode.

---

### Clock Algorithm (Second Chance)

LRU approximation used in real OS (Linux). Pages arranged in a circular list with a **reference bit**.

On page fault: if reference bit = 1 → clear it (give second chance) and advance. If bit = 0 → evict this page. O(1) amortised.

---

### Page Replacement Summary

| Algorithm | Page Faults | Implementation | Notes |
|---|---|---|---|
| FIFO | High | Easy (queue) | Belady's anomaly |
| Optimal | Lowest | Impossible (needs future) | Theoretical benchmark |
| LRU | Low | Moderate (list + hash map) | Best practical |
| Clock | Close to LRU | Simple | Used in Linux |

---

## 20. Thrashing

> **Interview Q:** *What is thrashing? How do you prevent it?*

**Thrashing** occurs when a process (or the OS) spends more time **swapping pages in and out** than actually executing useful work.

```
Cause:
  Too many processes → each gets too few frames
       ↓
  High page fault rate → CPU spends time doing disk I/O
       ↓
  CPU utilisation drops → OS admits MORE processes (thinking CPU is idle)
       ↓
  Even more page faults → system near halt
```

```
CPU Utilisation
    │   ↗ (more processes help up to a point)
    │         ↘ (thrashing begins here)
    └──────────────────────────────→ Degree of Multiprogramming
```

### Prevention

1. **Working Set Model** — track the set of pages used in the last Δ time units. Only admit a new process if enough frames exist for its working set.
2. **Page Fault Frequency (PFF)** — if PFF > upper threshold → give more frames. If PFF < lower threshold → take frames away.
3. **Reduce degree of multiprogramming** — suspend some processes, free their frames.
4. **Add more RAM** — simplest real-world fix.

---

# PART 6 — STORAGE & FILE SYSTEMS

---

## 21. File Systems

> **Interview Q:** *What is a file system? What are the methods of file allocation?*

A **file system** is the OS component that manages how data is stored and retrieved on storage devices. It provides abstraction over raw disk blocks.

### Key Components

| Component | Role |
|---|---|
| **Inode** | Metadata about a file (size, permissions, owner, block pointers) — NOT the filename |
| **Directory** | Maps filenames to inode numbers |
| **Superblock** | Metadata about the file system itself (total size, free block count, inode count) |
| **Block** | Fixed-size unit of disk storage (typically 4KB) |

```
Accessing a file by name:

filename
    ↓
Directory lookup → inode number
    ↓
Inode → block pointers
    ↓
Disk blocks → actual file data
```

### File Allocation Methods

**1. Contiguous Allocation**
- File occupies consecutive disk blocks.
- Fast sequential read. Simple.
- External fragmentation. File size must be known in advance.

**2. Linked Allocation**
- Each block contains a pointer to the next block.
- No fragmentation. File can grow dynamically.
- No random access (must traverse chain). Pointer corruption loses rest of file.
- **FAT (File Allocation Table)** stores all pointers in a table in memory → solves random access problem.

**3. Indexed Allocation**
- An index block contains all block pointers for a file.
- Random access in O(1). No external fragmentation.
- Index block overhead. Large files need multi-level indexes.
- **Unix/Linux inodes use this:**

```
Inode:
  direct[0..11]      → 12 data blocks directly
  single_indirect    → block of pointers → 1024 blocks
  double_indirect    → 1024 × 1024 blocks
  triple_indirect    → 1024 × 1024 × 1024 blocks
```

### File Permissions (Unix)

```
-rwxr-xr--  1 user group 4096 Jun 11 file.py
│└──┘└──┘└──┘
│  │   │   └── Others: r--  (read only)
│  │   └────── Group:  r-x  (read + execute)
│  └────────── Owner:  rwx  (read + write + execute)
└───────────── File type: - (regular), d (directory), l (symlink)
```

---

## 22. Disk Scheduling

> **Interview Q:** *What is disk scheduling? Explain SSTF and SCAN.*

**Disk scheduling** determines the order in which disk I/O requests are serviced to minimise **seek time** (time for the disk head to move to the correct track).

### Algorithms

| Algorithm | Strategy | Pro | Con |
|---|---|---|---|
| **FCFS** | Service in arrival order | Simple, fair | High seek time |
| **SSTF** | Service closest request to current head | Low seek time | Starvation of distant requests |
| **SCAN (Elevator)** | Head sweeps in one direction, reverses at end | Fair, good throughput | Slight wait variance |
| **C-SCAN** | Like SCAN but only services in ONE direction; jumps back to start | Uniform wait times | Slightly more movement |
| **C-LOOK** | Like C-SCAN but only goes as far as last request | Most efficient | — |

```
Requests: 98, 183, 37, 122, 14, 124, 65, 67
Head starts at: 53, moving toward higher values

FCFS:  53→98→183→37→122→14→124→65→67   total movement = 640
SSTF:  53→65→67→37→14→98→122→124→183   total movement = 236
SCAN:  53→65→67→98→122→124→183→37→14   total movement = 208
```

> C-LOOK is the most practical and is used in Linux I/O schedulers.

---

# PART 7 — INTERVIEW Q&A

---

## 23. Interview Q&A — Foundations & Processes

**Q: What is the difference between a program and a process?**
> A **program** is a static set of instructions stored on disk (passive). A **process** is a program in execution — dynamic, with its own memory space, CPU registers, open files, and state (active). One program can give rise to multiple processes (e.g., opening two terminal windows both run `bash` — two processes, one program).

**Q: What is a zombie process? How do you prevent it?**
> A zombie process is a process that has finished execution but its PCB still exists because the parent hasn't called `wait()` to collect its exit status. It occupies a PID slot and PCB entry but no other resources. Prevention: always call `wait()` or `waitpid()` in the parent, or set up a `SIGCHLD` signal handler that calls `wait()` asynchronously.

**Q: What is the difference between `fork()` and `exec()`?**
> `fork()` creates a **copy** of the current process — both parent and child continue from the same point, each with their own address space (copy-on-write). `exec()` **replaces** the current process's memory image with a new program — the PID stays the same but code, data, stack are replaced. Typically used together: `fork()` to create a child, then `exec()` in the child to run a different program.

**Q: What is copy-on-write (COW) in `fork()`?**
> After `fork()`, instead of immediately copying the parent's entire address space, the OS marks all pages as shared and read-only. When either process tries to **write** to a page, a protection fault occurs and the OS copies just that one page, giving each process its own independent copy. Result: `fork()` is very fast, especially when the child immediately calls `exec()` (no pages need to be copied). Python's `multiprocessing` relies on this.

**Q: What is a thread pool and why is it used?**
> A thread pool is a collection of pre-created threads waiting for tasks. Instead of creating and destroying a thread per task (expensive), tasks are submitted to a queue and idle threads pick them up. Benefits: eliminates thread creation/destruction overhead; limits total thread count (prevents system overload); tasks are queued if all threads are busy. Used in web servers, Python's `concurrent.futures.ThreadPoolExecutor`, Java's `ExecutorService`.

**Q: What is the difference between preemptive and non-preemptive scheduling?**
> In **non-preemptive** scheduling, once a process is given the CPU it runs until it voluntarily releases it (by finishing or blocking on I/O). In **preemptive** scheduling, the OS can forcibly take the CPU away — triggered by a timer interrupt or a higher-priority process becoming ready. Preemptive is better for responsiveness and fairness but has higher context-switch overhead.

---

## 24. Interview Q&A — Scheduling & Synchronisation

**Q: What is starvation? How is it different from deadlock? How is it solved?**
> **Deadlock:** A set of processes are ALL permanently stuck, each waiting for a resource held by another in the set — no one can proceed. **Starvation:** A process CAN proceed in theory but NEVER gets the CPU because higher-priority processes keep arriving. Deadlock requires a circular wait; starvation does not. Starvation solution: **aging** — gradually increase the priority of a waiting process so it eventually gets scheduled.

**Q: What is the difference between a mutex and a semaphore?**
> A **mutex** enforces ownership — only the thread that locked it can unlock it. Used for mutual exclusion over a single shared resource. A **semaphore** has no ownership — any thread can call signal. Can have a count > 1 (counting semaphore) to allow N concurrent accesses to a pool of resources. Use mutex to protect a shared resource; use semaphore for signalling between threads or managing a resource pool.

**Q: What is a deadlock? State the four conditions.**
> Deadlock is when processes permanently block waiting for resources held by each other. All four Coffman conditions must hold simultaneously: **(1) Mutual Exclusion** — resources are non-shareable; **(2) Hold and Wait** — process holds one resource while waiting for another; **(3) No Preemption** — resources can't be forcibly taken; **(4) Circular Wait** — circular chain of processes each waiting for the next.

**Q: What is the Banker's Algorithm?**
> The Banker's Algorithm is a deadlock **avoidance** algorithm. Before granting a resource request, it simulates the allocation and checks if the resulting state is **safe** (a safe sequence of process completions exists using available resources). If safe → grant the request. If unsafe → deny and make the process wait. Named after a bank that never lets deposits drop so low it can't satisfy all customers. **Limitation:** Requires knowing each process's maximum resource needs in advance — impractical for many real systems.

**Q: What is a race condition? Give an example.**
> A race condition is when the correctness of a program depends on the non-deterministic interleaving of multiple threads. Classic example: two threads both do `counter++`. This is NOT atomic — it expands to LOAD, ADD, STORE. If they interleave (T1 loads 5, T2 loads 5, T1 stores 6, T2 stores 6), the counter becomes 6 instead of 7 — one increment is lost. Fix: protect the critical section with a mutex.

**Q: What is the critical section problem and what are its three requirements?**
> The critical section problem is ensuring only one process executes the code that accesses shared resources at a time. A correct solution must satisfy all three: **(1) Mutual exclusion** — only one process in the critical section at a time; **(2) Progress** — if no process is in the CS and some want to enter, selection cannot be postponed indefinitely; **(3) Bounded waiting** — every process that wants to enter must eventually be allowed in.

---

## 25. Interview Q&A — Memory Management

**Q: What is virtual memory? What are its benefits?**
> Virtual memory gives each process the illusion of a large, private address space larger than physical RAM. The OS and MMU transparently map virtual addresses to physical frames; pages not in RAM are stored on disk (swap). Benefits: (1) Programs larger than RAM can run; (2) Processes are isolated — one can't corrupt another's memory; (3) More processes can run simultaneously; (4) Every process sees a uniform address layout, simplifying programming.

**Q: What is a page fault? Is every page fault bad?**
> A page fault occurs when a process accesses a virtual page not currently in physical RAM. Not all page faults are bad. A **minor page fault** (page is in memory but not yet mapped — e.g., a shared library page) is handled without disk I/O — fast. A **major page fault** requires reading from disk (~10ms) — expensive. Programs with good spatial/temporal locality have few major page faults.

**Q: What is the difference between paging and segmentation?**
> **Paging** divides memory into fixed-size pages (e.g., 4KB) — eliminates external fragmentation but introduces internal fragmentation. Transparent to the programmer. **Segmentation** divides memory into variable-size logical segments (code, stack, heap) — eliminates internal fragmentation but introduces external fragmentation. Segments correspond to programmer-visible logical units. Modern OS (Linux, Windows) use **paging** primarily; x86 hardware supports both but Linux uses segmentation only for privilege separation.

**Q: What is the TLB and why is it important?**
> The Translation Lookaside Buffer is a small, fast hardware cache (part of the MMU) storing recent virtual→physical page translations. Without TLB, every memory access needs two RAM accesses (page table lookup + actual data). With TLB, most translations cost just ~1 cycle (>99% hit rate due to locality). TLB miss triggers a page table walk (~50–200 cycles). On a process context switch, TLB is flushed — one key reason context switches are expensive.

**Q: What is thrashing and how do you prevent it?**
> Thrashing is when processes spend more time handling page faults than executing useful work. Happens when too many processes compete for too few frames — each process's working set doesn't fit in its allocated frames, causing continuous page fault storms. Prevention: (1) Working set model — only admit a process if its working set fits in available frames; (2) Page Fault Frequency control — monitor PFF and adjust allocation dynamically; (3) Reduce multiprogramming — suspend some processes.

**Q: What is Belady's Anomaly?**
> Belady's Anomaly is the counter-intuitive observation that with the **FIFO** page replacement algorithm, increasing the number of available frames can actually **increase** the number of page faults. This is specific to FIFO. Stack-based algorithms like LRU and Optimal are **monotone** — more frames never means more page faults.

---

## 26. Interview Q&A — Advanced & Tricky

**Q: How does the OS protect one process from reading another's memory?**
> Each process has its own **page table** managed by the OS. The hardware MMU uses the current process's page table for ALL address translations — a process can only access virtual addresses mapped in its own page table. Accessing an unmapped address triggers a **segmentation fault** (hardware protection fault), and the OS kills the process. Page tables themselves live in protected kernel space — user processes cannot modify them directly.

**Q: What is the difference between internal and external fragmentation?**
> **Internal fragmentation:** Allocated memory block is larger than requested — wasted space inside an allocated region. Caused by fixed-size allocation units (pages). Example: a file needing 4097 bytes gets two 4096-byte pages — 4095 bytes wasted. **External fragmentation:** Enough total free memory exists but it is scattered in small non-contiguous pieces — cannot satisfy a large contiguous request. Caused by variable-size allocation. Paging eliminates external fragmentation; segmentation eliminates internal fragmentation.

**Q: What is the difference between a hard link and a soft link (symlink)?**
> A **hard link** is a directory entry pointing to the same inode as the original file. Both names refer to the exact same data. Deleting the original doesn't delete the data (inode link count decrements; data deleted only when count reaches 0). Cannot cross file systems or link to directories. A **symlink** is a special file containing a path string. If the original is deleted, the symlink breaks (dangling). Can cross file systems and link to directories.

**Q: What happens step-by-step when you type `ls -l` in a terminal?**
> (1) Shell calls `fork()` to create a child process. (2) Child calls `exec("ls", ["-l"])` — replaces its memory with the `ls` program. (3) `ls` calls `opendir()` and `readdir()` (system calls) to read directory entries from the kernel. (4) For each file, `ls` calls `stat()` (system call) to get inode metadata (permissions, size, timestamps). (5) `ls` calls `write()` to output to stdout. (6) `ls` calls `exit()`. Kernel reclaims its resources. (7) Parent shell calls `wait()`, gets exit status, shows prompt again.

**Q: What is a spinlock? When is it better than a mutex?**
> A spinlock is a lock where the waiting thread **busy-waits** (loops checking the lock) rather than sleeping. Better than a mutex when: (1) lock hold time is very short (less than the cost of a context switch ~µs); (2) you're in interrupt context (cannot sleep); (3) on a multiprocessor where the holder may be running on another CPU simultaneously. Worse for long hold times — wastes CPU cycles. Used in OS kernel interrupt handlers.

**Q: What is demand paging vs pre-paging?**
> **Demand paging** loads a page only when it is first accessed (on page fault). Reduces startup time and memory usage but has higher initial fault rate. **Pre-paging** loads pages before they are needed — predicting future accesses based on locality. Reduces page fault rate at the cost of possibly loading unused pages. Modern OS use a hybrid: demand paging with read-ahead (pre-fetching nearby disk blocks after a fault).

**Q: What is the difference between logical and physical address?**
> A **logical (virtual) address** is the address generated by the CPU — what the program sees. Every process starts at logical address 0. A **physical address** is the actual address in RAM. The MMU (Memory Management Unit) translates logical → physical using the page table at runtime. Virtual memory works because every process has its own logical address space, but they all map to different physical frames.

---

## 27. Quick Reference Cheat Sheet

```
PROCESS vs THREAD
  Process : own address space | heavy creation (~ms) | isolated
  Thread  : shared address space | light creation (~µs) | shared heap
  Context switch cost: Process > Thread (TLB flush + cache cold start)

PROCESS STATES
  New → Ready → Running → Waiting → Terminated

PCB CONTENTS
  PID | state | program counter | registers | memory maps | open files

SCHEDULING ALGORITHMS
  FCFS    : non-preemptive | simple | convoy effect
  SJF     : non-preemptive | optimal avg wait | needs burst time | starvation
  SRTF    : preemptive SJF | optimal with arrivals
  RR      : preemptive | fair | time quantum 10-100ms
  Priority: preemptive/non | starvation → fix with aging
  MLFQ    : best general purpose | used in Linux/Windows

KEY FORMULAE
  Turnaround = Completion − Arrival
  Waiting    = Turnaround − Burst
  Response   = First CPU − Arrival
  EAT        = (1−p) × mem_access + p × page_fault_time

DEADLOCK — ALL 4 must hold simultaneously
  Mutual Exclusion | Hold & Wait | No Preemption | Circular Wait
  Prevention : break one condition
  Avoidance  : Banker's Algorithm (safe state simulation)
  Detection  : RAG cycle detection / matrix algorithm
  Ignorance  : Ostrich algorithm (most real OS)

SYNCHRONISATION
  Race condition   : outcome depends on thread interleaving order
  Critical section : requires mutual exclusion + progress + bounded wait
  Mutex    : ownership | binary | only locker can unlock
  Semaphore: no ownership | counting (0 to N) | P() wait / V() signal
  Monitor  : mutex + condition variables (Python threading.Condition)
  Spinlock : busy-wait | good for very short critical sections on SMP

MEMORY
  Virtual addr = Page Number + Offset
  Page table maps virtual pages → physical frames
  TLB     : hardware cache of recent translations | >99% hit rate
  Page fault : page not in RAM → OS loads from disk (major = slow ~10ms)
  Internal fragmentation : paging (last page not fully used)
  External fragmentation : variable-size allocation (not paging)

PAGE REPLACEMENT
  FIFO    : evict oldest | simple | Belady's anomaly
  Optimal : evict used farthest in future | impossible in practice | benchmark
  LRU     : evict least recently used | best practical | O(1) with list+hashmap
  Clock   : LRU approximation | O(1) | used in Linux

THRASHING
  Too many page faults → CPU doing more I/O than work
  Fix: working set model | PFF control | reduce multiprogramming | add RAM

FILE ALLOCATION
  Contiguous : fast sequential | external fragmentation | fixed size
  Linked     : flexible growth | no random access | pointer overhead
  Indexed    : O(1) random access | inode model used in Unix/Linux

IPC SPEED (fast → slow)
  Shared Memory > Pipe > Message Queue > Socket

LINUX SIGNALS
  SIGINT(2)   : Ctrl+C               SIGKILL(9)  : force kill (uncatchable)
  SIGTERM(15) : polite kill (catchable)   SIGSEGV(11) : segfault
  SIGCHLD(17) : child terminated     SIGSTOP(19) : pause (uncatchable)

KERNEL TYPES
  Monolithic : fast, large (Linux)
  Microkernel: stable, slow IPC (Minix, QNX)
  Hybrid     : Windows NT, macOS XNU
```

---

*Sources: InterviewBit OS Interview Questions (2026), GeeksforGeeks OS Top 100 (2025),
igmGuru OS Interview Guide (2026), Silberschatz "Operating System Concepts" 10th ed.,
OSTEP — Operating Systems: Three Easy Pieces (free online at ostep.org).*
