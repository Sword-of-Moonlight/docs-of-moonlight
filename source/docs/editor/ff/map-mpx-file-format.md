### Overview
MPX files are a compiled MAP file, plus MSM and MHM files. The data is directly storing the map data for play in the runtime.

### MPX Header
```c
typedef struct
{
    u32 flags;                      // BIT 0 = OPTIMIZED (BSP), BIT 1 = LIGHTMAPPED, OTHER BIT(S) = UNUSED/RESERVED.
    char mapName[32];               // The in game name of the map
    char musicFileName[32];         // The file name of the music to play on entering the map
    char mapImageAFileName[32];     // The file name of a map displayed when using an in game map item
    char mapImageBFileName[32];     //  ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^
    char mapImageCFileName[32];     //  ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^
} MPX_HEADER;
```
!!! information
    All character arrays must have a null terminator at the end of the string. 32 characters are just reserved. SoM likes to reuse buffers when it writes data to a file - so any characters after the null terminator but within the 32 bytes of a character array should be cleared before using the data.

### MPX Camera
```c
typedef struct
{
    float fov;      // The FoV for the camera in this map (seemingly ignored)
    float near;     // The near plane value of the camera
    float far;      // The far plane value of the camera
} MPX_CAMERA;
```

### MPX Enviroment
```c
typedef struct
{
    u8 colour[4];           // Colour of the directional light
    float direction[3];     // Direction the directional light is pointing in
} MPX_LIGHT_DIRECTIONAL;

typedef struct
{
    float fogDistance;                          // The distance of the fog
    u8 fogColour[4];                            // The colour of the fog. BGRX (X = unused)
    u8 ambientColour[4];                        // The ambinet colour of the map. BGRX (X = unused).
    MPX_LIGHT_DIRECTIONAL directionalLights[3]; // Directional lights in the map.
    u8 reserved[4];                             // Reserved/Unused data.
} MPX_ENVIROMENT;
```
!!! warning
    'ambientColour' is actually unused in rendering, since this is baked into the map. Likewise, 'directionalLights' is also ignored.

### MPX Player
```c
typedef struct
{
    float position[3];  // The position the player starts at (in raw value, not tiles)
    float direction;    // The direction of player facing at the start (in degrees)
} MPX_PLAYER;
```

### MPX Objects
```c
typedef struct
{
    u8 colour[4];       // The colour of the light
    float range;        // The maximum range of the light
    u8 affectsObjects;  // If the light should affect objects or not.
} MPX_OBJECT_FLAGS_LIGHTSOURCE;

typedef struct
{
    u16 declarationID;  // This is an ID into the PR2/PRO file
    u8 unkx02;          // Unknown.
    u8 animating;       // If the object starts out animating
    u8 visible;         // If the object starts out visible
    u8 unkx05[3];       // Unknown.
    float position[3];  // Position of the object (none tile)
    float rotation[3];  // Rotation of the object (eular angles)
    float scale;        // Uniform scale of the object

    union flags
    {
        MPX_OBJECT_FLAGS_LIGHTSOURCE lightSource;       // 9 bytes used, 23 bytes reserved.
        u8 raw[32];                                     // raw flag data
    };   
} MPX_OBJECT_INSTANCE;

typedef struct
{
    u32 objectCount;                            // The number of following object instances
    MPX_OBJECT_INSTANCE objects[objectCount];   // Array of object instances
} MPX_OBJECT_BLOCK;
```
!!! warning
    The flags data can contain any possible combination of types, so long as it fits within 32 bytes.
    This area has not been extensively researched per object type, and some findings may not be 100% correct until that research is complete.

### MPX Enemies and NPCs
```c
typedef struct
{
    u8 unkx00[4];           // Unknown.
    float position[3];      // Position of the entity (none tile)
    float rotation[3];      // Rotation of the entity (eular angles)
    float scale;            // Uniform scale of the entity
    u16 classID;            // This is an ID into the PR2/PRO file
    u8 unkx22[18];          // Unknown
} MPX_ENTITY_INSTANCE;

typedef struct
{
    u32 npcCount;                           // number of NPCs
    MPX_ENTITY_INSTANCE npcs[npcCount];     // Array of NPC instances
} MPX_NPC_BLOCK;

typedef struct
{
    u32 enemyCount;                             // number of enemies
    MPX_ENTITY_INSTANCE enemies[enemyCount];    // Array of enemy instances
} MPX_ENEMY_BLOCK;
```
!!! warning
    SoM appears to use the same data structure for both NPCs and enemies in the MPX files. These are not fully researched.

### MPX Items
```c
typedef struct
{
    u8 unkx00[28];
    u16 classID;
    u8 unkx1E[10];
} MPX_ITEM_INSTANCE;

typedef struct
{
    u32 itemCount;
    MPX_ITEM_INSTANCE items[itemCount];
} MPX_ITEM_BLOCK;
```
!!! warning
    Only enough is known about item blocks to traverse the data. Expect mostly the same as objects, so position,rotation,scale to be somewhere in here though.

### MPX World
```c
typedef struct
{
    u16 collisionMeshID;    // ID of an MHM file stored at the end of the MPX.
    u16 renderMeshID;       // This value is ignored, but can be used to track unique tiles.
                            // NOTE: when both of the above values are 0xFFFF, the tile is considered empty!

    float elevation;        // The elevation of the tile.
    u32 flags;              // Unexplored, but rotation, map icon and other flags must be stored here.
} MPX_TILE;

typedef struct
{
    u32 skyType;            // The type of sky used. No, this doesn't let you have more skies...
    u32 u32x04;             // Unknown. Unused?
    u32 width;              // Width of the tilemap
    u32 height;             // Height of the tilemap
    MPX_TILE tilemap[width * height];
} MPX_WORLD;
```

### MPX BSP
```c
typedef struct
{
	/**
     * Michael:
     *  This is the data responsible for detecting
     *  if cells are occluded in order to not draw
     *  them (when not hiding cells it shouldn't.)
     *
     *  It's an unexplored detail since SOM_MAP is
     *  able to disable it, but it still has to be
     *  crossed when not disabled by the first bit.
     *
     *  These structures are unpacked in the order
     *  they're defined below...
    **/

	struct struct1
	{
		ule32_t count;

		struct var_item
		{
			ule32_t unknown1;

			lefp_t xy1[2],xy2[2];

			ule32_t count;

			struct _item
			{
				ule32_t unknown2[2];
			};

			_item list[count];
		};

		var_item var_item_1[count];
	};

	struct struct2
	{
		ule32_t count;

		struct _item
		{
			ule32_t unknown1[4];

			lefp_t xy1[2],xy2[2];

			ule32_t unknown2[2];
				
			ule32_t unknown3;
		};

		_item list[count];
	};

	struct struct3
	{
		ule32_t count;

		struct _item
		{		
			lefp_t xy1[2],xy2[2];

			ule32_t unknown1[2];
		};

		_item list[count];
	};

    /**
     * Battenberg:
     *  This could be related to bits because of the alignment?
    **/
	uint8_t list[(struct1.count+7)/8*struct1.count];
} MPX_BSPBLOCK;
```
!!! warning
    This is nearly exactly copied from the SomEx source code! It's very rough, and this section of data has huge amounts of research to be done.

### MPX Textures
```c
typedef struct
{
    char filename[];    // Length is variable. This is a null terminated string.
} MPX_TEXTURE;

typedef struct
{
    u32 textureCount;                       // Number of textures
    MPX_TEXTURE textures[textureCount];     // Array of textures
} MPX_TEXTURE_BLOCK;
```

### MPX Render Meshes
```c
typedef struct
{
    float position[3];      // xyz of the vertex
    float texcoord[2];      // uv of the vertex
} MPX_VERTEX;

typedef struct
{
    u32 vertexCount;                    // number of vertices
    MPX_VERTEX vertices[vertexCount];   // array of vertices
} MPX_VERTEX_BLOCK;

typedef struct
{
    u16 textureID;      // ID of texture inside the texture block
    u16 numIndices;     // Number of indices in the MSX mesh
    u16 numVertices;    // Number of vertices in the MSX mesh
} MPX_MESH_MSX_HEADER;

typedef struct
{
    u32 blockVertexID;  // Index of a vertex within the global MPX vertex buffer (for position, texcoord)
    u8 colour[4];       // Colour of the vertex from the lightmap bake stage. BGRX (X = unused)
} MPX_MESH_MSX_PACKET;

typedef struct
{
    MPX_MESH_HEADER header;         // header of this mesh
    u16 indices[header.numIndices]; // indices to vertices ! INSIDE ! this mesh, not the global vertex buffer
    MPX_MESH_MSX_PACKET vertices[header.numVertices];   // vertex data for the MSX
} MPX_MESH_MSX_SUBMESH;

typedef struct
{
    u32 submeshCount;                               // Submesh count - split by textures
    MPX_MESH_MSX_SUBMESH submeshes[submeshCount];
} MPX_MESH_MSX;

typedef struct
{
    u32 mhmSize;
    MHM_LAYOUT collisionMesh;   // The size of the mesh is equaL TO mhmSize
} MPX_MHM;

typedef struct
{
    u32 numMHM;
    MPX_MHM mhms[numMHM];
} MPX_MHM_BLOCK;

```
!!! information
    The meshes inside MPX files are different to the other 4 types of meshes SoM already uses... I'm calling these 'MSX' meshes - but they don't officially have a name. MSX comes from the _MS_M file (which these are derived from), and the MP_X_ file (this, which these are inside).

    MSM meshes have their normals discarded when converted to MSX - they aren't needed any more since the normals have already been used during the light baking stage of map compilation.

    SoM is very inefficient in this area - and stores one mesh per single slot in the tile map...



### MPX Layout
This structure doesn't actually exist in MPX files, but offers an overview into how the above structures are arranged within it.

```c
typedef struct
{
    MPX_HEADER header;
    MPX_CAMERA camera;
    MPX_ENVIROMENT enviroment;
    MPX_PLAYER player;
    MPX_OBJECT_BLOCK objectBlock;
    MPX_ENEMY_BLOCK enemyBlock;
    MPX_NPC_BLOCK npcBlock;
    MPX_ITEM_BLOCK itemBlock;
    MPX_WORLD world;

    if (header.flags & 1)               //  THIS IS NOT INCLUDED WHEN header.flags first bit is NOT set.
        MPX_BSPBLOCK bspBlock;          //   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^   ^

    MPX_TEXTURE_BLOCK textureBlock;     // The file becomes completely unaligned after this.

    MPX_VERTEX_BLOCK vertexBlock;  // These vertices are shared by every single mesh in the MPX.
    MPX_MESH_MSX meshes[];         // Variable. There are as many meshes as there are -USED- tiles in MPX_WORLD.
    MPX_MHM_BLOCK mhmBlock;        // Variable. depends on number of mhms
} MPX_LAYOUT;
```