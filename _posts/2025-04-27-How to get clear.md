---
layout: post
title: "How to get clear - phase 1?"
date: 2025-04-24 12:00:00 +0900
categories: [clear]
tags: [clear]
---


### G'Day Mate
I had experienced that how much times we make our codes insecure and unstable method I know. I'm about to write down the more efficiency and robust method.  
Some ways are to be hard when our programs are so difference. But make sure the things i record are so not ambiguous.  
I will just write down the rules we can always keep 

1. Same contents, same functions
many of people, esp. junior, could program the functions for each. I see they are concentrating small part, not overall.
But when the program is bigger than start, It will be conplexable and sometimes be hard to manage all our codes. So, we always think about minimizing complexity of program.
if we consider that some paragraphs or functions is same or not, and then we call the same function, It will be more robust program.


#### Delcaration
```c++
class CObject
{
public:
    CObject();

public:
    bool init();

public:
    int m_i32Index = 0;
};
```

#### Definition
```c++
CObject::CObject()
{
    init();
}

bool CObject::init()
{
    m_i32Index = 0;

    return true;
}
```

According example above, the function, clear(), i use it for constructor. 'cause their contents is so same, of cause sometimes they are little difference.  
So, we consider the design they are same or not.  
Usually, small or simple project don't need to consider many things. But if you have experienced making big project, later. It's so important things.  
If we don't have this habit, it will be some issue like this,

```c++
class CObject
{
public:
    CObject();

public:
    bool init();

    bool api_0();
    bool api_1();
    bool api_2();
    bool api_3();
};
```

#### Definition
```c++
CObject::CObject()
{
    m_i32Index = 0;
}

bool CObject::init()
{
    m_i32Index = 0;

    return true;
}

bool CObject::api_0()
{
    m_i32Index = 0;

    // describe functions, blah, blah ...

    return true;
}

bool CObject::api_1()
{
    m_i32Index = 0;

    // describe functions, blah, blah ...

    return true;
}

bool CObject::api_2()
{
    m_i32Index = 0;

    // describe functions, blah, blah ...

    return true;
}

bool CObject::api_3()
{
    m_i32Index = 0;

    // describe functions, blah, blah ...

    return true;
}
```

As much as having various functions, we copy same contents. So it will be hard to maintain our program if the program is more complex than before.  
But if we consider this subject, we just revise Init() function when the program force us to revise many kind of status.


함수쪼개기, return 값
