---
layout: post
title: "Linked List"
date: 2025-12-06 12:00:00 +0900
author: kang
categories: [structure, linked list]
tags: [structure, linked list, node]
pin: false
math: true
mermaid: true
---

# Linked List
- One of the structure, this is connected by pointer from another node. So it features liner structure. A current node is pointing next node and so on. It does not need to reallocate the memory owing to insert or delete. This structure usually uses implementation of queue, stack and hash table and other schedule program. Time complexity of insert and delete is just O(1) but search is O(n) because it is not possible to approach index. And owing to connecting the node, this is not memory alignment

```cpp
struct LinkedList
{
    double f64Val = 0;
    LinkedList* llnext = nullptr;
}

LinkedList* pLlContainer = new LinkedList;
pLlContainer->f64Val = 1.234;
pLlContainer->llnext = nullptr;

# Insert New

LinkedList* pLlContainerNew = new LinkedList;
pLlContainerNew->f64Val = -.14;
pLlContainerNew->llnext = nullptr;
pLlContainer->llnext = pLlContainerNew;

# delete next
delete pLlContainer->llnext;
pLlContainer->llnext = nullptr;
```

>> Series of Linked List
    1. Singly Linked List
        Previous node points to next points, and impossible to back tracking.
        A -> B -> C -> NULL
    
    2. Doubly Linked List
        Not only previous node points to next node but also next node points previous node. In other word, it is possible to back tracking.
        NULL <- A <-> B <-> C -> NULL

    3. Circular Linked List
        It is almost same with single/doubly linked list and the last node points to head node.
        A -> B -> C ---|
        ^--------------|


> Feature of Array  

    - Memory Alignment
    - if we should insert and delete in the middle of array, it will need to cost O(n)
    - Search speed is O(1)
    - When size is changed, having to reallocate memory

> Feature of Linked List

    - Not Memory Alignment
    - Even though should insert and delete in the middle of array, it needs to cost O(1) becasue of just changing pointer
    - Search speed is O(n)
    - When size is changed, it is possible to dynamic expanding 

