---
layout: post
title: "Regular Expression"
date: 2026-04-16 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Regular Expressions</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is a Regular Expressions</b>

A **Regular Expression (Regex)** is a pattern used to **match, search, or extract text**. Instead of writing complex parsing logic, you describe patterns:

```text
"\\d+" → one or more digits
```

## <b>2. Why Use Regex?</b>

- extract data (emails, numbers, logs)
- validate input (passwords, IDs)
- search patterns
- text processing

Use when:
- pattern is complex
- structure is unknown
- flexibility needed

Avoid when:
- simple parsing
- performance critical
- tight loops

The Regex processing is expensive

## <b>3. How to Use Regex?</b>

#### ✔️ Literals

```text
abc   → matches "abc"
```

#### ✔️ Metacharacters

| Symbol | Meaning |
|--------|--------|
| `.` | any character |
| `\d` | digit |
| `\w` | word character |
| `\s` | whitespace |

#### ✔️ Quantifiers

| Pattern | Meaning |
|--------|--------|
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 |
| `{n}` | exactly n |
| `{n,m}` | between n and m |

#### ✔️ Examples

```text
\d+      → 123, 456
\w+      → hello, test123
a.*b     → a___b
```

#### Character Classes

```text
[abc]    → a or b or c
[^abc]   → NOT a, b, c
[a-z]    → lowercase letters
[A-Z]    → uppercase letters
[0-9]    → digits
```

#### Anchors

```text
^   → start of string
$   → end of string
```

#### ✔️ Example

```text
^abc   → starts with "abc"
xyz$   → ends with "xyz"
```

## <b>4. Groups and Captures</b>

#### ✔️ Grouping

```text
(abc)
```

#### ✔️ Capture Example

```text
(\w+)=(\d+)
```

Matches:

```text
age=30
```

- Group 1 → age  
- Group 2 → 30  

## <b>5. Miscellaneous</b>

#### Alternation

```text
cat|dog
```

matches "cat" or "dog"

#### Greedy vs Lazy

##### ✔️ Greedy (default)

```text
a.*b
```

matches longest possible

```
a123b456b → a [123b456] b
```

##### ✔️ Lazy

```text
a.*?b
```

matches shortest possible

```
a123b456b → a [123] b
```

## <b>6. Related functions in C++</b>

| Function | Description |
|----------|------------|
| `regex_match` | full match |
| `regex_search` | partial match |
| `regex_replace` | replace |
| `regex_iterator` | iterate matches |

## <b>7. Useful Patterns</b>

#### ✔️ Email

```text
\w+@\w+\.\w+

\\w+@\\w+\\.\\w+ (c++)
```

#### ✔️ Number

```text
\d+
```

#### ✔️ Date (YYYY-MM-DD)

```text
\d{4}-\d{2}-\d{2}
```

#### ✔️ Phone

```text
\d{3}-\d{3}-\d{4}
```
