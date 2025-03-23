# Authoring Custom Enemies

## Movement Control Points
SoM has an interesting way of handling movement, which revolves around control points - you give the engine two control points in your file, C0 and '0,0,255', or what we'll be calling P0 from now on.

C0 is your target position - in an animation, it's where the enemy will be during that exact frame, P0 is used in calculations with C0 in order to determin the heading of the enemy. While a little complicated, you can use these two to choreograph very nice movement from enemies... Snakes which actually follow the path of the animation they have exactly, all sorts of cool things.

## Animation IDs
| ID | Usage | Description |
|----|-------|-------------|
| 00 | Idle | Used while the enemy is idle. |
| 01 | Walk | Used while the enemy is walking. |
| 02 | Charge | Used while the enemy is dashing. |
| 03 | Unused | ... |
| 04 | Death | Used while the enemy is dying. |
| 05 | Unused | ... |
| 06 | Defend | Used while the enemy is defending. |
| 07 | Evade | Used while the enemy is evading. |
| 08 | Unused | ... |
| 09 | Trigger | Used when the enemy is triggered/activated. |
| 10 | Melee Attack #1 | Used for the enemies first melee attack. |
| 11 | Melee Attack #2 | Used for the enemies second melee attack. |
| 12 | Melee Attack #3 | Used for the enemies third melee attack. |
| 13 | Unused | ... |
| 14 | Unused | ... |
| 15 | Ranged Attack #1 | Used for the enemies first ranged attack. |
| 16 | Ranged Attack #2 | Used for the enemies second ranged attack. |
| 17 | Ranged Attack #3 | Used for the enemies third ranged attack. |
| 18 | Unused | ... |
| 19 | Unused | ... |
| 20 | Hit | Used for the enemies damage response. |
| 21 | Unused | ... |
| 22 | Critical Hit | Used for enemies extreme damage response. |