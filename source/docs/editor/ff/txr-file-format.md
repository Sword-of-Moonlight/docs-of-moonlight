### Overview
TXR files are used in SoM to store textures for 3D rendering in SoM. Usually, 2D elements use BMP (Microsoft Bitmap) files instead. They're super stripped back, but contain just about everything you really need.

### TXR Header
```c
typedef struct  // BYTE LENGTH: 16
{
    /* 0x00 */ u32 width;           // Image width
    /* 0x04 */ u32 height;          // Image height
    /* 0x08 */ u32 bitsPerPixel;    // How many bits are used to encode a single pixel. When 4 or 8, the TXR will contain a palette.
    /* 0x0C */ u32 mipmapNum;       // Number of mip maps stored after the main image.
} TXR_HEADER;
```

### TXR Colour
```c
typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u8 r;
    /* 0x01 */ u8 g;
    /* 0x02 */ u8 b;
    /* 0x03 */ u8 a;
} TXR_COLOUR;
```

### TXR Layout
```c
typedef struct  // BYTE LENGTH: variable, at least 16
{
    /* 0x00 */ TXR_HEADER header;                      // Header Data

    // The palette is only included when bitsPerPixel is less or equal to 8
    if (header.bitsPerPixel <= 8)
        TXR_COLOUR palette[(2 ^ header.bitsPerPixel)]   // 16 or 256 colours
    
    // Main pixel data
    u8 pixelBuffer[(header.width * (header.bitsPerPixel >> 3)) * header.height];

    if (header.mipmapNum > 0)
    {
        int mipW = header.width;
        int mipW = header.height;

        for (int i = 0; i < header.mipmapNum; ++i)
        {
            // Each mip map is 1/2 the size of the last, starting with 1/2 the size of the main pixel data
            mipW >>= 1;
            mipH >>= 1;

            u8 mipmap[(header.width * (header.bitsPerPixel >> 3)) * header.height];
        }
    }
} TXR_LAYOUT;
```

!!! warning
    This structure doesn't actually exist in SND files, but offers an overview into how a file in the format will be arranged.