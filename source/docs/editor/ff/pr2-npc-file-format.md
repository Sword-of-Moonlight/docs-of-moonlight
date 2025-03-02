### Overview
NPC.PR2 is a database file used for storing any PRF files used in the game.

### C Style Specification
```c

typedef struct
{
    char prfFile[31];       // The name of the origin PRF file
    char modelFile[31];     // The name of the model file
    u8 u8x3e;               // Unknown
    u8 u8x3f;               // Unknown
    float f32x40;           // Unknown
    float f32x44;           // Unknown
    float f32x48;           // Unknown
    u16 u16x4C;             // Unknown
    s16 s16x4E;             // Unknown
    u32 unkx50;             // Unknown.
    u32 unkx54;             // Unknown.
    u32 unkx58;             // Unknown. 
    u8 hasExternalTexture;  // 0 = No External Texture, 1 = Yes External Texture
    char textureFile[31];
} NPCPR2_ITEM;

// Layout Example
typedef struct
{
    u32 offsets[1024];

    // There is then a variable size buffer of NPCPR2_ITEM... Each of the offsets point to one of these.
    NPCPR2_ITEM items[];    
} NPCPR2_LAYOUT;
```