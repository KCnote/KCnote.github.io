---
layout: post
title: "Compiled And Interpreted Languages"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [Computer Structure, Miscellaneous]
tags: [Computer Structure, Miscellaneous, Compile, Interpret, Languages, Assembly, Linker]
pin: false
math: true
mermaid: true

---

# 🧠 Compiled vs Interpreted Languages & C Compilation Process

> A clear developer‑oriented guide to **compiled languages, interpreted languages, Godbolt usage, and the full C compilation pipeline**.

---

# 1️⃣ Compiled Language vs Interpreted Language

## 🔹 Compiled Language
Source code is **translated into machine code before execution**.

### Examples
- C, C++, Rust, Go

### Characteristics
✔ Fast execution  
✔ Errors caught early  
❌ Recompile required after changes  

---

## 🔹 Interpreted Language
Code is **executed line‑by‑line at runtime**.

### Examples
- Python, JavaScript, Ruby

### Characteristics
✔ Faster development cycle  
✔ Easier debugging  
❌ Slower runtime  

---

# 2️⃣ Godbolt — Compiler Explorer

🔗 https://godbolt.org/

**Godbolt** lets you view how high‑level code translates into:
- Assembly
- Machine code
- Optimized output

### Why it matters
✔ Learn compiler optimizations  
✔ Compare architectures (x86 vs ARM)  
✔ Study performance at instruction level  

---

# 3️⃣ C Language Compilation Pipeline

```
Source (.c)
  ↓
Preprocessor (.i)
  ↓
Compiler (.s)
  ↓
Assembler (.o)
  ↓
Linker (Executable)
```

---

# 4️⃣ Step‑by‑Step Breakdown

## 🧩 Step 1 — Preprocessor

Handles:
- `#include` — import external libraries  
- Macro expansion (`#define`)  
- Conditional compilation (`#ifdef`)  

### Command
```bash
gcc -E file.c -o file.i
vi file.i
```

👉 Outputs expanded source file

---

## 🧩 Step 2 — Compiler

Converts **preprocessed C → Assembly**.

### Command
```bash
gcc -S file.i -o file.s
# or
gcc -S file.c -o file.s
vi file.s
```

👉 Optimizes code and emits assembly

---

## 🧩 Step 3 — Assembler

Converts **assembly → machine code**  
Produces an **object file (.o)**

### Command
```bash
gcc -c file.s -o file.o
xxd file.o | less
```

👉 Output contains relocatable machine code

---

## 🧩 Step 4 — Linker

Combines:
- Object files
- Libraries
- Startup code

Produces **final executable**

### Role
✔ Resolves symbol references  
✔ Merges sections  
✔ Links libc and runtime  

---

# 5️⃣ Real‑World Example

### Source
```c
int add(int a, int b) {
    return a + b;
}
```

### Flow
```
add.c → add.i → add.s → add.o → a.out
```

---

# 6️⃣ Developer Takeaways

✔ Compilers transform C into CPU‑specific machine code  
✔ Each compilation stage has a distinct responsibility  
✔ Godbolt is powerful for performance learning  
✔ Understanding this helps debugging, optimization, and build systems  

---

# 🧩 One‑Line Mental Model

> **Source code becomes executable through preprocessing, compilation, assembly, and linking.**

