---
layout: post
title: "Process Management in Operating Systems"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [OS, Process]
tags: [OS, PCB, PID, States, Context Switching]
pin: false
math: true
mermaid: true

---

# 🧠 Process Management — States, PCB, Context Switching, and Memory Layout

> A structured guide to **how operating systems manage processes**,  
> including **foreground/background processes, PCB, scheduling, memory layout, and process creation**.

---

# 1️⃣ What Is a Process?

A **process** is a **program in execution**, requiring CPU time, memory, and system resources.

> All processes **share the CPU over time** — execution is divided using **timer interrupts**.

---

# 2️⃣ Foreground vs Background Processes

## 🔹 Foreground Process
- Runs in a **visible user space**
- Interacts directly with the user

## 🔹 Background Process
- Runs **without direct UI visibility**

### Types
✔ Interactive background processes (services, daemons)  
✔ Non‑interactive background processes

---

# 3️⃣ CPU Time Sharing & Timer Interrupt

- CPU gives **each process a limited time slice**
- When time expires, a **timer interrupt** forces a switch

> This enables **multitasking**.

---

# 4️⃣ Process Control Block (PCB)

The **PCB** is a **kernel‑resident data structure** that stores all process state.

### PCB Stores

| Category | Description |
|--------|------------|
| **PID** | Unique Process ID |
| **Register State** | PC, SP, CPU registers |
| **Process State** | Ready / Running / Waiting |
| **Scheduling Info** | Priority, time slice |
| **Memory Info** | Page table, address space |
| **I/O Info** | Open files, devices |
| **Accounting Info** | CPU usage statistics |

---

# 5️⃣ Process States & Transitions

| State | Meaning |
|------|--------|
| **New** | Process created |
| **Ready** | Waiting for CPU |
| **Running** | Executing |
| **Waiting (Blocked)** | Waiting for I/O |
| **Terminated** | Finished |

### State Flow

```
New → Ready → Running → Waiting → Ready → Terminated
```

### Key Transitions
- **Dispatch**: Ready → Running  
- **Timer Interrupt**: Running → Ready  
- **I/O Request**: Running → Waiting  
- **I/O Complete**: Waiting → Ready  

---

# 6️⃣ Context Switch

> A **context switch** saves the current process state and restores another process’s state.

### Steps
1. Save Process A context (registers, PC, SP, memory map)
2. Load Process B context from PCB
3. Resume execution

> Enables CPU to **rapidly alternate between processes**.

---

# 7️⃣ Process Memory Layout (User Space)

Each process has a **private virtual address space**:

| Segment | Purpose |
|--------|--------|
| **Text (Code)** | Executable instructions (read‑only) |
| **Data** | Global/static variables |
| **Heap** | Dynamic memory allocation |
| **Stack** | Function calls, local variables |

### Allocation Type
- Text + Data → **Static region**
- Heap + Stack → **Dynamic region**

---

# 8️⃣ Process Hierarchy (Parent–Child)

- A process can create another process via **system calls**
- Creator = **Parent process**
- Created = **Child process**
- Child has a unique **PID**
- Some OS track **PPID (Parent PID)**

---

# 9️⃣ Process Creation Mechanism — fork() & exec()

## 🔹 fork()
Creates a **child process as a copy of the parent**
- Inherits memory, file descriptors, environment

```c
pid_t pid = fork();
```

## 🔹 exec()
Replaces process memory with a **new program**

```c
execve("/bin/program", args, env);
```

> Common pattern:
```
fork() → exec()
```

---

# 🔟 Developer Takeaways

✔ Processes share CPU via time slicing  
✔ PCB is the OS memory of each process  
✔ Context switches enable multitasking  
✔ Each process has isolated memory  
✔ fork + exec power Unix process creation  
