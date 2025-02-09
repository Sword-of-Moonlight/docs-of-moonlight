This is a list of SoM bugs present in every game, and some known bugs worth mentioning which are only present in specific games.

### Gold/Item Drop Limit
_Severity: High_, _Scope: Most games_<br/>
The SoM Runtime has a nasty bug which can occure if you've been killing a lot of enemies which drop gold or items, and failing to pick them up. A maximum amount of 256 items can be loaded in at once in SoM, and if the number of items attempts to go higher than this - the game will crash.

!!! success
    This bug is fixed in games running SomEx.<br/>
    Most SoM developers chose to turn gold and item drops off so this doesn't happen.

### Sudden Performance Drop, Crash To Desktop
_Severity: High_, _Scope: All games_<br/>
The SoM runtime doesn't clean up it's memory properly, meaning that the longer you are playing the game - the less memory it has to work with. You might not think this matters with your 32GB of RAM, but because SoM is a 32-bit application, it can only use 2GB of your RAM.

!!! success
    This bug is fixed in games running SomEx.<br/>
    There is a [compatibility fix](comp-games.md/#problem-the-game-is-running-like-shitcrashed-after-a-long-session) to reduce crash/lag frequency.

### Height/Elevation not factored in interactions, walls not factored in interactions
_Severity: Moderate_, _Scope: All games_<br/>
The SoM runtime only checks your distance to objects on the X/Z planes, instead of including Y - meaning if you're 50 units above a switch, item or NPC you can still interact with them.

!!! success
    This bug is fixed in games running SomEx.

### Dynamic light sources don't affect objects
_Severity: Low_, _Scope: All games_<br/>
Lighting calculations are not performed on any objects in a map, meaning that there is a clear difference in between map tiles and objects. This affects all objects, including secret doors.

!!! success
    This bug is fixed in games running SomEx.

### Playtime isn't saved
_Severity: Low_, _Scope: All games_<br/>
The playtime display on the menu only applies to the current session, and isn't saved - meaning it starts from zero each time you load the game.

### Bugged "Stone Face" Projectiles
_Severity: Low_, _Scope: Specific Game - King's Field Sample_<br/>
The King's Field sample project included with SoM has a strange bug concerning the stone face enemies on floor three, where their projectiles will spawn then immediately disappear after colliding with the same face that shot them.

!!! warning
    While this bug _is_ confirmed, it may be actually be a result of the english translation patch.