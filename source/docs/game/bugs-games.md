This is a list of SoM bugs present in every game, and some known bugs worth mentioning which are only present in specific games.

### Gold/Item Drop Limit
_Severity: High_, _Scope: Most games_
The SoM Runtime has a nasty bug which can occure if you've been killing a lot of enemies which drop gold or items, and failing to pick them up. A maximum amount of 256 items can be loaded in at once in SoM, and if the number of items attempts to go higher than this - the game will crash.

!!! success
    This bug is fixed in games running SomEx.<br/>
    Most SoM developers chose to turn gold and item drops off so this doesn't happen.


### Sudden Performance Drop, Crash To Desktop
_Severity: High_, _Scope: All games_
The SoM Runtime doesn't clean up it's memory properly, meaning that the longer you are playing the game - the less memory it has to work with. You might not think this matters with your 32GB of RAM, but because SoM is a 32-bit application, it can only use 2GB of your RAM.

!!! success
    This bug is fixed in games running SomEx.<br/>
    There is a [compatibility fix](comp-games.md/#problem-the-game-is-running-like-shitcrashed-after-a-long-session) to reduce crash/lag frequency.