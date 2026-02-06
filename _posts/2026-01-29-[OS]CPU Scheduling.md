---
layout: post
title: "CPU Scheduling"
date: 2026-01-29 00:00:00 +0900
author: kang
categories: [OS, CPU Scheduling]
tags: [OS, Queues, Tradeoffs, CPU, Scheduling]
pin: false
math: true
mermaid: true

---

# 🧠 CPU Scheduling — Fair and Efficient CPU Allocation

> How an operating system **decides which process runs next**,  
> balancing **fairness, responsiveness, and performance**.

---

# 1️⃣ What Is CPU Scheduling?

**CPU Scheduling** = The OS policy for **distributing CPU time among processes**.

Goals:
✔ Fairness  
✔ High CPU utilization  
✔ Low waiting time  
✔ Good responsiveness  

> The “most fair” method is **not simply running processes in order**.

---

# 2️⃣ I/O‑Bound vs CPU‑Bound Processes

| Type | Behavior | Scheduling Priority |
|------|---------|----------------------|
| **I/O‑Bound** | Frequently waits for I/O | **Higher priority** |
| **CPU‑Bound** | Uses CPU heavily | Lower priority |

**Reason:** I/O‑bound processes often block — boosting them improves system responsiveness.

---

# 3️⃣ Priority & PCB

Each process has a **priority stored in its PCB**.

- Higher priority → scheduled sooner
- Tools like **Process Explorer / Task Manager** can display this

---

# 4️⃣ Scheduling Queues

Processes wait in queues depending on resource needs.

## 🔹 Ready Queue
Processes waiting to **use the CPU**

## 🔹 Waiting Queue
Processes waiting for **I/O devices**

- Device‑specific queues exist (disk queue, printer queue, etc.)
- When I/O finishes → process moves to **Ready Queue**

---

# 5️⃣ Preemptive vs Non‑Preemptive Scheduling

## 🔹 Preemptive
- OS can **interrupt a running process**
- Prevents CPU monopolization
- Improves fairness
- ❌ Higher context switch overhead

## 🔹 Non‑Preemptive
- Process keeps CPU until it finishes or blocks
- ❌ Risk of unfair CPU hogging
- ✔ Lower overhead

---

# 6️⃣ Major CPU Scheduling Algorithms

---

## 🔹 FCFS (First‑Come, First‑Served)

- Non‑preemptive
- Executes in arrival order
- ❌ **Convoy Effect**: long jobs delay short ones

---

## 🔹 SJF (Shortest Job First)

- Runs process with **shortest CPU burst**
- Reduces average waiting time
- ❌ Hard to predict job length

---

## 🔹 Round Robin (RR)

- FCFS + **Time Slice (Quantum)**
- Preemptive
- Each process gets CPU in turns
- ⚠️ Quantum size matters:
  - Too small → high overhead
  - Too large → behaves like FCFS

---

## 🔹 SRTF (Shortest Remaining Time First)

- Preemptive SJF
- Always runs process with **least remaining CPU time**

---

## 🔹 Priority Scheduling

- Highest priority runs first
- Equal priority → FCFS
- ❌ **Starvation** risk

### Fix: Aging
> Gradually **increase priority** of waiting processes

---

## 🔹 Multilevel Queue Scheduling

- Multiple ready queues by priority
- ❌ No movement between queues → starvation risk

---

## 🔹 Multilevel Feedback Queue (MLFQ)

✔ Processes **move between queues**  
✔ New jobs start high priority  
✔ CPU‑heavy jobs sink lower  
✔ Aging prevents starvation  

> One of the **most practical real‑world schedulers**

---

# 7️⃣ Scheduling Tradeoffs

| Goal | Best Choice |
|------|------------|
| Throughput | SJF |
| Response time | Round Robin |
| Fairness | MLFQ |
| Low overhead | FCFS |
| Real‑time | Priority / EDF |

---

# 8️⃣ Developer Takeaways

✔ Scheduling balances fairness & performance  
✔ I/O‑bound tasks benefit from higher priority  
✔ Time slice size critically affects latency  
✔ Starvation must be prevented with aging  
✔ MLFQ is widely used in modern OS kernels  


