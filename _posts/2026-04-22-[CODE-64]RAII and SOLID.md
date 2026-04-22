---
layout: post
title: "RAII & SOLID — Core Design Principles in C++"
date: 2026-04-22 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>RAII & SOLID — Core Design Principles</b>

---

### <b>Prerequisites</b>

    - C++

---

## <b>1. RAII (Resource Acquisition Is Initialization)</b>

> RAII is a C++ design principle where resource allocation and deallocation are tied to object lifetime.

* Resource is acquired in constructor
* Resource is released in destructor

#### Main Key Concept

* resource tied to object lifetime
* constructor → acquire
* destructor → release
* core idiom in C++

#### Why It Matters

In C++, you manage resources manually:

* memory
* file handles
* mutex locks

##### ❌ Without RAII

```cpp
void process()
{
    FILE* f = fopen("file.txt", "r");

    if (!f) return;

    // ... do work

    fclose(f); // might be skipped if error occurs
}
```

* early return
* exception
* missing cleanup

→ resource leak

##### ✔ With RAII

```cpp
#include <fstream>

void process()
{
    std::ifstream f("file.txt");

    // automatically closed when f goes out of scope
}
```

Destructor guarantees cleanup ✔

#### RAII with Mutex

```cpp
std::mutex m;

void work()
{
    std::lock_guard<std::mutex> lock(m);

    // critical section
}
```

- lock acquired
- automatically released at scope end

#### Key Benefits

* exception safety
* no resource leaks
* cleaner code
* deterministic destruction

## <b>2. SOLID Principles</b>

> SOLID is a set of design principles that make software more maintainable, flexible, and scalable.

#### <b>2-1. S — Single Responsibility Principle (SRP)</b>

> A class should have only one reason to change.

#### ❌ Bad

```cpp
class Report
{
public:
    void generate();
    void saveToFile();
};
```

- generation + file I/O mixed

#### ✔ Good

```cpp
class ReportGenerator
{
public:
    void generate();
};

class ReportSaver
{
public:
    void save();
};
```

#### <b>2-2. O — Open/Closed Principle (OCP)</b>

> Open for extension, closed for modification.

##### ❌ Bad

```cpp
if (type == "A") ...
else if (type == "B") ...
```

##### ✔ Good

```cpp
class Shape
{
public:
    virtual double area() = 0;
};

class Circle : public Shape
{
public:
    double area() override { ... }
};
```

- add new types without modifying existing code

#### <b>2-3. L — Liskov Substitution Principle (LSP)</b>

> Derived classes must be replaceable for their base class.

#### ❌ Bad

```cpp
class Bird
{
public:
    virtual void fly();
};

class Penguin : public Bird
{
public:
    void fly() override { throw; }
};
```

- violates expectation

##### ✔ Good

```cpp
class Bird
{
};

class FlyingBird : public Bird
{
public:
    virtual void fly() = 0;
};

class Sparrow : public FlyingBird
{
public:
    void fly() override { /* OK */ }
};

class Penguin : public Bird
{
};
```

#### <b>2-4. I — Interface Segregation Principle (ISP)</b>

> Do not force clients to depend on unused interfaces.

##### ❌ Bad

```cpp
class Machine
{
public:
    virtual void print();
    virtual void scan();
};
```

##### ✔ Good

```cpp
class Printer
{
public:
    virtual void print();
};

class Scanner
{
public:
    virtual void scan();
};
```

#### <b>2-5. D — Dependency Inversion Principle (DIP)</b>

> Depend on abstractions, not concrete implementations.

##### ❌ Bad

```cpp
class Engine {};

class Car
{
    Engine engine;
};
```

##### ✔ Good

```cpp
class IEngine
{
public:
    virtual void start() = 0;
};

class GasEngine : public IEngine
{
public:
    void start() override
    {
        std::cout << "Gas engine start\n";
    }
};

class ElectricEngine : public IEngine
{
public:
    void start() override
    {
        std::cout << "Electric engine start\n";
    }
};

class Car
{
    IEngine* engine;

public:
    Car(IEngine* e) : engine(e) {}

    void start()
    {
        engine->start(); // virtual dispatch
    }
};
```

```cpp
int main()
{
    GasEngine gas;
    ElectricEngine electric;

    Car car1(&gas);
    Car car2(&electric);

    car1.start(); // Gas engine start
    car2.start(); // Electric engine start
}
```

## <b>RAII & SOLID together in Practice</b>

In real systems:

* RAII → safe resource handling
* SOLID → clean architecture

#### Example

```cpp
class FileProcessor
{
    std::ifstream file; // RAII

public:
    void process();     // SRP
};
```