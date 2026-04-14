---
layout: post
title: "Miscellaneous - Instruction Cycle, Pipeline, Interrupts, and ISA"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [CS Fundamentals, CS Fundamentals - Computer Structure]
tags: [Computer Structure, Miscellaneous, CPU, Instruction, Instruction Cycle, Pipelines, Interrupts, ISA]
pin: false
math: true
mermaid: true

---

# 🧠 Instruction Cycle, Pipeline, Interrupts, and ISA

> A structured overview of **how CPUs execute instructions**, handle **interrupts**, improve **performance**, and differ by **instruction set architecture (ISA)**.

---

# 1️⃣ Instruction Cycle

The CPU repeatedly executes instructions using a cycle:

```
Fetch → Execute → Fetch → Execute
```

Sometimes an extra **Indirect Cycle** occurs when indirect addressing is used.

### Main Phases
- **Fetch** — Load instruction from memory
- **Decode** — Interpret instruction
- **Execute** — Perform operation
- **Writeback** — Store result

---

# 2️⃣ Interrupts — Handling Urgent Events

> Interrupts allow the CPU to **pause current work** to handle urgent tasks.

---

## 2.1 Synchronous Interrupts (Exceptions)

Triggered by CPU-detected conditions:
- Faults (e.g., page fault)
- Traps (software interrupts)
- Aborts (critical failures)

👉 Caused by **current instruction execution**

---

## 2.2 Asynchronous Interrupts (Hardware Interrupts)

Triggered by **external devices**:
- Keyboard
- Disk
- Network card

### Types
- **Maskable Interrupts** — Can be disabled
- **Non-Maskable Interrupts (NMI)** — Cannot be ignored

---

## Interrupt Handling Flow

1. Interrupt request signal occurs
2. CPU checks **Interrupt Flag** (in Flag Register)
3. CPU saves execution state to **stack**
4. Jump to **Interrupt Service Routine (ISR)**
5. ISR executes required work
6. CPU restores state and resumes execution

### Saved State Includes
- Program Counter (PC)
- Instruction Register (IR)
- Memory Address Register (MAR)
- Memory Buffer Register (MBR)

---

## Interrupt Vector

> A table that maps **interrupt signals → ISR addresses**

Allows CPU to quickly find correct handler.

---

# 3️⃣ CPU Performance Scaling

## Ways to Make CPUs Faster
✔ Higher clock speed (Hz)  
✔ More CPU cores  
✔ More hardware threads  
✔ Better instruction-level parallelism  

### Important Reality
> **Performance does NOT scale linearly with core count.**

---

## Hardware Threads vs Software Threads

| Type | Meaning |
|------|--------|
| **Hardware Threads** | Parallel execution units inside a core |
| **Software Threads** | Logical execution paths in programs |

👉 Multithreading works because **each thread has its own register state**.

---

# 4️⃣ Instruction Pipeline

> CPUs overlap instruction execution like an assembly line.

## Pipeline Stages

```
Fetch → Decode → Execute → Writeback
```

Allows multiple instructions to run **simultaneously**.

---

# 5️⃣ Pipeline Hazards

## 🔹 Data Hazard
Occurs when instructions depend on earlier results.

## 🔹 Control Hazard
Occurs when branches change Program Counter unexpectedly.
✔ Mitigated by **Branch Prediction**

## 🔹 Structural Hazard
Occurs when instructions compete for same CPU resource.

---

# 6️⃣ Superscalar Architecture

> A **superscalar CPU** executes **multiple instructions per cycle** using **multiple pipelines**.

### Tradeoff
More pipelines = more hazard complexity  
➡ Performance does **not scale perfectly**

---

# 7️⃣ Out-of-Order Execution

> CPU reorders instructions internally **as long as program results remain correct**.

✔ Improves efficiency  
✔ Reduces idle cycles  

---

# 8️⃣ Instruction Set Architecture (ISA)

> ISA defines **what instructions a CPU understands**.

It affects:
- Instruction formats
- Register count
- Pipeline design
- Performance characteristics

---

## 8.1 CISC — Complex Instruction Set Computer

### 🧪 Example Instructions (CISC — x86)

CISC instructions can perform **multiple operations in one instruction**.

```asm
REP MOVSB   ; Copy memory block (loop + load + store)
ADD [MEM], REG   ; Memory read + add + write back
```

👉 One instruction may involve **memory access, arithmetic, and looping**.


### Characteristics
- Variable-length instructions
- Many instruction types
- Fewer instructions per program

### Pros
✔ Compact programs  
### Cons
❌ Hard to pipeline  
❌ Variable execution time  

---

## 8.2 RISC — Reduced Instruction Set Computer

### 🧪 Example Instructions (RISC — ARM / RISC-V)

RISC instructions perform **one simple operation per instruction**.

```asm
LDR X0, [ADDR]   ; Load
ADD X1, X0, X2    ; Compute
STR X1, [ADDR]   ; Store
```

👉 Memory access is separated from computation.


### Characteristics
- Fixed-length instructions
- Load/Store architecture
- Many general-purpose registers
- Most instructions execute in **~1 clock cycle**

### Pros
✔ Pipeline-friendly  
✔ Predictable timing  
### Cons
❌ More instructions required  

---

## 🔹 CISC Example (x86)

CISC instructions often perform **multiple operations in one instruction**.

### Example
```asm
REP MOVSB
```
**Meaning:** Copy a block of memory (loop + load + store)

Equivalent conceptual steps:
```text
loop:
  load byte from source
  store byte to destination
  increment pointers
  repeat until done
```

### Another Example
```asm
ADD [MEM], REG
```
👉 Reads memory + adds + writes back in **one instruction**

---

## 🔹 RISC Example (ARM / RISC-V)

RISC instructions are **simple and fixed-length**, usually **one operation per instruction**.

### Example
```asm
LDR X0, [ADDR]   ; Load
ADD X1, X0, X2    ; Compute
STR X1, [ADDR]   ; Store
```

👉 Memory access is separated from arithmetic

---

## 🔹 Same Task — CISC vs RISC Comparison

### Task: `A = A + B`

### CISC
```asm
ADD [A], [B]
```

### RISC
```asm
LDR R0, [A]
LDR R1, [B]
ADD R0, R0, R1
STR R0, [A]
```

---

## 🧠 Key Insight

| Architecture | Style |
|-------------|------|
| **CISC** | Few powerful instructions |
| **RISC** | Many small, predictable instructions |

> CISC = complex instructions, fewer lines  
> RISC = simpler instructions, more lines, faster pipelines


---

# 9️⃣ Developer Takeaways

✔ Interrupts protect responsiveness  
✔ Pipeline improves throughput, but introduces hazards  
✔ Superscalar ≠ linear performance scaling  
✔ RISC simplifies pipelines  
✔ ISA choice shapes CPU behavior  

---

# 🧩 One-Line Mental Model

> **Modern CPUs execute many instructions in parallel while preserving correct program behavior.**
