### Overview
TXR files are a super striped back version raster image format used for textures.

### C Style Specification
```c
typedef struct 
{
    u32 width;          // Width
    u32 height;         // Height
    u32 bitsPerPixel;   // Number of bits used per pixel
    u32 depth;          // Depth - mipmap levels
} TXR_HEADER;

typedef struct
{
    u8 r;
    u8 g;
    u8 b;
    u8 a;
} TXR_COLOUR;

// This structure doesn't exist in TXR files, but represents the layout.
typedef struct
{
    TXR_HEADER header;                      // Header Data

    // The palette is only included with < 8bpp>
    if (header.bitsPerPixel <= 8)
        TXR_COLOUR palette[(2 ^ header.bitsPerPixel)]   // 16 or 256 colours
    
    // pixel data
    u8 pixelBuffer[(header.width * (header.bitsPerPixel / 8)) * header.height];

    u8[header.u16x0C * header.sampleCount]; // PCM Sample Buffer
} SND_LAYOUT;
```