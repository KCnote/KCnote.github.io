---
layout: post
title: "Heap Sort"
date: 2025-12-02 12:00:00 +0900
author: kang
categories: [algorithm, sort]
tags: [structure, algorithm, heap, sort]
pin: false
math: true
mermaid: true
---

# Heap Sort
- Base on heap structure, making sorting all elements of array. It is very simple principle.

#### Let's remember how to pick a max or min value. If we find root value during all of times, It means the result will be sorted.

#### Sorting
```
[5, 3, 8, 4, 1, 2]
 + Visualization

[1] Make Max Hip
        5
      /    \
    3       8
   / \     /  
  4   1   2  

[2] Extract root
        8
      /    \
    4       5
   / \     /  
  3   1   2  

[3] Make Max Hip -> Now, Heap size is changed 6 to 5.
        2
      /    \
    4       5
   / \       
  3   1   8  

[4] Extract root
        5
      /    \
    4       2
   / \       
  3   1   8  

[5] Make Max Hip -> Now, Heap size is changed 5 to 4.
        1
      /    \
    4       2
   /        
  3   5   8  

[6] Extract root
        4
      /    \
    3       2
   /        
  1   5   8  

[7] Make Max Hip -> Now, Heap size is changed 4 to 3.
        1
      /    \
    3       2
           
  4   5   8  

[8] Extract root
        3
      /    \
    1       2
           
  4   5   8  

[9] Make Max Hip -> Now, Heap size is changed 3 to 2.
        2
      /    
    1       3
           
  4   5   8  

[10] Extract root
        1
          
    2       3
           
  4   5   8  

[9] Make Max Hip -> Now, Heap size is changed 2 to 1. -> FINISH
        1
          
    2       3
           
  4   5   8  



 - Result of Array

 [1, 2, 3, 4, 5, 8]

```

#### Why we use heap sorting

  - Very stable performance of time complex O(n $\log{n}$)
  - There's no extra memory for sorting
  - If we need to prevent to worst case
