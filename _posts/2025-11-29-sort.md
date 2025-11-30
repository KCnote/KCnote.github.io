---
layout: post
title: "Sorting Algorithm"
date: 2025-11-28 12:00:00 +0900
author: kang
categories: [algorithm, sort]
tags: [algorithm, sort]
pin: false
math: true
mermaid: true
---

# What is SORT algorithms

#### MAIN TOPIC <span style="color:#0033FF"> Insert, Bubble, Quick, Merge </span>

* CHECK <span style="color:brown">INSERT</span> and <span style="color:brown">BUBBLE</span>
  - The simplest algorith of sorting
  - TIme complex is O($n^2$) 
  - Compare all of elements and fixed the position

| BUBBLE SORT

 > Example [5, 3, 1, 2, 4]

  ``` 
  [PHASE 1]
  index 0 vs 1 -> [3, 5, 1, 2, 4]
  index 1 vs 2 -> [3, 1, 5, 2, 4]
  index 2 vs 3 -> [3, 1, 2, 5, 4]
  index 3 vs 4 -> [3, 1, 2, 4, 5]

  [PHASE 2]
  index 0 vs 1 -> [1, 3, 2, 4, 5]
  index 1 vs 2 -> [1, 2, 3, 4, 5]
  index 2 vs 3 -> [1, 2, 3, 4, 5]

  [PHASE 3]
  index 0 vs 1 -> [1, 3, 2, 4, 5]
  index 1 vs 2 -> [1, 2, 3, 4, 5]

  [PHASE 4]
  index 0 vs 1 -> [1, 3, 2, 4, 5]
  ```

  >> <span style="color:RED">MAIN POINT</span> - when we finish one phase, it will be sure the lastest number is<span style="color:RED"> highest/lowerest</span> thing in the array.
  it is meaning, for just one number, we should use O(n) time complex (even though next phase is reduced just O(1) time complex)

| INSERT SORT

 > Example [5, 3, 1, 2, 4]

  ``` 
  [PHASE 1] index 1 - key number 5
  index 0 vs 1 -> [3, 5, 1, 2, 4]

  [PHASE 2] index 2 - key number 1
  index 1 vs 2 -> [3, 1, 5, 2, 4]
  index 0 vs 1 -> [1, 3, 5, 2, 4]

  [PHASE 3] index 3 - key number 2
  index 2 vs 3 -> [1, 3, 2, 5, 4]
  index 1 vs 2 -> [1, 2, 3, 5, 4]
  index 0 vs 1 -> [1, 2, 3, 5, 4] -> not swap and PHASE 3 stopped

  [PHASE 4] index 4 - key number 4
  index 3 vs 4 -> [1, 2, 3, 4, 5]
  index 2 vs 3 -> [1, 2, 3, 4, 5] -> not swap and PHASE 4 stopped
  ```
  >> <span style="color:RED">MAIN POINT</span> - when we finish one phase, it will be sure the array that <span style="color:RED">already</span> checked is sorted and next phase number will be found where it will be fixed.
  it is meaning, all the phase, making sorted array before the phase index. we should use O(n) time complex.


* CHECK <span style="color:brown">QUICK</span>
  - The best algorith of sorting
  - TIme complex is O(n $\log_{2}{n}$), but worst case is O($n^2$), apparently unstable.
  - BUT cache locality is high than other algorithms, it is the best speed in sorting.
  - You should pick pivot (whatever index you want)
  - Use both cursors of low side and high side
  - low side stop when it finds higher number than pivot.
  - high side stop when it finds lower number than pivot.
  - The exit condition is low index >= high index
  - not need additional memory.
  
> Example [4, 3, 7, 5, 6, 1, 2, 8]

  ``` 
  PIVOT = at the beginning index - number 4
  [PHASE 1]
  [4, 3, 7, 5, 6, 1, 2, 8] - low cursor 3 / high cursor 8 -> pass
  [4, 3, 7, 5, 6, 1, 2, 8] - low cursor 7 (stop) / high cursor 8 -> pass
  [4, 3, 2, 5, 6, 1, 7, 8] - low cursor 7 (stop) / high cursor 2 (stop) -> swap
  [4, 3, 2, 5, 6, 1, 7, 8] - low cursor 5 (stop) / high cursor 7 -> pass
  [4, 3, 2, 1, 6, 5, 7, 8] - low cursor 5 (stop) / high cursor 1 (stop) -> swap
  [4, 3, 2, 1, 6, 5, 7, 8] - low cursor 6 (stop) / high cursor 5 -> pass
  [4, 3, 2, 1, 6, 5, 7, 8] - low cursor 6 (stop) / high cursor 6 -> pass
  [4, 3, 2, 1, 6, 5, 7, 8] - low cursor 6 (stop) / high cursor 1 (stop) -> low cursor index is right more than high cursor index -> not swap, wait a moment.
  [1, 3, 2, 4, 6, 5, 7, 8] - swap the pivot and high cursor 1 -> finish

  [PHASE 2]
  [1, 3, 2, 4, 6, 5, 7, 8] - divide the array base on before pivot
  
    PIVOT = at the beginning index - number 1
    <PHASE 2-1> [1, 3, 2] 
    [1, 3, 2] - low cursor 3 (stop) / high cursor 2 -> pass
    [1, 3, 2] - low cursor 3 (stop) / high cursor 3 -> pass
    [1, 3, 2] - low cursor 3 (stop) / high cursor 1 (stop) -> both cursor is crossed. wait.
    [1, 3, 2] - swap the pivot and high cursor 1 -> but same index, it is same -> finish

    PIVOT = at the beginning index - number 6
    <PHASE 2-2> [6, 5, 7, 8] 
    [6, 5, 7, 8] - low cursor 5 / high cursor 8 -> pass
    [6, 5, 7, 8] - low cursor 7 (stop) / high cursor 8
    [6, 5, 7, 8] - low cursor 7 (stop) / high cursor 7
    [6, 5, 7, 8] - low cursor 7 (stop) / high cursor 5 (stop) -> low index >= high index. wait.
    [5, 6, 7, 8] - swap the pivot and high cursor 5 -> finish
   

  [PHASE 3]
  [1, 3, 2], [5, 6, 7, 8] - divide the array base on before pivot

    PIVOT = at the beginning index - number 3
    <PHASE 3-1> [3, 2] 
    [3, 2] - low cursor 2 (stop, it is end) / high cursor 2 -> pass
    [3, 2] - low cursor 2 (stop, it is end) / high cursor 2 (stop) -> low index >= high index. wait.
    [2, 3] - swap the pivot and high cursor 2 -> finish


    <PHASE 3-2> [5]
    element of array is just one -> finish

    PIVOT = at the beginning index - number 7
    <PHASE 3-3> [7, 8] 
    [7, 8] - low cursor 8 (stop, it is end) / high cursor 8 -> pass
    [7, 8] - low cursor 8 (stop, it is end) / high cursor 8 -> pass
    [7, 8] - low cursor 8 (stop, it is end) / high cursor 7 (stop) -> low index >= high index. wait.
    [7, 8] - swap the pivot and high cursor 7 -> but same index, it is same -> finish

  [PHASE 4]
  [2, 3], [7, 8] - divide the array base on before pivot

    <PHASE 4-1> [2]
    element of array is just one -> finish

    <PHASE 4-2> [8]
    element of array is just one -> finish

  ```
 
* CHECK <span style="color:brown">MERGE</span>
  - The one of the divide and conquer algorithm
  - Always time complex is O(n $\log_{2}{n}$). Apparently stable.
  - BUT cache locality is low. The real performance is not better than QuickSort
  - Must need to allocate additional memory O(n)

> Example [4, 3, 7, 5, 6, 1, 2, 8]

  ``` 
  Level 0  -> [4, 3, 7, 5, 6, 1, 2, 8]
  Level 1  -> [4, 3, 7, 5]   [6, 1, 2, 8]
  Level 2  -> [4, 3]   [7, 5]   [6, 1]   [2, 8]
  Level 3  -> [4]   [3]   [7]   [5]   [6]   [1]   [2]   [8]

  [PHASE 1] Level 3
  [4]   [3]   [7]   [5]   [6]   [1]   [2]   [8] -> finish.

  [PHASE 2] Level 2
  [4] + [3] -> compare 4, 3 = [3, 4]
  [7] + [5] -> compare 7, 5 = [5, 7]
  [6] + [1] -> compare 6, 1 = [1, 6]
  [2] + [8] -> compare 2, 8 = [2, 8]

  [PHASE 3] Level 1
  [3, 4] + [5, 7]
  -> 3 vs 5 -> 3 -> [3]
  -> 4 vs 5 -> 4 -> [3, 4]
  -> the front array is finished. the rest of array is just assigned -> [5, 7] -> [3, 4, 5, 7]

  [1, 6] + [2, 8]
  -> 1 vs 2 -> 1 -> [1]
  -> 6 vs 2 -> 2 -> [1, 2]
  -> 6 vs 8 -> 6 -> [1, 2, 6]
  -> the front array is finished. the rest of array is just assigned -> [8] -> [1, 2, 6, 8]

  [PHASE 4] Level 0
  [3, 4, 5, 7] + [1, 2, 6, 8]
  -> 3 vs 1 -> 1 -> [1]
  -> 3 vs 2 -> 2 -> [1, 2]
  -> 3 vs 6 -> 3 -> [1, 2, 3]
  -> 4 vs 6 -> 4 -> [1, 2, 3, 4]
  -> 5 vs 6 -> 5 -> [1, 2, 3, 4, 5]
  -> 7 vs 6 -> 6 -> [1, 2, 3, 4, 5, 6]
  -> 7 vs 8 -> 7 -> [1, 2, 3, 4, 5, 6, 7]
  -> the front array is finished. the rest of array is just assigned -> [8] -> [1, 2, 3, 4, 5, 6, 7, 8]

  ```