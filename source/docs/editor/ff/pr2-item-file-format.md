### Overview
ITEM.PR2 is a database file used for storing any PRF files used in the game.

### C Style Specification
```c

typedef struct
{
    char name[31];          // The name of the item
    char modelFile[31];     // The model file name
    u16 type;               // The type of item
    float center;           // Collision related?..
    u16 u16x11;             // Unknown.
    u16 u16x13;             // Unknown
    u8  animIds[4];         // Animations for the item. Test...
    u16 soundIDs[4];        // Sounds linked to the item? Test...
    u8 soundPitchs[4];      // Pitch to play sounds at? Test...
} ITEMPR2_ITEM;

// This structure doesn't exist in TXR files, but represents the layout.
typedef struct
{
    u32 itemCount;                  // The number of items in the database
    ITEMPR2_ITEM items[itemCount];  // All the items stored in the database
} ITEMPR2_LAYOUT;
```