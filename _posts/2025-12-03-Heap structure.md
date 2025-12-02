---
layout: post
title: "Heap Structure"
date: 2025-12-02 12:00:00 +0900
author: kang
categories: [structure, heap]
tags: [structure, algorithm, heap, sort]
pin: false
math: true
mermaid: true
---

# Heap structure
- One of binary tree, there are rules of making heap structure. For explaining this, visialization seems like tree but in real we use 1D array. There has relationship of parent and child it depend on Max-Heap or Min-Heap. And It is specialized about finding MAX/MIN value but insert and delete have time complex O( $\log{n}$).


#### Standard of rules
1. Parent to Child
    - Parent index = i
    - Child Left index = 2 * i
    - Child Right index = 2 * i + 1

2. Child to Parent
    - Child = i (it does not matter left or right)
    - Parent = i / 2

```
 + Visualization

        50
      /    \
    30      20
   / \     /  
 10  15   5  



 + In real array

 [50, 30, 20, 10, 15, 5]
```

#### FIND MAX or MIN
```
 + Visualization

[1] Just Check first value -> FINISH
        50 -> check
      /    \
    30      20
   / \     /  
 10  15   5  

```

#### EXTRACT MAX or MIN
```
 + Visualization

[1] Extract value of first index 
        50 -> pick it up
      /    \
    30      20
   / \     /  
 10  15   5  


[2]
        --
      /    \
    30      20
   / \     /  
 10  15   5  


[3] compare 5 with 30 or 20
         5
      /    \
    30      20
   / \       
 10  15     


[4] compare 5 with 10 or 15
        30
      /    \
    5      20
   / \       
 10  15     


[5] compare 5 with 10 or 15 -> FINISH
        30
      /    \
    15      20
   / \       
 10   5     

 - Result of Array

 [30, 15, 20, 10, 5]

```


#### Insert
```
 + Visualization

[1] Insert New value 40
        50 
      /    \
    30      20
   / \     /  \
 10  15   5    40

[2] Compare 40 with parent of 20 -> swap
        50 
      /    \
    30      40
   / \     /  \
 10  15   5    20

[3] Compare 40 with parent of 50 -> done -> FINISH
        50 
      /    \
    30      40
   / \     /  \
 10  15   5    20

 - Result of Array

 [50, 30, 40, 10, 15, 5, 20]

```

### The structure of heap has feature. Heap is sorted but the sort meaning is root is MAX/MIN, not sorting all of array. So Insert, delete(=extract), find is focusing the highest of root.