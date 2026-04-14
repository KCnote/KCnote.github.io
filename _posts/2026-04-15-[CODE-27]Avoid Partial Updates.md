---
layout: post
title: "Finalize Before Return: Avoid Partial Updates"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Finalize Before Return: Avoid Partial Updates in C++</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why This Matters</b>

    - Atomic Update Concept

> Always build the result completely, then update it once at the end.

When constructing or updating a result object:

- Partial updates can lead to **inconsistent states**
- Intermediate writes may cause **confusion or bugs**
- Repeated updates may introduce **unnecessary overhead**

> Do NOT update the output object step-by-step.  
> Build everything internally, and **commit once at the end**.

## <b>2. What the problem: Partial Updates</b>

#### <b>2-1. Example</b>

#### ❌ Bad Pattern

```cpp
void process(Result& out) 
{
    out.value = computeA();
    out.flag = checkSomething();

    if (!out.flag) 
        return;

    out.extra = computeB();  // partial update
}
```

- `out` is left in a **partially updated state**
- Hard to reason about correctness
- Risk of inconsistent data

#### Solution: Build Then Commit

##### ✔ Good Pattern

```cpp
void process(Result& out) 
{
    Result temp;

    temp.value = computeA();
    temp.flag = checkSomething();

    if (!temp.flag) 
        return;

    temp.extra = computeB();

    out = std::move(temp);  // commit once
}
```

```cpp
Result process() 
{
    Result res;

    res.value = computeA();
    res.flag = checkSomething();

    if (!res.flag) 
        return {};

    res.extra = computeB();

    return res;  // copy elision (C++17)
}
```

#### <b>2-2. Benefits</b>

- Output is always **valid or unchanged**
- Clear logic
- No partial state exposure
- Avoids multiple writes to output object
- Enables **copy elision / move optimization**
- Better cache locality (build locally, then commit)

##### ✔ DO

- Use local temporary object
- Validate before committing
- Return by value when possible
- Use `std::move` or rely on copy elision

##### ❌ DON'T

- Modify output object incrementally
- Leave partially valid state
- Mix computation and external state update
