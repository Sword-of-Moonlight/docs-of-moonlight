### Overview
Welcome to the beast that is MDL. This format contains a static/vertex animated/bone animated model, and a mixture of either vertex or skeleton animations. This is hell.

### Special Thanks
- Holy_Diver: Initial Findings, overall structure findings, animation IDs.
- bbMercy: Reversing the vertex animation header/keyframe definition format.

### MDL Header
```c
typedef struct  // BYTE LENGTH: 16
{
    /* 0x00 */ u8 flags;                    // 0 = Static, 1 = Skeleton Animation, 4 = Vertex Animation
    /* 0x01 */ u8 numSkeletalAnim;          // Number of skeletal animations contained in the file
    /* 0x02 */ u8 numVertexAnim;            // Number of vertex animations contained in the file
    /* 0x03 */ u8 numInternalTexture;       // Number of PS1 Textures (TIM) stored inside the file
    /* 0x04 */ u8 numTmdObject;             // Number of PS1 Objects (TMD) stored inside the file
    /* 0x05 */ u8 numUvBlocks;              // Number of UV blocks inside the file, but always 1 (?)
    /* 0x06 */ u16 meshDataSize;            // Size of mesh data?
    /* 0x08 */ u16 padx08;                  // Unused according to Ghidra.
    /* 0x0A */ u16 padx0A;                  // Unused according to Ghidra.
    /* 0x0C */ u16 skeletonAnimDataSize;    // Size of the skeleton animation data block in units of 4
    /* 0x0E */ u16 vertexAnimDataSize;      // Size of the vertex animation data block in units of 4
} MDL_HEADER;
```

### PSX SVECTOR
```c
typedef struct  // BYTE LENGTH: 8
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

### MDL UV
Really one of the weirdest features of MDL is the UV block, not very understood how it connects - but if the data exists it comes directly after the object data.

```c
typedef struct  // BYTE LENGTH: 12
{
    /* 0x00 */ u8 u1;       // UV #1
    /* 0x01 */ u8 v1;
    /* 0x02 */ u16 cba;     // CLUT position on PSX - I think it's used as a texture ID here though
    
    /* 0x04 */ u8 u2;       // UV #2
    /* 0x05 */ u8 v2;
    /* 0x06 */ u16 tsb;     // These flags... Controls alpha blend rate (ABR), TPAGE id (could also be used as the texture ID maybe) and colour mode.
    
    /* 0x08 */ u8 u3;       // UV #3
    /* 0x09 */ u8 v3;
    
    /* 0x0A */ u8 u4;       // UV #4 (This could also be padding if any 3 point triangles use these structures)
    /* 0x0B */ u8 v4;
} MDL_UVS_ITEM;

typedef struct  // BYTE LENGTH: Variable, at least 4.
{
    /* 0x00 */ int num;
    /* 0x04 */ MDL_UVS_ITEM[num];
} MDL_UVS_BLOCK;
```

### MDL Primitives
```c
typedef struct
{
    /* 0x00 */ s16 type;        // = -1 = end of list
    /* 0x02 */ s16 count;       // = -1 = end of list
} MDL_PRIMITIVETAG;

typedef struct
{
    /* 0x00 */ u8 colourR;
    /* 0x01 */ u8 colourG;
    /* 0x02 */ u8 colourB;
    /* 0x03 */ u8 psxMode;
    /* 0x04 */ u16 normal0;
    /* 0x06 */ u16 vertex0;
    /* 0x08 */ u16 vertex1;
    /* 0x0A */ u16 vertex2;
} MDL_PRIMITIVE_X0000;

typedef struct
{
    /* 0x00 */ u8 u0;
    /* 0x01 */ u8 v0;
    /* 0x02 */ u16 cba;

    /* 0x04 */ u8 u1;
    /* 0x05 */ u8 v1;
    /* 0x06 */ u16 tsb;

    /* 0x08 */ u8 u2;
    /* 0x09 */ u8 v2;
    /* 0x0A */ u8 unkx09;   // Probably just padding.
    /* 0x0B */ u8 psxMode;

    /* 0x0C */ u16 normal0;
    /* 0x0E */ u16 vertex0;
    /* 0x10 */ u16 vertex1;
    /* 0x12 */ u16 vertex2;
} MDL_PRIMITIVE_X0001;

typedef struct
{
    /* 0x00 */ u8 u0;
    /* 0x01 */ u8 v0;
    /* 0x02 */ u16 cba;

    /* 0x04 */ u8 u1;
    /* 0x05 */ u8 v1;
    /* 0x06 */ u16 tsb;

    /* 0x08 */ u8 u2;
    /* 0x09 */ u8 v2;
    /* 0x0A */ u8 unkx09;   // Probably just padding.
    /* 0x0B */ u8 psxMode;

    /* 0x0C */ u16 normal0;
    /* 0x0E */ u16 vertex0;
    /* 0x10 */ u16 normal1;
    /* 0x12 */ u16 vertex1;
    /* 0x14 */ u16 normal2;
    /* 0x16 */ u16 vertex2;
} MDL_PRIMITIVE_X0004;
```

# MDL Vertex Animation 
Thanks to Mercy for deciphering this mess

```c
typedef struct  // BYTE LENGTH: 4
{
    /* 0x00 */ u8 vertexBufferID;       // ID of a vertex buffer to use for the frame
    /* 0x02 */ u8 postEmptyFrames;      // Empty keyframes between this and the next 
} MDL_VANIM_KEYFRAME_DEF;

typedef struct  // BYTE LENGTH: Variable, at least 2.
{
    /* 0x00 */ u16 animationID;                         // The ID of the animation. Used by SoM to pick which animation to play, a "name"
    /* 0x02 */ MDL_VANIM_KEYFRAME_DEF keyframeDefs[n];  // Keyframe definitions. N number of which are present, until a magic "00 00" keyframe is read. 
} MDL_VANIM_DEF;

typedef struct  // BYTE LENGTH: Variable, at least 8.
{
    /* 0x00 */ u16 numberOfAnimations;                      // The number of animations contained in the vertex animation block
    /* 0x02 */ u16 blockSize;                               // Size of the block (header + frame def only) in units of 4
    /* 0x04 */ s16 unkx04;                                  // Always -512
    /* 0x06 */ s16 unkx06;                                  // Always -2
    /* 0x08 */ MDL_VANIM_DEF animDefs[numberOfAnimations];  // Animation definitions
    /* 0xnn */ u16 unkxnn;                                  // Probably a terminator.
} MDL_VANIM_BLOCK_HEADER;

9
typedef struct
{
    /* 0x00 */ u8 vdiffSize;    // The number of difference elements that are contained within 
} MDL_VANIM_BUFFER;

typedef struct // BYTE LENGTH: Variable, at least 8.
{
    /* 0x00 */ MDL_VANIM_BLOCK_HEADER header;       
    /* 0xnn */ u16 frameBufferOffsetList[n];        // A list of offset, length is equal to max frame index in MDL_VANIM_DEF of header. Each in units of 4.
    /* 0xnn */ MDL_VANIM_BUFFER frameBuffers[n];    // A list of frame buffers.
} MDL_VANIM_BLOCK;

```