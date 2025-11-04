### Overview
SYS.DAT is the file for game system specific information. It's another sloppy direct to memory format, and as such is terrible to read.

### Sequence / Image
```c
typedef struct
{
    u8 titleSequenceMode;           // 0 = None, 1 = Video, 2 = Slideshow
    char titleSequenceVideo[31];    // When 'titleSequenceMode' is '1', this is the filename of a video. Null terminated.
    char titleImage[31];            // The image displayed on the title screen
    u8 openingSequenceMode;         // See 'titleSequenceMode'
    char openingSequenceVideo[31];  // See 'titleSequenceVideo'
    u8 gameEnd1SequenceMode;        // See 'titleSequenceMode'
    char gameEnd1SequenceVideo[31]; // See 'titleSequenceVideo'
    u8 gameEnd2SequenceMode;        // See 'titleSequenceMode'
    char gameEnd2SequenceVideo[31]; // See 'titleSequenceVideo'
    u8 gameEnd3SequenceMode;        // See 'titleSequenceMode'
    char gameEnd3SequenceVideo[31]; // See 'titleSequenceVideo'
    u8 staffSequenceMode;           // See 'titleSequenceMode'
    char staffSequenceVideo[31];	// See 'titleSequenceVideo'
    char staffSequenceStill[31];	// This is the filename for a still image displayed at the end of the staff sequence.
} SYSDAT_SEQIMAGE;
```

### Player
```c
typedef struct
{
    float walkSpeed;    // Speed of normal movement
    float dashSpeed;    // Speed of movement when dashing
    u16 turnSpeed;      // Turn speed in degrees per second
} SYSDAT_PLAYERSPEEDS;

typedef struct
{
    u16 startStrength;          // The starting strength of the player
    u16 startMagic;             // The starting magic of the player
    u16 startHP;                // The starting HP of the player
    u16 startMP;                // The starting MP of the player
    u32 startCoin;              // The starting amount of coin the player has
    u32 startExperience;        // The starting amount of experience the player has
    u8  startLevel;             // The starting level of the player
    u8  startEquipment[8];      // The starting equipment of the player
    u8  startInventory[251];    // The starting inventory of the player
} SYSDAT_PLAYERCONFIG;
```

### Classes
```c
typedef struct
{
    char characters[15];    // In S-JIS, this would be roughly 7 characters + the null terminator.
} SYSDAT_CLASSTEXT;

typedef struct
{
    SYSDAT_CLASSTEXT names[25];    // Max of 25 classes.
    u16 strengthThresholds[4];     // The minimum strength required for the row solve.
    u16 magicThreshold[4];         // The minimum magic required for the column solve.
} SYSDAT_CLASSDATA;
```

### Spells
```c
typedef struct
{
    u8 IDs[32];                         // The IDs of spells in the slots
    u8 levelRequired[32];               // The level required to unlock the spell in the slot.
} SYSDAT_SPELLDATA;
```

### Menu Configuration
```c
typedef struct
{
    u8 allowSaveInMenu;                 // If saving in menu should be enabled
    u8 enableEncumbrance;               // If player encumbrance should be enabled
    u8 defaultCompassType;              // The default compass type
    u8 defaultGaugeType;                // The default guage type (HP/MP/Stamina/Focus display)
    u8 padx4;                           // Unused ? Always 0.
    u8 defaultMenuStyle;                // The default menu type
} SYSDAT_MENUCONF
```

### Messages
```c
typedef struct
{
    char characters[41];    // In S-JIS, this would be roughly 20 characters + the null terminator.
} SYSDAT_MESSAGETEXT;

typedef struct
{
    SYSDAT_MESSAGETEXT messages[13];    // More than the amount required? Very odd...
} SYSDAT_MESSAGESA;

typedef struct
{
    SYSDAT_MESSAGETEXT messages[3];     // Extra messages, for some reason not stored above. ONLY 12 MESSAGES ARE USABLE IN SOM!!!!!!!
} SYSDAT_MESSAGESB;
```

### Counters
```c
typedef struct
{
    char name[31];
} SYSDAT_COUNTERTEXT;
```

### Layout
This structure doesn't actually exist in the SYS.DAT file, but offers an overview into how the above structures are arranged within it. Positions have been included here so you can snoop inside the file with a hexeditor if you want.

```c
typedef struct
{
    //                                                                                                          [POSITION]
    SYSDAT_SEQIMAGE sequencesAndImages;     // Sequence and Image setup                                         0x00000000
    u8 enableDash;                          // If dashing is enabled for the game                               0x000000FE
    u8 padx00FF;                            // Unused ? Always 0.                                               0x000000FF
    SYSDAT_PLAYERSPEEDS playerSpeed;        // Settings for player speed.                                       0x00000100
    u8 padx010A;                            // Maybe not padding, this actually unaligns the data...            0x0000010A
    SYSDAT_CLASSDATA classData;             // Class name data, including the row/column tier values            0x0000010B
    SYSDAT_SPELLDATA spellData;             // Spell unlock data.                                               0x00000292
    SYSDAT_MENUCONF menuConfiguration;      // Configuration for the menus/GUIs                                 0x000002D2
    SYSDAT_MESSAGESA messagesA;             // Block of 13 on screen messages. More than editable?              0x000002D8
    char coinSymbol[3];                     // 'COIN' symbol. 1 S-JIS + null terminator.                        0x000004ED
    SYSDAT_PLAYERCONFIG playerConfig;       // Initial player configuration (standard)                          0x000004F0
    SYSDAT_PLAYERCONFIG playerDebugConfig;  // Initial player configuration (debug)                             0x00000604
    u8 initialMap;                          // Starting map ID.                                                 0x00000718
    SYSDAT_COUNTERTEXT counterNames[1024];  // Event counter names.                                             0x00000719
    u8 padx8319;                            // Unused? Always 0.                                                0x00008319
    u16 sounds[16];                         // Event callable sound IDs.                                        0x0000831A
    char menuBackground[38];                // Menu background filename.                                        0x0000833A
    SYSDAT_MESSAGESB messagesB;             // Block of 3  on screen messages. More than editable?              0x00008360
    u8 menuSoundType;                       // ID of menu sound set to use.                                     0x000083DB
    u8 padx83DC;                            // Unused? Always 0.                                                0x000083DC
} SYSDAT_LAYOUT;
```