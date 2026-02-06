---
layout: post
title: "CPU Registers"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [Computer Structure, Components]
tags: [Computer Structure, Components, CPU, Registers]
pin: false
math: true
mermaid: true

---

# 🧠 CPU Registers — Types, Roles, and Addressing Modes (Developer Guide)

> A practical overview of **CPU registers**, what they store, and how they support **instruction execution, memory access, and addressing**.

---

# 1️⃣ What Is a Register?

A **register** is a **small, ultra-fast storage location inside the CPU**.

Registers store:
- Instructions
- Memory addresses
- Operands (data)
- Execution state

📌 Registers are **much faster than cache or RAM** and are essential for performance.

---

# 2️⃣ Core CPU Registers

## 🟦 Program Counter (PC)
> Holds the **address of the next instruction to fetch**.

- Automatically updated after each instruction
- Changed by jumps, calls, interrupts

---

## 🟦 Memory Address Register (MAR)
> Stores the **memory address being accessed**.

Used when:
- Reading from memory
- Writing to memory

---

## 🟦 Memory Buffer Register (MBR / MDR)
> Stores **data being transferred** between CPU and memory.

- Can hold instructions or data
- Temporary transfer buffer

---

## 🟦 Instruction Register (IR)
> Holds the **current instruction being decoded or executed**.

---

## 🟦 Flag Register (Status Register)
> Stores **execution state and arithmetic results**.

Common flags:
- Zero (Z)
- Carry (C)
- Overflow (V)
- Sign (S)
- Parity (P)
- Interrupt Enable
- Privilege Mode

---

## 🟦 General-Purpose Registers (GPRs)
> Used to store **temporary values and operands**.

Examples:
- x86: `RAX`, `RBX`, `RCX`, `RDX`
- ARM: `X0–X30`

---

## 🟦 Stack Pointer (SP)
> Points to the **top of the stack**.

Used for:
- Function calls
- Local variables
- Return addresses

### Stack Addressing Mode
The stack pointer enables **stack-based addressing**:
```
PUSH value → SP moves
POP value → SP moves
```

---

## 🟦 Base Register (BR)
> Stores a **base address used for effective address calculation**.

Used to access:
- Arrays
- Structures
- Memory segments

---

# 3️⃣ Addressing Modes Using Registers

Addressing modes determine **how effective memory addresses are calculated**.

---

## 🔹 Displacement Addressing
```
Effective Address = Register + Offset
```
Used for structured data and arrays.

---

## 🔹 Relative Addressing
```
Effective Address = PC + Offset
```
Used for:
- Branches
- Loops
- Position-independent code

---

## 🔹 Base Register Addressing
```
Effective Address = Base Register + Offset
```
Used in:
- Stack frames
- Dynamic memory access

---

## 🔹 Stack Addressing
```
Effective Address = SP + Offset
```
Used for:
- Function calls
- Temporary storage

---

# 4️⃣ Why Registers Matter for Performance

✔ Reduce memory access  
✔ Speed up arithmetic operations  
✔ Enable fast function calls  
✔ Support efficient addressing  

> **More register usage = fewer RAM accesses = faster execution**

---

# 5️⃣ Registers in Real CPU Architectures

### x86-64
- `RAX`, `RBX`, `RCX`, `RDX`
- `RSP` (Stack Pointer)
- `RIP` (Program Counter)
- `RFLAGS`

### ARM64
- `X0–X30`
- `SP`, `PC`, `PSTATE`

---

# 6️⃣ Developer Takeaways

✔ Registers are the fastest storage in the CPU  
✔ PC controls execution flow  
✔ SP manages function call stacks  
✔ Base registers enable flexible memory access  
✔ Effective addressing reduces instruction complexity  

---

# 🧩 One-Line Mental Model

> **Registers are the CPU’s working memory for executing instructions efficiently.**
