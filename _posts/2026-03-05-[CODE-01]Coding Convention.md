---
layout: post
title: "Coding Convention"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Coding Convention</b>
---
### <b>Coding Convention</b>
    Data Type

---
## <b>What is Coding Convention</b>

### 1. What is Coding Convention?

Coding Convention is kind of appointment of team about code variable name. We sometimes see the unacceptable name of code or confuse us owing to name.

It depends on company, team and peers and so on. There no general rule for code convention. But The best of convention is if someone who see code at the first time know the flow of code, it is very important and critial part of convention.

But I want to show my convention and why is it good convention for someone who do with our projects.

### 2. The standard of coding convention

    <type-prefix><VariableName>

When we distinguish the data type, count of bit and so on. The main point is when we understand the code flow without data type.

> Rule 1. How many bite it has.

> Rule 2. What the type is difference of another types.

> Rule 3. Multi consequence type is using like that


### 3. List of Coding Convention

> 3-1. Integer Types

| Type | Prefix | Example |
|-----|------|------|
| signed char | `i8` | `i8Value` |
| short | `i16` | `i16Index` |
| int | `i32` | `i32Count` |
| long long | `i64` | `i64Total` |
| unsigned char | `u8` | `u8Mask` |
| unsigned short | `u16` | `u16Size` |
| unsigned int | `u32` | `u32Flags` |
| unsigned long long | `u64` | `u64Timestamp` |

> 3-2. Character Types

| Type | Prefix | Example |
|-----|------|------|
| wchar_t | `wc` | `wcName` |

> 3-3. Platform Dependent Types

```cpp
#ifdef _WIN64
    typedef unsigned __int64 size_t;
    typedef __int64          ptrdiff_t;
    typedef __int64          intptr_t;
#else
    typedef unsigned int     size_t;
    typedef int              ptrdiff_t;
    typedef int              intptr_t;
#endif
````

| Type      | Prefix | Example       |
| --------- | ------ | ------------- |
| size_t    | `s`    | `sLength`     |
| ptrdiff_t | `pdf`  | `pdfOffset`   |
| intptr_t  | `iptr` | `iptrAddress` |

> 3-4. User Defined Types

| Type          | Prefix | Example           |
| ------------- | ------ | ----------------- |
| class         | `c`    | `cImageProcessor` |
| struct        | `s`    | `sPoint2D`        |
| enum          | `e`    | `eState`          |
| template type | `t`    | `tValue`          |

> 3-5. String Types

| Type   | Prefix | Example   |
| ------ | ------ | --------- |
| string | `str`  | `strPath` |

> 3-6. Pointer Types

| Type             | Prefix | Example      |
| ---------------- | ------ | ------------ |
| pointer          | `p`    | `pData`      |
| double pointer   | `p2`   | `p2Buffer`   |
| triple pointer   | `p3`   | `p3Matrix`   |
| function pointer | `fp`   | `fpCallback` |

> 3-7. Iteration Types

| Type     | Prefix | Example    |
| -------- | ------ | ---------- |
| iterator | `iter` | `iterNode` |

> 3-8. Container Types

| Type           | Prefix | Example     |
| -------------- | ------ | ----------- |
| array          | `arr`  | `arrValues` |
| vector         | `vct`  | `vctPoints` |
| vector< vector> | `vct2` | `vct2Grid`  |

> 3-9. Multi-Dimensional Arrays

| Type      | Prefix     | Example          |
| --------- | ---------- | ---------------- |
| int[ ][ ]   | `arrI32`   | `arrI32Image`    |
| int**[ ][ ] | `arrP2I32` | `arrP2I32Tensor` |

> 3-10. Concurrency Types

| Type   | Prefix | Example    |
| ------ | ------ | ---------- |
| thread | `th`   | `thWorker` |
| mutex  | `mtx`  | `mtxLock`  |

> 3-11. Function Types

| Type            | Prefix | Example     |
| --------------- | ------ | ----------- |
| lambda function | `lmd`  | `lmdFilter` |

> 3-12. File Types

| Type  | Prefix | Example |
| ----- | ------ | ------- |
| FILE* | `fp`   | `fpLog` |

> 3-13. Data / Math Types

| Type   | Prefix | Example        |
| ------ | ------ | -------------- |
| node   | `node` | `nodeRoot`     |
| tensor | `ts`   | `tsFeature`    |
| matrix | `mat`  | `matTransform` |

