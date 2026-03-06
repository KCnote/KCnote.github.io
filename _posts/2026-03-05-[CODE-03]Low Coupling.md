---
layout: post
title: "Low Coupling"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Low Coupling</b>
---
### <b>Prerequites</b>
    NOTHING

---
## <b>What is Low Coupling</b>

### 1. What is Low Coupling?

Low Coupling is principle of software design. Low Coupling is about keeping low depencey between functions and it is to be robust for software.

Actually During my first two years as a junior developer at the company, i didn't care this. Because making good performance and developing as fast as possible is the most important part of developer. Yes, I think so too, now also. But After that assume to develop the good program explictly, I found the design of my function is too bad.

As time is passed, my algorithm is to be more complex and not to easily change the code when i have issue unexpected.

The vicious circle is like that:

    Catch A function → Revise A function
    → Execute the program 
    → Something wrong i didn't expect → Check programs
    → Catch B function issued after revising A fucntion →  Revise B function
    → Execute the program
    → Something wrong i didn't expect → Check programs
    → Catch C function issued after revising A or B fucntion →  Revise C function
    → Execute the program
    → Something wrong i didn't expect → Check programs
    → And So on..... Annoying 

And the worst case is i found if i want to revise A fucntion, i should revise B ~ Z fucntions and change the member variables and so on.

I think everyone have experienced about that. Because even though you know the design, unlike mathematics there are no strictly defined rules, so sometimes it can vary subjectively depending on the situation and design decisions.

### 2. How to deal with Low Coupling

There's no rule, Very ambigious BUT i will post my rule for Low Coupling.

> #### Step 1. Ambigious status

When we design the architectures, very hard to define couplings. So at the first time, i just program the architectures without strictly rules but trying to split the code into functionality unit or semantic unit.

> #### Step 2. Implementation status

First time, i divide units of function and think that how can someone who didn't know the function understand the flow by my split
    
> #### Step 3. Check the dependency of function's parameters

The More it is divided, The less depency between parameters of different functions. So i check the parameter dependency. If i change a parameter from A function, it can effect the parameter from B function directly

> #### Step 4. Semantic Unit

The last thing is i think beacuse the split should have semantic information, checking the semantic duplicate between different functions.

If the semantic of function is ambigious, i should be more think about it is really independent or not.

### 3. What is effect with Low Coupling

Let's see the vicious circle is like that:

    Catch A function → Revise A function
    → Execute the program 
    → Something wrong i didn't expect → Check programs
    → Catch B function issued after revising A fucntion →  Revise B function
    → Execute the program
    → Something wrong i didn't expect → Check programs
    → Catch C function issued after revising A or B fucntion →  Revise C function
    → Execute the program
    → Something wrong i didn't expect → Check programs
    → And So on..... Annoying 

Assume they are independent:

By means that is the cycle of correction and happening is stopped thanks to no dependency.

    Catch A function → Revise A function
    → Execute the program 
    →  Finish

Even though this situation is happend to us

    Catch A function → Revise A function
    → Execute the program 
    → Something wrong i didn't expect → Check programs

    → Catch B function issued WITHOUT revising A fucntion →  Revise B function

The issue <span style="color:blue">arose from problems derived from the original problem itself</span>, <span style="color:red">NOT from function dependencies</span>.