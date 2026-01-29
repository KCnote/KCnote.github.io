---
layout: post
title: "Instruction Structure & Addressing Modes"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [Computer Structure, Miscellaneous]
tags: [Computer Structure, Miscellaneous, Instruction, Addressing Modes]
pin: false
math: true
mermaid: true

---

# 🧠 Instruction Structure & Addressing Modes

> A clear guide to **instruction format**, **opcode**, **operands**, **effective address**, and **addressing modes**.

---

# 1️⃣ Instruction Structure

A machine instruction typically consists of:

## 🔹 1. Opcode (Operation Code)
Specifies **what operation to perform**.

## 🔹 2. Operand
Specifies **data** or **where the data is located**.

> In most real CPUs, operands usually store **addresses**, not raw data,  
> because **instruction bit‑width is limited**.

```
[ OPCODE | OPERAND / ADDRESS FIELD ]
```

---

# 2️⃣ Opcode Categories

## 🔹 Data Transfer
MOVE, LOAD, STORE, FETCH, PUSH, POP

## 🔹 Arithmetic & Logic
ADD, SUBTRACT, MULTIPLY, DIVIDE  
INCREMENT, DECREMENT  
AND, OR, NOT, COMPARE

## 🔹 Control Flow
JUMP, CONDITIONAL JUMP  
CALL, RETURN  
HALT

## 🔹 Input / Output
I/O READ, I/O WRITE, DEVICE CONTROL

---

# 3️⃣ Effective Address

> The **Effective Address (EA)** is the **actual memory location**  
> where the operand data is stored.

### Example
```
LOAD R1, [0x1000]
```
👉 Effective Address = `0x1000`

---

# 4️⃣ Addressing Modes

Addressing modes define **how the operand location is interpreted**.

---

## 4.1 Immediate Addressing Mode
> Operand contains the **actual constant value**.

```asm
MOV R1, #10      ; R1 = 10
ADD R2, R2, #5   ; R2 = R2 + 5
```
**C example:** `x = 10;`

Operand **contains the actual value**.

```asm
MOV R1, #10
```
✔ Fast  
❌ Limited by instruction size  

---

## 4.2 Direct Addressing Mode
> Operand contains a **fixed memory address**.

```asm
LOAD R1, [0x1000]   ; R1 = Memory[0x1000]
STORE [0x1000], R1  ; Memory[0x1000] = R1
```
**C example:** `x = global_var;`

Operand **contains a memory address**.

```asm
LOAD R1, [0x1000]
```
✔ Simple  
❌ Limited address range  

---

## 4.3 Indirect Addressing Mode
> Operand points to a **memory location that stores another address**.

```asm
LOAD R1, [[0x2000]]
```
If:
- Memory[0x2000] = 0x5000
- Memory[0x5000] = 42

Then:
- R1 = 42

**C example:** `value = **ptr;`

Operand points to **another address** that contains the real data address.

```asm
LOAD R1, [[0x2000]]
```
✔ Flexible  
❌ Slower (extra memory access)  

---

## 4.4 Register Addressing Mode
> Operand is stored in a **CPU register**.

```asm
ADD R1, R2   ; R1 = R1 + R2
MOV R3, R4   ; R3 = R4
```
**C example:** `a = b + c;`

Operand refers to a **register**.

```asm
ADD R1, R2
```
✔ Very fast  
✔ No memory access  

---

## 4.5 Register Indirect Addressing Mode
> Register stores a **memory address**.

```asm
LOAD R1, [R2]   ; R1 = Memory[R2]
STORE [R2], R1  ; Memory[R2] = R1
```
If R2 = 0x4000 → R1 = Memory[0x4000]

**C example:** `value = *ptr;`

Register contains the **memory address** of the operand.

```asm
LOAD R1, [R2]
```
✔ Powerful  
✔ Used for arrays & pointers  

---

# 5️⃣ Summary Table

| Mode | Operand Contains | Example | Speed |
|------|-----------------|--------|------|
| Immediate | Value | `MOV R1, #5` | Fast |
| Direct | Address | `LOAD R1, [0x1000]` | Medium |
| Indirect | Address of Address | `LOAD R1, [[0x2000]]` | Slow |
| Register | Register Name | `ADD R1, R2` | Fastest |
| Register Indirect | Address in Register | `LOAD R1, [R2]` | Fast |

---

# 6️⃣ Developer Takeaways

✔ Most operands store **addresses**, not raw values  
✔ Effective Address determines **real data location**  
✔ Register modes are fastest  
✔ Indirect modes trade speed for flexibility  

---

# 🧩 One‑Line Mental Model

> **Addressing modes describe how the CPU finds the actual operand data.**

