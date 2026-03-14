---
layout: post
title: "Encapsulation"
date: 2026-03-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Practice]
tags: [CODE, CODE - Practice]
pin: false
math: true
mermaid: true
---

# <b>Encapsulation</b>
---
### <b>Prerequites</b>
    C++

---
## <b>What is Encapsulation</b>

If we make program codes that is just for me or public, it is not needed to hide the code. But in commerical industry, we usually use our skill that we should hide for producing to another companies. So we should keep our skill but deploy the program to customers.

So we should design our code to encapsulate the program and provide APIs while preventing external access to internal technical details.

The methods of desgin we can encapuslate are many different approaches. In this post, separating the classes to control what is hidden and what is exposed.

### 1. Internal class and Public class

First of all, the public class is for public and internal class is for private limited permission. When we make a class, making two class included in public and internal.

### 2. Example of Internal class and Public class

Let's see the example I think it is better than any sentences.

    Without Encapsulation
```cpp
# COperator header
class COperator
{
...

public:
    bool Operate();

};

# COperator cpp
bool COperator::Operate()
{
    ...
    return a + b;
}
```


    With Encapsulation
```cpp
# COperator header
class COperator
{
...

public:
    bool Operate();

private:
    CInternalOperator* m_pInternal;
};

# CInternalOperator header
class CInternalOperator
{
...

public:
    bool Operate();
};

# COperator cpp
bool COperator::Operate()
{
    return m_pInternal->Operate();
}

# CInternalOperator cpp
bool CInternalOperator::Operate()
{
    return a+b;
}
```

This separating between classes is to be much more easy to program the internal code without having to think about security. And other advantage is the API class is more focus on readability.

For clients, for developers it is very simple but good design.s 