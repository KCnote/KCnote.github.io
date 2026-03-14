---
layout: post
title: "Function return"
date: 2026-03-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Function Return</b>
---
### <b>Prerequites</b>
    C++

---
## <b>What is Function Return</b>

when we make functions, some developers do not deeply think about RETURN. But I think Return of function is very important part especially we don't need to get back any variable.
I think the return should be fixed <b>bool</b> or <b>enum</b> type if the function is not too much simple.
The meaning of bool or enum is about whether this function is to be success or fail.


### 1. Rule of Return

I have simple two rules of deciding return type. 

    Return: bool or enum type

First, All or most of functions that i don't know what is the best return or i know what is the return exactly should be bool or enum type.

both clients or developers want to know what function is sucess. When we debug the program or flow, we usually should check the status after functions are finished. So if we use return void, they are very confused where the process have problems.

    Return: Any data type

Second, I want to say if we use a data type as return, it is SPECIAL CASE. There are some condition why we can use a data type as return.

1. NEVER happen error when we call the function.
2. TOO MUCH HARE to use as getting the output from parameters
3. If we use the this case, we should make overload same operation

``` cpp
# 1.
int GetResultCount()
{
    return m_i32Count;
}

# 2.
bool GetResultCount(int* pI32Count)
{
    bool bOK = false;

    do
    {
        if(!pI32Count)
            break;

        *pI32Count = m_i32Count;

        bOK = true;
    }
    while(false);

    return bOK;
}

# Too much complex to use
int main()
{
    ...
    int i32Count;
    CData.GetResultCount(&i32Count);

    ...

    return;
}

# 3.
int GetResultCount(); # commonly used functions
bool GetResultCount(int* pI32Count); # when clinets have problem, using function. 
```

