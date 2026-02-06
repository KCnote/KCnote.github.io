---
layout: post
title: "Recursion"
date: 2025-11-30 12:00:00 +0900
author: kang
categories: [algorithm, recursion]
tags: [algorithm, recursion]
pin: false
math: true
mermaid: true
---

# Recursion
- A method where a function calls itself to break a problem into smaller subproblems and solve them step by step

▽ Binary Serach
``` cpp
int binarySearch(int arrI32Data[], int i32Left, int i32Right, int i32Target) 
{
    int i32Return = -1;

    do
    {
        if (i32Left > i32Right)
            break;

        int i32Mid = (i32Left + i32Right) / 2;

        if (arrI32Data[i32Mid] == i32Target) 
            i32Return = i32Mid;
        else if (arrI32Data[i32Mid] > i32Target)
            i32Return = binarySearch(arrI32Data, i32Left, i32Mid - 1, i32Target);
        else
            i32Return = binarySearch(arrI32Data, i32Mid + 1, i32Right, i32Target);
    }
    while(false);

    return i32Return;
}
```

>> Call stack

When we call a function, we have to use a stack. The size of allocated stack depends on function info. So in recursive function, we can check the call stack address.  

``` Cpp
void ShowStack(int i32Depth) {
    int i32Address = i32Depth;

    cout << "Depth: " << i32Depth 
         << ", localVar address: " << &i32Address << endl;

    if (i32Depth >= 10) 
        return;

    ShowStack(i32Depth + 1);
}
```
![CallStack Test](/assets/img/develop/2025-11-30-CallStack.jpg)

Depth 1 : 0x31399FB44

Depth 2 : 0x31399FA24   (0x120 decrease)

Depth 3 : 0x31399F904   (0x120 decrease)

Depth 4 : 0x31399F7E4   (0x120 decrease)

...

>> Typcial example
 - MergeSort
 - QuickSort
 - In function, Too Much allocate memory
 - 

>> Stack overflow
 - In recursion, there is no base case
 - Depth is so deep

 It is not only recursion but also all function and program. the amount of stack is depended on OS.
  - WINDOW : 1MB (main thread)
  - Linux : 8MB
  - macOS : 8MB

▽ Window 11, Stack overflow memory Test

> 4byte x 250 x 1000 Allocated : 0.953MB -> Not Stack overflow

![StackOverflow Test](/assets/img/develop/2025-11-30-NotStackOverflow.jpg)

> 4byte x 260 x 1000 Allocated : 0.992MB -> Stack overflow

![StackOverflow Test](/assets/img/develop/2025-11-30-StackOverflow.jpg)

That is, The design of recursion is always call a lot of stack (call stack), we should check how much, many stack we will use.

>> Base case

Recursive function must have base case. The base case is termination condition of recursion. Most of error is caused by base case.
