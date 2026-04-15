---
layout: post
title: "std::forward"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::forward`</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is `std::forward`?</b>

`std::forward` is a utility used to **preserve the value category** of an argument.

That means it keeps whether an argument is:

- an **lvalue**
- or an **rvalue**

```cpp
#include <utility>
```

It is mainly used in **template code** with **forwarding references**.

Use it when:

- you are inside a template
- parameter is a forwarding reference: `T&&`
- you want to pass the argument onward while preserving its original category

## <b>2. Why Do We Need It?</b>

Consider this function:

```cpp
void process(int& x)
{
    std::cout << "lvalue\n";
}

void process(int&& x)
{
    std::cout << "rvalue\n";
}
```

Now:

```cpp
template<typename T>
void wrapper(T&& arg)
{
    process(arg);
}
```

At first glance, it looks fine.

But actually:

```cpp
int x = 10;
wrapper(x);   // lvalue
wrapper(10);  // rvalue?
```

#### Problem:

Inside `wrapper`, `arg` itself is always a **named variable**.
A named variable is always treated as an **lvalue**

So this:

```cpp
process(arg);
```

will always call:

```cpp
process(int&)
```

even if the original argument was an rvalue.

#### The Solution: `std::forward`

```cpp
template<typename T>
void wrapper(T&& arg)
{
    process(std::forward<T>(arg));
}
```

Now:

```cpp
int x = 10;
wrapper(x);   // lvalue
wrapper(10);  // rvalue
```

Correct overload is preserved

`std::forward<T>(arg)` conditionally casts `arg` to:

- `T&` if original argument was an lvalue
- `T&&` if original argument was an rvalue

In other words, it forwards the argument **exactly as it was received**

#### Forwarding Reference

`std::forward` is usually used with this pattern:

```cpp
template<typename T>
void func(T&& arg)
```

This `T&&` is called a:

- **forwarding reference**
- sometimes called **universal reference**

This only works in template type deduction contexts.

#### Example Without `std::forward`

```cpp
#include <iostream>
#include <utility>

void process(int& x)
{
    std::cout << "lvalue\n";
}

void process(int&& x)
{
    std::cout << "rvalue\n";
}

template<typename T>
void wrapper(T&& arg)
{
    process(arg);
}

int main()
{
    int x = 10;

    wrapper(x);   // lvalue
    wrapper(20);  // still lvalue ❌
}
```

The second call is wrong because `arg` is a named variable.

#### Example With `std::forward`

```cpp
#include <iostream>
#include <utility>

void process(int& x)
{
    std::cout << "lvalue\n";
}

void process(int&& x)
{
    std::cout << "rvalue\n";
}

template<typename T>
void wrapper(T&& arg)
{
    process(std::forward<T>(arg));
}

int main()
{
    int x = 10;

    wrapper(x);   // lvalue
    wrapper(20);  // rvalue ✅
}
```

#### `std::move` vs `std::forward`

This is the most important comparison.

##### ✔️ `std::move`

```cpp
std::move(x)
```

Always casts to **rvalue**

##### ✔️ `std::forward`

```cpp
std::forward<T>(x)
```

Conditionally preserves lvalue/rvalue nature

## <b>3. Common Mistakes</b>

#### ❌ Using `std::forward` outside forwarding context

```cpp
int x = 10;
std::forward<int>(x);  // meaningless / dangerous
```

`std::forward` is mainly for template forwarding references

#### ❌ Using `std::move` instead of `std::forward`

```cpp
template<typename T>
void wrapper(T&& arg)
{
    process(std::move(arg));  // ❌ forces rvalue
}
```

Problem:
- lvalues are also converted into rvalues
- may accidentally move from objects you should not move from

#### ❌ Forgetting that named rvalue references are lvalues

```cpp
int&& x = 10;
process(x);  // calls lvalue overload
```

Even though `x` is declared as `int&&`, the variable `x` itself is an lvalue because it has a name.

## <b>4. Performance Perspective</b>

`std::forward` itself is extremely cheap. It is basically a cast resolved at compile time.

#### Performance benefits:
- avoids unnecessary copies
- preserves move opportunities
- enables efficient generic code

#### Common use cases:
- wrapper functions
- factory functions
- emplace-style APIs
- generic libraries

Without `std::forward`, rvalues may degrade into lvalues, causing:
- extra copies
- loss of move semantics
- worse performance
