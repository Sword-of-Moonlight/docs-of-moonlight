### Overview
SND files are weird propritary nonsense cooked up by FromSoftware, instead of using a standard format such as WAV... Which ironically, they already _were_ using, but only for music. 

### C Style Specification
```c
typedef struct 
{
    u16 u16x00;         // Unknown. Usually 1.
    u16 u16x02;         // Unknown. Usually 1.
    u32 sampleRate1;    // Sample Rate #1. Usually 11025.
    u32 sampleRate2;    // Sample Rate #2. Usually 22050.
    u16 u16x0C;         // Bytes per sample? Usually 2.
    u16 bitdepth;       // Bits per Sample. Usually 16.
    u32 bufferSize;     // The number of bytes which make up the sound buffer
    u16 u16x14;         // Unknown. Usually 0.
} SND_HEADER;

// This structure doesn't exist in SND files, and is just representation of how the data is laid out.
typedef struct
{
    SND_HEADER header;              // SND Header
    u8 sampleBuffer[bufferSize];    // PCM Sample Buffer
} SND_LAYOUT;
```