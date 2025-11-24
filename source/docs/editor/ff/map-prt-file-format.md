### Overview
PRT files are used to define a tile for the Sword of Moonlight editor.  This data type is only used while editing, and is stripped from the final game as data contained within is included in the MPX file.

### PRT Format
```c
typedef struct
{
    char msmFile[32];        // Name of the MSM file (render mesh)
    char mhmFile[32];        // Name of the MHM file (collision mesh)     
    u8 blinders;             // (According to Michael) Bits 0 - 3 = occludes in north, west, south and east respectively.
    u8 mapIconID;            // The icon to use for the icon in the in game map
    u8 flags;                // Bit 0 = poison, Bit 1 = Damage
    u8 reservedx43;          // Reserved.
    
    char bmpFile[32];        // Name of the BMP file for the editor icon
    
    char description[128];   // Description of the tile (S-JIS encoding)
} PRT_FILE;
```