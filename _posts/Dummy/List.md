---
layout: post
title: "CODE"
date: 2026-03-05 00:00:00 +0900
author: kang
categories: [plan, improvement for me]
tags: [plan]
pin: false
math: true
mermaid: true
---

Stable

0. coding convention
1. do~while
2. independent functions (Init, optim, execute(preprocess, cal ..))
3. class, internal
4. NEVER make duplicate same functions, constructor
5. Initialization of parameters
6. avoid void return
7. automatically paramters fun(aim = 4)
8. constructor, destrcutor (little bit differnece with init, clear fun)
9. common ultilies class
10. divide function Execute { Init, optim, execute, ....} - Add/Subtract
11. 
#ifdef USE_MINUS
#define OP -
#else
#define OP +
#endif
12. Minimize Conditional Complexity
13. Explicit Input and Output
14. operator=, standard setting (constructor,,)


optimization
1. point parameters
2. loop unrolling
3. for loop minimized, release the code. (CONV)
4. SIMD
5. precalcuated table (reference cost Vs cacluation cost)
6. on add operation, paste method in same image , 4 directions
7. double pointer -> pointer (table reference)
8. dynmaic static, openMp
 


etc.
1. as parameters of pointer (new allocate)
2. new vs malloc

readable
1. with { }, local variable limited
2. #define 
3. without comments, variable name
4. template #define, reduce copy code and easily complie


parallel
1. intergral
2. divide work unit
3. local value
