---
layout: post
title: "Components - I/O Device"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [CS Fundamentals, CS Fundamentals - Computer Structure]
tags: [Computer Structure, Components, I/O]
pin: false
math: true
mermaid: true

---

# 🧠 Device Controllers, Drivers, and I/O Mechanisms

> A structured explanation of **how CPUs communicate with I/O devices**, why **device controllers and drivers exist**, and how **programmed I/O, interrupts, and DMA** work.

---

# 1️⃣ Why I/O Devices Need Controllers

Communicating directly with I/O devices is difficult because:

1. There are **many different device types**
2. **CPU & memory are fast**, but **I/O devices are relatively slow**
3. Devices require **special protocols, timing, and error handling**

### Solution → **Device Controller**
A **device controller** acts as a **hardware mediator** between:
- CPU
- Memory
- I/O devices

### Key Roles
✔ Translate CPU commands into device-specific operations  
✔ Detect & report errors  
✔ Buffer data to handle **speed mismatches**  
✔ Reduce CPU workload  

---

# 2️⃣ Device Controller Architecture (Hardware)

A device controller connects to the **system bus** and exposes registers:

## Controller Registers

| Register | Purpose |
|---------|--------|
| **Data Register** | Stores data being transferred |
| **Status Register** | Indicates device state (ready/busy/error) |
| **Control Register** | Receives commands from CPU |

```
CPU ↔ Controller Registers ↔ I/O Device
```

> The CPU interacts with devices by **reading and writing controller registers**.

---

# 3️⃣ Device Driver (Software Layer)

A **device driver** is a **software program** that:
- Controls the device controller
- Translates OS requests into hardware commands
- Handles device events & errors

### Hardware vs Software Roles

| Component | Role |
|---------|------|
| Device Controller | Hardware logic |
| Device Driver | Software control layer |

---

## 4.1️⃣ Programmed I/O (Polling)

> The CPU actively **checks device status and transfers data itself**.

### How It Works (Disk Backup Example)

1. CPU writes **WRITE command** to controller control register  
2. Controller checks disk readiness → updates **status register**  
3. CPU repeatedly polls status register (**busy waiting**)  
4. When ready, CPU writes data to **data register**  
5. Repeat until complete  

### Pros
✔ Simple implementation  

### Cons
❌ CPU wasted polling  
❌ Slow for high-volume I/O  

---

## 4.2️⃣ Memory-Mapped I/O vs Isolated I/O

### Memory-Mapped I/O
- I/O registers share the **same address space as memory**
- CPU uses **normal load/store instructions**
- Reduces special I/O instructions
- **Tradeoff**: reduces available memory address range

> Why memory space shrinks?  
> Because **part of the address range is reserved for device registers**.

---

### Isolated I/O
- Separate **I/O address space**
- Uses **special IN/OUT instructions**
- Memory address space remains untouched

---

## 4.3️⃣ Interrupt-Driven I/O

> Devices **notify the CPU only when work is complete**.

### How It Works
1. CPU starts I/O and continues executing other tasks
2. Device finishes work → sends **hardware interrupt**
3. CPU suspends current task → runs **Interrupt Service Routine (ISR)**

### Handling Multiple Interrupts
- Not handled strictly in arrival order
- Priority-based handling
- Managed by **PIC / APIC / GIC**
- **NMI** has highest priority

### Pros
✔ Efficient CPU utilization  
### Cons
❌ Interrupt overhead  

---

## 4.4️⃣ DMA (Direct Memory Access)

> Transfers data **between device and memory without CPU involvement**.

### Traditional Flow
```
Device → CPU → Memory
```

### DMA Flow
```
Device → Memory (Direct)
```

### Benefits
✔ Frees CPU from data copying  
✔ Faster large transfers  
✔ Ideal for disks, GPUs, NICs  

---

# 5️⃣ I/O Bus — Reducing System Bus Traffic

If all controllers used the **system bus**, DMA would **consume bus bandwidth**.

### Solution → **Dedicated I/O Bus**
- Keeps system bus available for CPU & memory
- Improves scalability and throughput

Examples:
- PCIe
- USB
- SATA

---

# 6️⃣ CPU–I/O Communication Summary

| Method | CPU Involvement | Efficiency |
|------|-----------------|-----------|
| Programmed I/O | High | Low |
| Interrupt I/O | Medium | Medium |
| DMA | Low | High |

---

# 🎯 Developer Takeaways

✔ Controllers buffer & translate device communication  
✔ Drivers abstract hardware differences  
✔ Polling wastes CPU cycles  
✔ Interrupts improve responsiveness  
✔ DMA maximizes throughput  
✔ Dedicated I/O buses reduce contention  

---

# 🧩 One-Line Mental Model

> **Device controllers handle hardware complexity, while drivers let the OS control devices efficiently.**