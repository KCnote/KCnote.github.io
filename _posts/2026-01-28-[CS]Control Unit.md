---
layout: post
title: "Control Unit"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [Computer Structure, Components]
tags: [Computer Structure, Components, CPU, Control Unit]
pin: false
math: true
mermaid: true

---

# 🎛 Control Unit (CU) — How the CPU Orchestrates Execution

> A clear explanation of the **Control Unit (CU)**, its role in instruction execution, clock timing, registers, and internal/external control signals.

---

# 1️⃣ What Is the Control Unit?

The **Control Unit (CU)** is the part of the CPU that **directs and coordinates all operations**.

> It does **not compute data** — it **controls when and how components act**.

### Core Responsibilities
✔ Fetch instructions  
✔ Decode instructions  
✔ Generate control signals  
✔ Coordinate ALU, registers, memory, and I/O  

---

# 2️⃣ Clock — The CPU Timekeeper

## What Is a Clock?
The **clock** provides a **timing signal** that synchronizes all CPU operations.

### Why It Matters
- Defines **when operations start and finish**
- Controls **instruction pacing**
- Higher clock speed = **more cycles per second**

```
1 Clock Cycle = 1 Tick of CPU Time
```

> The CU uses clock cycles to **sequence operations step-by-step**.

---

# 3️⃣ Instruction Register (IR)

The **Instruction Register** stores the **current instruction being decoded and executed**.

### Role in Execution Flow
```
Fetch → IR → Decode → Execute
```

The CU reads the IR to determine:
- Operation type
- Operand locations
- Required control signals

---

# 4️⃣ Flag Register (Status Register)

Stores **status and condition flags** updated by the ALU.

### Common Flags
| Flag | Meaning |
|------|--------|
| Zero (Z) | Result = 0 |
| Carry (C) | Carry out |
| Overflow (V) | Signed overflow |
| Sign (S) | Negative result |
| Interrupt Enable | Interrupt allowed |
| Privilege Mode | Kernel vs User |

> The CU uses flags to make **branching decisions**.

---

# 5️⃣ Control Signals — Inside the CPU

> Control signals coordinate **internal CPU components**.

### Examples
- Select ALU operation (ADD, SUB, AND)
- Enable register write
- Select input multiplexers
- Advance Program Counter (PC)

```
CU → Registers
CU → ALU
CU → Pipeline Stages
```

---

# 6️⃣ Control Signals — Outside the CPU

> The CU also controls **memory and I/O devices**.

### External Signals Examples
✔ Memory Read / Write  
✔ I/O Read / Write  
✔ Interrupt Acknowledge  
✔ Bus Request / Grant  

```
CU → Memory Controller
CU → Device Controllers
```

---

# 7️⃣ How the Control Unit Executes an Instruction

```
1. Fetch instruction from memory
2. Load into Instruction Register (IR)
3. Decode instruction
4. Generate control signals
5. Execute ALU / Memory / I/O operations
6. Update flags & Program Counter
```

---

# 8️⃣ Hardwired Control vs Microprogrammed Control

## Hardwired Control
✔ Faster  
❌ Hard to modify  

## Microprogrammed Control
✔ Flexible  
❌ Slightly slower  

---

# 9️⃣ Developer Takeaways

✔ The CU is the CPU’s **orchestra conductor**  
✔ Clock cycles define execution timing  
✔ Flags guide conditional branches  
✔ Control signals drive every CPU action  
✔ CU design impacts CPU performance  

---

# 🧩 One-Line Mental Model

> **The Control Unit tells every part of the CPU what to do and when to do it.**
