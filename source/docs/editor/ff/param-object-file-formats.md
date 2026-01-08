### Overview
Objects (as far as data goes) are made up of three file formats in Sword of Moonlight: .PR2, .PRM, .PRF.  Each of these files handles a different part of object definition.

### Special Thanks
- HwitVulf: Initial PRF findings.
- Holy_Diver: Further PRF findings.
- kurobake: Further PRF findings, validating prior findings.
- bbMercy: Testing and validating prior findings.

### Object PRF Format
The Object PRF (\[PR\]o\[F\]ile) defines basic constant data for an object, without any user properties.  These are used within the SOM_EDITOR only as loose files, and are how objects are registered for use in the editor.

```c
typedef struct  // BYTE LENGTH: 108
{
    /* 0x00 */ char name[31];               // Profile name. S-JIS, 15 usable 2 byte characters + null terminator.
    /* 0x1F */ char modelFile[31];          // Model name.   ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x3E */ u8 billboard;                // 0 or 1. If object is billboarded (Y axis only)
    /* 0x3F */ u8 openable;                 // 0 or 1. If object is openable (door, chest... unknown function - crashes game in certain circumstances)

    /* 0x40 */ f32 colliderHeight;          // Collider height
    /* 0x44 */ f32 colliderRW;              // Collider radius or height
    /* 0x48 */ f32 colliderRD;              // Collider radius or depth

    /* 0x4C */ f32 f32x4C;                  // Seemingly junk. 1.0F in a few files.

    /* 0x50 */ u8 colliderMode;             // 0 to 2. 0 = Box, 1 = Cylinder, 2 = Special (Traps?.. Probably mesh collision)
    /* 0x51 */ u8 hasScrollingTexture1;     // 0 to 2. 0 = no, 1 = horizontal, 2 = vertical. If the texture 1's UVs should be scrolled.
    /* 0x52 */ s16 objectClass;             // The type of object. 0 = Static, 1 = Light, 20 = Container, 21 = Chest, 22 = Corpse, 40 = Switch, 

    /* 0x54 */ s16 effectID;                // An ID of an effect to use on the object. Non billboard effects will be spawned at 0,0,0 in the world.
    /* 0x56 */ u8 effectControlPointAnchor; // Which control point (green) the effect is anchored to
    /* 0x57 */ u8 effectAnimationRate;      // How fast the effect animates. 1 = No animation

    /* 0x58 */ s16 loopingSoundFxID;        // ID of a sound effect to loop (trap only?). -1 = None.
    /* 0x5A */ s16 openingSoundFxID;        // ID of a sound effect to play when opening (chest/door only?). -1 = None.
    /* 0x5C */ s16 closingSoundFxID;        // ID of a sound effect to play when closing (chest/door only?). -1 = None.
    /* 0x5E */ u8 loopingSoundFxDelay;      // Delay before playing the looping SFX. 0 = Does not play (broken, or intentionally?)
    /* 0x5F */ u8 openingSoundFxDelay;      // Delay before playing the opening SFX. ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x60 */ u8 closingSoundFxDelay;      // Delay before playing the closing SFX. ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x61 */ s8 loopingSoundFxPitch;      // Pitch to play looping SFX (In semitones/keys: -24 to 24. 0 = normal)
    /* 0x62 */ s8 openingSoundFxPitch;      // Pitch to play opening SFX ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  
    /* 0x63 */ s8 closingSoundFxPitch;      // Pitch to play closing SFX ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  

    /* 0x64 */ s16 trapEffectID;            // 0 or 1. Effect for traps to produce. Projectile traps only?
    /* 0x66 */ u8 trapEffectOrientate;      // 0 or 1. If the effect should orientate itself in the same direction as the model?.. (needs test)
    /* 0x67 */ u8 trapEffectVisible;        // 0 or 1. If the effect is visible.

    /* 0x68 */ u8 loopAnimation;            // If animation should loop (assuming trap)
    /* 0x69 */ u8 invisible;                // If the object is invisible.
    /* 0x6A */ u8 slotKeyID;                // Special ID which must match with an items to be able to slot the item into the object. See Item PRF.
    /* 0x6B */ u8 allowXZRotation;          // If rotation is allowed on X and Z axis.
} OBJECT_PRF;
```

### Object PR2 Format
The Object PR2 Format acts as a simple database of PRF files, and is used to store used PRF types and base object data within the editor and games.

```c
typedef struct  // BYTE LENGTH: Variable, at least 4.
{
    /* 0x00 */ u32 itemNum;
    /* 0x04 */ OBJECT_PRF items[itemNum];
} OBJECT_PR2;
```

### Object PRM Format
The Object PRM (\[P\]a\[R\]a\[M\]eters) is used to store registry data from the editor.

```c
typedef struct  // BYTE LENGTH: 16.
{
    /* 0x00 */ f32 range;       // Range of the trap projectile
    /* 0x04 */ u8 slashDamage;  // Slash damage value
    /* 0x05 */ u8 smashDamage;  // Smash damage value
    /* 0x06 */ u8 stabDamage;   // Stab damage value
    /* 0x07 */ u8 flameDamage;  // Flame damage value
    /* 0x08 */ u8 earthDamage;  // Earth damage value
    /* 0x09 */ u8 windDamage;   // Wind damage value
    /* 0x0A */ u8 waterDamage;  // Water damage value
    /* 0x0B */ u8 holyDamage;   // Holy damage value
    /* 0x0C */ u8 statusAdd;    // Status which is added to the player. 0 = None, 1 = Poison, 2 = Paralyze, 3 = Dark, 4 = Curse, 5 = Slow, 9 = Default (!?)
    /* 0x0D */ u8 statusChance; // % chance of the statusAdd being added to the player.
    /* 0x0E */ u8 unkx0E;       // Unknown. Reserved?
    /* 0x0F */ u8 unkx0F;       //  ^  ^  ^  ^  ^  ^
} OBJECT_PRM_DATA_TRAP;

typedef struct  // BYTE LENGTH: 56.
{
    /* 0x00 */ char name[31];       // User name of the object.
    /* 0x1F */ u8 revealed;         // If the object is revealed by "secret uncovering effects"
    /* 0x20 */ f32 scale;           // Uniform scale of the object.
    /* 0x24 */ s16 pr2Index;        // Index into the PR2 file for base PRF data.
    /* 0x26 */ u16 unkx26;          // Unknown Use, Unknown Type.
    /* 0x28 */ union {
        /* ---- */ OBJECT_PRM_DATA_TRAP trapData;   // Data when the PRF is a trap of any type.

        /* ---- */ u8 raw[16];                      // Raw Data
    };
} OBJECT_PRM;

typedef struct  // BYTE LENGTH: 57,344. (Fixed size buffer)
{
    OBJECT_PRM items[1024];     // There is always 1024 items, which matches the amount of objects you can register in the parameter editor.
} OBJECT_PRM_LAYOUT;
```