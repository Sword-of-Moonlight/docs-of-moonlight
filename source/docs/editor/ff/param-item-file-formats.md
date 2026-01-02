### Overview
Items (as far as data goes) are made up of three file formats in Sword of Moonlight: .PR2, .PRM, .PRF.  Each of these files handles a different part of item definition.

A special thanks to Holy_Diver/Michael (R.I.P) for his initial findings on these formats.

### Item PRF Format
The Item PRF (\[PR\]o\[F\]ile) defines basic constant data for an item, without any user properties.  These are used within the SOM_EDITOR only as loose files, and are how items are registered for use in the editor.

```c
typedef struct  // BYTE LENGTH: 16.
{
    /* 0x00 */ u32 unkx00;          // Unknown.
    /* 0x04 */ u8 slotKeyID;        // Special ID which must match with an objects to be able to slot the item into the object. See Object PRF.
    /* 0x05 */ u8 unkx05;           // Unknown.
    /* 0x06 */ u16 unkx06;          // Unknown.
    /* 0x08 */ u32 unkx08;          // Unknown.
    /* 0x0C */ u32 unkx0C;          // Unknown.
} ITEM_PRF_DATA_USABLE;

typedef struct  // BYTE LENGTH: 16.
{
    /* 0x00 */ u8 swingAnimation;   // The animation ID (in arm.mdl) to use for the swing
    /* 0x01 */ u8 soundDelay;       // Delay after swing when sound will play
    /* 0x02 */ s16 soundID;         // The sound effect to play for the swing.
    /* 0x04 */ u8 hitWindowStart;   // The starting frame where hits will register on an entity
    /* 0x05 */ u8 hitWindowEnd;     // The ending frame where hits will no longer register on an entity
    /* 0x06 */ u16 attackArc;       // The arc of the attack in degrees, either side of the player.
    /* 0x08 */ f32 attackRange;     // The range of the attack. 1 = 1 metre
    /* 0x0C */ u16 unkx0C;          // Could be padding? Not sure. Sound pitch maybe?.. (needs testing)
    /* 0x0E */ u8 magicWindowStart; // The starting frame where sword magic can be cast
    /* 0x0F */ u8 magicWindowEnd;   // The ending frame where sword magic can no longer be cast
} ITEM_PRF_DATA_WEAPON;

typedef struct  // BYTE LENGTH: 16.
{
    /* 0x00 */ u8 equipType;        // The type of this piece of armour. 0 = Helm, 1 = Body, 2 = Arms, 3 = Boots, 4 = Suit, 5 = Shield, 6 = Accessory
    /* 0x01 */ u8 unkx01;           // Unknown. These first few values are mixes of either 00 or FF (probably signed), They probably have _some_ purpose...
    /* 0x02 */ u16 unkx02;          // Unknown. ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x04 */ u32 unkx04;          // Unknown. ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x08 */ u32 unkx08;          // Unknown.
    /* 0x0C */ u32 unkx0C;          // Unknown.
} ITEM_PRF_DATA_ARMOUR;

typedef struct  // BYTE LENGTH: 88
{
    /* 0x00 */ char name[31];               // Profile name. S-JIS, 15 usable 2 byte characters + null terminator.
    /* 0x1F */ char modelFile[31];          // Model name.   ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x3E */ u16 type;                    // type. 0 = Usable, 1 = Weapon, 2 = Armour
    /* 0x40 */ float menuElevationOffset;   // Elevation offset in the menu
    /* 0x44 */ ushort menuTiltDegrees;      // Tilt of the item when displayed in the menu.
    /* 0x46 */ ushort worldTiltDegrees;     // Tilt of the item when displayed in the world.

    /* 0x48 */ union {
        /* ---- */ ITEM_PRF_DATA_USABLE usableData;  // Data when the PRF type == usable
        /* ---- */ ITEM_PRF_DATA_WEAPON weaponData;  // Data when the PRF type == weapon
        /* ---- */ ITEM_PRF_DATA_ARMOUR armourData;  // Data when the PRF type == armour

        /* ---- */ u8 raw[16];                      // Raw Data
    };
} ITEM_PRF;
```

### Item PR2 Format
The Item PR2 Format acts as a simple database of PRF files, and is used to store used PRF types and base item data within the editor and games.

```c
typedef struct  // BYTE LENGTH: Variable, at least 4.
{
    /* 0x00 */ u32 itemNum;
    /* 0x04 */ OBJECT_PRF items[itemNum];
} ITEM_PR2;
```