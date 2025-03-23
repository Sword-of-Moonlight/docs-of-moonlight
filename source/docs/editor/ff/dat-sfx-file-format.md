### Overview
Sfx.dat is a database file used for registering FX.

### C Style Specification
```c
typedef struct
{
    u8 controllerID;    // The ID of the internal controller to use
    u8 modelID;         // The ID of the MDL file to use
    u8 u8x02;
    u8 u8x03;
    u16 u16x04;
    u16 u16x06;
    u16 u16x08;
    u8 unkx0a;
    u8 unkx0b;
    f32 f32x0c;
    f32 f32x10;
    f32 animSpeed;  // The speed at which the FX animation is played
    f32 scale;      // The scale of the FX.
    f32 f32x1c;
    f32 f32x20;
    u32 unkx24;
    u32 unkx28;
    u32 unkx2c;
} SFXDAT_ITEM;

// Layout example. Does not actually exist in the format.
typedef struct
{
    SFXDAT_ITEM items[1024];
} SFXDAT_LAYOUT;
```