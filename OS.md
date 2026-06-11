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

An **Operating System** is system software that acts as an
**intermediary between hardware and user programs**. It manages
hardware resources and provides services to applications so
programmers don't have to deal with raw hardware.

User Applications  (Chrome, VS Code, Python script)
↓
Operating System  (Windows, Linux, macOS)
↓
Hardware  (CPU, RAM, Disk, GPU, Network)

### Five Core Functions

| Function | What the OS does |
|---|---|
| **Process Management** | Create, schedule, terminate processes; allocate CPU |
| **Memory Management** | Allocate/deallocate RAM; virtual memory; paging |
| **File System Management** | Files, directories, permissions, storage abstraction |
| **Device Management** | Drivers, I/O scheduling, hardware abstraction |
| **Security & Protection** | Access control, user isolation, authentication |

### Analogy
> The OS is like a **government**. Citizens (programs) don't
> directly access land (hardware). The government (OS) manages
> land allocation (memory), assigns jobs (CPU scheduling),
> maintains roads (I/O), and enforces laws (security).

---

## 2. Kernel & System Calls

> **Interview Q:** *What is a kernel? What is a system call?
> What is the difference between user mode and kernel mode?*

### Kernel

The **kernel** is the core of the OS — the part that runs with
full hardware privileges. It manages CPU, memory, and devices
directly. Everything else (shell, GUI, apps) runs on top of it.

┌─────────────────────────────────┐
│       User Space                │  ← low privilege
│  (shell, apps, libraries)       │
├─────────────────────────────────┤
│       Kernel Space              │  ← full hardware access
│  (scheduler, memory mgr, VFS)  │
├─────────────────────────────────┤
│       Hardware                  │
└─────────────────────────────────┘

### User Mode vs Kernel Mode

| | User Mode | Kernel Mode |
|---|---|---|
| Privilege | Restricted | Full (ring 0) |
| Hardware access | No — goes through kernel | Direct |
| Crash impact | Only that process crashes | Entire system can crash |
| Examples | Your Python script, Chrome | OS scheduler, device drivers |

### System Calls

A **system call** is the mechanism a user-space program uses to
request a service from the kernel — the only legal way to cross
the user↔kernel boundary.

User program calls: read(fd, buf, n)
↓
C library (glibc) wraps it → software interrupt / syscall instruction
↓
CPU switches to kernel mode
↓
Kernel handles request (reads from disk/buffer)
↓
CPU switches back to user mode
↓
Return value back to program

**Common system calls:**

| Category | Examples |
|---|---|
| Process | `fork()`, `exec()`, `exit()`, `wait()` |
| File | `open()`, `read()`, `write()`, `close()` |
| Memory | `mmap()`, `brk()` |
| Device | `ioctl()` |
| Network | `socket()`, `connect()`, `send()`, `recv()` |
| IPC | `pipe()`, `shmget()`, `msgget()` |

> **System call vs function call:**
> A regular function call stays in user mode — just a jump to
> another address. A system call crosses into kernel mode via a
> software interrupt — has significant overhead (~100s of ns).
> This is why we batch I/O operations instead of calling
> `write()` one byte at a time.

### Kernel Types

| Type | Design | Examples | Pro/Con |
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
| **Embedded** | Minimal OS for dedicated hardware | Android (phones), RTOS (microcontrollers) |
| **Network OS** | Manages network resources | Novell NetWare |

**Hard vs Soft Real-Time:**
- **Hard RTOS:** Missing a deadline = system failure (airbag
  controller, pacemaker)
- **Soft RTOS:** Missing a deadline = degraded quality (video
  streaming, online gaming)

---

# PART 2 — PROCESSES & THREADS

---

## 4. Processes

> **Interview Q:** *What is a process? What is a PCB?
> Explain process states.*

### What is a Process?

A **process** is a **program in execution** — an active instance
of a program. A program is just code sitting on disk; a process
is that code loaded into memory with its own resources.

Program (disk)  +  Execution context (CPU state, memory, files) =  Process

### Memory Layout of a Process
High address ┌──────────────┐
│   Stack      │ ← grows downward (function calls, local vars)
  │      ↓       │
│   (gap)      │
│      ↑       │
│   Heap       │ ← grows upward (malloc/new)
│──────────────│
│   BSS        │ ← uninitialised global/static vars
│──────────────│
        │   Data       │ ← initialised global/static vars
│──────────────│
│   Text/Code  │ ← program instructions (read-only)
Low address  └──────────────┘

### Process Control Block (PCB)

The PCB is a **data structure maintained by the OS** for every
process — the OS's complete record of a process.

PCB contains:
├── Process ID (PID)
├── Process State (running, ready, waiting...)
├── Program Counter (address of next instruction)
├── CPU Registers (all register values)
├── CPU Scheduling info (priority, queue pointers)
├── Memory Management info (page tables, segment tables)
├── I/O Status info (open files, devices allocated)
└── Accounting info (CPU time used, time limits)

> **Analogy:** PCB is like a **bookmark + sticky note** for a
> process. When the OS pauses a process (context switch), it
> saves everything to the PCB. When the process resumes, it
> restores from the PCB — as if it never stopped.

### Process States (5-State Model)
        fork()
NEW ──────────────────→ READY
                           │  ↑
       scheduler    │  │  preemption /
              dispatches   │  │  time slice ends
↓  │
RUNNING ──────→ TERMINATED
       │               (exit())
       I/O or       │
event wait   │
   ↓
WAITING/BLOCKED
    │
I/O done /   │
           event occurs │
              ↓
  READY

  | State | Meaning |
|---|---|
| **New** | Process being created |
| **Ready** | Waiting for CPU; in ready queue |
| **Running** | Currently executing on CPU |
| **Waiting/Blocked** | Waiting for I/O, event, or resource |
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

- **`fork()`** — duplicates the calling process (parent & child
  run from same point). Child gets a copy of parent's address
  space (copy-on-write in modern OSes).
- **`exec()`** — replaces the current process's memory with a
  new program. PID stays the same.
- **Zombie process** — child finishes but parent hasn't called
  `wait()` yet. PCB still exists.
- **Orphan process** — parent dies before child. Adopted by
  `init` (PID 1).

---

## 5. Threads

> **Interview Q:** *What is a thread? What is the difference
> between user-level and kernel-level threads?*

### What is a Thread?

A **thread** is the smallest unit of CPU execution. It is a
**lightweight process** — a sequence of instructions within a
process that can be scheduled independently.

A process starts with one thread (main thread) and can create
more. All threads in a process share:
- Code segment
- Data segment (global variables)
- Heap
- Open file descriptors

But each thread has its own:
- Stack (local variables, function call frames)
- Program Counter
- CPU Registers
- Thread ID

Process
├── Shared: code, heap, data, files
├── Thread 1: stack, PC, registers
├── Thread 2: stack, PC, registers
└── Thread 3: stack, PC, registers

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
| Examples | POSIX green threads (old), Java green threads | Linux pthreads, Windows threads |

### Threading Models

| Model | Description |
|---|---|
| **Many-to-One** | Many ULTs → 1 KLT. No parallelism. One blocks = all block |
| **One-to-One** | 1 ULT → 1 KLT. True parallelism. Linux/Windows default |
| **Many-to-Many** | Many ULTs → Many KLTs. Best flexibility. Complex to implement |

### Multithreading in Python vs C++

```python
# Python — GIL limits true parallelism for CPU-bound tasks
import threading
def task(): ...
t = threading.Thread(target=task)
t.start(); t.join()
# Use multiprocessing for CPU-bound, threading for I/O-bound
```

```cpp
// C++ — true parallelism, no GIL
#include <thread>
void task() { ... }
std::thread t(task);
t.join();   // wait for thread to finish
```

---

## 6. Process vs Thread

> **Interview Q:** *What is the difference between a process
> and a thread? Which is faster to create?*

| Dimension | Process | Thread |
|---|---|---|
| Definition | Independent program in execution | Execution unit within a process |
| Memory | Own address space | Shares process address space |
| Creation cost | Heavy (~ms, allocate memory + PCB) | Light (~µs, just stack + TCB) |
| Communication | IPC (pipes, sockets, shared memory) | Direct (shared heap/globals) |
| Context switch | Expensive (full address space switch) | Cheaper (same address space) |
| Crash isolation | One process crash doesn't affect others | One thread crash can kill the process |
| Synchronisation | Rarely needed (isolated memory) | Always needed (shared memory) |
| Example use | Chrome tab (isolated for safety) | Chrome renderer threads in one tab |

> **Real-world analogy:**
> Processes are like separate **houses** — each has its own
> rooms (memory). Threads are like **people in the same house**
> — they share the kitchen (heap) and living room (globals) but
> have their own bedrooms (stack).

---

## 7. Context Switching

> **Interview Q:** *What is a context switch? What does it
> involve? Why is it expensive?*

A **context switch** is the OS saving the state of the currently
running process/thread and restoring the state of the next one
to run on the CPU.

### Steps in a Context Switch
Running: Process A
↓

Timer interrupt / voluntary yield / I/O wait
↓
OS saves Process A's context into A's PCB:

Program Counter
All CPU registers (general, floating point, SIMD)
Stack pointer
↓

OS runs scheduler → selects Process B
↓
OS restores Process B's context from B's PCB
↓
Running: Process B

### Why Context Switches Are Expensive

- **Direct cost:** Save/restore registers (dozens of values)
- **TLB flush:** Translation Lookaside Buffer must be flushed
  on process switch (not thread switch — same address space)
- **Cache cold:** CPU cache filled with A's data; now needs
  B's data → cache misses spike until warmed up
- **Typical cost:** Process context switch ~1–10µs;
  Thread context switch ~100ns–1µs

> **Thread context switch is faster** than process context
> switch because threads share the same address space — the
> TLB doesn't need flushing and the memory mappings don't
> change.

---

## 8. Inter-Process Communication

> **Interview Q:** *What are the methods of IPC?
> Compare shared memory and message passing.*

Since processes have isolated address spaces, they need explicit
mechanisms to communicate.

### IPC Methods

| Method | How | Speed | Use case |
|---|---|---|---|
| **Pipe** | Unidirectional byte stream; parent↔child | Fast | Shell pipes (`ls \| grep`) |
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
| Processes | Usually same machine | Same machine or across network |
| Example | `mmap()`, `shmget()` | `pipe()`, `send()`/`recv()` |

### Common Signals (Linux)

| Signal | Value | Meaning |
|---|---|---|
| `SIGINT` | 2 | Ctrl+C — interrupt |
| `SIGKILL` | 9 | Force kill — cannot be caught or ignored |
| `SIGTERM` | 15 | Polite termination request — can be caught |
| `SIGSEGV` | 11 | Segmentation fault (invalid memory access) |
| `SIGCHLD` | 17 | Child process stopped or terminated |
| `SIGSTOP` | 19 | Pause process — cannot be caught |

---

# PART 3 — CPU SCHEDULING

---

## 9. Scheduling Concepts & Criteria

> **Interview Q:** *What is CPU scheduling? What are the
> criteria used to evaluate scheduling algorithms?*

**CPU Scheduling** is the OS deciding which process in the
ready queue gets the CPU next. Essential for multiprogramming
— keeping the CPU busy while other processes wait for I/O.

### Scheduling Queues
New → Job Queue (all processes)
↓
Ready Queue (in RAM, ready to run) → CPU
↓  ↑ (after I/O done)
Wait Queue (blocked on I/O / event)
↓
I/O Device Queue (waiting for specific device)
### Types of Schedulers

| Scheduler | Role | Frequency |
|---|---|---|
| **Long-term (Job scheduler)** | Admits processes from disk to RAM | Infrequent (seconds–minutes) |
| **Short-term (CPU scheduler)** | Picks next process from ready queue | Very frequent (~100ms) |
| **Medium-term** | Swaps processes in/out of RAM (swapping) | Moderate |

### Scheduling Criteria

| Metric | Definition | Goal |
|---|---|---|
| **CPU Utilisation** | % of time CPU is busy | Maximise (target: 40–90%) |
| **Throughput** | Processes completed per unit time | Maximise |
| **Turnaround Time** | Total time from submission to completion | Minimise |
| **Waiting Time** | Time spent in ready queue | Minimise |
| **Response Time** | Time from request to first response | Minimise (interactive systems) |

Turnaround Time = Completion Time − Arrival Time
Waiting Time    = Turnaround Time − Burst Time
Response Time   = Time of first CPU − Arrival Time

### Preemptive vs Non-Preemptive

| | Non-Preemptive | Preemptive |
|---|---|---|
| Once CPU given | Process runs until it finishes or blocks | OS can interrupt and take CPU back |
| Fairness | Poor (long jobs starve others) | Better |
| Overhead | Low | Higher (more context switches) |
| Examples | FCFS, non-preemptive SJF | Round Robin, SRTF, Priority (preemptive) |

---

## 10. Scheduling Algorithms

> **Interview Q:** *Explain FCFS, SJF, Round Robin, and
> Priority scheduling. What are their trade-offs?*

### FCFS — First Come First Served

- Non-preemptive. Process that arrives first runs first.
- Implemented with a FIFO queue.
Process:  P1(bt=24)  P2(bt=3)  P3(bt=3)   (all arrive at t=0)
Gantt:  |---P1(0-24)---|P2(24-27)|P3(27-30)|
Waiting: P1=0, P2=24, P3=27
Avg waiting = (0+24+27)/3 = 17ms

**Problem: Convoy Effect** — short processes stuck behind one
long process. Like a semi-truck ahead of you at a drive-through.

### SJF — Shortest Job First

- Non-preemptive. Process with the smallest burst time runs next.
- **Optimal** — gives minimum average waiting time.
- **Problem:** Requires knowing burst time in advance (often
  unknown). Can cause **starvation** of long processes.

Process:  P1(bt=6) P2(bt=8) P3(bt=7) P4(bt=3) (all arrive t=0)
Gantt:  |P4(0-3)|P1(3-9)|P3(9-16)|P2(16-24)|
Avg waiting = (3+16+9+0)/4 = 7ms   (vs FCFS = 10.25ms)

### SRTF — Shortest Remaining Time First

- Preemptive version of SJF. If a new process arrives with a
  shorter remaining time than current, preempt current.
- **Optimal** for minimising average waiting time with arrivals.
- **Problem:** High context switch overhead; starvation.

### Round Robin (RR)

- Preemptive. Each process gets a fixed **time quantum (q)**.
  If not done, preempted and placed at the back of the queue.
- **Designed for time-sharing systems.**
  Time Quantum = 4ms
Processes: P1(bt=24), P2(bt=3), P3(bt=3)
Gantt: |P1(0-4)|P2(4-7)|P3(7-10)|P1(10-14)|P1(14-18)|P1(18-22)|P1(22-26)|P1(26-30)|
Avg waiting = (6+4+7)/3 = 5.66ms

**Choosing time quantum:**
- Too small → too many context switches → overhead dominates
- Too large → degenerates to FCFS
- Rule of thumb: 80% of CPU bursts should be shorter than q
  (typically 10–100ms)

### Priority Scheduling

- Each process has a priority. Highest priority runs first.
- Can be preemptive or non-preemptive.
- **Problem: Starvation** — low-priority processes may never run.
- **Solution: Aging** — gradually increase priority of waiting
  processes over time.

  Process: P1(priority=3) P2(priority=1) P3(priority=4) P4(priority=5) P5(priority=2)
(lower number = higher priority)
Order: P2 → P5 → P1 → P3 → P4
### Multilevel Queue Scheduling

- Ready queue split into multiple queues (e.g., foreground/
  background) each with its own algorithm.
- Processes permanently assigned to a queue.

┌──────────────────────┐  ← Foreground (interactive): Round Robin
│  System processes    │
│  Interactive procs   │
├──────────────────────┤  ← Background (batch): FCFS
│  Batch processes     │
│  Student processes   │
└──────────────────────┘
### Multilevel Feedback Queue (MLFQ)

- Like multilevel queue but processes can **move between queues**.
- New processes start at highest priority. If they use full
  quantum → move down (CPU-bound). If they do I/O → move up.
- **Most flexible and widely used** (used in Linux CFS, Windows).

  
Queue 1 (q=8ms, highest priority)   — new / I/O-bound
Queue 2 (q=16ms)
Queue 3 (FCFS, lowest priority)     — CPU-bound long jobs
### Summary Table

| Algorithm | Preemptive | Starvation | Avg Wait | Overhead | Best for |
|---|---|---|---|---|---|
| FCFS | No | No | High | Low | Batch, simple |
| SJF | No | Yes | Optimal | Low | Known burst times |
| SRTF | Yes | Yes | Optimal | High | Known burst times |
| Round Robin | Yes | No | Medium | Medium | Time-sharing |
| Priority | Both | Yes | Low (high prio) | Medium | Real-time |
| MLFQ | Yes | No (aging) | Good | High | General purpose |

---

# PART 4 — SYNCHRONISATION & DEADLOCK

---

## 11. Race Conditions & Critical Section

> **Interview Q:** *What is a race condition? What is the
> critical section problem?*

### Race Condition

A **race condition** occurs when multiple threads/processes
access shared data concurrently and the outcome depends on
the **order of execution** — which is non-deterministic.

```c
// Global: int counter = 0;
// Thread 1 and Thread 2 both run this simultaneously:
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
// T2: ADD → 6, STORE → counter = 6   ← should be 7!
```

### Critical Section

A **critical section** is a segment of code that accesses
shared resources (shared variables, files, hardware) and must
not be executed by more than one process/thread simultaneously.

### Requirements for a Solution

Any valid solution to the critical section problem must satisfy
**all three**:

| Requirement | Meaning |
|---|---|
| **Mutual Exclusion** | Only ONE process in critical section at a time |
| **Progress** | If no process is in CS and some want to enter, selection can't be postponed indefinitely |
| **Bounded Waiting** | A process that wants to enter CS must eventually be allowed in (no starvation) |

---

## 12. Mutex, Semaphore, Monitor

> **Interview Q:** *What is the difference between a mutex and
> a semaphore? What is a monitor?*

### Mutex (Mutual Exclusion Lock)

A **mutex** is a locking mechanism — only the thread that
**locked** it can **unlock** it. Binary: locked or unlocked.

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
    with lock:          # acquire on enter, release on exit (even on exception)
        counter += 1
```

### Semaphore

A **semaphore** is a signalling mechanism — a non-negative
integer with two atomic operations:
- **`wait()` / `P()`** — decrement; if result < 0, block
- **`signal()` / `V()`** — increment; wake one blocked process

```c
sem_t sem;
sem_init(&sem, 0, 1);   // initial value = 1 (binary semaphore)

sem_wait(&sem);   // P() — decrement
// critical section
sem_post(&sem);   // V() — increment
```

**Binary semaphore (value 0 or 1):** works like a mutex, but
the locker and unlocker can be *different threads* — used for
signalling.

**Counting semaphore (value 0 to N):** controls access to a
pool of N resources (e.g., 5 database connections).

```c
sem_t db_pool;
sem_init(&db_pool, 0, 5);   // 5 connections available

// Thread wanting a connection:
sem_wait(&db_pool);   // blocks if all 5 are in use
// use connection
sem_post(&db_pool);   // release back to pool
```

### Mutex vs Semaphore

| | Mutex | Semaphore |
|---|---|---|
| Purpose | Mutual exclusion (ownership) | Signalling + resource counting |
| Value | Binary (locked/unlocked) | Integer (0 to N) |
| Ownership | Yes — only locker can unlock | No — any thread can signal |
| Use case | Protect a single shared resource | Limit concurrent access, producer-consumer |
| Deadlock risk | Yes (if not careful) | Yes |

> **Key analogy:**
> Mutex = a **toilet key** — only one person has it, only they
> can give it back.
> Semaphore = **parking lot slots** — multiple slots, anyone
> can leave (signal) freeing a slot for waiting cars.

### Monitor

A **monitor** is a high-level synchronisation construct that
combines:
- A mutex (implicit — only one thread active at a time)
- Condition variables (for threads to wait/signal each other)

Used in Java (`synchronized`), Python (`threading.Condition`).

```python
import threading

class BoundedBuffer:
    def __init__(self, size):
        self.buffer = []
        self.size = size
        self.lock = threading.Lock()
        self.not_full = threading.Condition(self.lock)
        self.not_empty = threading.Condition(self.lock)

    def produce(self, item):
        with self.not_full:
            while len(self.buffer) == self.size:
                self.not_full.wait()      # release lock + block
            self.buffer.append(item)
            self.not_empty.notify()       # wake a consumer

    def consume(self):
        with self.not_empty:
            while not self.buffer:
                self.not_empty.wait()
            item = self.buffer.pop(0)
            self.not_full.notify()        # wake a producer
            return item
```

### Spinlock

A lock where the waiting thread **busy-waits** (loops checking
the lock) instead of blocking. Fast when lock is held briefly
(no context switch overhead). Wastes CPU if held long.
Used in OS kernel internals, interrupt handlers.

---

## 13. Classical Synchronisation Problems

> **Interview Q:** *Explain the Producer-Consumer problem /
> Dining Philosophers / Readers-Writers.*

### Producer-Consumer (Bounded Buffer)

- Producer creates items and puts in buffer
- Consumer takes items from buffer
- Buffer has limited size N

  Semaphores:
mutex   = 1    (mutual exclusion for buffer access)
empty   = N    (count of empty slots)
full    = 0    (count of filled slots)
Producer:
wait(empty)    ← block if no empty slot
wait(mutex)    ← lock buffer
add item to buffer
signal(mutex)  ← unlock
signal(full)   ← one more full slot
Consumer:
wait(full)     ← block if no item
wait(mutex)
remove item from buffer
signal(mutex)
signal(empty)  ← one more empty slot
### Dining Philosophers

- 5 philosophers around a table; 5 chopsticks between them
- Each needs 2 chopsticks (left + right) to eat
- **Problem:** If all pick up left chopstick simultaneously →
  circular wait → deadlock

**Solutions:**
1. Allow at most 4 philosophers to sit at once
2. Asymmetric solution — odd philosophers pick left first,
   even philosophers pick right first
3. Use a semaphore to allow only one philosopher to pick up
   both chopsticks atomically

### Readers-Writers

- Multiple readers can read simultaneously (no conflict)
- Writers need exclusive access (no readers or writers)

  Semaphores:
mutex      = 1   (protect read_count)
write_lock = 1   (exclusive write access)
read_count = 0
Reader:
wait(mutex)
read_count++
if read_count == 1: wait(write_lock)   ← first reader blocks writers
signal(mutex)
← read ←
wait(mutex)
read_count--
if read_count == 0: signal(write_lock) ← last reader allows writers
signal(mutex)
Writer:
wait(write_lock)
← write ←
signal(write_lock)
Problem: Writers can starve if readers keep arriving.
Fix: Give writers priority or use a fair queue.
---

## 14. Deadlock

> **Interview Q:** *What is a deadlock? What are the four
> necessary conditions? Explain Banker's algorithm.*

### What is a Deadlock?

A **deadlock** is a situation where a set of processes are
**permanently blocked**, each waiting for a resource held by
another process in the set.
---

## 14. Deadlock

> **Interview Q:** *What is a deadlock? What are the four
> necessary conditions? Explain Banker's algorithm.*

### What is a Deadlock?

A **deadlock** is a situation where a set of processes are
**permanently blocked**, each waiting for a resource held by
another process in the set.
Process A holds Resource 1, needs Resource 2
Process B holds Resource 2, needs Resource 1
→ Both wait forever. Neither can proceed.
### Four Necessary Conditions (Coffman Conditions)

**ALL FOUR must hold simultaneously for deadlock:**

| Condition | Meaning |
|---|---|
| **Mutual Exclusion** | At least one resource must be non-shareable |
| **Hold and Wait** | Process holds ≥1 resource while waiting for more |
| **No Preemption** | Resources can't be forcibly taken — only voluntarily released |
| **Circular Wait** | Circular chain: P1 waits for P2, P2 waits for P3, ... Pn waits for P1 |

> **Tip to remember: MHNC → "My House, No Compromise"**

### Resource Allocation Graph (RAG)

- **Request edge:** P → R (process requests resource)
- **Assignment edge:** R → P (resource assigned to process)
- **Cycle in RAG with single-instance resources → deadlock**
- **Cycle in RAG with multi-instance resources → may or may
  not be deadlock**

### Dealing with Deadlock

| Approach | Strategy | Cost |
|---|---|---|
| **Prevention** | Ensure at least one Coffman condition never holds | Restrictive, low utilisation |
| **Avoidance** | Dynamically ensure system stays in safe state (Banker's) | Requires advance info |
| **Detection & Recovery** | Allow deadlocks, detect and recover | Recovery is expensive |
| **Ignorance** | Pretend it doesn't happen (ostrich algorithm) | Used in many real OS! |

### Deadlock Prevention — Breaking Each Condition

| Condition to Break | Method |
|---|---|
| Mutual Exclusion | Make resources shareable (often impossible) |
| Hold and Wait | Request ALL resources at once before starting; release all before requesting more |
| No Preemption | If process can't get resource, release what it holds; restart later |
| Circular Wait | Impose total ordering on resources; always request in order |

### Deadlock Avoidance — Banker's Algorithm

**Idea:** Before granting a resource, simulate the allocation.
If the resulting state is **safe** → grant. If **unsafe** → deny.

**Safe State:** A state is safe if there exists a **safe
sequence** — an ordering of all processes such that each
process can complete using currently available resources +
resources released by processes before it in the sequence.
Example:
Resources: A=10, B=5, C=7  (total instances)

| Process | Allocation (A B C) | Max Need (A B C) | Need = Max - Allocation (A B C) |
|----------|-------------------|------------------|----------------------------------|
| P0 | 0 1 0 | 7 5 3 | 7 4 3 |
| P1 | 2 0 0 | 3 2 2 | 1 2 2 |
| P2 | 3 0 2 | 9 0 2 | 6 0 0 |
| P3 | 2 1 1 | 2 2 2 | 0 1 1 |
| P4 | 0 0 2 | 4 3 3 | 4 3 1 |

 A=3, B=3, C=2

 
Safe sequence: P1 → P3 → P4 → P0 → P2
Step 1: P1 needs (1,2,2). Available (3,3,2) ≥ (1,2,2) ✓
Grant. P1 finishes. Available = (3,3,2)+(2,0,0) = (5,3,2)
Step 2: P3 needs (0,1,1). Available (5,3,2) ≥ (0,1,1) ✓
... and so on.
→ Safe state. System is deadlock-free.
### Deadlock Detection & Recovery

**Detection:** Run resource allocation graph cycle detection
periodically. For multi-instance: use matrix-based algorithm
similar to Banker's.

**Recovery:**
1. **Process termination** — kill one or all deadlocked
   processes (lose their work)
2. **Resource preemption** — take resources from a process,
   roll it back to safe state, restart it

---

# PART 5 — MEMORY MANAGEMENT

---

## 15. Memory Layout of a Process

> **Interview Q:** *How is a process's memory organised?
> What is the difference between stack and heap?*

## Virtual Address Space of a Process

```text
Higher Memory Addresses
0xFFFFFFFF  ┌─────────────────────────────┐
            │        Kernel Space         │
            │  OS code & kernel memory    │
            │ (not accessible to users)   │
0xC0000000  ├─────────────────────────────┤
            │            Stack            │
            │      Local variables        │
            │     Function call frames    │
            │      Return addresses       │
            │                             │
            │      ↓ Grows Downward       │
            ├─────────────────────────────┤
            │                             │
            │         Free Space          │
            │                             │
            ├─────────────────────────────┤
            │            Heap             │
            │    Dynamic memory (malloc,  │
            │         calloc, new)        │
            │                             │
            │       ↑ Grows Upward        │
            ├─────────────────────────────┤
            │        BSS Segment          │
            │ Uninitialized globals/static│
            ├─────────────────────────────┤
            │        Data Segment         │
            │ Initialized globals/static  │
            ├─────────────────────────────┤
            │       Text / Code           │
            │ Program instructions (RO)   │
0x00000000  └─────────────────────────────┘
Lower Memory Addresses
```

| Region | Contents | Size | Lifetime |
|---|---|---|---|
| **Text** | Machine code | Fixed | Program lifetime |
| **Data** | Initialised globals | Fixed | Program lifetime |
| **BSS** | Uninitialised globals | Fixed | Program lifetime |
| **Heap** | Dynamic allocations | Grows at runtime | Until freed |
| **Stack** | Function frames, locals | Grows at runtime | Until function returns |

**Stack overflow** — stack grows into heap (recursive calls too
deep). **Heap overflow** — no more memory for malloc.

---

## 16. Paging

> **Interview Q:** *What is paging? What is a page table?
> What is internal fragmentation?*

### The Memory Problem

Without paging, a process must be loaded into **contiguous**
memory. Problems:
- **External fragmentation:** free memory exists but in
  scattered pieces — no single large chunk
- **Variable partition sizes** are hard to manage

### Paging Solution

**Paging** divides both physical memory and logical (virtual)
address space into fixed-size blocks:
- **Page** — fixed-size block of **logical/virtual** memory
- **Frame** — fixed-size block of **physical** memory
- Page size = Frame size (typically 4KB)

Pages of a process can be placed in **any free frames** —
no need for contiguous physical memory.

Virtual Address  →  [Page Number | Page Offset]
↓
Page Table lookup
↓
Physical Address → [Frame Number | Page Offset]


### Page Table

The OS maintains a **page table** per process mapping page
numbers to frame numbers.


Page Table (for Process P):
Page 0 → Frame 3
Page 1 → Frame 7
Page 2 → Frame 2
Page 3 → Frame 5
Virtual address 0x1234 (page=1, offset=0x234):
→ Frame 7, offset 0x234
→ Physical address = 7 * page_size + 0x234
### TLB — Translation Lookaside Buffer

Page table is in memory → every memory access needs TWO
memory accesses (page table + actual data). Too slow!

**TLB** is a small, fast hardware cache (~64–1024 entries)
storing recent page→frame mappings.

Virtual Address
↓
TLB lookup
↙         ↘
TLB hit      TLB miss
(~1 cycle)   → page table walk (~100s cycles)
↓                ↓
Physical Address   Update TLB, retry

**TLB Hit Rate** typically > 99% — locality of reference means
same pages are accessed repeatedly.

### Fragmentation

| Type | Cause | Solution |
|---|---|---|
| **Internal Fragmentation** | Allocated block larger than needed (page not fully used) | Smaller page size (but larger page table) |
| **External Fragmentation** | Free memory scattered; no contiguous block large enough | Paging, compaction |

> Paging **eliminates external fragmentation** but introduces
> **internal fragmentation** (last page of a process is
> usually not fully used — wastes up to page_size - 1 bytes).

### Multi-Level Paging

For 64-bit systems, a single-level page table would be enormous
(2^52 entries × 8 bytes = petabytes). Solution: hierarchical
page tables.

Virtual Address = [L1 index | L2 index | L3 index | Offset]
↓
L1 Page Table (in RAM)
↓
L2 Page Table (only allocated if L1 entry present)
↓
L3 Page Table
↓
Physical Frame

Linux uses 4-level paging (PGD → PUD → PMD → PTE).

---

## 17. Segmentation

> **Interview Q:** *What is segmentation? How does it differ
> from paging?*

**Segmentation** divides memory into variable-size segments
based on **logical units** (code, stack, heap, data). Each
segment has a **base address** and **limit**.


Segment Table:
Segment 0 (code):  base=1400, limit=1000
Segment 1 (data):  base=6300, limit=400
Segment 2 (stack): base=4300, limit=1100
Virtual Address = [Segment Number | Offset]
If offset > limit → segmentation fault!
Physical Address = base + offset
| | Paging | Segmentation |
|---|---|---|
| Division | Fixed-size pages | Variable-size segments |
| Unit | Arbitrary (hardware-defined) | Logical (code, data, stack) |
| External fragmentation | None | Yes |
| Internal fragmentation | Yes | None |
| User visibility | Transparent (user doesn't see pages) | Visible (programmer defines segments) |
| Protection | Per-page | Per-segment (different permissions) |

**Most modern OS use paging** (or combined paging+segmentation).

---

## 18. Virtual Memory & Demand Paging

> **Interview Q:** *What is virtual memory? What is demand
> paging? What is a page fault?*

### Virtual Memory

**Virtual memory** allows a process to use more memory than
physically available RAM by storing some pages on disk (swap
space). Each process gets an illusion of a large, contiguous
private address space.

RAM (physical) = 8GB
Disk swap      = 16GB
Process sees   = 16GB+ virtual address space
Pages actively used → in RAM (fast)
Pages not recently used → swapped to disk (slow to access)

### Demand Paging

**Demand paging** loads pages into RAM **only when accessed**
(on demand) — not at process startup. This reduces startup
time and memory usage.

- Pages start **not present** (valid bit = 0 in page table)
- When process accesses a page not in RAM → **page fault**

### Page Fault Handling
Process accesses virtual address
↓
MMU checks page table
↓
Valid bit = 0 (page not in RAM)
↓
PAGE FAULT exception → control to OS
↓
OS finds a free frame (or evicts one using page replacement)
↓
OS reads page from disk into free frame
↓
OS updates page table: frame number + valid bit = 1
↓
Restart the faulting instruction
↓
Process continues (transparently)

**Page fault is expensive:** Disk I/O takes ~10ms vs RAM
access ~100ns → 100,000× slower. Minimising page faults is
critical for performance.

### Effective Access Time
EAT = (1 - p) × memory_access_time + p × page_fault_time
Where p = page fault rate
memory_access_time ≈ 100ns
page_fault_time ≈ 10ms (10,000,000ns)
If p = 0.001 (1 in 1000 accesses cause page fault):
EAT = 0.999 × 100 + 0.001 × 10,000,000
= 99.9 + 10,000
≈ 10,100ns  (100× slower than no page faults)

---

## 19. Page Replacement Algorithms

> **Interview Q:** *Explain FIFO, LRU, and Optimal page
> replacement. What is Belady's anomaly?*

When a page fault occurs and RAM is full, the OS must evict a
page to disk. Which one?

### FIFO — First In First Out

Evict the page that has been in memory the **longest**.

Reference string: 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1
Frames = 3
7 | 7 0 | 7 0 1 | 2 0 1 | 2 0 3 | 2 0 3...
↑ fault each new page
Page faults: 15

**Belady's Anomaly:** FIFO can have MORE page faults with MORE
frames. Counter-intuitive! Other algorithms don't suffer this.

Example: reference string 1 2 3 4 1 2 5 1 2 3 4 5
3 frames: 9 page faults
4 frames: 10 page faults  ← MORE faults with more memory!

### Optimal (OPT / MIN)

Evict the page that **will not be used for the longest time**
in the future. Gives the minimum page faults.

**Problem:** Requires future knowledge — impossible to implement
in practice. Used as a **benchmark** to compare other algorithms.

### LRU — Least Recently Used

Evict the page that was **used least recently** (farthest in
the past).


Reference string: 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1
Frames = 3
Page faults: 12  (better than FIFO's 15, close to OPT's ~9)

**Implementation:**
- **Counter:** Each page has a timestamp of last use.
  Evict lowest timestamp. O(n) to find minimum.
- **Stack:** Move page to top on access. Bottom = LRU.
  O(1) evict, O(n) find.
- **Doubly linked list + hash map:** O(1) for both access and
  eviction. Used in practice (LRU Cache — classic LeetCode problem).

### LRU Approximations (used in real OS)

**Clock Algorithm (Second Chance):**
- Pages arranged in circular list with a reference bit.
- On page fault: if reference bit = 1, clear it (give second
  chance) and move clock hand. If bit = 0, evict this page.
- O(1) — used in Linux.

### Comparison

| Algorithm | Page Faults | Implementation | Notes |
|---|---|---|---|
| FIFO | High | Easy (queue) | Belady's anomaly |
| Optimal | Lowest | Impossible (needs future) | Theoretical benchmark |
| LRU | Low | Complex (timestamp / list+map) | Best practical |
| Clock | Close to LRU | Moderate | Used in Linux |

---

## 20. Thrashing

> **Interview Q:** *What is thrashing? How do you prevent it?*

**Thrashing** occurs when a process (or the OS) spends more
time **swapping pages in and out** than actually executing.

Cause:
Too many processes → each gets too few frames →
high page fault rate → CPU spends time doing I/O →
CPU utilisation drops → OS admits MORE processes
(thinking CPU is underutilised) → even more page faults
→ system grinds to a near halt

CPU Utilisation
│  ↗ (good — more processes help)
│      ↘ (bad — thrashing begins here)
└──────────────────────────────→ Degree of Multiprogramming

### Prevention

1. **Working Set Model:** Track the set of pages a process has
   used in the last Δ time units (working set). Only admit a
   process if enough frames are available for its working set.
2. **Page Fault Frequency (PFF):** If page fault rate > upper
   threshold → give process more frames. If < lower threshold
   → take frames away.
3. **Reduce degree of multiprogramming:** Suspend some processes,
   free their frames for others.
4. **Increase RAM** (simplest real-world fix).

---

# PART 6 — STORAGE & FILE SYSTEMS

---

## 21. File Systems

> **Interview Q:** *What is a file system? What are the
> methods of file allocation?*

A **file system** is the OS component that manages how data is
stored and retrieved on storage devices. It provides
abstraction over raw disk blocks.

### Key Components

| Component | Role |
|---|---|
| **Inode** | Metadata about a file (size, permissions, owner, block pointers) — NOT the name |
| **Directory** | Maps filenames to inodes |
| **Superblock** | Metadata about the file system itself (size, free blocks, inode count) |
| **Block** | Fixed-size unit of disk storage (typically 4KB) |

File access: filename
↓
Directory lookup → inode number
↓
Inode → block pointers
↓
Disk blocks → actual data

### File Allocation Methods

**1. Contiguous Allocation**
- File occupies consecutive disk blocks.
- Fast read (sequential). Simple.
- External fragmentation. File size must be known in advance.

**2. Linked Allocation**
- Each block contains a pointer to the next block.
- No fragmentation. File can grow dynamically.
- No random access (must traverse chain). Pointer overhead.
  Pointer corruption → lose rest of file.
- FAT (File Allocation Table) stores all pointers in a table
  in memory → solves random access problem.

**3. Indexed Allocation**
- An index block contains all block pointers for a file.
- Random access in O(1). No external fragmentation.
- Index block overhead. Large files need multi-level indexes.
- **Unix/Linux inodes use this**: direct pointers (12 blocks),
  single indirect (points to a block of pointers), double
  indirect, triple indirect.

  
Inode:
direct[0..11]         → 12 blocks directly
single_indirect       → block of 1024 pointers → 1024 blocks
double_indirect       → 1024 × 1024 blocks
triple_indirect       → 1024 × 1024 × 1024 blocks

### File Permissions (Unix)
-rwxr-xr--  1 user group 4096 Jun 11 09:00 file.py
│└──┘└──┘└──┘
│  │   │   └── Others: r-- (read only)
│  │   └────── Group:  r-x (read + execute)
│  └────────── Owner:  rwx (read + write + execute)
└───────────── File type: - (regular), d (directory), l (symlink)

---

## 22. Disk Scheduling

> **Interview Q:** *What is disk scheduling? Explain SSTF
> and SCAN algorithms.*

**Disk scheduling** determines the order in which disk I/O
requests are serviced to minimise **seek time** (time for
disk head to move to the right track).

### Algorithms

**FCFS — First Come First Served**
- Requests served in arrival order.
- Simple, fair. High seek time.

**SSTF — Shortest Seek Time First**
- Service the request closest to current head position.
- Low seek time. Can cause **starvation** of far requests.

**SCAN (Elevator Algorithm)**
- Head moves in one direction servicing requests, reaches end,
  reverses direction.
- Like an elevator. Fair. Good throughput.

**C-SCAN (Circular SCAN)**
- Like SCAN but only services requests in ONE direction.
  When reaching end, jumps back to beginning without servicing.
- More uniform wait times than SCAN.

**C-LOOK**
- Like C-SCAN but only goes as far as the last request in each
  direction (doesn't go all the way to disk end).
- Most practical. Used in Linux I/O schedulers.
  
Requests: 98, 183, 37, 122, 14, 124, 65, 67
Head starts at: 53, moving toward higher
FCFS order: 53→98→183→37→122→14→124→65→67  (total movement: 640)
SSTF order: 53→65→67→37→14→98→122→124→183  (total movement: 236)
SCAN order: 53→65→67→98→122→124→183→37→14  (total movement: 208)

---

# PART 7 — INTERVIEW Q&A

---

## 23. Interview Q&A — Foundations & Processes

**Q: What is the difference between a program and a process?**
> A **program** is a static set of instructions stored on disk.
> A **process** is a program in execution — dynamic, with its
> own memory space, CPU registers, and state. One program can
> give rise to multiple processes (e.g., opening two terminal
> windows both run `bash` — two processes, one program).

**Q: What is a zombie process? How do you prevent it?**
> A zombie process is a process that has finished execution but
> its PCB still exists because the parent hasn't called
> `wait()` to collect its exit status. It occupies a PID slot
> and a PCB entry — no other resources. Prevention: always call
> `wait()` or `waitpid()` in the parent. In servers: use
> `SIGCHLD` handler to `wait()` asynchronously, or double-fork
> (child forks again and exits; grandchild is orphaned to init).

**Q: What is the difference between `fork()` and `exec()`?**
> `fork()` creates a **copy** of the current process — both
> parent and child continue from the same point. The child gets
> a copy of the parent's address space (copy-on-write). `exec()`
> **replaces** the current process's memory image with a new
> program — the PID stays the same but everything else changes.
> Typically used together: `fork()` to create child, `exec()`
> in child to run a different program.

**Q: What is copy-on-write (COW) in `fork()`?**
> After `fork()`, instead of immediately copying the parent's
> entire address space to the child, the OS marks all pages as
> shared and read-only. When either process tries to **write**
> to a page, a fault occurs, the OS copies just that page and
> gives each process its own copy. Result: `fork()` is very
> fast when the child immediately calls `exec()` (no pages need
> copying). Python's `multiprocessing` uses this.

**Q: What is a thread pool and why is it used?**
> A thread pool is a collection of pre-created threads waiting
> for tasks. Instead of creating and destroying threads for
> each task (expensive — ~µs each), you submit tasks to a queue
> and idle threads pick them up. Benefits: (1) eliminates
> thread creation/destruction overhead; (2) limits total thread
> count (prevents system overload); (3) tasks are queued if all
> threads are busy. Used in web servers (every HTTP request is
> a task), Python's `concurrent.futures.ThreadPoolExecutor`.

**Q: What is the difference between preemptive and
non-preemptive scheduling?**
> In **non-preemptive** scheduling, once a process is given
> the CPU, it runs until it voluntarily releases the CPU (by
> finishing or blocking on I/O). In **preemptive** scheduling,
> the OS can forcibly take the CPU from a running process —
> triggered by a timer interrupt or a higher-priority process
> becoming ready. Preemptive is better for responsiveness and
> fairness but has higher context-switch overhead.

---

## 24. Interview Q&A — Scheduling & Synchronisation

**Q: What is starvation? How is it different from deadlock?
How is it solved?**
> **Deadlock:** A set of processes are ALL stuck, each waiting
> for another in the set. No process can proceed.
> **Starvation:** A process CAN proceed but NEVER gets the CPU
> because higher-priority processes keep arriving (indefinitely
> delayed). Deadlock requires a circular wait; starvation does
> not. Solution for starvation: **aging** — gradually increase
> the priority of a waiting process over time so it eventually
> gets scheduled.

**Q: What is the difference between a mutex and a semaphore?**
> A **mutex** enforces ownership — only the thread that locked
> it can unlock it. Used for mutual exclusion (protecting a
> single shared resource). A **semaphore** has no ownership —
> any thread can signal it. Can have a count > 1 (counting
> semaphore) to allow N concurrent accesses. Use mutex to
> protect a resource; use semaphore for signalling between
> threads or limiting concurrent access to a pool.

**Q: What is a deadlock? Give the four conditions.**
> Deadlock is when processes permanently block waiting for
> resources held by each other. Four conditions (ALL must hold):
> (1) **Mutual Exclusion** — resources are non-shareable;
> (2) **Hold and Wait** — process holds one resource while
> waiting for another;
> (3) **No Preemption** — resources can't be forcibly taken;
> (4) **Circular Wait** — circular chain of processes, each
> waiting for the next.

**Q: What is the Banker's Algorithm?**
> The Banker's Algorithm is a deadlock **avoidance** algorithm.
> Before granting a resource request, it simulates the
> allocation and checks if the resulting state is **safe** (a
> safe sequence of process completion exists). If safe → grant.
> If unsafe → deny and make the process wait. Named after a
> bank that never allows customers to withdraw more than what
> would leave it unable to serve all remaining customers.
> **Limitation:** Requires knowing maximum resource needs in
> advance; impractical for many real systems.

**Q: What is the critical section problem?**
> The critical section problem is ensuring that when one
> process is executing in its critical section (code accessing
> shared resources), no other process is in its critical
> section simultaneously. A correct solution must satisfy:
> (1) **Mutual exclusion** — only one at a time;
> (2) **Progress** — if no one is in CS, selection must happen;
> (3) **Bounded waiting** — no process waits forever.

**Q: What is a race condition? Give an example.**
> A race condition is when the outcome of a program depends on
> the non-deterministic interleaving of multiple threads. Classic
> example: two threads both do `counter++` (which is LOAD,
> ADD, STORE — 3 operations). If they interleave, one store can
> overwrite the other → counter incremented once instead of
> twice. Fix: use a mutex/lock around the increment operation.

---

## 25. Interview Q&A — Memory Management

**Q: What is virtual memory? What are its benefits?**
> Virtual memory gives each process the illusion of a large,
> private address space larger than physical RAM. The OS and
> hardware (MMU) transparently map virtual addresses to physical
> frames; pages not in RAM are stored on disk (swap). Benefits:
> (1) Programs larger than RAM can run;
> (2) Processes are isolated — one process can't corrupt another's memory;
> (3) More processes can run simultaneously (each uses only what it actively needs);
> (4) Simplifies programming (every process sees the same address layout).

**Q: What is a page fault? Is it always bad?**
> A page fault occurs when a process accesses a virtual page
> that is not currently in physical RAM. The OS handles it by
> loading the page from disk. It is NOT always bad:
> **minor page fault** — page is in memory but not mapped (e.g.,
> shared library just needs to map it — fast).
> **major page fault** — page is on disk and must be read — slow
> (~10ms). Programs with good locality have few major page
> faults.

**Q: What is the difference between paging and segmentation?**
> **Paging** divides memory into fixed-size pages (e.g., 4KB).
> Eliminates external fragmentation, introduces internal
> fragmentation. User program doesn't see pages.
> **Segmentation** divides memory into variable-size logical
> segments (code, stack, heap). Eliminates internal
> fragmentation, introduces external fragmentation. Segments
> correspond to programmer-visible logical units.
> Modern OS (Linux, Windows) use **paging** (sometimes combined
> with segmentation — e.g., x86 uses segments for privilege
> separation but paging for address translation).

**Q: What is the TLB and why is it important?**
> The Translation Lookaside Buffer is a small, fast hardware
> cache (part of the MMU) that stores recent virtual→physical
> page translations. Without TLB, every memory access requires
> two RAM accesses (page table + actual data). With TLB hits
> (~99% of accesses), translation costs just 1 cycle. TLB miss
> → page table walk (~50–200 cycles). On a context switch
> (different process), TLB is flushed (different address space),
> causing a TLB cold-start penalty — one reason context
> switches are expensive.

**Q: What is thrashing and how do you prevent it?**
> Thrashing is when processes spend more time handling page
> faults than executing useful work. Happens when too many
> processes compete for too few frames — each process's working
> set doesn't fit in its allocated frames. Prevention:
> (1) **Working set model** — only admit a process if enough
> frames exist for its working set;
> (2) **Page fault frequency control** — monitor PFF and adjust
> frame allocation dynamically;
> (3) **Reduce multiprogramming** — swap out some processes.

**Q: What is Belady's Anomaly?**
> Belady's Anomaly is the counter-intuitive observation that
> with the **FIFO** page replacement algorithm, **increasing
> the number of frames can increase the number of page faults**.
> This is specific to FIFO. LRU, Optimal, and most stack
> algorithms are monotone — more frames never means more faults.

---

## 26. Interview Q&A — Advanced & Tricky

**Q: How does the OS protect one process from reading
another process's memory?**
> Each process has its own **page table** managed by the OS.
> The hardware MMU uses the current process's page table for
> all address translations — a process can only access virtual
> addresses mapped in its own page table. If it tries to access
> an unmapped address, the MMU triggers a **segmentation fault**
> (protection fault), the OS kills the process. The OS page
> tables themselves are in protected kernel space — user
> processes can't modify page tables directly.

**Q: What is the difference between internal and external
fragmentation?**
> **Internal fragmentation:** Allocated memory block is larger
> than what was requested — wasted space inside an allocated
> region. Caused by fixed-size allocation units (e.g., pages).
> Example: File needs 4097 bytes, gets two 4096-byte pages →
> 4095 bytes wasted.
> **External fragmentation:** Free memory exists in total but
> is scattered in small non-contiguous chunks — can't satisfy
> a large allocation. Caused by variable-size allocation.
> Paging eliminates external fragmentation; segmentation
> eliminates internal fragmentation.

**Q: What is the difference between a hard link and a
soft link (symlink)?**
> A **hard link** is a directory entry pointing to the same
> inode as the original file. Both names refer to the exact
> same file data. Deleting the original doesn't delete the data
> (inode's link count decrements; data deleted only when
> count = 0). Can't cross file systems or link to directories.
> A **soft link (symlink)** is a special file containing a
> path string. If the original is deleted, the symlink breaks
> (dangling link). Can cross file systems and link to
> directories.

**Q: What happens when you type `ls -l` in a terminal?**
> (1) Shell calls `fork()` to create a child process.
> (2) Child calls `exec("ls", ["-l"])` — replaces its memory
> with the `ls` program.
> (3) `ls` calls `opendir()` and `readdir()` (system calls) to
> read directory entries from the kernel.
> (4) For each file, `ls` calls `stat()` (syscall) to get
> inode info (permissions, size, timestamps).
> (5) `ls` calls `write()` (syscall) to output to stdout.
> (6) `ls` calls `exit()`. Kernel reclaims resources.
> (7) Parent shell calls `wait()`, gets exit status, shows
> prompt again.

**Q: What is a context switch? What is saved during it?**
> A context switch is the OS saving the state of the current
> process and restoring another. Saved to PCB:
> Program Counter, all CPU registers (general purpose, floating
> point, SIMD/SSE), stack pointer, memory mappings metadata.
> The TLB may be flushed (process switch) or not (thread switch
> within same process). Typical cost: 1–10µs for processes,
> ~100ns for threads. Cost comes from: saving/restoring
> registers, TLB flush + cold-start, cache eviction.

**Q: What is a spinlock? When is it better than a mutex?**
> A spinlock is a lock where the waiting thread busy-waits
> (loops checking the lock status) rather than sleeping.
> Better than mutex when: (1) lock hold time is very short
> (less than the cost of a context switch ~µs); (2) you're in
> interrupt context (can't sleep); (3) running on a
> multiprocessor where the holder may be running on another CPU.
> Worse than mutex for long hold times — wastes CPU cycles.
> Used in OS kernel interrupt handlers, lock-free data
> structures.

**Q: What is demand paging vs pre-paging?**
> **Demand paging** loads a page only when it is first
> accessed (on page fault). Reduces startup time and memory
> usage. Higher initial page fault rate.
> **Pre-paging** loads pages before they are needed —
> predicting what will be needed (based on spatial locality,
> access patterns). Reduces page fault rate at the cost of
> possibly loading unused pages. Modern OS use a hybrid:
> demand paging with read-ahead (pre-fetch nearby disk blocks).

---

## 27. Quick Reference Cheat Sheet

PROCESS vs THREAD
Process: own address space, heavy (ms to create), isolated
Thread: shared address space, light (µs to create), shared heap
Context switch: Process > Thread (TLB flush + cache cold)
PROCESS STATES: New → Ready → Running → Waiting → Terminated
PCB contains: PID, state, PC, registers, memory maps, open files
SCHEDULING ALGORITHMS
FCFS:  non-preemptive, simple, convoy effect
SJF:   non-preemptive, optimal avg wait, needs burst time, starvation
SRTF:  preemptive SJF, optimal with arrivals, high overhead
RR:    preemptive, fair, time quantum q (10-100ms typical)
Priority: preemptive/non, starvation → fix with aging
MLFQ:  best general purpose, used in Linux/Windows
KEY FORMULAE
Turnaround = Completion − Arrival
Waiting    = Turnaround − Burst
Response   = First CPU − Arrival
EAT        = (1-p)×mem_access + p×page_fault_time
DEADLOCK: all 4 must hold
Mutual Exclusion + Hold & Wait + No Preemption + Circular Wait
Prevention: break one condition
Avoidance: Banker's Algorithm (safe state check)
Detection: RAG cycle / matrix algorithm
SYNCHRONISATION
Race condition: outcome depends on thread interleaving
Critical section: 3 requirements: mutual exclusion, progress, bounded wait
Mutex: ownership, binary, only locker can unlock
Semaphore: no ownership, counting (0 to N), P()/V() operations
Monitor: mutex + condition variables (Python threading.Condition)
Spinlock: busy-wait, good for short critical sections on multiprocessor
MEMORY
Virtual address = Page Number + Offset
Page table maps virtual pages to physical frames
TLB: hardware cache of recent translations (~99% hit rate)
Page fault: page not in RAM → OS loads from disk (major = slow)
Internal fragmentation: paging (last page not fully used)
External fragmentation: segmentation / variable-size allocation
PAGE REPLACEMENT
FIFO:    oldest page evicted. Simple. Belady's anomaly.
Optimal: evict page used farthest in future. Impossible in practice.
LRU:     evict least recently used. Best practical. O(1) with list+hashmap.
Clock:   LRU approximation. O(1). Used in Linux.
THRASHING: too many page faults, CPU doing more I/O than work
Fix: working set model, PFF control, reduce multiprogramming
FILE ALLOCATION
Contiguous: fast, external fragmentation, fixed size
Linked:     flexible, no random access, pointer overhead
Indexed:    fast random access, inode model (Unix)
IPC METHODS (fast → slow): Shared Memory > Pipe > Message Queue > Socket
SIGNALS (Linux)
SIGINT(2):  Ctrl+C         SIGKILL(9): force kill (uncatchable)
SIGTERM(15): polite kill   SIGSEGV(11): segfault
SIGCHLD(17): child died    SIGSTOP(19): pause (uncatchable)
KERNEL TYPES
Monolithic: fast, large (Linux)
Microkernel: stable, slow IPC (Minix, QNX)
Hybrid: Windows NT, macOS

---

*Sources: InterviewBit OS Interview Questions (2026),
GeeksforGeeks OS Top 100 (2025), LeetCode FAANG OS Questions,
igmGuru OS Interview Guide (2026), Silberschatz "Operating
System Concepts" (10th ed.), OSTEP (Operating Systems: Three
Easy Pieces — free online).*
