# Rule tags: the shared vocabulary

157 distinct tags across 34 rules files. The main observation:
the set of tags is **not a property of the file, but a shared vocabulary**.
Each file uses its own subset, and a tag missing from one file can still be
legitimate in another.

This is knowledge about the data: the set of tags belongs to the game
engine, not to any single rules file.

## Who uses what

| File | Tags in blocks | Documented in comments |
|---|---:|---:|
| `unitrules` | 64 | 56 |
| `buildingrules` | 53 | 56 |
| `techrules` | 27 | 14 |
| `craftrules` | 47 | 34 |
| `itemrules` | 9 | 0 |
| `al rukh` | 21 | 0 |
| `condottieri` | 15 | 0 |
| `ctwalim` | 119 | 0 |
| `ctwalimrules` | 18 | 0 |
| `ctwbonuses` | 36 | 0 |
| `ctwcuotl` | 126 | 0 |
| `ctwcuotlrules` | 19 | 0 |
| `ctwheroes` | 122 | 0 |
| `ctwrules` | 8 | 0 |
| `ctwvinci` | 122 | 0 |
| `ctwvincirules` | 23 | 0 |
| `dark alim` | 29 | 0 |
| `death god` | 19 | 0 |
| `desert monsters` | 15 | 0 |
| `dmrules` | 6 | 0 |
| `felignans` | 27 | 0 |
| `gustians` | 23 | 0 |
| `heretics` | 29 | 0 |
| `mianans` | 20 | 0 |
| `moon god` | 19 | 0 |
| `narr saghir` | 19 | 0 |
| `pirata` | 21 | 0 |
| `scavengers` | 15 | 0 |
| `scrubs` | 20 | 0 |
| `storm goddess` | 19 | 0 |
| `sun god` | 19 | 0 |
| `taronans` | 19 | 0 |
| `unallied pirata` | 15 | 0 |
| `venuccians` | 27 | 0 |

## Tags are optional

Tag sets differ from block to block, so the engine cannot read fields by
column number — skipping one tag would shift every following one:

| File | Blocks | Distinct tag sequences |
|---|---:|---:|
| `unitrules` | 335 | 19 |
| `buildingrules` | 142 | 16 |
| `techrules` | 141 | 5 |
| `craftrules` | 509 | 78 |

## Documented but never used

29 names the developers documented in `COLUMNINFO`, but no rules file
contains them. Whether the engine understands them has not been checked;
these are candidates, not confirmed fields.

```
ATTACK  ATTENUATE  BASE_ARROWS  BEHIND_HEIGHT  DURATION_UPGRADE
GARRISON_MAX  GUY_SPACING  JOB_TIME  JUMP  MISERY  OBSOLETE  PUSH_CIRCLES
SCIENCE_LOS  SHOW  SPLASH_AREA  SPLASH_CENTER_PERCENT  SPLASH_EDGE_PERCENT
SPLASH_FRAMES  SPLASH_PERCENT  SPLASH_RADIUS  SUPPORT0  SUPPORT1
SUPPORTVALUE0  SUPPORTVALUE1  TO_HIT  TYPE_NAME  WONDER_VAL  X_SPACING
Y_SPACING
```

## What third-party mods add

Published third-party mods work, and their own rules files carry tags beyond the original ones. That means the parser survives a tag that was not previously in the file. Whether the engine acts on such a tag is a question for the code, not for the data.

| File | Tags absent from the original |
|---|---|
| `unitrules` | `CREW_SIZE`, `ICONGRAFT` |
| `buildingrules` | `GRID_X2`, `GRID_Y2`, `ICONGRAFT`, `PLUNDER_GOOD`, `SPELL_GRAFT` |
| `techrules` | `ICONGRAFT` |
| `craftrules` | `PREQ1`, `RANK` |

## The full vocabulary

The number is how many rules files the tag occurs in.

| Tag | Files | Where exactly |
|---|---:|---|
| `AA` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ACCEL` | 2 | `unitrules`, `ctwheroes` |
| `AGE` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `AI_DATA` | 7 | `al rukh`, `dark alim`, `felignans`, `gustians`, `heretics`, `taronans` and 1 more |
| `AM` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `AMMO_PER_ATT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ANIM` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `AR` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ARMIES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 23 more |
| `ARMOR` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ARMY` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `ART` | 4 | `unitrules`, `ctwalim`, `ctwheroes`, `ctwvinci` |
| `BA` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `BACKUP_BUILD_CONTINENT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `BASE_AGE_NAME` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `BLOCK_RADIUS` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `BR` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `BUILD` | 1 | `pirata` |
| `BUILDINGMASKS` | 1 | `buildingrules` |
| `BUILD_CONTINENT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `BUILD_FLAGS` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CAP` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CARRY` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CARRY_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CAT` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CHAIN` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CIRCLE_RADIUS` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CITIES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 23 more |
| `CITY` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 23 more |
| `CODETAG` | 9 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` and 3 more |
| `COMMERCE_CAP` | 2 | `buildingrules`, `ctwcuotl` |
| `COOLDOWN` | 8 | `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl`, `ctwcuotlrules`, `ctwheroes` and 2 more |
| `COST` | 13 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwalimrules` and 7 more |
| `COST_LEVEL` | 2 | `craftrules`, `ctwheroes` |
| `COUNTERED_BY` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CREW_SIZE` | 3 | `ctwalim`, `ctwheroes`, `ctwvinci` |
| `DAMAGE` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `DATA0` | 8 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` and 2 more |
| `DATA1` | 7 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` and 1 more |
| `DATA2` | 7 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` and 1 more |
| `DATA3` | 7 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` and 1 more |
| `DESC` | 1 | `ctwvincirules` |
| `DOMAIN` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `DURATION` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `EXCLUSIVE` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `EXIT_TIME` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FAST` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLAGS` | 11 | `unitrules`, `techrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwcuotlrules` and 5 more |
| `FLAGS2` | 2 | `unitrules`, `ctwheroes` |
| `FLOAT_HEIGHT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLY` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLY_HIGH` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLY_LOW` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FROM` | 12 | `unitrules`, `buildingrules`, `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl` and 6 more |
| `GA` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GM` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GR` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GRAFT` | 10 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` and 4 more |
| `GRAPH` | 12 | `unitrules`, `buildingrules`, `craftrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses` and 6 more |
| `GRID_X` | 12 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` and 6 more |
| `GRID_X2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GRID_Y` | 11 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` and 5 more |
| `GRID_Y2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `HELP` | 13 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` and 7 more |
| `HERO` | 2 | `techrules`, `ctwbonuses` |
| `HITS` | 8 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` and 2 more |
| `ICONGRAFT` | 10 | `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes`, `ctwvinci` and 4 more |
| `INCOME` | 3 | `buildingrules`, `ctwcuotl`, `ctwheroes` |
| `LEADER` | 28 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 22 more |
| `LEADERS` | 28 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 22 more |
| `LOS` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MANA` | 6 | `unitrules`, `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MAX_RANK` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MOST_SHOTS` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MOVES` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `NAME` | 14 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` and 8 more |
| `NEED` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `NEEDS_TRIBE` | 24 | `al rukh`, `condottieri`, `ctwalim`, `ctwcuotl`, `ctwcuotlrules`, `ctwrules` and 18 more |
| `OBJECTMASKS` | 1 | `buildingrules` |
| `OBJ_MASK` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `OBJ_MASKS` | 6 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci`, `dmrules` |
| `OPTION_FROM` | 2 | `buildingrules`, `ctwbonuses` |
| `PLUNDER` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PLUNDER_GOOD` | 4 | `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `POP` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `POP_CAP` | 2 | `buildingrules`, `ctwcuotl` |
| `PREQ0` | 18 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwalimrules` and 12 more |
| `PREQ1` | 11 | `unitrules`, `buildingrules`, `techrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses` and 5 more |
| `PREQ2` | 8 | `buildingrules`, `techrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 2 more |
| `PRODAI_SCRIPT` | 10 | `al rukh`, `ctwcuotl`, `dark alim`, `death god`, `heretics`, `moon god` and 4 more |
| `PROJ_SPEED` | 6 | `unitrules`, `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PURCHASE_COST` | 4 | `buildingrules`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PUSH` | 6 | `buildingrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PUSH_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RADIUS` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RAMP` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RAMP_COST` | 11 | `unitrules`, `buildingrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 5 more |
| `RAMP_TIME` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RANGE` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RANK` | 3 | `ctwalim`, `ctwheroes`, `ctwvinci` |
| `RANK_TIME` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RECHARGE` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RESEARCH_COST` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RESEARCH_PTS` | 1 | `buildingrules` |
| `RESOURCE` | 10 | `dark alim`, `death god`, `felignans`, `gustians`, `mianans`, `moon god` and 4 more |
| `RESOURCES` | 1 | `itemrules` |
| `RES_TIME` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SCRIPT_FILE` | 4 | `craftrules`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SCRIPT_FLAGS` | 4 | `craftrules`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SHIELD` | 7 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` and 1 more |
| `SHORT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SHOW0` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `SHOW1` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPACING` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPAWN` | 6 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci`, `dark alim` |
| `SPELL_FLAGS` | 7 | `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl`, `ctwcuotlrules`, `ctwheroes` and 1 more |
| `SPELL_FLAGS2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPELL_FLAGS3` | 2 | `craftrules`, `ctwheroes` |
| `SPELL_GRAFT` | 6 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci`, `dark alim` |
| `SPELL_RANGE` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPIKY` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPLAT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `START` | 12 | `al rukh`, `dark alim`, `death god`, `felignans`, `gustians`, `heretics` and 6 more |
| `STARTS` | 12 | `al rukh`, `dark alim`, `death god`, `felignans`, `gustians`, `heretics` and 6 more |
| `STATS_BONUS` | 1 | `techrules` |
| `STORM` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `STRING` | 7 | `al rukh`, `dark alim`, `felignans`, `gustians`, `heretics`, `taronans` and 1 more |
| `TARGET_FLAGS` | 8 | `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl`, `ctwcuotlrules`, `ctwheroes` and 2 more |
| `TARGET_FLAGS2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TARGET_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_CLIP` | 6 | `craftrules`, `itemrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_COL` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_ID` | 6 | `craftrules`, `itemrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_ROW` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_X` | 1 | `itemrules` |
| `TEX_Y` | 1 | `itemrules` |
| `TIME` | 9 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` and 3 more |
| `TOWN_HITS` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TRAMPLE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TRIBES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 23 more |
| `TRIBE_MASK` | 9 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` and 3 more |
| `TRIBE_MASKS` | 2 | `buildingrules`, `techrules` |
| `TURN` | 1 | `buildingrules` |
| `TURN_SPEED` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TYPENAME` | 14 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` and 8 more |
| `TYPES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 23 more |
| `UBER_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `UNIT_CONTINENT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `VAL` | 7 | `al rukh`, `dark alim`, `felignans`, `gustians`, `heretics`, `taronans` and 1 more |
| `WANT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` and 14 more |
| `WEAPONFLAGS` | 1 | `buildingrules` |
| `WHERE` | 14 | `unitrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl` and 8 more |
| `WOUNDS` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `XP` | 10 | `techrules`, `craftrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` and 4 more |
| `X_SIZE` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `Y_SIZE` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
