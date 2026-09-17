# Game data: where everything lives

Russian: [game-data.md](game-data.md)

Rise of Legends stores everything it ships in `.big` archives in the `BIGS\`
folder of the installation: rules, text, models, textures, effects, shaders,
scripts. 46 archives, 11 591 entries between them. The engine does not read
loose files sitting next to the archives — you cannot drop a mod in as a
separate folder; the only thing you can change is what is inside the archives.

What follows is a map: which archive is responsible for what, which rule files
live in it, how the files point at each other, and which format traps catch
everyone opening this data for the first time. There are five traps, and each
one costs an evening to the reader who did not know about it.

## Archives by role

| Group | Archives | Entries | What's inside |
|---|---|---|---|
| **Rules and data** | `mod_data`, `multiplayer_data`, `data` | 81 / 142 / 44 | rule and settings XML, the `data\` tree |
| **Localization and text** | `strings`, `nonloc_strings`, `gametextablemgr` | 39 / 1 / 1 | `loc\<language>\`, string tables |
| **Unit art** | `vinci_units`, `alim_units`, `cuotl_units`, `misc_units` | 1342 / 986 / 521 / 355 | `.bh3` meshes, `.bha` animations, `.tga` textures |
| **Building art** | `vinci_buildings`, `alim_buildings`, `cuotl_buildings`, `misc_buildings` | 313 / 143 / 128 / 142 | `.pfb` prefabs, `.bha`, `.tga` |
| **World** | `biomes`, `trees`, `landscapes`, `terrain`, `sky` | 1030 / 659 / 270 / 10 / 10 | vegetation prefabs, landscapes |
| **Tilesets** | `arctic`, `bavarian`, `jungle`, `jungle_dish`, `volcanic`, `dark_forest`, `dark_glass_2`, `dark_glass_3` | 9…89 | textures for specific biomes |
| **Interface** | `interface`, `ui`, `cursors`, `dds_files`, `storyboards`, `credits` | 247 / 42 / 35 / 10 / 5 / 213 | icon atlases, cursors, screens |
| **CTW campaign** | `ctw`, `ctw_generic`, `ctw_scenario_images` | 323 / 125 / 44 | the campaign map and its objects |
| **Art descriptions** | `art` | 2810 | XML describing terrain, buildings, CTW, prefabs |
| **Service** | `effects`, `fxobjects`, `prefabs`, `scripts`, `soundinfo`, `game_constants`, `game_eye_candy_dish`, `ui_render_manager`, `g15` | 706 / 234 / 13 / 128 / 1 / 1 / 14 / 1 / 9 | effects, `.fxo` shaders, `.bhs` scripts, registries |

The archives are not isolated: the paths of all entries add up to one shared
tree, and the same folder is filled by several archives at once.

| Folder | Entries | Which archives fill it |
|---|---|---|
| `art\` | 10 344 | 30 archives; inside it `terrain` 3327, `units` 3205, `buildings` 1440, `effects` 706, `ctw` 689, `trees` 659, `interface` 167 |
| `data\` | 286 | `multiplayer_data` 142, `mod_data` 81, `data` 42, `scripts` 21 |
| `landscapes\` | 270 | `landscapes` |
| `bigs\` | 234 | `fxobjects` — compiled `.fxo` shaders |
| `credits\`, `ui\`, `campaigns\`, `loc\`, `rules\`, `maps\` | 213 / 78 / 53 / 39 / 32 / 22 | one or two archives each |
| files with no folder | 18 | `game_eye_candy_dish` (14 preset sets), `gametextablemgr`, `game_constants`, `GameSounds`, `init_icon_texs` |

Every entry in an archive carries a type. 9688 are raw (`type=''`), 1483 are
`bxml`, 234 `fxo`, 128 `bhs`, 40 `string_lookup`, the rest are one-off
registries. By volume: `.tga` 577 MB (3771 files), `.pfb` 238 MB, `.bha`
114 MB, `.xml` 78 MB, `.bh3` 61 MB.

## The four rule files, and a fifth

Everything the game treats as an entity is described by four files in `data\`:

| File | Block | Records | What it describes |
|---|---|---|---|
| `data\unitrules.xml` | `<UNIT>` | 335 | units, heroes included |
| `data\buildingrules.xml` | `<BUILDING>` | 142 | buildings |
| `data\techrules.xml` | `<TECH>` | 141 | technologies |
| `data\craftrules.xml` | `<CRAFT>` | 509 | crafts: abilities, spells, orders |

The key of a record is `TYPENAME`, not `NAME`. Names repeat freely: three
different units carry `NAME = Miner`, and the only way to tell them apart is
`TYPENAME` (`Priest`, `Worker`, `Miner`). Full listings are in the reference
tables: [units](reference/units.en.md),
[buildings](reference/buildings.en.md), [techs](reference/techs.en.md),
[crafts](reference/crafts.en.md).

The fifth file, `data\rules.xml`, is built differently: not entities, but the
global numbers the engine asks for by name — 836 parameters in 61 sections
covering economy, combat, healing, borders, victory conditions, hero and
faction bonuses. That is where, for instance, the miner price ceiling comes
from (`unit_worker_ramp_max = 1000` percent over the base price, which is why
the price tops out at 77 with a base `COST 7m`). Full list:
[reference/rules-constants.en.md](reference/rules-constants.en.md).

`data\` also holds `itemrules`, `rarerules`, `resourcerules`, `help`,
`tips_{basic,advanced,alim,cuotl,vinci}`, the `data\tribes\` folder describing
the factions, and `data\tribes\ctw\` — 29 campaign files (`ctwalim`,
`ctwcuotl`, `ctwvinci`, `ctwheroes`, `ctwbonuses`, plus factions such as
`pirata`, `mianans`, `scavengers`).

The tags inside the blocks are **a shared engine vocabulary, not a property of
a file**: 157 names across 34 rule files, each file using its own subset, and a
tag missing from one file is perfectly legal in another
([reference/rule-tags.en.md](reference/rule-tags.en.md)).

## Three archives hold `data\` — and one of them wins

The `data\` tree is filled by three archives, and their contents overlap:

- `multiplayer_data.big` — 142 files, nearly everything;
- `data.big` — an engine subset, 42 files;
- `mod_data.big` — 81 entries, **the game's modifiable surface**.

```
data\           unitrules  buildingrules  techrules  craftrules  itemrules
                rarerules  resourcerules  rules  help  tips_{basic,advanced,alim,cuotl,vinci}
data\tribes\    alim  cuotl  vinci  empty
data\tribes\ctw\ 29 campaign files
```

57 paths exist in more than one archive: 42 in the `data + multiplayer_data`
pair, 14 in the `mod_data + multiplayer_data` pair (these are the four rule
files plus `rules`, `help`, `itemrules`, `rarerules`, `resourcerules`,
`tips_*`), and one (`ui\modifiers\environment.xml`) in `data + ui`.

Because of that overlap it matters where an edit goes. For the four rule files
`mod_data.big` wins: an edit made only there shows up in the game. The reverse
was checked too — deleting `rules.xml` from **both** archives kills startup
with "File not found", while deleting it only from `mod_data.big` is harmless,
because the fallback copy stays in `multiplayer_data.big`.

Reaching the file is not the same as reaching the battlefield: a record has its
own layers of checks, covered in [rule-layers.en.md](rule-layers.en.md).

## Five format traps

**1. Text in an archive is Latin-1, not UTF-8.** Text entries are read with
code page 28591 (ISO-8859-1): every byte maps to one character and back
without loss. Read such a file as UTF-8 and write it out again and you corrupt
everything above ASCII, shifting lengths along the way. An edit made on top of
Latin-1 leaves the untouched regions byte for byte identical.

**2. One path, two forms.** Some files sit in the same archive **twice**: as
raw text (`type=''`) and as compiled binary XML (`type='bxml'`). Both carry the
same `.xml` extension:

```
data\rules.xml            305 023 B  type=''      and  338 412 B  type='bxml'
data\tribes\alim.xml       17 718 B  type=''      and   18 968 B  type='bxml'
data\tribes\ctw\*.xml      all 29 files — both forms
```

Take the first match by name and the form you get is a coin toss. The four main
rule files (`unitrules`, `buildingrules`, `techrules`, `craftrules`) are safe —
they exist as text only. But any work on `rules.xml`, the tribes or CTW starts
with deciding which form the game reads, and being able to write that one.

**3. A `.tga` inside an archive is really a DDS.** The packer converted the
textures on the way in and kept the old name. Open them as DDS; a TGA loader
trips on the very first byte.

**4. An entry written into an archive must be zlib-compressed.** The format
lets you put the bytes in raw, and the engine then dies on `unknown compression
method`. While you are there: an entry has a type field, and it has to hold at
least an empty string rather than a null.

**5. The game reads its archives once, at startup.** Swapping an archive while
the game is running changes nothing on screen: every check of an edit needs a
full exit and a fresh launch.

And the rule that covers all of them: **keep a copy of the original archive**.
It is the only way to restore what you break.

## How the files point at each other

References do not all resolve — the ratio is given alongside, and any parser
has to survive a miss.

```
unitrules.xml <UNIT>
  ├─ WHERE    ─→ buildingrules TYPENAME        129 / 138 (rest are none/leader/disable)
  ├─ GRAPH    ─→ unit_graphics.xml <UNIT type> 313 / 334
  ├─ HELP     ─→ help.xml <ENTRY name>         250 / 329
  ├─ FROM     ─→ the upgrade chain
  └─ CODETAG  ─→ an identifier for the engine

unit_graphics.xml <UNIT type="WORKER">
  ├─ <MODEL file=".\art\units\vinci\worker_LOD_0.BH3"/>
  ├─ <LOD level="1..3">  its own MODEL/SKELETON/MATERIAL
  ├─ <SKELETON name="Worker" type="units">   ─→ unit_skeletons.xml
  └─ <MATERIAL name="Worker.fx" type="units">─→ unit_materials.xml (425 materials)

unit_materials.xml <MATERIAL material_name="Worker.fx">
  ├─ <TEXTURE role="diffusemap"     name="vinci_Worker_diffuse.tga" category="vinci"/>
  ├─ <TEXTURE role="normalmap"      name="vinci_Worker_dot3.tga"    category="vinci"/>
  └─ <TEXTURE role="teamcolormap"   name="vinci_Worker_team.tga"    category="vinci"/>
        files: art\units\<category>\<name>.tga   (DDS inside)
```

The `GRAPH` references that fail are not lost units but internal placeholder
entities: `BaseHeroUnit`, `BaseGrunt`, `Spell Obj`, `Static Areaspell Unit`,
`old_*`.

**Buildings work differently.** `buildingrules.GRAPH` leads into
`building_graphics.xml`, where the element is called `<BUILD type=… file=…>`
and points at a **prefab** `.pfb` (453 references, every one of them a prefab)
rather than at a mesh directly. 94 of 142 resolve; the ones that do not are
cities and districts (`Small City`, `Military District`, `Guild District`) —
they are assembled procedurally, their art coming through `citytemplates.xml`
and `districtconnections.xml`.

**Technologies** are plainer: across 141 `techrules` blocks there are only 7
distinct `WHERE` values, and just 4 of those are real buildings.

Asset name suffixes are uniform: `_diffuse` is color, `_dot3` a normal map,
`_team` the team-color mask, `_hi` / `_mid` size variants, `_lod0…_lod3` /
`_LOD_N` levels of detail. The model and animation formats themselves are in
[model-formats.en.md](model-formats.en.md).

## Icons

An entity is tied to its icon by `data\uitextable.xml` — 3944 `<ENTRY>`
records, keyed by a name that **matches `TYPENAME`** (case-insensitively):

```xml
<ENTRY name="Worker" button_texture="ICONS_VINCI_SMALL"  button_column="1"  button_row="0"
                     portrait_texture="ICONS_VINCI_LARGE" portrait_column="1" portrait_row="3"
                     timer_texture="ICONS_VINCI_TIMERS"/>
<TEXLOAD id="ICONS_VINCI_SMALL" file=".\art\interface\icons\icons_vinci_small.tga"/>
```

The whole chain is `TYPENAME → ENTRY → *_texture → TEXLOAD → .tga`, with
`column/row` addressing a cell in the atlas. There are six icon roles:
`button` (the recruit button, 2254 records), `timer` (build progress, 1537),
`portrait` (1028), `buff` (797), `bw_button` (the inactive variant, 347),
`hero_button` (313).

| File | Entities | Have an `ENTRY` | Have an icon |
|---|---|---|---|
| `unitrules` | 335 | 335 | 321 |
| `buildingrules` | 142 | 142 | 134 |
| `craftrules` | 509 | 508 | 363 |
| `techrules` | 141 | 155 matches | 140 |

**The cell geometry is written into the atlas itself.** Cells are separated by
solid magenta lines (`#FF00FF`, alpha 255) one pixel wide. In every icon atlas
those lines are solid — over 98 % of the pixels in the column or row — and
there is not a single stray magenta pixel inside the artwork. Reading the
markup beats computing the step from the atlas size: the step between lines is
a whole number, **51** for small icons (a 50 px cell plus the separator) and
**85** for large ones (84 plus the separator), whereas "512 / 10 = 51.2" drifts
two pixels off by the far cells and swallows the neighbour's line. The grid is
not always square either: in `icons_heros_small.tga` the step is 51 across and
61 down, a 50×60 cell — hero icons are taller than the rest.

Two facts without which the chain does not close. `uitextable.xml` lives in
`multiplayer_data.big` — not in `mod_data.big`, not in `data.big` — while the
atlases themselves are in `interface.big`. And 27 units carry
`button_texture="????"` (11 carry `portrait_texture="????"`): the record
exists, but no `TEXLOAD` has that identifier. The game simply never assigned an
icon, which is not the same thing as having no `ENTRY` at all.

The icon atlases are **uncompressed** DDS: `ddspf.dwFlags = 0x41`
(`DDPF_RGB | DDPF_ALPHAPIXELS`), 32 bits, `A8R8G8B8` masks, no mipmaps, file
size exactly 128 + w·h·4 bytes. No DXT block decoder is needed: to read one is
to skip the header and copy the tail.

Separately, `data\icon_texs.xml` (and its binary twin `init_icon_texs` from
`ui_render_manager.big`) describes the resource and interface icons. It also
carries a warning from the developers: the records must not be reordered,
because the code reaches them by a hardcoded index.

## Localization: the name the player sees does not live in the rules

Russian text — or any other language — is **not in the rule files** but in
compiled string tables: `string_lookup.xml`, `help_lookup.xml`,
`loc_table_lookup.xml`. The `.xml` extension is a lie; the entry type in the
archive is `string_lookup`, and inside is a binary format holding UTF-16.

**The tables live in an archive, not as loose files on disk.** The path
`loc\<language>\stringlookups\` is a path *inside* an entry of `strings.big`,
not a filesystem path: on disk those folders are not created at all for most
languages, and where they are (`loc\RU\`, `loc\en\`) they are empty. All three
tables for 13 languages are packed into a single `strings.big`, 39 entries. The
entry path is `.\loc\ru\stringlookups\string_lookup.xml` — **the language name
lowercase**, while `loc.ini` stores `LANG=RU` uppercase, so the two have to be
compared case-insensitively. The languages present: `1337, ba, ce, cz, de, en,
es, fr, it, ja, pl, ru, tc`.

What goes where is decided by `data\loc_manifest.xml`, the build recipe:

```xml
<FILE name="..\game\data\help.xml"  output_file="help_lookup.xml"><QUERY element="TEXT"/></FILE>
<FILE name="..\game\data\*.xml"     output_file="string_lookup.xml">
  <QUERY element="NAME"/> <QUERY element="SHORT"/> <QUERY element="DESC"/> …
```

So `<NAME>` and `<SHORT>` from the four rule files are extracted into
`string_lookup` at build time, while the `<TEXT>` bodies from `help.xml`,
`tips_*.xml` and `auto_help.xml` go into `help_lookup`. The traversal order is
deterministic: manifest entries, then files, then elements within a document.

**The consequence everyone trips over.** Editing `<NAME>` in the rules **does
not change the displayed name** in a localized game — the engine takes the
string from the table. No experiment is needed to prove it: `unitrules.xml`
says `NAME=Miner`, and the Russian build shows "Рудокоп". You cannot rename a
unit by editing the rules.

### The `string_lookup` format

```
[u32 file size]       matches the real size — useful as a sanity check
[u32 version = 1]
[u32 N — string count]
[u32 M — source file count]
table:  N records of 10 bytes        field meanings never worked out, never needed
marker: [u32 N][u16 0xFFFF][u8 0]    the anchor: the blob starts right after it
blob:   N times in a row [u32 length in characters][UTF-16LE, no terminator]
tail:   4 bytes
```

| File | Strings | Sources |
|---|---|---|
| `string_lookup.xml` | 9204 | 470 |
| `help_lookup.xml` | 3887 | 559 |

**The tables of every language are parallel by index**: string `i` in `en` and
string `i` in `ru` are the same string. That makes an "English name →
translation" dictionary a matter of adding two files together, with no
understanding of the record table required.

What comes out of it:

| Measure | Result |
|---|---|
| Unique English strings | 9187 of 9204 |
| Strings translated differently in different places | 4 (`Land`, `Performance`, `Easy`, `Tough`) |
| Unit names translated | 335 of 335 |
| Building names | 142 of 142 |
| Technology names | 141 of 141 |
| Craft names | 507 of 509 |
| `help.xml` descriptions | translated along with the `#ICON1`, `$NUM0` substitutions |

An example that also settles a long-running confusion about miners:

| `TYPENAME` | `NAME` | In Russian | Recruited at |
|---|---|---|---|
| `Spirit` | Spirit Miner | Дух-рудокоп | Alim Mine |
| `Priest` | Miner | Рудокоп | Cuotl Mine |
| `Worker` | Miner | Рудокоп | Vinci Mine |
| `Miner` | Miner | Рудокоп | Mining Company |
| `Hardy Miner` | Armed Miner | Вооруженный рудокоп | Vinci Mine |
| `Clockwork Miner` | Clockwork Miner | Механический рудокоп | Vinci Mine |

The three "Рудокоп" entries are indistinguishable in the Russian game as well —
that is a property of the game, not of the translation. `help.xml` does explain
what makes `TYPENAME = Miner` different: "A neutral Timonium gatherer. Will
work for whoever currently controls the mine" — which lines up with its
`TRIBE_MASK = 1111`.

**What the tables do not contain: field names.** `HITS`, `POP`, `RAMP_COST` are
never shown to the player, so no translation for them exists in any language.

### `HELP` is a key, not a text

An entity's `<HELP>WORKER</HELP>` is not the description but the **name of a
record** in `data\help.xml`:

```xml
<ENTRY name="WORKER"><STRING><TEXT>Vinci #ICON1 Timonium gatherer. Adds +$NUM0 to your gather rate (more if at a mine).</TEXT></STRING></ENTRY>
```

`#ICONn` substitutes an icon, `$NUMn` a numeric parameter from the rules.

The subtlety that breaks a naive translation: a single record often holds
several texts — 588 records of 1656 do. **Each `<TEXT>` is translated on its
own**, because the table builds its dictionary from each `<TEXT>`
individually; a string concatenated before translation would not be found in
`help_lookup` at all. The numbers from the archive: 1656 raw `<ENTRY>` records
over 1445 distinct names (211 repeats), and 67 records with no `<TEXT>` at all,
which take no part. When a name repeats, the first occurrence wins. And
`<TEXT>` is XML element content, so `&amp;`, `&lt;`, `&gt;` decode back on
parse: without that, the `FOREIGNTRADE` entry with its `&amp;` would never be
found in the translation table.

## The developers left documentation inside the files

The XML comments are not decoration but a format specification — in places the
only one there is.

| File | Comments | What it documents |
|---|---|---|
| `building_skeletons.xml` | 597 | building nodes and bones |
| `unitrules.xml` | 214 | `OBJ_MASK`, `FLAGS`, `CAT`, `TRIBE_MASK`, `COST`/`RAMP_COST`, `SPAWN`, `POP`/`CAP`, `PREQ`/`WHERE`/`FROM` — every flag letter spelled out |
| `craftrules.xml` | 194 | spells and abilities |
| `unit_skeletons.xml` | 183 | skeletons and attach points |
| `buildingrules.xml` | 147 | building fields |
| `unit_graphics.xml` | 112 | LOD, CREW, OC3 rigging, lip sync |
| `skeletons.xml` | 96 | the skeleton system at large |
| `techrules.xml` | 67 | technology fields |
| `schema.xml` | 67 | the data schema |

`rules.xml` stands apart: every named constant there has its own `<COMMENT>`.
The practical upshot is that a "tag → human name" dictionary does not have to
be invented — it is already inside the game files. `help.xml` gives the
official description of an entity, the `unitrules` comments give the purpose of
every field, and `rules.xml` the meaning of every global constant. Flags
(`OBJ_MASK`, `FLAGS`, `SPAWN_FLAGS`, `TRIBE_MASK`) are documented letter by
letter, and an entity's faction can be read straight off `TRIBE_MASK` (`0001`
Alin, `0010` Kahan, `0100` Cuotl, `1000` Vinci) — which is what tells the three
miners apart.

One caveat: the developers' comments do not always agree with the data. Three
separate checks turned up a disagreement, and each time the data was right. A
comment is a hint; a measurement is the answer.

## The patch folders are never read

`BIGS\` also contains `patches\patch1..8`, `patches\rc7` and `BIGS\patch8` —
sets of archives from earlier versions, declared in `bigmanifest` under a
`BIG_PATCH` section. These are the studio's build instructions and payload for
an external updater, not game data: over a full session the engine opens 45
archives, every one of them from the root of `BIGS\`, and not one file from the
patch folders. Even the declared `patches\patch9` slot, filled in by hand,
stays unread. Shipping a mod as a separate patch folder instead of editing the
archive is not possible.

The gap between 45 and the 46 archives in the root is not a typo and not a
patch folder being read: one root archive simply went untouched over that
session, and which one was not established — a single session has no reason to
load every tileset. What the measurement settles is the other direction:
everything the engine did open came from the root.

A side note: `patches\patch4\strings.big`, `patch5\*` and `patch6\*` were not
built on the build server — their headers carry the machine names `PLAYTEST-10`
and `DESKTOP-MATT` instead of `WAR-BUILDER`. Libraries that check that marker
strictly refuse to open such files ("This is not a Big file!"). This does not
affect the four rule files: every copy of them is marked `WAR-BUILDER`.
