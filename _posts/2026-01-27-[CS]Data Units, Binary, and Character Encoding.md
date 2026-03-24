---
layout: post
title: "Data Units, Binary, and Character Encoding"
date: 2026-01-27 00:00:00 +0900
author: kang
categories: [Computer Structure, Computer Structure - Miscellaneous]
tags: [Computer Structure, Miscellaneous, Data Units, Binary, Character Encoding, UTF, Decimal, ASCII, Byte Order]
pin: false
math: true
mermaid: true

---

# 🧠 Data Units, Binary, and Character Encoding

> A structured guide to **CPU data units**, **binary representation**, and **character encoding**,  
> focused on **how computers actually store and process data**.

---

# 1️⃣ Word — CPU Processing Unit

## What is a Word?
A **word** is the **natural data size a CPU can process in one operation**.

It is tied to:
- Register width
- ALU operation size
- CPU architecture (16-bit, 32-bit, 64-bit)

---

## Common Data Units

| Unit | Meaning | Typical Size |
|------|--------|--------------|
| **Half Word** | Half of a word | 16 bits |
| **Word** | CPU-native size | 32 or 64 bits |
| **Double Word (DWORD)** | Twice a word | 64 bits |
| **Quad Word (QWORD)** | Twice a double word | 128 bits (SIMD) |

📌 On modern systems, **word = 64 bits**.

---

## Why Word Size Matters
- Determines integer & pointer size
- Affects memory alignment
- Impacts CPU performance

> Misaligned memory access can slow execution.

---

# 2️⃣ Binary vs Decimal — Number Representation

## Binary (Base-2)
- Uses **0 and 1**
- Native format for **hardware logic**

Example:
```
Binary: 10000₂
Decimal: 16₁₀
```

## Decimal (Base-10)
- Human-friendly system
- Converted to binary internally

📌 CPUs **always operate on binary**.

---

# 3️⃣ Signed vs Unsigned Integers

| Type | Range (8-bit Example) |
|------|------------------------|
| Unsigned | 0 to 255 |
| Signed (Two’s Complement) | -128 to 127 |

Two’s complement allows **efficient negative number arithmetic**.

---

# 4️⃣ Character Sets — Mapping Characters to Numbers

A **character set** maps text characters to numeric **code points**.

Example:
```
'A' → 65
```

It defines:
- Supported characters
- Numeric values assigned to them

---

# 5️⃣ Encoding vs Decoding

## Encoding
Characters → Bytes

## Decoding
Bytes → Characters

```
Text → Encode → Bytes → Decode → Text
```

Encoding mismatch causes **mojibake (broken text)**.

---

# 6️⃣ ASCII — The First Standard

- **7-bit code points** (0–127)
- Supports English letters, digits, symbols

### Structure
- 7 bits = character
- 1 bit = optional parity bit

Example:
```
'A' = 65 = 1000001₂
```

Limitations:
❌ No multilingual support

---

# 7️⃣ Unicode — Universal Character Set

Unicode assigns a **unique code point** to every character worldwide.

Examples:
```
'A' → U+0041
'한' → U+D55C
'😀' → U+1F600
```

Unicode defines characters, **not storage format**.

---

# 8️⃣ Unicode Encodings — UTF-8, UTF-16, UTF-32

Unicode must be encoded into bytes.

## UTF-8
- **1–4 bytes**
- ASCII compatible
- Web & Linux standard

## UTF-16
- **2–4 bytes**
- Efficient for East Asian text
- Used in Windows, Java

## UTF-32
- **4 bytes fixed**
- Simple indexing
- Memory heavy

---

## Encoding Comparison

| Encoding | Byte Size | Pros | Cons |
|---------|----------|------|------|
| UTF-8 | 1–4 | Efficient, standard | Variable length |
| UTF-16 | 2–4 | Good for CJK | Surrogate pairs |
| UTF-32 | 4 | Simple | High memory use |

---

# 9️⃣ How CPUs See Text

> CPUs **do not understand characters** — only **binary numbers**.

Processing flow:
```
Characters → Code Points → Encoded Bytes → Binary → CPU
```

---

# 🔟 Endianness — Byte Order

| Type | Order |
|------|------|
| Little Endian | Least significant byte first |
| Big Endian | Most significant byte first |

Important for **networking & file formats**.

---

# 🎯 Developer Takeaways

✔ Word size impacts memory & speed  
✔ Binary is the CPU’s native format  
✔ Unicode is required for global text  
✔ UTF-8 is the modern default  
✔ Encoding mismatch breaks text  
✔ Endianness matters in low-level programming  

