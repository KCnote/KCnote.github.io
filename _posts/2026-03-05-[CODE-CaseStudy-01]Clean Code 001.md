---
layout: post
title: "Case Study - Clean Code 001"
date: 2026-03-07 00:00:00 +0900
author: kang
categories: [CODE, CODE - Case Study]
tags: [CODE, CODE - Case Study]
pin: false
math: true
mermaid: true
---

# <b>Case Study - Clean Code 001</b>
---
### <b>Prerequites</b>
    Coding Convention
    CODE - Pattern Posting

---
## <b>Clean Code</b>

LINK: {% raw %}[BMP Format]({% post_url 2026-03-07-Vision-01-BMP-Format %}){% endraw %}

```cpp
#include <iostream>

#pragma pack(push,1)
struct sBMPFileHeader
{
    uint16_t u16Type;
    uint32_t u32Size;
    uint16_t u16Reserved1;
    uint16_t u16Reserved2;
    uint32_t u32OffBits;
};

struct sBMPInfoHeader
{
    uint32_t u32Size;
    int32_t i32Width;
    int32_t i32Height;
    uint16_t u16Planes;
    uint16_t u16BitCount;
    uint32_t u32Compression;
    uint32_t u32SizeImage;
    int32_t i32XPelsPerMeter;
    int32_t i32YPelsPerMeter;
    uint32_t u32ClrUsed;
    uint32_t u32ClrImportant;
};
#pragma pack(pop)

class CImage
{
public:
    CImage() 
    { 
        i32Width = 0;
        i32Height = 0;
        i32BytesPerPixel = 0;
        i32Stride = 0;
        pU8Buffer = nullptr;
    }
    ~CImage() 
    { 
        if (pU8Buffer)
        {
            free(pU8Buffer);
            pU8Buffer = nullptr;
        }
    }

public:
    int i32Width;
    int i32Height;
    int i32BytesPerPixel;
    int i32Stride;
    uint8_t* pU8Buffer;
};

enum EFileStatus
{
    EFileStatus_OK = 0,
    EFileStatus_Unknown = 1,
    EFileStatus_NullPointer = 2,
    EFileStatus_FailedtoOpen = 3,
    EFileStatus_FailedtoClose = 4,
    EFileStatus_FailedtoRead = 5,
    EFileStatus_FailedtoWrite = 6,
    EFileStatus_WrongFormat = 7,
};

EFileStatus LoadBMP(const char* filePath, CImage* pImgOutput)
{
    EFileStatus eFileProcess = EFileStatus_Unknown;
    FILE* fpImage = nullptr;

    do
    {
        if (!pImgOutput)
            break;

        fopen_s(&fpImage, filePath, "rb");

        if (!fpImage)
        {
            eFileProcess = EFileStatus_FailedtoOpen;
            break;
        }

        sBMPFileHeader sFileHeader;
        {
            if (!fread(&sFileHeader, sizeof(sBMPFileHeader), 1, fpImage))
            {
                eFileProcess = EFileStatus_FailedtoRead;
                break;
            }

            if (sFileHeader.u16Type != 0x4D42)
            {
                eFileProcess = EFileStatus_WrongFormat;
                break;
            }
        }

        sBMPInfoHeader sInfoHeader;
        {
            if (!fread(&sInfoHeader, sizeof(sBMPInfoHeader), 1, fpImage))
            {
                eFileProcess = EFileStatus_FailedtoRead;
                break;
            }

            if (sInfoHeader.u16BitCount != 24 && sInfoHeader.u16BitCount != 32)
            {
                eFileProcess = EFileStatus_WrongFormat;
                break;
            }
        }

        if(fseek(fpImage, sFileHeader.u32OffBits, SEEK_SET))
        {
            eFileProcess = EFileStatus_FailedtoRead;
            break;
        }

        int32_t i32Width = sInfoHeader.i32Width;
        int32_t i32Height = sInfoHeader.i32Height;
        int32_t i32BytesPerPixel = sInfoHeader.u16BitCount / 8;
        int32_t i32Stride = (i32Width * i32BytesPerPixel + 3) & (~3);
        int32_t i32BufferSize = i32Stride * i32Height * sizeof(uint8_t);
        uint8_t* pU8Buffer = (uint8_t*)malloc(i32BufferSize);

        if(!fread(pU8Buffer, i32BufferSize, 1, fpImage))
        {
            eFileProcess = EFileStatus_FailedtoRead;
            break;
        }

        if (pImgOutput->pU8Buffer)
        {
            free(pImgOutput->pU8Buffer);
            pImgOutput->pU8Buffer = nullptr;
        }

        pImgOutput->i32Width = i32Width;
        pImgOutput->i32Height = i32Height;
        pImgOutput->i32BytesPerPixel = i32BytesPerPixel;
        pImgOutput->i32Stride = i32Stride;
        pImgOutput->pU8Buffer = pU8Buffer;

        eFileProcess = EFileStatus_OK;
    } 
    while (false);

    if (fpImage)
    {
        fclose(fpImage);
        fpImage = nullptr;
    }

    return eFileProcess;
}
```

1. Keep Coding Convention.
2. Use curly bracket with semantic units when the local variable is just for updating some variables.
3. For stable code, write the code of constructor and destructor you should update or check variables.
4. Use enum or bool when you return value for other user understanding the flow status.
5. Use do~while and break for check whether problems are happened or not.
6. Do NOT update the output variables until all operations are confirmed to be successful. Update them at once only after completion is confirmed.
7. Keep the same scope of variables's allocation and deallocation.
