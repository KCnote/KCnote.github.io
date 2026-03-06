---
layout: post
title: "Stable Control Flow"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Stable Control Flow</b>
---
### <b>Prerequites</b>
    do ~ while
    break
    pointer
    allocation
    deallocation

---
## <b>What is Stable Control Flow</b>

### 1. What is Stable Control Flow?

All of developer face a difficult situation that get to be too much complex or can not control many flows. In this post, i will show the control of the tiniest units. Robustness of controling the tiniest units can make a good program without bugs. ALWAYS!.
Because even the smallest unit can always be controlled independently from the overall execution flow.

### 2. What kind of things we have to control?

```cpp
bool bOK = false;
int* pI32Container = nullptr;

do
{
    if(i32cnt < 0)
        break;

    if(pI32Container)
    {
        free(pI32Container);
        pI32Container = nullptr;
    }

    pI32Container = (int*)malloc(sizeof(int) * 16);


    bOK = true;
}
while(false)

if(pI32Container)
{
    free(pI32Container);
    pI32Container = nullptr;
}
```

There're usually 3 cases of difficult control. Definitely the amount of code is small, it is very easy task. But when we are programming some tasks, the amount of flow is way too many and if we lose the flow, finding the issues also is difficult because the flows are usually very dependent.

So we should convert large-scale units to small-scale units and one block to small units and so on.

#### 2-1. Sucess Flag
It is very simple, but it is the fundamental elment of making stable function.

The reason that it is very important part of stable flow is it will be always confirming the status of program even though a function is very simple.

The main point is When we finish the function or debug the issue, we easily find where the process is failed with false flag.

If the execution does not reach this point, the operation is treated as a failure since the initial value remains false.

```cpp
bool bOK = false;

do
{
    if(bFail)
        break;

    // Sucess Flag
    // ------------------------- 
    bOK = true;
    // ------------------------- 
}
while(false)
```


#### 2-2. Unconditional Flow Checker

The operation behaves the same regardless of whether do~while(false) is included. However it is very different from a flow perspective. It is easy method even though you're junior or no experience of programming, but it make your code robust.

```cpp
bool bOK = false;

do
{
    // Flow Checker
    // ------------------------- 
    if(bFail)
        break;
    // ------------------------- 

    bOK = true;
}
while(false)
```

Using break to exit the flow when conditions are not satisfied prevents incorrect processing from continuing. 
When the conditions are satisfied, the subsequent code requires fewer conditional checks, making the implementation simpler and more concise.

#### 2-3. Control the heap memory leak

It is very much important part of all of C++ developer. I bet there're no people without suffer from memory leak.

But I did not have been suffered from memory leak because of this design. The main point is i always think about <b>two secetion and one principle</b>.

The two section is allocation and deallocation section. 

#### <b>[Allocation section]</b>
If i will allocate memory on a function, i declare the data type in front of the function with null pointer. 

```cpp
// Allocation section
// ------------------------- 
int* pI32Container = nullptr;
// ------------------------- 

do
{
    bOK = true;
}
while(false)

```

#### <b>[Deallocation section]</b>
It is kind of rule. When we finish the function, it must be get it back the memory.

```cpp
do
{
    bOK = true;
}
while(false)

// Deallocation section
// ------------------------- 
if(pI32Container)
{
    free(pI32Container);
    pI32Container = nullptr;
}
// ------------------------- 
```

And now i just dedicate the flow of program without worry about memory leak issue. 

#### <b>[One principle]</b>

My one principle is if i want to reuse the pointer, we should, <b>must keep the step, deallocation and then reallocation</b>.

```cpp
int* pI32Container = nullptr;

do
{
    // One principle
    // ------------------------- 
    if(pI32Container)
    {
        free(pI32Container);
        pI32Container = nullptr;
    }

   pI32Container = (int*)malloc(sizeof(int) * 16);
   // ------------------------- 

   bOK = true;
}
while(false)
```

I know this is obvious. Most of someone who especially have been over 2 years as a developer maybe say like this <span style="color:red">"I KNOW I KNOW!, But our program is NOT simple like that!"</span>

Yes I know, But our program is also very complex than other pepole because i usually code our program over amount of 1K~5K lines.

There're some cases that is looked like difficult to adapt to this.

<span style="color:white; background-color:black"><b>1. Some values is not just for a function. the values should be not deallocate for other process.</b></span>

    Inside Declare → OutSide process (x)
    Outside Declare → Temporary use in a function → OutSide process (o)

There're no problem if we use the variable within a function. I think this is main reason. Let's see the problem.

First of all, the point where the variable is declared is this function and we should use the variable outside. I think the design of this itself is very poor. beacuse if we do not align the point, it will be complex that we can not control if the process is to be huge. Like this situation, I must declare the pointer outside and this function get the pointer as parameters.
I interpret the pointer is not for the function, for multiple functions. That's why it is be fine and the interpret of flow also correct.
And the variable declared outside the scope should be controlled within same scope

In other word, <span style="color:cyan; background-color:blue"><b>KEEP THE SCOPE SPACE WHERE VARIABLE DECLARED</b></span>

So sometimes <b>the variable can be located from member variables and higher-level scopes down to the lowest-level scope</b>.

<span style="color:white; background-color:black">2. If all of the pointer should be declare in front of function, it feel like mess that location.</span>

Yeah, I think so too. So for good design, this should be defined rigorously about the scope. if a scope have so much pointer variables and it is needed, even though it feel like little bit mess but it's alright. But i usually see the mess of design from no considering the define of scope rigorously

<span style="color:white; background-color:black">3. Sometimes it is waste of cost, one principle, because we can reuse the memory like previous memory is 32 but now i use the 16 of memory.</span>

I think we should seperate this, between design and optimizing. So i usually refactorizing this problem for high-performance. I know sometimes we give up the readable or good flow design for optimization. But The main point is even though when we optimize the function, we keep in mind this design and try to change something we should be changed.

#### Tip. For readability code

sometimes i use the define method just for readability. if the code line is so too long on vertical, the readability is getting worse.

so i collect the define for readability and use like that

``` cpp
#define MemoryFree      if(x)            \
                        {               \
                            free(x);    \
                            x = nullptr;\
                        }               \
```


```cpp
bool bOK = false;
int* pI32Container = nullptr;

do
{
    if(i32cnt < 0)
        break;

    MemoryFree(pI32Container)
    pI32Container = (int*)malloc(sizeof(int) * 16);

    bOK = true;
}
while(false)

MemoryFree(pI32Container)
```

Beacuse MemoryFree define is not main logic of function but design. So when we see the process flow, we can ignore the design and read the main flow. But if we should debug the process, easily check the section.

And if we often use some actions and combine by define. it can prevent typo from ours.