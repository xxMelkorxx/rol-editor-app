# A look at other people's mods

Russian: [mods.md](mods.md)

Four third-party mod archives for Rise of Legends are analyzed here, all
dated 2017. Their file names look like the work of four different people, but
the archives' own content tells a different story: one author, four packages.
The evidence for that is textual, not a guess from matching file names. What
follows is what each archive adds, how their solutions differ from each other
and from the approach used in
[campaign-to-skirmish.en.md](campaign-to-skirmish.en.md) and
[rule-layers.en.md](rule-layers.en.md), and what could actually be pinned down
about the author.

Everything here comes from the archives' own content: their `Read Me` text,
comments left inside the rule files themselves, and a direct block-by-block
comparison against the game's own original files, matched by `TYPENAME`. The
archives themselves are not distributed from here — someone else's work
carries its own terms, and the point of this section is to explain the game's
data, not to redistribute other people's content.

| Archive | Size | What it does |
|---|---|---|
| `..._Campaign_Heroes_In_Skirmish_Mod_-_version_3.zip` | 1.5 MB | carries campaign heroes and their abilities into skirmish |
| `..._Expansion_-_version_2.zip` | 2.0 MB | adds heroes, buildings and tech to both campaign and skirmish, in two builds — plain and balanced |
| `..._New_Nations_Mod_-_version_4.zip` | 1.4 MB | turns campaign sub-nations into playable skirmish nations |
| `rise_of_legends_motters_new_nations_mod.zip` | 14.4 MB | the same idea, a separate bundle — with ready-made maps and bonus factions |

Sizes were measured directly from the files.

## Who wrote them

The word "Motters" shared by all four file names proves nothing on its own —
it could be a mangled nickname or a project name picked at random. The proof
sits inside the archives.

The expansion mod's `ReadMe.txt` ends with the signature "Otter": a
first-person letter, install instructions, the "Rise of Legends Heaven" forum
named outright, and a name at the very end. The same name signs
`Modding Guide\Modding Heroes.txt` inside the campaign-heroes mod — a separate
write-up entirely devoted to enabling the hero Battaglion, ending with the
same name on its own last line. The request that prompted it is named not in
that file itself but in the neighboring `Modding Guide\ReadMe.txt` in the same
folder: "This is a modding guide I wrote by request for a user on ROL
Heaven" — that file's only line, carrying no signature of its own. Two
independent files in two different archives, carrying the same signature —
that is enough to name the author outright: **Otter**.

The remaining two archives, `New_Nations_Mod_-_version_4` and the large
bundle `rise_of_legends_motters_new_nations_mod`, carry no signature at all.
But the eight numbered steps of the "HOW TO ENABLE NATIONS" section in their
`Read Me.txt` files match word for word between the two, down to the scenario
editor's button names — the large bundle only adds its own intro line and a
separate sub-nations paragraph on top; both are written in the first person
("thanks for trying out my new mod"), both mention not having written a full
AI for the sub-nations, and both point to "Rise of Legends Heaven". That is a
match in method and voice, not a signature — not quite enough to state
authorship as fact, but nothing in the text contradicts it either. For these
two archives, authorship is not directly signed here, only described as
apparently the same author, going by the text.

The place of publication all four mods name for themselves is the "Rise of
Legends Heaven" forum. None of the files inside the archives carry its
address, and no address is given here either — a link that cannot be checked
against a source is not reproduced.

## What each one adds

Counted by matching a mod's blocks against the game's original files by
`TYPENAME`; the number in parentheses is how many records dropped out along
the way.

| Mod | `<UNIT>` | `<BUILDING>` | `<TECH>` | `<BONUS>` | `<CRAFT>` |
|---|---|---|---|---|---|
| original | 335 | 142 | 141 | 317 | 509 |
| heroes-in-skirmish v3 | 414 (+80) | 145 (+3) | 143 (+2) | 385 (+68) | 680 (+171) |
| expansion v2 (balanced build) | 346 (+12, −1) | 144 (+2) | 141 | 324 (+7) | 520 (+11) |
| nations v4 | 465 (+130) | 175 (+33) | 181 (+40) | 392 (+76, −1) | 767 (+271, −13) |
| nations, large bundle | 457 (+123, −1) | 168 (+26) | 183 (+42) | 391 (+75, −1) | 741 (+232) |

The original's `<BONUS>` count reads 317 rather than 322 here for the same
reason it does everywhere else in this knowledge base: only blocks that
carry a `TYPENAME` are counted, and five of the base game's bonuses have
none.

**Heroes-in-skirmish v3** adds exactly 80 units: every campaign hero of all
three nations, with every one of their level forms. 171 new abilities and 68
bonuses ride along with them, plus three tiers of the `Defense Gun` building.
A detailed walk-through of one hero from this set, the seven pitfalls of
carrying a hero across, and what is worth borrowing from the author's own
choices, live in [campaign-to-skirmish.en.md](campaign-to-skirmish.en.md).

**Expansion v2** is the smallest of the four by record count (12 units, 2
buildings, 11 abilities, 7 bonuses) and the only one that touches individual
campaign-map scenarios, not just the shared rule pool and the shared AI
script library (more on that in the differences section below). Among the
additions: a five-level `Ix, the Moon God` for the Cuotl, a five-level
`Petruzzo, Lord of Miana` for the Vinci, the `Clockwork Scrap Yard` and
`Imperial Observatory` buildings, and the `Laser Tank` and `Tank Proto`
technologies. The tooltip text file grows too: 1,748 `ENTRY` records against
1,710 in the original. This mod ships two builds, "Original" and "Balanced" —
they diverge across all four rule files, though not by much in bulk: the five
`Data\` files of the balanced build add up to 71,237 lines of text against
73,071 for the plain one, a difference of about 2.5%.

**Nations v4** turns campaign sub-nations into playable ones, adding 130 new
units, among them `Acerbus, the Mystic`, `Marwan, the Dark Alim`,
`Desert Mystic`, `Doge Guard`. It also drops exactly thirteen `Summon Army *`
ability records compared with the original game files: `Summon Army 2`, `3`
and `4` outright, plus ten `Summon Army Part 0`–`4` pieces (including the
lettered `1a`, `2a`, `2b`, `3a`, `3b`) — a clean removal, not a rename: the
army-summoning ability is rebuilt from scratch in this mod, and the old parts
have no further use.

**The large nations bundle** does the same thing with a different set of
additions (a five-level `Dark Genie`, `Kakoolha, King of the Cuotl`,
`Fallen Sentinels`, among others) and three things no other archive here
carries:

- **five ready-made skirmish maps** — `Ashfall`, `Dark Chasm`, `Death Basin`,
  `Pillars of Thuran`, `Maldini Heights` — with all seven mainline nations
  already wired into their `<TRIBES>` block;
- a **`Bonus\Bonus Tribes` folder** of rewritten faction files (`dark genie`,
  `pirata`, `mianans` and others), for adding a sub-nation that isn't on any
  of the five ready-made maps;
- a **`Bonus\Acerbus` folder** — a hero the author judged too strong for the
  main package, enabled as a separate, optional step.

## How their solutions differ from each other

All four archives chase the same broad goal — give the player something the
stock game does not — but they reach for different points among the three
rule layers covered in [rule-layers.en.md](rule-layers.en.md), and that is
worth seeing side by side rather than one mod at a time.

**Heroes-in-skirmish** works entirely through layer one: new units, abilities
and bonuses are simply appended to the shared rule pool with `TRIBE_MASK` set.
That is the exact same route
[campaign-to-skirmish.en.md](campaign-to-skirmish.en.md) uses to carry a
single hero across by hand — this mod just does it for every hero at once.

**Expansion** also writes new records into layer one, but unlike the other
three, among its 19 `.bhs` files are scripts for individual campaign
missions — `campaigns\<nation>\Maps\*.bhs`. None of the other three archives
carries a file like that: expansion alone touches the logic of a specific
campaign map, not just the shared rule pool and the shared script library.

The shared AI script library (`rules\scripts\vinci.bhs`, `cuotl.bhs`,
`alim.bhs`, and their `*_spellai.bhs` ability-AI counterparts) is actually
carried by all four archives, and in every one of them those files differ
from the original — unsurprising, since a mod that adds new units, buildings
and abilities needs the very files that decide who gets hired and what gets
researched to know those new records exist. That is not something a file
listing alone can tell apart; it takes a byte-for-byte comparison against the
original, and by that comparison all four differ. So the install
instructions' requirement to delete `scripts.big` applies equally to all
four, not just to expansion (what that archive is and why it overrides
`rules\` on disk is covered in [rule-layers.en.md](rule-layers.en.md)).

**Both nations mods** also write new records into layer one — some of the
added units carry a mainline nation's `TRIBE_MASK` outright and are available
immediately, the same way the heroes mod's additions are. But the actual
point of these mods — sub-nations like the Pirates or the Mianans — works
differently: `Acerbus, the Mystic`, `Marwan, the Dark Alim`, `Desert Mystic`
and `Doge Guard` all carry `TRIBE_MASK` `0000` in the mod's own files, the
same "belongs to no one" value a campaign hero starts with before being
carried across. What grants access to them isn't a mask, it's the `own` list
inside a faction file (`data\tribes\ctw\...`) plus wiring that set into a
map's `<TRIBES>` block — layer two and three at once, exactly the "other"
route [rule-layers.en.md](rule-layers.en.md) describes for handing an entity
to the player. What separates the two nations mods isn't the method, it's how
much of the legwork is already done: v4 leaves the scenario-editor step to
the player for every map they want it on, while the large bundle ships five
maps where that step is already done, plus a separate set of rewritten
faction files for the sub-nations that aren't on any of those five.

All four share one thing done four different ways: an optional extra, bolted
on rather than baked in. The heroes mod packages its `Extras` folder: an AI
change to let it hire more than two heroes at once (with the author's own
warning about the performance hit), plus a `Fallen Refuge` for the Cuotl and
a `Clockwork Scrapyard` for the Vinci, each switched on separately. Nations
v4 itself solves the one-hero-too-strong problem differently: `Acerbus, the
Mystic` sits right in the main package with `TRIBE_MASK` `0000`, and that
mod's own `Read Me.txt` tells the player to open `unitrules.xml` in Notepad
and flip the last digit of the mask by hand. The large nations bundle solves
the same problem a third way: Acerbus isn't in its main package at all, he
ships as a separate `Bonus\Acerbus` folder that has to be copied in as an
extra step if wanted — and his mask is still `0000` even inside that separate
folder, so exactly how copying those files alone is meant to enable him,
without the same manual mask edit, isn't something the readme actually says.

## How these mods install, and why this project does it differently

All four install the same way: copy the folders they ship (`Data\`, `rules\`,
and `campaigns\` where present) over the matching folders in the game's
install, overwriting what's there. The author insists on two conditions for
every one of the four: the game has to be patched and its own unpacking
utility has to have been run once, to turn `Data\`, `rules\` and `campaigns\`
from archives into editable files on disk — and the `scripts.big` file inside
`BIGS\` has to be deleted, or the engine keeps reading AI scripts from it and
simply ignores anything written to `rules\`.

The install this knowledge base's other documents were written against never
had that unpacking utility run, and `scripts.big` is still in place. That is
why every edit described elsewhere here is written straight into the game's
own archives — a method verified in place — rather than by copying loose
files the way these four mods do, a method never tested against this
particular install.

## What was not checked

None of the four mods was installed in full. The only thing actually run in
the game is a single hero's blocks, pulled from the campaign-heroes mod and
covered in [campaign-to-skirmish.en.md](campaign-to-skirmish.en.md). Every
number and claim in this document comes from reading the archives' own
files, not from launching the game with any of these mods installed.
