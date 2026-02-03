---
layout: post
title: "Bitwise Operations in Image Processing"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image Processing, Bitwise]
tags: [Computer Vision, Image Processing, Bitwise, AND, OR, XOR, NOT, NAND, NOR, Masking, Binary Image]
pin: false
math: true
mermaid: true
---

## 🔢 Why Bitwise Operations Matter in Image Processing

Bitwise operations work **at the bit level**, not on numeric magnitude.
They are fundamental for:

- Binary image processing
- Mask-based ROI extraction
- Fast logical filtering
- Segmentation pipelines
- Hardware-friendly (SIMD, FPGA, ISP)

Pixels are usually **8-bit unsigned integers**:

```
Pixel = 173 (decimal)
Binary = 10101101
```

---

## 🔲 AND Operation (Masking)

### Definition
$$
A \land B
$$

### Binary Behavior
| A | B | Result |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

### Example
```
Pixel = 11001010
Mask  = 11110000
AND   = 11000000
```

### Image Processing Usage
- ROI masking
- Background removal
- Applying segmentation results

### Pros / Cons
**Pros**
- Extremely fast
- Deterministic
- No floating-point error

**Cons**
- Binary only
- No intensity blending

---

## 🔳 OR Operation (Merge)

### Definition
$$
A \lor B
$$

### Binary Behavior
| A | B | Result |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

### Example
```
A = 10100000
B = 00001111
OR = 10101111
```

### Image Processing Usage
- Merge binary objects
- Combine multiple masks

### Pros / Cons
**Pros**
- Simple logical union
- Useful for mask fusion

**Cons**
- Cannot represent dominance or priority

---

## ❌ XOR Operation (Exclusive OR)

### Definition
$$
A \oplus B
$$

### Binary Behavior
| A | B | Result |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Example
```
A   = 11110000
B   = 11001100
XOR = 00111100
```

### Image Processing Usage
- Change detection
- Motion highlighting
- Debugging segmentation errors

### Pros / Cons
**Pros**
- Highlights differences only

**Cons**
- Very sensitive to noise

---

## 🔁 NOT Operation (Inversion)

### Definition
$$
\lnot A
$$

### Example
```
A   = 00010101
NOT = 11101010
```

### Image Processing Usage
- Binary inversion
- Foreground / background swap

### Pros / Cons
**Pros**
- Cheapest operation
- Simple logic inversion

**Cons**
- Meaningless for grayscale semantics

---

## 🚫 NAND Operation

### Definition
$$
\lnot (A \land B)
$$

### Binary Behavior
| A | B | NAND |
|---|---|------|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Image Processing Usage
- Rare directly
- Used implicitly in hardware logic
- Conditional exclusion masks

### Pros / Cons
**Pros**
- Functionally complete logic gate

**Cons**
- Low readability in software

---

## 🚫 NOR Operation

### Definition
$$
\lnot (A \lor B)
$$

### Binary Behavior
| A | B | NOR |
|---|---|-----|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

### Image Processing Usage
- Inverse-union masking
- Special-case logic filters

### Pros / Cons
**Pros**
- Clean exclusion logic

**Cons**
- Rare in high-level CV code

---

## ⚖️ Operation Comparison

| Operation | Purpose | Typical Use |
|---------|--------|-------------|
| AND | Intersection | ROI masking |
| OR | Union | Mask merge |
| XOR | Difference | Change detection |
| NOT | Inversion | Binary flip |
| NAND | NOT AND | Hardware logic |
| NOR | NOT OR | Exclusion logic |

---

## 🧠 Practical Insight

- **AND + NOT** → Mask subtraction
- **OR** → Mask accumulation
- **XOR** → Change visualization
- **NAND / NOR** → Mostly hardware / theoretical

💡 In real-world CV:
> AND, OR, XOR, NOT cover **99%** of use cases.

---

## 🚀 Takeaway

Bitwise operations:
- Are faster than arithmetic
- Are deterministic
- Scale perfectly to SIMD & hardware

Mastering them gives you **clean, fast, and robust pipelines**.
