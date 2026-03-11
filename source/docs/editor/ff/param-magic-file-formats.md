### Overview
Magic files are a little unique from others, in that they are stored completely seperately from other PRFs in the general "my" folder, and additionally do not allow much customisation.

### Special Thanks
- Holy_Diver: Initial format findings.
- kurobake: Additional format findings.

### Magic PRF Format
The Magic PRF (\[PR\]o\[F\]ile) defines (very) basic constant data for a spell, without any user properties.  

```c
typedef struct
{
    /* 0x00 */ char name[31];       // Name of the PRF
    /* 0x1F */ u8 screenspace;      // If the spell effect is a screen space effect
    /* 0X20 */
} MAGIC_PRF;
```
