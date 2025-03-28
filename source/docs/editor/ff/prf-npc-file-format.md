# NPC [PR]o[F]ile File Format

### Overview
This file defines basic properties for an NPC asset in the Sword of Moonlight engine. Details such as the model file, collision and sounds are described here.

### C Style Specification
```c
typedef struct
{
    uint16_t numberOfItems; // The number of items in this entry
    uint16_t offsetOfItems; // The absolute offset to the items.
} NPC_FX_SND_ENTRY;

typedef struct
{
    uint8_t unkx00;     // One of these is control point ID, and one is the delay. Not sure which is what though.
    uint8_t unkx01;     // ^ ^ ^ ^
    uint16_t sfxID;
} NPC_FX_ITEM;

typedef struct
{
    uint8_t frameDelay; // The delay in frames from the start of the animation to play the sound
    int8_t pitch;       // The pitch to play the sound at (-24 to 24 half steps/semi tones/half tones, i.e +-two octave range)
    uint16_t sndID;     // related to the file names, '{sndID:D4}.snd' in C# string interpolation
} NPC_SND_ITEM;

typedef struct 
{
    char name[31];                  // The name of the profile
    char modleFile[31];             // The model file used by the profile
    int16_t unkx03E;                // Unknown. Always 0x0100 ?
    float collisionHeight;          // The height of the cylinder collider
    float shadowRadius;             // The radius of the blob shadow
    float collisionRadius;          // The radius of the cylinder collider
    int16_t unkx04A;                // Unknown.
    int16_t fxID;                   // The ID of the sprite FX to display on the NPC. -1 = NONE.
    uint8_t fxcpID;                 // The ID of the control point to display the fx on. (green channel)
    uint8_t fxFramerate;            // The framerate or speed of the FX? 
    int16_t unkx052;                // Unknown. Always 0x0000 ?
    float turnSpeed;                // The speed at which the NPC turns. Usually 2.
    uint8_t talkStandFFrames;       // The frame length of the talk forward animation.
    uint8_t talkStandLRFrames;      // The frame length of the talk left-right animation.
    uint8_t talkSitFFrames;         // The frame length of the talk forward + sitting animation.
    uint8_t talkSitLRFrames;        // The frame length of the talk left-right + sitting animation.
    uint8_t useExternalTexture;     // Set if the NPC uses an external texture for it's model.
    char externalTextureFile[31];   // The file name of the external texture.
    uint32_t unkx07C;               // Padding??? Always 0x00000000 ???????

    // Now comes a list of FX entries, linked to specific animation IDs. (I.E the second slot is the fx for the "walk" animation)
    NPC_FX_SND_ENTRY fxEntries[32];

    // And a list of SND entries, again linked to animation IDs. (I.E the first slot is the snds for the "idle" animation)
    NPC_FX_SND_ENTRY sfxEntries[32];

    // 0x0180 ... 0xnnnn
    // After this point the file becomes variable in size. The previously mentioned *Entries point into a buffer here,
    // where the 'NPC_FX_ITEM' and 'NPC_SND_ITEM' structures reside...
} NPC_PRF;
```