### Overview
OBJ.PR2 is a database file used for storing any PRF files used in the game.

### C Style Specification
```c

typedef struct
{
    char name[31];          // The name of the item
    char modelFile[31];     // The model file name
    u16 unkx3E;             // Unknown. Padding for the upcoming 4 byte reads?
    float collisionHeight;  // Height of collision shape
    float collisionRadius1; // Radius or Width
    float collisionRadius2; // Radius or Depth
    u32 unkx4C;             // Unknown Use. Padding maybe, according to HwitVlf
    u8 collisionType;       // 0 = BOX, 1 = Cylinder
    u8 useMovingTexture;    // 0 = No, 1 = Yes
    u16 objectType;         // 0 = BORING, 1 = LIGHT, 20 = BOX, 21 = CHEST, 22 = CORPSE, 40 = SWITCH. Could also be u8, unknown...
    s16 animSprite;         // Type of attatched sprite. 0xFFFF = None, 0x0B00 = Flame, 0x0C00 = Flame, 0x3200 = light wall, 0x3300 = light wall, 0x3400 = splash, 0x3700 = light wall, 0x3800 = light wall, 0xC401 = flame
    u16 animSpriteSpeed;    // Speed of animation for attatched sprite
    u32 unkx58;             // Speculating: These unknowns could be animation and sound related?
    u32 unkx5C;
    u32 unkx60;
    u32 unkx64;
    u16 unkx68;
    u8 keyItemID;           // The ID of the item which is the key... Unknown how this works. Match?
    u8 allowRotateXZ;       // == 1 when rotation on the X and Z axis is allowed in the editor.
} OBJPR2_ITEM;

// Layout
typedef struct
{
    u32 itemCount;                 // The number of items in the database
    OBJPR2_ITEM items[itemCount];  // All the items stored in the database
} OBJPR2_LAYOUT;
```