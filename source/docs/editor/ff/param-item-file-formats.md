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
    /* 0x04 */ ITEM_PRF items[itemNum];
} ITEM_PR2;
```

### Item PRM Format
The Item PRM (\[P\]a\[R\]a\[M\]eters) is used to store registry data from the editor.

```c
typedef struct {  // BYTE LENGTH: 40.
    /* 0x00 */ f32 weight;          // Equip weight
    /* 0x04 */ u8 slashDef;         // Slash Defence
    /* 0x05 */ u8 smashDef;         // Smash Defence
    /* 0x06 */ u8 stabDef;          // Stab Defence
    /* 0x07 */ u8 fireDef;          // Fire Defence
    /* 0x08 */ u8 earthDef;         // Earth Defence
    /* 0x09 */ u8 windDef;          // Wind Defence
    /* 0x0A */ u8 waterDef;         // Water Defence
    /* 0x0B */ u8 holyDef;          // Holy Defence
    /* 0x0C */ u8 effectType;       // 0 = None, 1 = Dark, 2 = Curse, 3 = HP Recover, 4 = HP Drain, 5 = HP Absorb, 6 = MP Recover, 7 = MP Drain, 8 = MP Absorb, 9 = Strength Up, 10 = Magic Up, 11 = Poison Resist, 12 = Paralyse Resist, 13 = Dark Resist, 14 = Curse Resist, 15 = Slow Resist
    /* 0x0D */ u8 effectPotency;    // The strength at which effectType is applied at 
    /* 0x0E */ u8 unkx0E[26];       // Unknown Bytes. Always 0/garbage?
} ITEM_PRM_DATA_ARMOUR;

typedef struct {  // BYTE LENGTH: 40.
    /* 0x00 */ f32 weight;          // Equip weight
    /* 0x04 */ u8 slashDamage;      // Slash Damage
    /* 0x05 */ u8 smashDamage;      // Smash Damage
    /* 0x06 */ u8 stabDamage;       // Stab Damage
    /* 0x07 */ u8 fireDamage;       // Fire Damage
    /* 0x08 */ u8 earthDamage;      // Earth Damage
    /* 0x09 */ u8 windDamage;       // Wind Damage
    /* 0x0A */ u8 waterDamage;      // Water Damage
    /* 0x0B */ u8 holyDamage;       // Holy Damage
    /* 0x0C */ u8 effectType;       // 0 = None, 1 = Dark, 2 = Curse, 3 = HP Recover, 4 = HP Drain, 5 = HP Absorb, 6 = MP Recover, 7 = MP Drain, 8 = MP Absorb, 9 = Strength Up, 10 = Magic Up, 11 = Poison Resist, 12 = Paralyse Resist, 13 = Dark Resist, 14 = Curse Resist, 15 = Slow Resist
    /* 0x0D */ u8 effectPotency;    // The strength at which effectType is applied at 
    /* 0x0E */ u8 magicID;          // Magic to cast.
    /* 0x0F */ u8 unkx0F[25];       // Unknown Bytes. Always 0/garbage?
} ITEM_PRM_DATA_WEAPON;

typedef struct 
{
    /* 0x00 */ u16 hpRecover;       // The amount of hp recovered
    /* 0x02 */ u16 mpRecover;       // The amount of mp recovered
    /* 0x04 */ u8 curePoison;       // If poison status is removed by the item
    /* 0x05 */ u8 cureParalyse;     // If paralyse status is removed by the item
    /* 0x06 */ u8 cureDark;         // If dark status is removed by the item
    /* 0x07 */ u8 cureCurse;        // If curse status is removed by the item
    /* 0x08 */ u8 cureSlow;         // If slow status is removed by the item
    /* 0X09 */ u8 unknownBytes[23]; // Unknown bytes.
} ITEM_PRM_DATA_USABLE_RECOVERY;

typedef struct
{
    /* 0x00 */ s8 displayType;          // -1 = None, 0 = Automap, 1 = Picture, 2 = Zone Defined Map (#1), 3 = Zone Defined Map (#2), 4 = Zone Defined Map (#3)
    /* 0x01 */ char pictureFile[31];    // when displayType == 1. WARNING: CAN OVERFLOW
} ITEM_PRM_DATA_USABLE_DISPLAYMAP;

typedef struct {
    /* 0x00 */ u8 usableType;   // The type of usable. 0 = Normal, 1 = Recovery, 2 = Display Map, 3 = Assess, 4 = Reveals
    /* 0x01 */ u8 unusable;     // If the item cannot be used (weird anti logic, fromsoft). 0 = can use, 1 = can't use
    /* 0x02 */ u8 dontConsume;  // If the item is not consumed. 0 = consumed, 1 = not consumed
    /* 0X03 */ u8 unkx03[5];    // Unknown Bytes.

    /* 0x08 */ union
    {
        /* ---- */ ITEM_PRM_DATA_USABLE_RECOVERY recoveryData;      // Data when usableType == recovery
        /* ---- */ ITEM_PRM_DATA_USABLE_DISPLAYMAP displayMapData;  // Data when usableType == display map

        /* ---- */ u8 raw[32];  // I believe assess, reveals and normal don't have additional data.
    };
} ITEM_PRM_DATA_USABLE;

typedef struct  // BYTE LENGTH: 336.
{
    /* 0x000 */ s16 pr2Index;            // Index into the PR2 file for base PRF data.
    /* 0x002 */ char name[31];           // Name
    /* 0x021 */ char description[241];   // Description
    /* 0x112 */ u8 unkx112[16];          // Unknown Bytes. Always 0?
    /* 0x122 */ u8 priority;             // 0 = Default, 1 = Crucial (does not despawn)
    /* 0x123 */ u8 unkx123[5];           // Unknown Bytes. Always 0?

    /* 0x128 */ union {
        /* ----- */ ITEM_PRM_DATA_ARMOUR armourData;     // Data when the PRF is armour (equipment?)
        /* ----- */ ITEM_PRM_DATA_WEAPON weaponData;     // Data when the PRF is weapon (equipment?) 
        /* ----- */ ITEM_PRM_DATA_USABLE usableData;     // Data when the PRF is usable

        /* ----- */ u8 raw[40];                      // Raw Data
    };
} ITEM_PRM;

typedef struct  // BYTE LENGTH: 84,000. (Fixed size buffer)
{
    ITEM_PRM items[250];     // Always 250
} ITEM_PRM_LAYOUT;
```

!!! warning
    The pictureFile field in ITEM_PRM_DATA_USABLE_DISPLAYMAP <CAN> overflow! be aware if you have a filename to long, it will destroy n amount of subsiquent item definitions in the PRM file.