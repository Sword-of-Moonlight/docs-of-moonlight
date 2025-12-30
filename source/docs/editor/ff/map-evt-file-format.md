### Overview
EVT files are used to store map events.

Thanks to Mercy for documenting this format.

### EVT Header
```c
typedef struct
{
    u32 eventCount; // Number of events in the EVT file (always 1024)
} EVT_HEADER;
```

### EVT Definition
```c
typedef struct
{
	u16 compareType;    // Condition Mode. 0 = None, 1 = Item Quantity, 2 = Gold Quantity, 3 = Strength, 4 = Magic, 5 = Level, 6 = Counter
	u16 compareID;      // When compareType == 1: 0x00->0xF9 = Item ID, 0xFF = None. When compareType == 6: 0x0000->0x3FF = Counter ID
	u16 comparedValue;  // The value to check against in various modes (signed?..)
	u16 comparisonType; // 0 = Equals, 1 = Not Equals, 2 = Greater Than, 3 = Less Than
} EVT_CONDITION;

typedef struct
{
    u32 payloadOffset;           // op code offset in file.
    EVT_CONDITION startCondition;   // Condition to be met in order for page to run
} EVT_PAGE;

typedef struct
{
    char name[31];           // S-JIS
    s8 targetType;           // 0 = NPC, 1 = ENEMY, 2 = OBJECT, -2 = SYSTEM, -1 = NONE
    s16 targetID;            // Map ID of npc/enemy/object
    u8 triggerType;          // 0x01 = Examine, 0x02 = Use Item, 0x04 = Approach (Rectangle), 0x08 = Approach (Circle), 0x10 = Death (Enemy/NPC), 0x20 = Always On, 0x40 = Use Item
    u8 triggerItem;          // 0x00->0xF9 = Item ID, 0xFF = None
    u16 triggerCone;         // View activation cone (degrees)
    u16 u16x26;			     // Padding ?
    f32 triggerRectWE;       // West -> East rectangle Coverage
    f32 triggerRectNS;       // North -> South rectangle coverage
    f32 triggerRadius;       // Trigger radius
    EVT_CONDITION condition; // Event start condition.
    EVT_PAGE pages[16];      // Event page declarations.
} EVT_DEFINITION;
```

### EVT Operations
EVT operations can either be present in a file or not, and in any order. These are what is stored at the payloadOffset within an EVT_PAGE.

```c
typedef struct
{
    s16 opID;   // Operation ID. See operation definitions below for valid/known values.
    u16 opSize; // Size of the operation (including the this EVT_OPCODE.)
} EVT_OPCODE;

typedef struct
{
    EVT_OPCOCE op;              // opID = 0, opSize = variable
    char text[op.opSize-8];
    u32 padding;
} EVT_OP_DISPMESSAGE;

typedef struct
{
    EVT_OPCOCE op;              // opID = 1, opSize = variable
    u32 textColour;             // RGBX format (8-bits per component)
    u16 fontWeight;             // GDI font weight (0-550 = Normal, 551-999 Bold)
    u16 padding1;
    char text[*];
    u8 textTerminator;
    char fontName[*];
    u8 fontNameTerminator;
    u8 padding2;
} EVT_OP_DISPMESSAGEFMT;

90 - Change Counter's Value
    | event structure |
    00 # buffer
    0C 00 # number of bytes for this event block (starting from EventType)
    46 00 # Counter
    64 00 #Value
    01 # bool 00 - 01 Value / Counter's Value
    02 # Conditional 00 - 03 (SetTo, Raise, Lower)
    00 00 # buffer


typedef struct
{
    EVT_OPCODE op;              // opID = 144, opSize = 12
    u16 value;                  // A exact value to use
    u16 targetCounter;          // A counter to get the value from
    u8 useTarget;               // When 0 = use exact value, when 1 = use value from targetCounter
    u8 mode;                    // 0 = Set To, 1 = Increment By, 2 = Decrement By
    u16 padding;
} EVT_OP_CHANGECOUNTER;

typedef struct
{
    EVT_OPCODE op;              // opID = 145, opSize = variable
    s16 target;                 // 0x000 -> 0x3FF = other, 0xFFFF = self
    u8 changeType;              // 0 = Forward, 1 = Back, 2 = Specific
    u8 changeSpecificID;        // ID to use when changeType == 2 (0 - 15)
} EVT_OP_CHANGEPAGE;

typedef struct
{
    EVT_OPCODE op;              // opID = -1, opSize = 4
} EVT_OP_RETURN;
```