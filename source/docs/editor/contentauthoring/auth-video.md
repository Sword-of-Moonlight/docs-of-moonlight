Setting up video files in SoM can be quite the challenge, certainly when you want them to work for a number of different users. Luckily, the hard work of puzzling out which codecs to use has been done for you already.

SoM takes advantage of [DirectShow](https://en.wikipedia.org/wiki/DirectShow), which is media playback solution Microsoft was pushing for windows in the late 1990's and early 2000's.  DirectShow worked by having access to a pool of possible codecs it could use to decode video and audio data from a video file.

The absolute most common codec across all windows versions, is Windows Media Video (WMV), which is Microsoft's implementation of MPEG4. 

### That's nice.
Yeah.  The basic idea of this page is not to give a full tutorial, but to tell you to use the following encoding for your video content:
- WMV for the video stream
- PCM16le for the audio stream

Using these will ensure that your video will play back through SoM on the majority of installs.