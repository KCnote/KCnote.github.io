---
layout: post
title: "Mistake Coding"
date: 2026-03-07 00:00:00 +0900
author: kang
categories: [CODE, CODE - Practice]
tags: [CODE, CODE - Practice]
pin: false
math: true
mermaid: true
---

# <b>Mistake Coding</b>
---
### <b>Prerequites</b>
    Basic knowledge of programming language

---
## <b>What is Mistake Coding</b>

I will post the mistake when we're programming. Someones are very easy but others are little bit hard sometimes. This post I don't care the level of programmer, just listing the easily mistake to be happened.

### 1. Parameter of pointer

Let's find the mistake of this. Please ignore the memory leak, just focusing the swap status

```cpp
void Fun(int* pI32Input)
{
    int* pI32New = new int;
    *pI32New = 2;

    std::swap(pI32Input, pI32New);

    return;
}

int main()
{
    int* pI32Test = new int;
    *pI32Test = 1;

    Fun(pI32Test);

    return 0;
}
```

After Fun function, there's no change but we hope to change the pI32Test pointer variable after the function.

The main problem is parameter of point is just copy variable and after exit the function, the copy variable memory is changed but the pI32Test is not changed because on function process, there's no variable about pI32Test.

If we want to change the pointer itself like that:

```cpp
void Fun(int** ppI32Input)
{
    int* pI32New = new int;
    *pI32New = 2;

    std::swap(*ppI32Input, pI32New);

    return;
}

int main()
{
    int* pI32Test = new int;
    *pI32Test = 1;

    Fun(&pI32Test);

    return 0;
}
```

If we want to change the dereferenced value of the pointer itself like that:

```cpp
void Fun(int** ppI32Input)
{
    int* pI32New = new int;
    *pI32New = 2;

    std::swap(*(*ppI32Input), *pI32New);

    return;
}

int main()
{
    int* pI32Test = new int;
    *pI32Test = 1;

    Fun(&pI32Test);

    return 0;
}
```

### 2. ....
