### Overview
Welcome to the beast that is MDL. This format contains a static/vertex animated/bone animated model, and a mixture of either vertex or skeleton animations. This is hell.

Special thanks to Micheal/Holy_Diver and Mercy for their findings on this format.

### MDL Header
```c
typedef struct  // BYTE LENGTH: 16
{
    /* 0x00 */ u8 flags;                    // 0 = Static, 1 = Vertex Animation, 4 = Skeleton Animation
    /* 0x01 */ u8 numVertexAnim;            // Number of vertex animations contained in the file
    /* 0x02 */ u8 numSkeletalAnim;          // Number of skeletal animations contained in the file
    /* 0x03 */ u8 numInternalTexture;       // Number of PS1 Textures (TIM) stored inside the file
    /* 0x04 */ u8 numTmdObject;             // Number of PS1 Objects (TMD) stored inside the file
    /* 0x05 */ u8 unkx05;                   // No clue. Always 1?
    /* 0x06 */ u16 unkx06;                  // Seems to be an offset to the texture data?
    /* 0x08 */ u16 objectDataSize;          // Size of TMD Object Data Block
    /* 0x0A */ u16 unkx08;                  // No clue.
    /* 0x0C */ u16 skeletonAnimDataSize;    // Size of the skeleton animation data block
    /* 0x0E */ u16 vertexAnimDataSize;      // Size of the vertex animation data block
} MDL_HEADER;
```

### PSX SVECTOR
```c
typedef struct
{
    /* 0x00 */ s16 VX;
    /* 0x02 */ s16 VY;
    /* 0x04 */ s16 VZ;
    /* 0x06 */ s16 VW;  // Unused padding from PSX.
} PS1_SVECTOR;
```

!!! information
    Yes - AGAIN FromSoft just stole something from the PS1 SDK. The SVECTOR here is used to define normals (which are Q3.12 Fixed Point) and vertices, which are alledly stored as milimeters in regular short format, not fixed point.

### TMD Object
```c
typedef struct // BYTE LENGTH: 24
{
    /* 0x00 */ u32 vertexBase;      // Units of 4. (<< 2 to restore actual offset)
    /* 0x04 */ u32 vertexNum;       // Number of vertices
    /* 0x08 */ u32 normalBase;      // Units of 4.
    /* 0x0C */ u32 normalNum;       // Number of normals
    /* 0x10 */ u32 primitiveBase;   // Units of 4.
    /* 0x14 */ u32 primitiveNum;    // Number of primitives
    /* 0x18 */ s32 scale;           // Unused by MDL - but still here...
} TMD_OBJECT;
```

!!! information
    Yes - MDL uses PlayStation TMD Objects to define mesh data. The only tweaks is that the base is not an exact value any more - it is in units of four, and the size of the header is not included in the offset (though, that's the same is PS1)

    In order to restore the offsets, you must first take the size of the header (16 bytes), then add the base with a shift of 2 to the left.

    The data at the offsets you calculate are arrays of 'PS1_SVECTOR'.

!!! warning
    in addition to the above information, know that when the original SoM tools were building TMD_OBJECT, they neglected to set the offset to 0 where no data exists - if a base is none zero, but the number _is_ zero be assured that no data exists.