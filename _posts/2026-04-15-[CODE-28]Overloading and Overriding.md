---
layout: post
title: "Function Overloading vs Overriding in C++"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Function Overloading vs Overriding in C++</b>

---

### <b>Prerequisites</b>


---

## <b>1. What are Overloading and Overriding</b>

C++ supports two important polymorphism concepts:

- **Overloading** → Same function name, different parameters  
- **Overriding** → Same function signature, different implementation (inheritance)

| Feature | Overloading | Overriding |
|--------|-----------|-----------|
| Scope | Same class | Inheritance |
| Function name | Same | Same |
| Parameters | Different | Same |
| Binding | Compile-time | Runtime |
| Keyword | None | `virtual`, `override` |

## <b>2. Overloading</b>

> Multiple functions with the **same name** but **different parameter lists**

##### ✔ Example

```cpp
int add(int a, int b) 
{
    return a + b;
}

double add(double a, double b) 
{
    return a + b;
}
```

Same name `add`, different parameter types  

- Happens at **compile-time** (static polymorphism)
- Based on:
  - Number of parameters
  - Type of parameters
- Return type alone is NOT enough

##### ❌ Invalid Overloading

```cpp
int foo(int a);
double foo(int a);  // ❌ error
```

- Resolved at compile-time → fast

## <b>3. Overriding</b>

> Derived class provides a **new implementation** of a base class virtual function

##### ✔ Example

```cpp
class Base 
{
public:
    virtual void print()
    {
        std::cout << "Base\n";
    }
};

class Derived : public Base 
{
public:
    void print() override 
    {
        std::cout << "Derived\n";
    }
};
```

##### ✔ Usage

```cpp
Base* obj = new Derived();
obj->print();  // Output: Derived
```

Runtime polymorphism  

- Requires `virtual` function
- Happens at **runtime**
- Uses **vtable (virtual table)**
- Uses vtable lookup → small runtime cost
