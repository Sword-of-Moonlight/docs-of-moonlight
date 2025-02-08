Here is a small list of compatibility fixes and tips in the form of a troubleshooting guide, to help you get SoM games running on your hardware. But first...

### A brief note about SomEx...
A lot of these fixes won't apply to games which were created using SomEx, since a lot of these issues don't exist in SomEx. It's unknown if adding these compatibility fixes on top of SomEx will cause any issues, so it's better than you don't - and in order to do that, you're going to need to know how to identify if you're game is running SomEx.

In the root folder of the game (Where the 'DATA' and 'PARAM' folders are located), look for a file matching this format: '<GAME NAME> Ex.ini' (You might not see the .ini extension, if your operating system is not set up to view file extensions). If you find this file, you can rest assured that your game is running SomEx and you won't have to go through this guide. Enjoy!

### You: "Why not just put SomEx on everything?"
That's a nice thought, sadly however SomEx requires the game developer to have used a modified editor which _also_ uses SomEx, otherwise the game won't be able to take advantage of the fixes and features SomEx provides. A very old version of SomEx (literally, the first version) _does exist_ which can be dropped on top of an existing game, however it also suffers from a whole pages worth of its own issues which makes using it worse than just following the compatibility fix guide.

### You: "Well, why don't the developers use it!?"
SomEx is _extremely_ complicated to set up and use, a lot of folk working on SoM games just don't want to go through the required steps to get it running on their system, after already going through the trouble of getting the basic SoM editor running... On top of that, SomEx has basically _zero_ documentation, so even after a developer has got it set up and installed - they have no clue how to actually use it in most situations.

### You: "Then document it, scrub!!!"
Hey, come on now... (We're working on it!)

### Problem: "The controls are crap! Start/Back move the camera up/down?!"
While the keyboard controls are an issue that can't be resolved (yet), the controller issue is simply a matter of SoM being made with old DINPUT7 controllers in mind, and that your controller is likely made with XINPUT (or at the very least DINPUT8) in mind. We can solve that by using [dinputto8](https://serve.swordofmoonlight.com/Games/Fixes/dinput.dll). Simply drag the downloaded file into the game root.

### Problem: "The compass looks weird and glitchy! There's black boxes around it!"
Unsuprisingly SoM is based on DirectX7 - this means it doesn't play nice with modern GPUs, which haven't included true support for anything older than DirectX11 for a while now. You can fix this issue by grabbing a copy of [dgVoodoo2](https://serve.swordofmoonlight.com/Games/Fixes/dsoal-graphics-fix.7z) from us (preconfigured), or from the [official website](https://dege.freeweb.hu/dgVoodoo2/dgVoodoo2/). To install our preconfigured dgVoodoo2, first close the game - then open the archive and drag all the files into the game root. If you're using the one from the official website (it's probably more up to date), it's still recommended you download our version and use our dgvoodoo.conf file

### Problem: "Sounds keep cutting out!"
How unfortunate. Sadly, FromSoftware seemingly put the worst programmer possible in charge of writing up the code responsible for sound in the runtime, so it's extremely unreliable at best. Also sadly, the version of DirectSound SOM is using doesn't seem to play nice with modern hardware. While not a complete fix (you're still going to experience sound fx dropping occasionally), you can use our preconfigured [DSOAL](https://serve.swordofmoonlight.com/Games/Fixes/dsoal-sound-fix.7z) to at least improve sound related issues. To install DSOAL, first make sure the game is closed - then open the archive and drag all the files from within it to game root.

### Problem: "Theres holes in the map at the edges of my screen!"
Oh no, really? Looks like you're trying to play a SoM game in a non 4:3 aspect ratio resolution. The only solution for this is to boot the game in a 4:3 resolution, such as **640x480**, **1024x768**, **1280x960**, **1440x1080**, **1600x1200** or **1920x1440**. It's best to choose a 4:3 resolution that is smaller than your native resolution. Even better is a 4:3 resolution which matches your native horizontal size, since then you will not suffer any scaling artifects - for example, if your native resolution is **1920x1080**, you should use **1440x1080**... Or, if you're a fancy pants with a 2K monitor (**2560x1440**), you should use **1920x1440**.

### Problem: "My screen is black, but I can hear audio in the background!"
Wow! It's most likely the game your trying to play has video cutscenes (that's a rare treat!). What you're seeing is result of your Windows installation missing the codecs which are required for the movie to be decoded by the operating system (most likely CINEPAK). Thankfully, the solution is just to install a codec pack. We recommend [CCCP](https://serve.swordofmoonlight.com/Games/Fixes/combined-community-codec-pack.exe) (LOL, "Combined Community Codec Pack") because it contains nearly every codec imaginable.

### Problem: "The game is running like shit/crashed after a long session!"
Classic. Sadly SoM leaks a lot of memory, so if you're moving between maps a lot - you're going to flood process with RAM it can't release, and this is the result. While this issue cannot be fixed without extensive code re-writes, it can be mitigated by using a LAA (Large address aware) patcher, which boosts the amount of RAM a 32-bit application can use from 2GB to 4GB, theoretically doubling the time it takes to drop perf or crash. To do this, download the [LAA Patcher](https://serve.swordofmoonlight.com/Games/Fixes/laa_patcher.exe) and run it. It'll ask you to select an executable - that's where you add the SoM game executable you want to patch, and the tool should explain the rest.

### Problem: "The game is super bright/dark!"
I have no idea.