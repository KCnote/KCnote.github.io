---
layout: post
title: "BMP Format"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Fundamental]
tags: [Computer Vision, Computer Vision - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>BMP Format</b>
---
### <b>Prerequites</b>
    NOTHING

---
## <b>What is BMP Format</b>

### 1. What is BMP Format?

File has format that depends on extension.

### 1. Sequence of BMP Format?

![BMP](/assets/img/develop/BMPFormat.bmp)

I will analyze above the image.

> 1-1. File Header

14 bytes

```cpp
42 4D / 62 09 01 00 / 00 00 / 00 00 / 8A 00 00 00

struct BMPFileHeader
{
    uint16_t bfType;      // 0x4D42
    uint32_t bfSize;      // 0x00010962
    uint16_t bfReserved1; // 0x0000
    uint16_t bfReserved2; // 0x0000
    uint32_t bfOffBits;   // 0x0000008A
};
```

- First 2 btyes is about extension-type like bmp. jpg. png ...
- Next 4 btyes is about file size.
- Next 2 btyes is 
- Next 2 btyes is 
- Next 4 btyes is about offest where the first pixel is located on the file.

> 2-2. Info Header

40 bytes

```cpp
7C 00 00 00 \
96 00 00 00 \
96 00 00 00 \
01 00       \
18 00       \
00 00 00 00 \
D8 08 01 00 \
00 00 00 00 \
00 00 00 00 \
00 00 00 00 \
00 00 00 00

struct BMPInfoHeader
{
    uint32_t biSize;            // 0x0000007C
    int32_t  biWidth;           // 0x00000096
    int32_t  biHeight;          // 0x00000096
    uint16_t biPlanes;          // 0x0001
    uint16_t biBitCount;        // 0x0018
    uint32_t biCompression;     // 0x0000
    uint32_t biSizeImage;       // 0x000108D8
    int32_t  biXPelsPerMeter;   // 0x00000000
    int32_t  biYPelsPerMeter;   // 0x00000000
    uint32_t biClrUsed;         // 0x00000000
    uint32_t biClrImportant;    // 0x00000000
};
```

- First 4 btyes is BITMAPV5HEADER size
- Next 4 btyes is image width
- Next 2 btyes is image height
- Next 2 btyes is 
- Next 2 btyes is bit count (RGB 3 bytes per pixel)
- Next 4 btyes is compression
- Next 4 btyes is size of image
- Next 4 btyes is usually 0
- Next 4 btyes is usually 0
- Next 4 btyes is usually 0
- Next 4 btyes is usually 0

> 2-3. Color Information (Optional)

```cpp
// Color Mask
00 00 FF 00 \
00 FF 00 00 \
FF 00 00 00 \
00 00 00 FF \

// Color Table
42 47 52 73 \
8F C2 F5 28 \
51 B8 1E 15 \
1E 85 EB 01 \
33 33 33 13 \
66 66 66 26 \
66 66 66 06 \
99 99 99 09 \
3D 0A D7 03 \
28 5C 8F 32 \

// Color Management
00 00 00 00 \
00 00 00 00 \
00 00 00 00 \
04 00 00 00 \
00 00 00 00 \
00 00 00 00 \
00 00 00 00

struct BMPColorInformation
{
    uint32_t bV5RedMask;    // 0x00FF0000
    uint32_t bV5GreenMask;  // 0x0000FF00
    uint32_t bV5BlueMask;   // 0x000000FF
    uint32_t bV5AlphaMask;  // 0xFF000000

    uint32_t bV5CSType;     // 0x73524742
    int32_t  bV5Endpoints[9]; 
                            // 0x28F5C28F 0x151EB851 0x01EB851E 0x13333333 
                            // 0x26666666 0x06666666 0x09999999 0x03D70A3D 0x328F5C28}

    uint32_t bV5GammaRed;       // 0x00000000
    uint32_t bV5GammaGreen;     // 0x00000000 
    uint32_t bV5GammaBlue;      // 0x00000000
    uint32_t bV5Intent;         // 0x00000004
    uint32_t bV5ProfileData;    // 0x00000000 
    uint32_t bV5ProfileSize;    // 0x00000000
    uint32_t bV5Reserved;       // 0x00000000
}
```

> 2-4. Pixel Data

[B G R B G R B ....]


BMP file is fundamental format. The file do not usually have any compression.



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

struct CImage
{
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