---
layout: post
title: "Slide Window"
date: 2025-12-08 12:00:00 +0900
author: kang
categories: [algorithm, pattern]
tags: [algorithm, pattern, technique, slide window]
pin: false
math: true
mermaid: true
---

# Slide Window
- When we meet continuous range for sum and so on, for minimizing cost we reuse the sections and use window size that sometimes it is fixed size or variable size. The main point is we should think what condition they have and how to define the window style. The style is not fixed but it is very variable to problem we have to solve.

#### Fixed-size Window
```
[2, 1, 5, 1, 3, 2] and max value in 3 zone on array.

[2 1 5] -> [1 5 1] -> [5 1 3] -> [1 3 2]

when from [2 1 5] to [1 5 1], just using minus front (2), and plus back (1). If we use like this window, we can use just minimzined cost not use all of values.

All of values calculated with window -> O(n)
```

#### Variable-size Window
```
The idea is same with fixed-size window. But the point is the size is variable. So we should use two pointer, like low and high pointer. According to condition from problem, It will be changed the size but the inner of window is satisfied with condition of problem.

- leetcode 03. longest substring without repeating characters
Given a string s, find the length of the longest substring without duplicate characters.

### Example.
 Input: s = "pwwkew"

 Output: 3

1. We don't know what optimized window size it is.
2. Start high pointer

low pointer == p (index 0) 
p -> pw -> pww ----> stop high pointer (index 1) -> max event = 2

renewal low pointer == w (index 2)
[pw]w -> [pw]wk -> [pw]wke -> [pw]wkew ----> stop high pointer (index 4) -> max event = 3

renewal low pointer == w (index 5)
[pwwke]w -> FINISH
```

And it related to with leetcode 03. longest substring without repeating characters