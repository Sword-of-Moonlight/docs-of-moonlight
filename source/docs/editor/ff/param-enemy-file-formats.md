
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

### Enemy PRM Format
The enemy PRM (\[P\]a\[R\]a\[M\]eters) is used to store registry data from the editor.

```c
typedef struct  // BYTE LENGTH: 24
{
    /* 0x00 */ u8 emphasis;                 // How prioritised this attack is, how often it is used or favoured?
    /* 0x01 */ u8 facePlayer;               // If enemy must be facing the player. (0 = No, 1 = Yes)
    /* 0x02 */ u16 angle;                   // Indirect only. The angle at which the attack can be used. 
    /* 0x04 */ f32 minimumRange;            // Indirect only. The minimum range at which the attack can be used.
    /* 0x08 */ u32 unkx08;                  // Indirect only. The maximum range at which the attack can be used.
    /* 0x0C */ u8 slashAtk;                 // Slash attack
    /* 0x0D */ u8 smashAtk;                 // Smash attack
    /* 0x0E */ u8 stabAtk;                  // Stab attack
    /* 0x0F */ u8 flameAtk;                 // Flame attack
    /* 0x10 */ u8 earthAtk;                 // Earth attack
    /* 0x11 */ u8 windAtk;                  // Wind attack
    /* 0x12 */ u8 waterAtk;                 // Water attack
    /* 0x13 */ u8 holyAtk;                  // Holy attack
    /* 0x14 */ u8 statusEffect;             // 0 = None, 1 = Poison, 2 = Paralyse, 3 = Dark, 4 = Curse, 5 = Slow
    /* 0x15 */ u8 statusProbability;        // Chance for statusEffect to be applied
    /* 0x16 */ u16 unkx16;                  // Unknown. Likely padding.
} ENEMY_PRM_ATTACK;

typedef struct  // BYTE LENGTH: 488
{
    /* 0x000 */ s16 pr2Index;                           // Index into the PR2 file for base PRF data.
    /* 0x002 */ u8 unkx002;                             // Unknown
    /* 0x003 */ u8 unkx003;                             // Unknown
    /* 0x004 */ f32 scale;                              // Uniform scale of the enemy
    /* 0x008 */ char[31] name;                          // Name
    /* 0x027 */ char[236] description;                  // Description
    /* 0x113 */ u8 unkx113[21];                         // Unknown. Possibily more description bytes.
    /* 0x128 */ u16 hp;                                 // HP amount of the enemy
    /* 0x12A */ u8 unkx12A;                             // Unknown
    /* 0x12B */ u8 unkx12B;                             // Unknown
    /* 0x12C */ u8 basePhysicalDef;                     // Base physical defense
    /* 0x12D */ u8 baseMagicDef;                        // Base magic defense
    /* 0x12E */ u8 slashDef;                            // Slash defense
    /* 0x12F */ u8 smashDef;                            // Smash defense
    /* 0x130 */ u8 stabDef;                             // Stab defense
    /* 0x131 */ u8 flameDef;                            // Flame defense
    /* 0x132 */ u8 earthDef;                            // Earth defense
    /* 0x133 */ u8 windDef;                             // Wind defense
    /* 0x134 */ u8 waterDef;                            // Water defense
    /* 0x135 */ u8 holyDef;                             // Holy defense
    /* 0x136 */ u16 visionConeAngle;                    // vision cone angle
    /* 0x138 */ f32 visionDistance;                     // vision cone distance
    /* 0x13C */ u8 recognitionChance;                   // Chance that when player is in vision cone, they will be recognised/seen
    /* 0x13D */ u8 attackBehaviour;                     // 0 = Close, 1 = Far, 9 = Default !?
    /* 0x13E */ u8 attackReactTime;                     // When an enemy can attack, this is the time it will take before doing so
    /* 0x13F */ u8 attackFrequency;                     // When an enemy can attack, this is how often it will
    /* 0x140 */ f32 turnRate;                           // The turn speed of the enemy. 0 = no turn.
    /* 0x144 */ u8 defendBehaviour;                     // When an enemy is attacked, this is the action type to respond with (0 = None, 1 = Evade, 2 = Defend, 3 = Counter)
    /* 0x145 */ u8 defendProbability;                   // When an enemy is attacked, this is the chance they will defend
    /* 0x146 */ u8 defendBuff;                          // When an enemy is attacked and defends, this is the additional defence amount granted.
    /* 0x147 */ u8 counterAttack;                       // What attack an enemy should counter with (0 = direct a, 1 = direct b, 2 = direct c, 3 = indirect a, 4 = indirect b, 5 = indirect c)
    /* 0x148 */ u8 regainsHP;                           // If the enemy should regenerate HP when damaged. 0 = No, 1 = Yes
    /* 0x149 */ u8 unkx149;                             // Unknown. Needs test. (Possible isKillable from NPC?..)
    /* 0x14A */ u16 maxCoinDrop;                        // Maximum amount of coin to drop on death.
    /* 0x14C */ u16 expValue;                           // Amount of EXP to award on death
    /* 0x14E */ u8 droppedItemID;                       // Item to drop on death. 255 = None.
    /* 0x14F */ u8 droppedItemChance;                   // Item drop chance. % value.
    /* 0x150 */ f32 movementTriggerRange;               // When movementBehaviour == Trigger, the distance at which the enemy becomes active
    /* 0x154 */ u8 movementBehaviour;                   // How any enemy should move. 0 = Normal, 1 = Trigger, 2 = Wait (?), 3 = Stay (?)
    /* 0x155 */ u8 unkx155;                             // Unknown
    /* 0x156 */ u8 unkx156;                             // Unknown
    /* 0x157 */ u8 unkx157;                             // Unknown 
    /* 0x158 */ ENEMY_PRM_ATTACK directAttackA;         // Settings for direct attack a
    /* 0x170 */ ENEMY_PRM_ATTACK directAttackB;         // Settings for direct attack b
    /* 0x188 */ ENEMY_PRM_ATTACK directAttackC;         // Settings for direct attack c
    /* 0x1A0 */ ENEMY_PRM_ATTACK indirectAttackA;       // Settings for indirect attack a
    /* 0x1B8 */ ENEMY_PRM_ATTACK indirectAttackB;       // Settings for indirect attack b
    /* 0x1D0 */ ENEMY_PRM_ATTACK indirectAttackC;       // Settings for indirect attack c
} ENEMY_PRM;
```