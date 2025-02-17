Here is a small list of compatibility fixes and tips in the form of a troubleshooting guide, to help you get SoM games running on your hardware. But first...

### A brief note about SomEx...
A lot of these fixes won't apply to games which were created using SomEx, since a lot of these issues don't exist in SomEx. It's unknown if adding these compatibility fixes on top of SomEx will cause any issues, so it's better that you don't - and in order not to do that, you're going to need to know how to identify if your game is running SomEx.

In the game root, look for a file matching this format: '<GAME NAME> Ex.ini' (you might not see the .ini extension, if your operating system is not set up to view file extensions). If you find this file, you can rest assured that your game is running SomEx and you won't have to go through this section of the documentation. Enjoy!

### You: "Why not just put SomEx on everything?"
That's a nice thought, however sadly SomEx requires the game developer to have used a modified editor which _also_ uses SomEx, otherwise the game won't be able to take advantage of the fixes and features SomEx provides. A very old version of SomEx (literally, the first version) _does exist_ which can be dropped on top of an existing game, however being such an old version it suffers from its own issues which make using it less desirable than just troubleshooting the issues.

### You: "Well, why don't the developers use it!?"
SomEx is _extremely_ complicated to set up and use, a lot of folks working on SoM games just don't want to go through the required steps to get it set up and running on their system, after already going through the trouble of getting the basic SoM editor running... On top of that, SomEx has basically _zero_ documentation, so even after a developer has got it set up and installed - they have no clue how to actually use it in most situations.

### You: "Then document it, scrub!!!"
Hey, come on now... (We're working on it!)

### Problem: "The controls are crap! Start/Back move the camera up/down?!"
While the keyboard controls are an issue that can't be resolved (yet), the controller issue is simply a matter of SoM being made with old DINPUT7 controllers in mind, and that your controller is likely made with XINPUT (or at the very least DINPUT8) in mind. We can solve that by using [dinputto8](https://serve.swordofmoonlight.com/Games/Fixes/dinput.dll). Simply drag the downloaded file into the game root. Afterwards, you should be able to map the controls to a traditional tank control scheme.

### Problem: "The compass looks weird and glitchy! There's black boxes around it!"
Unsuprisingly SoM is based on DirectX7 - this means it doesn't play nice with modern GPUs, which haven't included true support for anything older than DirectX10 for a while now. You can fix this issue by grabbing a copy of [dgVoodoo2](https://serve.swordofmoonlight.com/Games/Fixes/dgvoodoo-graphics-fix.7z) from us (pre-configured), or from the [official website](https://dege.freeweb.hu/dgVoodoo2/dgVoodoo2/). To install our pre-configured dgVoodoo2, first close the game - then open the archive and drag all the files into the game root. If you're using the one from the official website (it's probably more up to date), it's still recommended you download our version and use our dgvoodoo.conf file.

### Problem: "Sounds keep cutting out!"
How unfortunate. Sadly, FromSoftware seemingly put the worst programmer possible in charge of writing up the code responsible for sound in the runtime, so it's extremely unreliable at best. Also sadly, the version of Direct Sound SoM is using doesn't seem to play nice with modern hardware. While not a complete fix (you're still going to experience sound fx dropping occasionally), you can use our pre-configured [DSOAL](https://serve.swordofmoonlight.com/Games/Fixes/dsoal-sound-fix.7z) to at least improve the issue. To install DSOAL, first make sure the game is closed - then open the archive and drag all the files into game root.

### Problem: "Theres holes in the map at the edges of my screen!"
Oh no, really? Looks like you're trying to play a SoM game in a non 4:3 aspect ratio resolution. The only solution for this is to boot the game in a 4:3 resolution, such as **640x480**, **1024x768**, **1280x960**, **1440x1080**, **1600x1200** or **1920x1440**. It's best to choose a 4:3 resolution that is smaller than your native resolution. Even better is a 4:3 resolution which matches your native resolution vertically since then you will not suffer any scaling artifects - for example, if your native resolution is **1920x1080**, you could use **1440x1080**... Or, if you're a fancy pants with a 2K monitor (**2560x1440**), you could use **1920x1440**. If non of these resolutions are avaliable in game, you can manually set it in the game configuration file ('<GAME NAME>.ini'), by setting the width and height fields.

### Problem: "My screen is black, but I can hear audio in the background!"
Wow! It's most likely the game your trying to play has video cutscenes (that's a rare treat!). What you're seeing is result of your Windows installation missing the codecs which are required for the movie to be decoded by the operating system (most likely CINEPAK). Thankfully, the solution is just to install a codec pack. We recommend [CCCP](https://serve.swordofmoonlight.com/Games/Fixes/combined-community-codec-pack.exe) (LOL, "Combined Community Codec Pack") because it contains nearly every codec imaginable.

### Problem: "The game is running like shit/crashed after a long session!"
Classic. Sadly SoM leaks a lot of memory, so if you're moving between maps a lot - you're going to flood the games process with RAM it can't release. While this issue cannot be fixed without extensive code re-writes, it can be mitigated by using a LAA (Large address aware) patcher, which boosts the amount of RAM a 32-bit application can use from 2GB to 4GB, theoretically doubling the time it takes to drop performance or crash. To do this, download the [LAA Patcher](https://serve.swordofmoonlight.com/Games/Fixes/laa_patcher.exe) and run it. It'll ask you to select an executable - that's where you add the executable of a SoM game you want to patch, and the tool should explain the rest.

### Problem: "The game is super bright/dark!"
This is an issue caused by some systems with certain gamma/hdr settings, on modern operating systems. The fix is to use dgVoodoo2, which is already covered in an [above problem](comp-games.md/#problem-the-compass-looks-weird-and-glitchy-theres-black-boxes-around-it).
