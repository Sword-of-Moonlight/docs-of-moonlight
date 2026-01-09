### Overview
SND Files are almost exactly a copy of WAVEFORMATEX from the WAV format

### SND Header
```c
typedef struct // BYTE LENGTH: 22
{
    /* 0x00 */ u16 wavFormatType;  // The wave audio type (usually WAVE_FORMAT_PCM, 1)
    /* 0x02 */ u16 channelNum;     // The number of channels, 1 = mono, 2 = stereo etc.
    /* 0x04 */ u32 sampleRate;     // The sample rate of the file. normal SoM SNDs are usually 11025Hz.
    /* 0x08 */ u32 byteRate;       // The byte rate of the file. (((bitsPerSample >> 3) * sampleRate) * channelNum)
    /* 0x0C */ u16 blockAlignment; // The sample block alignment. Usually 2.
    /* 0x0E */ u16 bitsPerSample;  // Number of bits per sample. Normal SoM SNDs use 16.
    /* 0x10 */ u16 cbSize;         // Unused cbSize field - used for extra data after the header in WAVEFORMATEX but doesn't work here.
    /* 0x12 */ u32 bufferSize;     // Size of the sample buffer (in bytes)
} SND_HEADER;
```

### SND Layout
```c
typedef struct  // BYTE LENGTH: variable, at least 22.
{
    /* 0x00 */ SND_HEADER header;                     // SND Header
    /* 0x16 */ u8 sampleBuffer[header.bufferSize];    // PCM Sample Buffer
} SND_LAYOUT;
```

!!! warning
    This structure doesn't actually exist in SND files, but offers an overview into how a file in the format will be arranged.
