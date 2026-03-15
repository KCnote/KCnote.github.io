---
layout: post
title: "Similar Function, But long code"
date: 2026-03-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Similar Function, But long code</b>
---
### <b>Prerequites</b>
    C++

---
## <b>What is Similar Function, But long code</b>

If we make a simple class, but the code line is so long over 10K. And that time, we should make a new simple class. The only difference of between classes is former class is add operation and later class is minus operation. But there are so many code lines because there're initilization parameters, make region and optimizing table and so on but calculation is difference only + or -.

If we want to make two class, we should copy and paste from add class to minus class. However it is sometimes time-consuming or waste of effort.

### 1. How to deal with this case - 1 

```cpp
// macro.h
#ifdef USE_MINUS
#define OP -
#else
#define OP +
#endif

// add.h
class CAdd
{
public:
    int Operation();
};

// minus.h
class CMinus
{
public:
    int Operation();
};

// add.cpp
#include "macro.h"
#include "c1.h"

int CAdd::Operation()
{
    return 1 OP 2;
}

// minus.cpp
#define USE_MINUS
#include "macro.h"
#include "c2.h"

int CMinus::Operation()
{
    return 1 OP 2;
}

// main.cpp
#include "add.h"
#include "minus.h"
#include <iostream>

int main()
{
    CAdd cAdd;
    CMinus cMinus;

    int i32Result1 = cAdd.Operation();
    int i32Result2 = cMinus.Operation();

    std::cout << i32Result1 << '\n';
    std::cout << i32Result2 << '\n';

    return 0;
}
```

The code is same, add.h, cpp and minus.h, cpp but the only difference is the setting of #define. 
It means using #define is good method of making difference.

### 2. How to deal with this case - 2 

Just use macro, but it is not to good at debugging later, so if we want to adapt to class, it is not good, but just for understanding design to minimize code copy and paste.

```cpp
#define DEFINE_OPERATION_CLASS(CLASSNAME, OPERATOR) \
class CLASSNAME \
{ \
public: \
    int Operation() \
    { \
        return 1 OPERATOR 2; \
    } \
};

DEFINE_OPERATION_CLASS(CAdd, +)
DEFINE_OPERATION_CLASS(CMinus, -)

//main.cpp
CAdd add;
CMinus minus;

add.Operation();   // 3
minus.Operation(); // -1
```