---
layout: post
title: "Mini.Project 01-02. Deploy: Integrated CMake, Docker and CI/CD"
date: 2026-04-01 00:00:00 +0900
author: kang
categories: [Mini-Project, Mini-Project - Deployment]
tags: [Mini-Project , Mini-Project - Deployment]
pin: false
math: true
mermaid: true
---

# <b>Mini.Project 01-02. Deploy: Integrated CMake, Docker and CI/CD</b>

---

### <b>Prerequisites</b>
        C++

---

## <b>1. Design of hierarchy</b>

> C++ Project

```yml
/Project:
  - main.cpp
  - /tests:
    - test_svstring.cpp
  - /lib:
    - /structlib:
      - /src:
        - SVBase.cpp
        - SVString.cpp
      - /include:
        - SVBase.h
        - SVString.h
```

* Project/lib/structlib: Create a library (.lib)
* Project/lib: Collect multiple libraries
* Project/main.cpp: Entry point that depends on libraries

## <b>2. Design of this project</b>

In this post, I create basic C++ code. The code don't contain meaningful logic. So I will attach the last section.

I mainly explain the dependency between files.

 - SVBase.cpp < SVBase.h
 - SVString.cpp < SVString.h < SVBase.h
 - main.cpp < SVString.h
 - test_svstring.cpp < SVString.h

this meaning is:

 - The SVBase cpp has independent from all of source
 - The SVString cpp has dependent from SVBase class because of inheritance structure
 - The main cpp has dependent from library that consists of SVBase and SVString
 - The test_svstring cpp is same level of main cpp

### <b>3. Why I consist this hierarchy</b>

The executable program can depend on multiple libraries. Therefore the main cpp depends on the library. Test program follows the same structure.

Libraries can be created and extended independently. Considering scalability, I seperated them /lib and sub-library directories such as /XXXlib

The source files of library can be dependent on each sources. Therefore I explicitly designed the dependency between source files

## <b>4. Code</b>

> main.cpp

```cpp
#include <iostream>
#include "SVString.h"

int main()
{
    CSVString str1;
    str1.setSize(10);

    std::cout << "Size of str1: " << str1.getSize() << std::endl;
    std::cin.get();

    return 0;
}
```


> SVBase.h

```cpp
class CSVBase
{
    public:
        CSVBase();
        CSVBase(const CSVBase& obj);
        CSVBase(const CSVBase* pOj);
        virtual ~CSVBase();

        CSVBase& operator=(const CSVBase& obj);

    public:
        virtual bool clear();
        bool assign(const CSVBase& obj);
        bool assign(const CSVBase* pObj);

    private:
        static int m_i32ID;
};
```

> SVString.h

```cpp
#include "SVBase.h"

class CSVString : CSVBase
{
    public:
        CSVString();
        CSVString(const CSVString& obj);
        CSVString(const CSVString* pOj);
        virtual ~CSVString();

        CSVString& operator=(const CSVString& obj);

    public:
        virtual bool clear() override;
        bool assign(const CSVString& obj);
        bool assign(const CSVString* pObj);

        bool setSize(int i32Size);
        int getSize();

    private:
        char* m_pS8String;
        int m_i32Size;
};
```

> SVBase.cpp

```cpp
#include "../include/SVBase.h"

int CSVBase::m_i32ID = 0;

CSVBase::CSVBase()
{
    ++m_i32ID;

    clear();
}

CSVBase::CSVBase(const CSVBase& obj)
{
    assign(obj);
}

CSVBase::CSVBase(const CSVBase* pObj)
{
    assign(pObj);
}

CSVBase::~CSVBase()
{
    clear();
}

CSVBase& CSVBase::operator=(const CSVBase& obj)
{
    assign(obj);
 
    return *this;
}

bool CSVBase::clear()
{
    return true;
}

bool CSVBase::assign(const CSVBase& obj)
{
    assign(&obj);

    return true;
}

bool CSVBase::assign(const CSVBase* pObj)
{
    bool bReturn = false;

    do
    {
        if(!pObj)
            break;

        bReturn = true;
    }
    while(false);

    return bReturn;
}
```

> SVString.cpp

```cpp
#include "../include/SVString.h"
#include <iostream>

CSVString::CSVString() : CSVBase()
{
    m_pS8String = nullptr;

    clear();
}

CSVString::CSVString(const CSVString& obj) : CSVBase(obj)
{
    assign(obj);
}

CSVString::CSVString(const CSVString* pOj) : CSVBase(pOj)
{
    assign(pOj);
}

CSVString::~CSVString()
{
    clear();
}

CSVString& CSVString::operator=(const CSVString& obj)
{
    CSVBase::operator=(obj);
    assign(obj);
 
    return *this;
}

bool CSVString::clear()
{
    bool bReturn = false;

    do
    {
        if(!CSVBase::clear())
            break;

        if(m_pS8String)
        {
            free(m_pS8String);
            m_pS8String = nullptr;
        }
        
        bReturn = true;
    } 
    while(false);

    return bReturn;
}

bool CSVString::assign(const CSVString& obj)
{
    return assign(&obj);
}

bool CSVString::assign(const CSVString* pObj)
{
    bool bReturn = false;

    do
    {
        if(!pObj)
            break;

        if(!CSVBase::assign(pObj))
            break;

        bReturn = true;
    }
    while(false);

    return bReturn;
}

bool CSVString::setSize(int i32Size)
{
    bool bReturn = false;

    do
    {
        if(i32Size <= 0)
            break;

        if(m_pS8String)
        {
            free(m_pS8String);
            m_pS8String = nullptr;
        }

        m_pS8String = (char*)malloc(sizeof(char) * i32Size);

        if(!m_pS8String)
            break;

        m_i32Size = i32Size;
        bReturn = true;
    }
    while(false);

    return bReturn;
}

int CSVString::getSize()
{
    return m_i32Size;
}
```

>test_svstring.cpp

```cpp
#include <iostream>
#include "SVString.h"

int main() 
{
    CSVString str;
    int i32Size = 10;
    str.setSize(i32Size);

    if (str.getSize() != i32Size)
     {
        std::cerr << i32Size << "Test failed: expected 10\n" << str.getSize();
        return 1;
    }

    std::cout << "Test passed\n";
    return 0;
}
```