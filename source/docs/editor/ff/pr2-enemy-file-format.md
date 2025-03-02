### Overview
ENEMY.PR2 is a database file used for storing any PRF files used in the game.

### C Style Specification
```c

typedef struct
{
    char prfFile[31];       // The name of the origin PRF file
    char modelFile[31];     // The name of the model file
    u8 u8x3e;               // Unknown
    u8 u8x3f;               // Unknown
    u8 hasExternalTexture;  // 0 = No External Texture, 1 = Yes External Texture
    char textureFile[31];

    // OTHER STUFF TO-DO: https://forum.swordofmoonlight.com/Thread-PRF-info
} ENEMYPR2_ITEM;

// This structure doesn't exist in TXR files, but represents the layout.
typedef struct
{
    u32 offsets[1024];  // The file begins with 1024 offsets, which point to and enemy definition.
} ENEMYPR2_LAYOUT;
```