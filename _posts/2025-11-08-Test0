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

##1. Same contents, same functions
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

##2. Don't include everything in just one function

It is also many developer have bad programming style. we should do think about "Indenpentent" contents. if someone put in many contents without seperating, the functions will be just a container, not function. It couldn't use various situation. but if we do seperating, it's going to be able to many type of situations.

#### Declaration
```c++
class CManager
{
public:
    int findMembershipNumber(std::string strName);

public:
    std::vector<std::string> m_vctList;
};
```

#### Definition
```
int CManager::findMembershipNumber(std::string strName)
{
    m_vctList.clear();

    m_vctList.push_back("Cray");
    m_vctList.push_back("Tomy");
    m_vctList.push_back("Mona");
    m_vctList.push_back("Woon");

    int i32Index = -1;

    for (size_t i = 0; i < m_vctList.size(); ++i)
    {
        if (m_vctList[i] == strName)
        {
            i32Index = i;
            break;
        }
    }

    return i32Index;
}
```

In this case, if i would like to revise member Information, I just revise the FinMembershipNumber() function. But What if the function will be so complexity and so long codes in this function. it will be hard to revise the function as more as time's going. So if i program this status to revise clear codes, i will be like this example below.

#### Declaration
```c++
class CManager
{
public:
    int findMembershipNumber(std::string strName);

private:
    bool clearMembers();
    bool updateMembers();
    bool convertMemberToNumber(std::string strName, int& i32Result);

public:
    std::vector<std::string> m_vctList;
};
```

#### Definition
```
int CManager::findMembershipNumber(std::string strName)
{
    int i32Index = -1;

    clearMembers();
    updateMembers();
    convertMemberToNumber(strName, i32Index);

    return i32Index;
}

bool CManager::clearMembers()
{
    m_vctList.clear();

    return true;
}

bool CManager::updateMembers()
{
    m_vctList.push_back("Cray");
    m_vctList.push_back("Tomy");
    m_vctList.push_back("Mona");
    m_vctList.push_back("Woon");

    return true;
}

bool CManager::convertMemberToNumber(std::string strName, int& i32Result)
{
    bool bReturn = false;

    for (size_t i = 0; i < m_vctList.size(); ++i)
    {
        if (m_vctList[i] == strName)
        {
            i32Result = i;
            bReturn = true;
            break;
        }
    }

    return bReturn;
}
```

you bet, it is not efficient because the function, findMembershipNumber(), is always update the clear and update the members. I hope to just concentrate on the function seperatation considering the features.

##3. Seperate between the real operation and flow operation
I use the same example like "2."
#### Definition
```
int CManager::findMembershipNumber(std::string strName)
{
    int i32Index = -1;

    clearMembers();
    updateMembers();
    convertMemberToNumber(strName, i32Index);

    return i32Index;
}

bool CManager::clearMembers()
{
    m_vctList.clear();

    return true;
}

bool CManager::updateMembers()
{
    m_vctList.push_back("Cray");
    m_vctList.push_back("Tomy");
    m_vctList.push_back("Mona");
    m_vctList.push_back("Woon");

    return true;
}

bool CManager::convertMemberToNumber(std::string strName, int& i32Result)
{
    bool bReturn = false;

    for (size_t i = 0; i < m_vctList.size(); ++i)
    {
        if (m_vctList[i] == strName)
        {
            i32Result = i;
            bReturn = true;
            break;
        }
    }

    return bReturn;
}
```

When I make the function, findMembershipNumber(), I try to use the part of real operation and minimize other process. That's why I had experienced to hard to check some part what i want, i need to change. If I can improve the readability codes, I would find the part that i need to fix, but that's time I don't know that. So I change my lousy codes that they aren't distinguished the real and flow operations.
As soon as revising the problem, make it easiler changing programs than before when we should be responable to customer needs. If i need to find member issue, my check order is just findMembershipNumber() - updateMembers(), and if someone find the wrong return number, I just find the findMembershipNumber() - convertMemberToNumber().
Maybe someone think it is unimpressed, but if you have a very big and complexity program, I don't think you don't need it.

5. 



반환에 유의하기(모두 기억할수 없음) 성공실패를 알아야만함 그래야 코드흐름에 대해 손쉽게 흐름이 파악됨
break의 중요성 하기
return 값
