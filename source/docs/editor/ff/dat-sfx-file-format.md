### Overview
Sfx.dat is a database file used for registering FX.

### C Style Specification
```c
/* Controller IDs:
    0x00 = Projectile (Light Needle)
    0x01 = Projectile (Wind Cutter)
    0x02 = Projectile (Billboard)
    0x03 = Projectile (Sphere)
    0x04 = Projectile (Sphere 2)
    0x05 = Projectile (Wind Cutter #2)
    0x09 = Missle
    0x0C = Fire Column
    0x0E = Needle Down?..
    0x0F = Weird Curve Thing
    0x14 = Earthwave
    0x15 = Fire Wall
    0x16 = Expolsion
    0x1E = Haze
    0x1F = Vortex
    0x20 = Dragon 
    0x22 = Tornado
    0x28 = Lightning
    0x29 = ???
    0x2A = ???
    0x2B = ???
    0x2D = Moonlight Sword
    0x2E = Triple Fang
    0x64 = Explode 2D
    0x65 = Explode 3D
    0x80 = ???
    0x81 = ???
    0x83 = ???
    0x84 = candle
    0x86 = ???
*/

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