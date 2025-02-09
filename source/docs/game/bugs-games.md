This is a list of SoM bugs present in every game, and some known bugs worth mentioning which are only present in specific games.

### Sudden Performance Drop, Crash To Desktop
_Severity: High_, _Scope: All games_<br/>
The SoM runtime doesn't clean up it's memory properly, meaning that the longer you are playing the game - the less memory it has to work with. You might not think this matters with your 32GB of RAM, but because SoM is a 32-bit application, it can only use 2GB of your RAM.

!!! success
    This bug is fixed in games running SomEx.<br/>
    There is a [compatibility fix](comp-games.md/#problem-the-game-is-running-like-shitcrashed-after-a-long-session) to reduce crash/lag frequency.

### Gold/Item Drop Limit
_Severity: High_, _Scope: Most games_<br/>
The SoM Runtime has a nasty bug which can occure if you've been killing a lot of enemies which drop gold or items, and failing to pick them up. A maximum amount of 256 items can be loaded in at once in SoM, and if the number of items attempts to go higher than this - the game will crash.

!!! success
    This bug is fixed in games running SomEx.<br/>
    Most SoM developers chose to turn gold and item drops off so this doesn't happen.

### Flashing Dark Status
_Severity: High_, _Scope: Some games_<br/>
The dark effect added to the player by events is synced to CPU time, meaning on modern hardware it will flash rapidly - and may actually induce seizures. It's unknown if any SoM games use this effect, since by the time games were being made with SoM - CPUs were already fast enough for it to cause problems.

!!! success
    This bug is fixed in games running SomEx.<br/>

### Height/Elevation Not Factored in Interactions
_Severity: Moderate_, _Scope: All games_<br/>
The SoM runtime only checks your distance to objects on the X/Z planes, instead of including Y - meaning if you're 50 units above a switch, item or NPC you can still interact with them.

!!! success
    This bug is fixed in games running SomEx.

### Walls Not Factored in Interactions
_Severity: Moderate_, _Scope: All games_<br/>
The SoM runtime doesn't check if anything is inbetween you and an interactable (NPC, Item, Object), meaning you can interact with things through walls.

!!! success
    This bug is fixed in games running SomEx.

### Dynamic Light Sources Don't Affect Objects
_Severity: Low_, _Scope: All games_<br/>
Lighting calculations are not performed on any objects in a map, meaning that there is a clear difference in between map tiles and objects. This affects all objects, including secret doors.

!!! success
    This bug is fixed in games running SomEx.

### Playtime Isn't Saved
_Severity: Low_, _Scope: All games_<br/>
The playtime display on the menu only applies to the current session, and isn't saved - meaning it starts from zero each time you load the game.

### Staggered Enemies Don't Take Damage
_Severity: Low_, _Scope: All games_<br/>
Enemies can't be damaged again until they have recovered from its stagger animation.

### Bugged "Stone Face" Projectiles
_Severity: Low_, _Scope: All games_<br/>
The stone face enemies in SoM have a broken projectile spawn point, which causes the projectile to spawn inside them, immediately collide with them, and then despawn.

!!! warning
    While this bug _is_ confirmed, it may be actually be a result of the english translation patch.