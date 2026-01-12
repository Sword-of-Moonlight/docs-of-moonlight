
### Overview
Like objects, enemies are made up of the same three "database" files (PRF/PR2/PRM). 

### Special Thanks
- HwitVulf: Initial PRF findings
- Holy_Diver: Additional PRF findings
- kurobake: Additional PRF findings

### Enemy PRF Format
The enemy PRF (\[PR\]o\[F\]ile) defines basic constant data for an enemy, without any user properties.  These are used within the SOM_EDITOR only as loose files, and are how enemies are registered for use in the editor.

```c
typedef struct  // BYTE LENGTH: 21
{
    char attackName[21];
} ENEMY_PRF_ATTACK_NAME;

typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u16 itemNum;           // The number of items in the list
    /* 0x02 */ u16 firstItemOffset;   // Absolute file offset to the first item in the list
} ENEMY_PRF_LIST;

typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u8 delay;    // Delay (in frames, from the start of the animation) for sound to play. 0 = Sound doesn't play.
    /* 0x01 */ s8 pitch;    // Pitch to play the sound at. (-24 to 24 half steps/semi tones/half tones, i.e two octave range)
    /* 0x02 */ s16 soundID; // The ID of the sound to play. -1 = none
} ENEMY_PRF_SND_ITEM;

typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u8 delay;    // Delay (in frames, from the start of the animation) for the effect to spawn. 0 = SFX doesn't spawn.
    /* 0x01 */ u8 cpID;     // ID of a control point to use for the root of the effect
    /* 0x02 */ s16 sfxID;   // The ID of an effect to spawn. -1 = none
} ENEMY_PRF_SFX_ITEM;

typedef struct  // BYTE LENGTH: Variable, at least 564
{
    /* 0x000 */ char prfFile[31];                       // The name of the origin PRF file
    /* 0x01F */ char modelFile[31];                     // The name of the model file
    /* 0x03E */ u8 unkx3e;                              // Unknown. Always zero in known files. Test.
    /* 0x03F */ u8 unkx3f;                              // Unknown. Always zero in known files. Test.
    /* 0x040 */ u8 useExternalTexture;                  // 0 = No External Texture, 1 = Yes External Texture
    /* 0x041 */ char textureFile[31];
    /* 0x060 */ bitfield u8 {
        /* ----- */ movement : 4;                       // 4 bits to define movement. 0 = Grounded, 1 = Floating (No Move), 2 = Floating, 5 = (No Move, No Turn)
        /* ----- */ behaviour : 4;                      // 4 bits to define behaviour. These are flags. Bit 0 = ?, Bit 1 = Flying related?, Bit 2 = Evades, Bit 3 = Defends
    }
    /* 0x061 */ u8 trigger;                             // 0 - Cannot be triggered, 1 = Can be triggered
    /* 0x062 */ u8 unkx62;                              // Unknown. Always zero in known files. Test.
    /* 0x063 */ u8 unkx63;                              // Unknown. Always zero in known files. Test.
    /* 0x064 */ f32 colliderHeight;                     // Height of a cylinder collider.
    /* 0x068 */ f32 shadowRadius;                       // Radius of the blob shadow.
    /* 0x06C */ f32 colliderRadius;                     // Radius of the cylinder collider.
    /* 0x070 */ f32 f32x70;                             // Unknown.
    /* 0x074 */ f32 f32x74;                             // Unknown.
    /* 0x078 */ f32 f32x78;                             // Unknown.
    /* 0x07C */ u32 unkx7C;                             // Unknown. Always zero in known files. Test.
    /* 0x080 */ u8 directAHitDelays[3];                 // Hit delays (in frame values) for direct attack a
    /* 0x083 */ u8 directBHitDelays[3];                 // Hit delays (in frame values) for direct attack b
    /* 0x086 */ u8 directCHitDelays[3];                 // Hit delays (in frame values) for direct attack c
    /* 0x089 */ u8 defendWindowStartFrame;              // The start frame for defend window
    /* 0x08A */ u8 defendWindowEndFrame;                // The end frame for the defend window
    /* 0x08B */ u8 unkx8B;                              // Could be padding. Always 0, and does do a 4 byte align.
    /* 0x08C */ s16 effectID;                           // An ID of an effect to spawn on a CP.
    /* 0x08E */ u8 effectControlPointAnchor;            // Which control point (green) the effect is anchored to.
    /* 0x08F */ u8 effectAnimationRate;                 // How fast the effect animates. 1 = No animation.
    /* 0x090 */ u8 directAttackNum;                     // Number of direct attacks
    /* 0x091 */ u8 indrectAttackNum;                    // Number of indirect attacks
    /* 0x092 */ ENEMY_PRF_ATTACK_NAME attackNames[6];   // Name of each attack (even unused ones). S-JIS, 7 usable 2 byte characters + null terminator.
    /* 0x110 */ f32 unkx110;                            // Unknown.
    /* 0x114 */ f32 unkx114;                            // Unknown.
    /* 0x118 */ f32 unkx118;                            // Unknown.
    /* 0x11C */ f32 directARange;                       // Range (in meters) of direct attack a
    /* 0x120 */ f32 directBRange;                       // Range (in meters) of direct attack b
    /* 0x124 */ f32 directCRange;                       // Range (in meters) of direct attack c
    /* 0x128 */ u16 directAArc;                         // Arc (in degrees) of direct attack a
    /* 0x12A */ u16 directBArc;                         // Arc (in degrees) of direct attack b
    /* 0x12C */ u16 directCArc;                         // Arc (in degrees) of direct attack c
    /* 0x12E */ u16 unkx12E;                            // Unknown. Always zero in known files. Test.
    /* 0x130 */ u32 unkx130;                            // Unknown. Always zero in known files. Test.
    /* 0x134 */ ENEMY_PRF_LIST effectLists[32];         // There are always 32, which corrospond to the 32 animation IDs.
    /* 0x1B4 */ ENEMY_PRF_LIST soundLists[32];          // There are always 32, which corrospond to the 32 animation IDs.

    /**
     * Past 0x234 is a variable size buffer, which contains ENEMY_PRF_SFX_ITEM and ENEMY_PRF_SND_ITEM items - these are what the 'effectLists' and 'soundLists' point too.
    **/
} ENEMY_PRF;
```

### Enemy PR2 Format
The enemy PR2 Format acts as a simple database of PRF files, and is used to store used PRF types within the game.

```c
typedef struct  // BYTE LENGTH: Variable, at least 4096
{
    /* 0x0000 */ u32 prfOffset[1024]; // Each U32 is an offset in the file to a PRF. 0 = no PRF
    /* 0x1000 */ // -- First PRF file. ENEMY_PRF.
} ENEMY_PR2;
```