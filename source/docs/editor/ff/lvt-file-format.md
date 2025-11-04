### Overview
LVT files (Level Table ?) are how SoM stores data for player level progression, aka 'experience graphs'. It is a direct to memory format, and as such does not contain any parsing information.

### LVT Level Entry
```c
typedef struct
{
    u32 experienceRequired;     // Total experience required for this level to be held
    u8  addedHP;                // The amount of HP given when gaining this level
    u8  addedMP;                // The amount of MP given when gaining this level
    u8  addedStrength;          // The amount of strength points given when gaining this level
    u8  addedMagic;             // The amount of magic points given when gaining this level
} LVT_TableEntry;
```

### LVT Layout
This structure doesn't actually exist in LVT files, but offers an overview into how the above structures are arranged within it.

```c
typedef struct
{
    LVT_TableEntry entries[99]; // Always 99 entries.
} LVT_LAYOUT;

```

!!! information
    There is always exactly 99 entries in this table. The first entry is always zero for each field, as the first level values are assigned to the player. It's _unknown_ if values being in level 1 would actually be added to the starting stats.

    If you want less than 99 levels, simply filling the remaining slots past your final level with 0's will stop the level from increasing.

    Despite there always being 99 entries, it's probably good practice to calculate the number of entries in a LVT with the following calculation: _(lvtFileSize / 8)_...