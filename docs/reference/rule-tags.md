# Теги правил: общий словарь

English: [rule-tags.en.md](rule-tags.en.md)

157 различных тегов в 34 файлах правил. Главное наблюдение:
набор тегов — **не свойство файла, а общий словарь**. Каждый файл
пользуется своим подмножеством, и тег, которого в одном файле нет, бывает
законным в другом.

Это знание о данных: набор тегов принадлежит движку игры, а не отдельному
файлу правил.

## Кто чем пользуется

| Файл | Тегов в блоках | Описано в комментариях |
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

## Теги необязательны

Наборы тегов различаются от блока к блоку, поэтому читать поля по номеру
колонки движок не может — пропуск одного тега сдвинул бы все следующие:

| Файл | Блоков | Разных последовательностей тегов |
|---|---:|---:|
| `unitrules` | 335 | 19 |
| `buildingrules` | 142 | 16 |
| `techrules` | 141 | 5 |
| `craftrules` | 509 | 78 |

## Описано, но не использовано нигде

29 имён разработчики описали в `COLUMNINFO`, но ни один файл
правил их не содержит. Понимает ли их движок — не проверено; это
кандидаты, а не подтверждённые поля.

```
ATTACK  ATTENUATE  BASE_ARROWS  BEHIND_HEIGHT  DURATION_UPGRADE
GARRISON_MAX  GUY_SPACING  JOB_TIME  JUMP  MISERY  OBSOLETE  PUSH_CIRCLES
SCIENCE_LOS  SHOW  SPLASH_AREA  SPLASH_CENTER_PERCENT  SPLASH_EDGE_PERCENT
SPLASH_FRAMES  SPLASH_PERCENT  SPLASH_RADIUS  SUPPORT0  SUPPORT1
SUPPORTVALUE0  SUPPORTVALUE1  TO_HIT  TYPE_NAME  WONDER_VAL  X_SPACING
Y_SPACING
```

## Что добавляют чужие моды

Опубликованные сторонние моды работают, а в их собственных файлах правил встречаются теги сверх оригинальных. Значит разбор переживает тег, которого в файле раньше не было. Действует ли движок на такой тег — вопрос к коду, не к данным.

| Файл | Теги, которых нет в оригинале |
|---|---|
| `unitrules` | `CREW_SIZE`, `ICONGRAFT` |
| `buildingrules` | `GRID_X2`, `GRID_Y2`, `ICONGRAFT`, `PLUNDER_GOOD`, `SPELL_GRAFT` |
| `techrules` | `ICONGRAFT` |
| `craftrules` | `PREQ1`, `RANK` |

## Словарь целиком

Число — в скольких файлах правил тег встречается.

| Тег | Файлов | Где именно |
|---|---:|---|
| `AA` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ACCEL` | 2 | `unitrules`, `ctwheroes` |
| `AGE` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `AI_DATA` | 7 | `al rukh`, `dark alim`, `felignans`, `gustians`, `heretics`, `taronans` и ещё 1 |
| `AM` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `AMMO_PER_ATT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ANIM` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `AR` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ARMIES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 23 |
| `ARMOR` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `ARMY` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `ART` | 4 | `unitrules`, `ctwalim`, `ctwheroes`, `ctwvinci` |
| `BA` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `BACKUP_BUILD_CONTINENT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `BASE_AGE_NAME` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `BLOCK_RADIUS` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `BR` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `BUILD` | 1 | `pirata` |
| `BUILDINGMASKS` | 1 | `buildingrules` |
| `BUILD_CONTINENT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `BUILD_FLAGS` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CAP` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CARRY` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CARRY_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CAT` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CHAIN` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CIRCLE_RADIUS` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CITIES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 23 |
| `CITY` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 23 |
| `CODETAG` | 9 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` и ещё 3 |
| `COMMERCE_CAP` | 2 | `buildingrules`, `ctwcuotl` |
| `COOLDOWN` | 8 | `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl`, `ctwcuotlrules`, `ctwheroes` и ещё 2 |
| `COST` | 13 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwalimrules` и ещё 7 |
| `COST_LEVEL` | 2 | `craftrules`, `ctwheroes` |
| `COUNTERED_BY` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `CREW_SIZE` | 3 | `ctwalim`, `ctwheroes`, `ctwvinci` |
| `DAMAGE` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `DATA0` | 8 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` и ещё 2 |
| `DATA1` | 7 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` и ещё 1 |
| `DATA2` | 7 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` и ещё 1 |
| `DATA3` | 7 | `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes` и ещё 1 |
| `DESC` | 1 | `ctwvincirules` |
| `DOMAIN` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `DURATION` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `EXCLUSIVE` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `EXIT_TIME` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FAST` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLAGS` | 11 | `unitrules`, `techrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwcuotlrules` и ещё 5 |
| `FLAGS2` | 2 | `unitrules`, `ctwheroes` |
| `FLOAT_HEIGHT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLY` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLY_HIGH` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FLY_LOW` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `FROM` | 12 | `unitrules`, `buildingrules`, `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl` и ещё 6 |
| `GA` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GM` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GR` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GRAFT` | 10 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` и ещё 4 |
| `GRAPH` | 12 | `unitrules`, `buildingrules`, `craftrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses` и ещё 6 |
| `GRID_X` | 12 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` и ещё 6 |
| `GRID_X2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `GRID_Y` | 11 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` и ещё 5 |
| `GRID_Y2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `HELP` | 13 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` и ещё 7 |
| `HERO` | 2 | `techrules`, `ctwbonuses` |
| `HITS` | 8 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` и ещё 2 |
| `ICONGRAFT` | 10 | `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes`, `ctwvinci` и ещё 4 |
| `INCOME` | 3 | `buildingrules`, `ctwcuotl`, `ctwheroes` |
| `LEADER` | 28 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 22 |
| `LEADERS` | 28 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 22 |
| `LOS` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MANA` | 6 | `unitrules`, `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MAX_RANK` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MOST_SHOTS` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `MOVES` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `NAME` | 14 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` и ещё 8 |
| `NEED` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `NEEDS_TRIBE` | 24 | `al rukh`, `condottieri`, `ctwalim`, `ctwcuotl`, `ctwcuotlrules`, `ctwrules` и ещё 18 |
| `OBJECTMASKS` | 1 | `buildingrules` |
| `OBJ_MASK` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `OBJ_MASKS` | 6 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci`, `dmrules` |
| `OPTION_FROM` | 2 | `buildingrules`, `ctwbonuses` |
| `PLUNDER` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PLUNDER_GOOD` | 4 | `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `POP` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `POP_CAP` | 2 | `buildingrules`, `ctwcuotl` |
| `PREQ0` | 18 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwalimrules` и ещё 12 |
| `PREQ1` | 11 | `unitrules`, `buildingrules`, `techrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses` и ещё 5 |
| `PREQ2` | 8 | `buildingrules`, `techrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 2 |
| `PRODAI_SCRIPT` | 10 | `al rukh`, `ctwcuotl`, `dark alim`, `death god`, `heretics`, `moon god` и ещё 4 |
| `PROJ_SPEED` | 6 | `unitrules`, `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PURCHASE_COST` | 4 | `buildingrules`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PUSH` | 6 | `buildingrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `PUSH_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RADIUS` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RAMP` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RAMP_COST` | 11 | `unitrules`, `buildingrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 5 |
| `RAMP_TIME` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RANGE` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RANK` | 3 | `ctwalim`, `ctwheroes`, `ctwvinci` |
| `RANK_TIME` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RECHARGE` | 6 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RESEARCH_COST` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `RESEARCH_PTS` | 1 | `buildingrules` |
| `RESOURCE` | 10 | `dark alim`, `death god`, `felignans`, `gustians`, `mianans`, `moon god` и ещё 4 |
| `RESOURCES` | 1 | `itemrules` |
| `RES_TIME` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SCRIPT_FILE` | 4 | `craftrules`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SCRIPT_FLAGS` | 4 | `craftrules`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SHIELD` | 7 | `unitrules`, `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` и ещё 1 |
| `SHORT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SHOW0` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `SHOW1` | 4 | `techrules`, `ctwalim`, `ctwbonuses`, `ctwvinci` |
| `SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPACING` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPAWN` | 6 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci`, `dark alim` |
| `SPELL_FLAGS` | 7 | `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl`, `ctwcuotlrules`, `ctwheroes` и ещё 1 |
| `SPELL_FLAGS2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPELL_FLAGS3` | 2 | `craftrules`, `ctwheroes` |
| `SPELL_GRAFT` | 6 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci`, `dark alim` |
| `SPELL_RANGE` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPIKY` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `SPLAT` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `START` | 12 | `al rukh`, `dark alim`, `death god`, `felignans`, `gustians`, `heretics` и ещё 6 |
| `STARTS` | 12 | `al rukh`, `dark alim`, `death god`, `felignans`, `gustians`, `heretics` и ещё 6 |
| `STATS_BONUS` | 1 | `techrules` |
| `STORM` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `STRING` | 7 | `al rukh`, `dark alim`, `felignans`, `gustians`, `heretics`, `taronans` и ещё 1 |
| `TARGET_FLAGS` | 8 | `craftrules`, `ctwalim`, `ctwalimrules`, `ctwcuotl`, `ctwcuotlrules`, `ctwheroes` и ещё 2 |
| `TARGET_FLAGS2` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TARGET_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_CLIP` | 6 | `craftrules`, `itemrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_COL` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_ID` | 6 | `craftrules`, `itemrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_ROW` | 5 | `craftrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TEX_X` | 1 | `itemrules` |
| `TEX_Y` | 1 | `itemrules` |
| `TIME` | 9 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` и ещё 3 |
| `TOWN_HITS` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TRAMPLE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TRIBES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 23 |
| `TRIBE_MASK` | 9 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses` и ещё 3 |
| `TRIBE_MASKS` | 2 | `buildingrules`, `techrules` |
| `TURN` | 1 | `buildingrules` |
| `TURN_SPEED` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `TYPENAME` | 14 | `unitrules`, `buildingrules`, `techrules`, `craftrules`, `itemrules`, `ctwalim` и ещё 8 |
| `TYPES` | 29 | `al rukh`, `condottieri`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 23 |
| `UBER_SIZE` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `UNIT_CONTINENT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `VAL` | 7 | `al rukh`, `dark alim`, `felignans`, `gustians`, `heretics`, `taronans` и ещё 1 |
| `WANT` | 20 | `al rukh`, `condottieri`, `ctwcuotl`, `dark alim`, `death god`, `desert monsters` и ещё 14 |
| `WEAPONFLAGS` | 1 | `buildingrules` |
| `WHERE` | 14 | `unitrules`, `techrules`, `craftrules`, `ctwalim`, `ctwbonuses`, `ctwcuotl` и ещё 8 |
| `WOUNDS` | 5 | `unitrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `XP` | 10 | `techrules`, `craftrules`, `ctwalim`, `ctwalimrules`, `ctwbonuses`, `ctwcuotl` и ещё 4 |
| `X_SIZE` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
| `Y_SIZE` | 5 | `buildingrules`, `ctwalim`, `ctwcuotl`, `ctwheroes`, `ctwvinci` |
