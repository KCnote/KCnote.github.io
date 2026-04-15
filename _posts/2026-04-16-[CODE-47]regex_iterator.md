---
layout: post
title: "std::regex_iterator"
date: 2026-04-16 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::regex_iterator`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is an `std::regex_iterator`</b>

`std::regex_iterator` is an iterator used to **iterate over all regex matches in a string**.

Instead of finding just one match, it allows you to **loop through every match**

```text
string + regex → multiple matches → iterate one by one
```

```cpp
#include <regex>

std::regex pattern("...");
std::sregex_iterator it(begin, end, pattern);
std::sregex_iterator end;
```

#### Example

```cpp
#include <iostream>
#include <regex>

int main()
{
    std::string text = "abc 123 def 456";

    std::regex pattern("\\d+");  // match numbers

    std::sregex_iterator it(text.begin(), text.end(), pattern);
    std::sregex_iterator end;

    for (; it != end; ++it)
        std::cout << it->str() << "\n";
}
```

```
123
456
```

#### What Does Iterator Hold?

Each iterator element is:

```cpp
std::match_results<std::string::const_iterator>
```

##### ✔️ Access Data

```cpp
it->str();        // full match
it->position();   // position in string
it->length();     // length of match
```

When we use:
- log parsing
- extracting numbers
- parsing structured text
- simple DSL parsing
- config parsing

## <b>2. Capture Groups</b>

#### Example

```cpp
#include <iostream>
#include <regex>

int main()
{
    std::string text = "Name: Kang Age: 30";

    std::regex pattern("(\\w+):\\s*(\\w+)");

    std::sregex_iterator it(text.begin(), text.end(), pattern);
    std::sregex_iterator end;

    for (; it != end; ++it)
    {
        std::cout << "Key: " << (*it)[1] << "\n";
        std::cout << "Value: " << (*it)[2] << "\n";
    }
}
```

```
Key: Name
Value: Kang
Key: Age
Value: 30
```

```
(\\w+):\\s*(\\w+)

1. it[0]: (\\w+):\\s*(\\w+)
2. it[1]: first parentheses (\\w+)
2. it[2]: second parentheses (\\w+)
```

| Type | Description |
|------|------------|
| `std::sregex_iterator` | string iterator |
| `std::cregex_iterator` | C-string iterator |
| `std::regex_iterator` | generic template |

## <b>3. Performance Considerations</b>

#### Regex is Expensive

- Uses complex pattern engine
- Backtracking possible
- Not cache-friendly

| Method | Speed |
|--------|------|
| manual parsing | 🔥 fastest |
| string functions | ⚡ fast |
| regex_iterator | 🐢 slow |

#### <b>3-1. Optimization Tips</b>

- compile regex once

```cpp
std::regex pattern("...");
```

- avoid inside loops

## <b>4. `regex_iterator` vs `regex_search`</b>

#### `regex_search`

```cpp
std::smatch match;
std::regex_search(text, match, pattern);
```

```cpp
std::string text = "abc 123 def 456";
std::regex pattern("\\d+");

std::smatch match;
std::regex_search(text, match, pattern);

std::cout << match.str();
```

finds **first match only**

#### `regex_iterator`

```cpp
for (...) { ... }
```

```cpp
std::string text = "abc 123 def 456";
std::regex pattern("\\d+");

std::sregex_iterator it(text.begin(), text.end(), pattern);
std::sregex_iterator end;

for (; it != end; ++it)
    std::cout << it->str() << "\n";
```

finds **all matches**

## <b>5. Common Mistakes</b>

#### ❌ Recreating regex inside loop

```cpp
for (...)
{
    std::regex pattern("...");  // ❌ slow
}
```

#### ❌ Forgetting escape

```cpp
"\d+"   // ❌ wrong
"\\d+"  // ✅ correct
```

#### ❌ Assuming fast performance

👉 regex is **not for high-frequency loops**

## <b>6. Example</b>

```cpp
#include <iostream>
#include <regex>

int main()
{
    std::string text = "Contact: test@mail.com or hello@world.com";

    std::regex pattern("\\w+@\\w+\\.\\w+");

    std::sregex_iterator it(text.begin(), text.end(), pattern);
    std::sregex_iterator end;

    for (; it != end; ++it)
        std::cout << it->str() << "\n";
}
```