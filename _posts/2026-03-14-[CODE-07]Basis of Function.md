---
layout: post
title: "Basis of Function"
date: 2026-03-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Basis of Function</b>
---
### <b>Prerequites</b>
    C++

---
## <b>What is Basis of Function</b>

At the first time, i don't think about any design of basis. That time i just think if i need the member function, i made it. 

Regardless of function, i usually make main key for easy to use. I think the more we build the main key of function, the more to be stable design. 

### 1. Declaration: Basis of Function

```cpp
class CArithmetic
{
public:
    CArithmetic();
    CArithmetic(const CArithmetic& alg);
    CArithmetic(const CArithmetic* pAlg);
    virtual ~CArithmetic();

    CArithmetic& operator=(const CArithmetic& alg);

public:
    bool Clear();
    bool Assign(const CArithmetic& alg);
    bool Assign(const CArithmetic* pAlg);
};
```

### 2. Definition: Basis of Function

```cpp
CArithmetic::CArithmetic()
{
    Clear();
}

CArithmetic::CArithmetic(const CArithmetic& alg)
{
    Assign(alg);
}

CArithmetic::CArithmetic(const CArithmetic* pAlg)
{
    Assign(pAlg);
}

CArithmetic::~CArithmetic()
{
    Clear();
}

CArithmetic& CArithmetic::operator=(const CArithmetic& alg)
{
    Assign(alg);

    return *this;
}

bool CArithmetic::Clear()
{
    // implement

    return true;
}
bool CArithmetic::Assign(const CArithmetic& alg)
{
    Assign(&alg);
}

bool CArithmetic::Assign(const CArithmetic* pAlg)
{
    bool bOk = false;

    do
    {
        if(!pAlg)
            break;

        // implement

        bOK = true;
    }
    while(false);

    return bOK;
}
```