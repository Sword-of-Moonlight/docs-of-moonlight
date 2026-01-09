
### Overview
Like objects, NPCs are made up of the same three "database" files (PRF/PR2/PRM). 

### Special Thanks
- Holy_Diver: Initial PRF findings

### NPC PRF Format
The npc PRF (\[PR\]o\[F\]ile) defines basic constant data for an NPC, without any user properties.  These are used within the SOM_EDITOR only as loose files, and are how NPCS are registered for use in the editor.

```c
typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u16 itemNum;           // The number of items in the list
    /* 0x02 */ u16 firstItemOffset;   // Absolute file offset to the first item in the list
} NPC_PRF_LIST;

typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u8 delay;    // Delay (in frames, from the start of the animation) for sound to play. 0 = Sound doesn't play.
    /* 0x01 */ s8 pitch;    // Pitch to play the sound at. (-24 to 24 half steps/semi tones/half tones, i.e two octave range)
    /* 0x02 */ s16 soundID; // The ID of the sound to play. -1 = none
} NPC_PRF_SND_ITEM;

typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u8 delay;    // Delay (in frames, from the start of the animation) for the effect to spawn. 0 = SFX doesn't spawn.
    /* 0x01 */ u8 cpID;     // ID of a control point to use for the root of the effect
    /* 0x02 */ s16 sfxID;   // The ID of an effect to spawn. -1 = none
} NPC_PRF_SFX_ITEM;

typedef struct  // BYTE LENGTH: variable, at least 384
{
    /* 0x000 */ char name[31];                   // Profile name. S-JIS, 15 usable 2 byte characters + null terminator.
    /* 0x01F */ char modelFile[31];              // Model name.   ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
    /* 0x03E */ u8 unkx3E;                       // Unknown. Usually 0. Needs test.
    /* 0x03F */ u8 unkx3F;                       // Unknown. Usually 1. Needs test.
    /* 0x040 */ f32 colliderHeight;              // Height of a cylinder collider.
    /* 0x044 */ f32 shadowRadius;                // Radius of the blob shadow.
    /* 0x048 */ f32 colliderRadius;              // Radius of the cylinder collider.
    /* 0x04C */ u16 unkx4C;                      // Unknown. Usually 0? Needs test.
    /* 0x04E */ s16 effectID;                    // An ID of an effect to spawn on a CP.
    /* 0x050 */ u8 effectControlPointAnchor;     // Which control point (green) the effect is anchored to.
    /* 0x051 */ u8 effectAnimationRate;          // How fast the effect animates. 1 = No animation.
    /* 0x052 */ s16 unkx52;                      // Unknown. Usually 0? Needs test.
    /* 0x054 */ f32 turnSpeed;                   // The turn speed of the NPC. 0 = no turn.
    /* 0x058 */ u8 standTalkForwardDelayFrames;  // Delay (in frames from the start of the talk forward animation) before running an event.
    /* 0x059 */ u8 standTalkSideDelayFrames;     // Delay (in frames from the start of the talk side animation) before running an event. 
    /* 0x05A */ u8 sitTalkForwardDelayFrames;    // Delay (in frames from the start of the sitting talk forward animation) before running an event
    /* 0x05B */ u8 sitTalkSideDelayFrames;       // Delay (in frames from the start of the sitting talk side animation) before running an event
    /* 0x05C */ u8 useExternalTexture;           // If an external texture should be used. 0 = No, 1 = Yes.
    /* 0x05D */ char textureFile[31];            // Texture name. S-JIS, 15 usable 2 byte characters + null terminator.
    /* 0x07C */ u32 unkx7C;                      // Unknown. Usually 0? Padding? Needs test.

    /* 0x080 */ NPC_PRF_LIST effectLists[32];    // There are always 32, which corrospond to the 32 animation IDs.
    /* 0x100 */ NPC_PRF_LIST soundLists[32];     // There are always 32, which corrospond to the 32 animation IDs.

    /**
     * Past 0x180 is a variable size buffer, which contains NPC_PRF_SFX_ITEM and NPC_PRF_SND_ITEM items - these are what the 'effectLists' and 'soundLists' point too.
    **/
} NPC_PRF;
```

### NPC PR2 Format
The NPC PR2 Format acts as a simple database of PRF files, and is used to store used PRF types within the game.

```c
typedef struct  // BYTE LENGTH: Variable, at least 4096
{
    /* 0x0000 */ u32 prfOffset[1024]; // Each U32 is an offset in the file to a PRF. 0 = no PRF
    /* 0x1000 */ // -- First PRF file. NPC_PRF.
} NPC_PR2;
```