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
named outright, and a name at the very end. The same name signs the author's
own `Modding Guide\Modding Heroes.txt` inside the campaign-heroes mod — a
separate write-up that, by its own words, was written "by request for a user
on ROL Heaven". Two independent files in two different archives, carrying the
same signature — that is enough to name the author outright: **Otter**.

The remaining two archives, `New_Nations_Mod_-_version_4` and the large
bundle `rise_of_legends_motters_new_nations_mod`, carry no signature at all.
But the "HOW TO ENABLE NATIONS" section of their `Read Me.txt` files matches
word for word between the two, down to the scenario editor's button names;
both are written in the first person ("thanks for trying out my new mod"),
both mention not having written a full AI for the sub-nations, and both point
to "Rise of Legends Heaven". That is a match in method and voice, not a
signature — not quite enough to state authorship as fact, but nothing in the
text contradicts it either. For these two archives, authorship is not
directly signed here, only described as apparently the same author, going by
the text.

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
buildings, 11 abilities, 7 bonuses) and the only one that touches more than
the rule files. Among the additions: a five-level `Ix, the Moon God` for the
Cuotl, a five-level `Petruzzo, Lord of Miana` for the Vinci, the
`Clockwork Scrap Yard` and `Imperial Observatory` buildings, and the
`Laser Tank` and `Tank Proto` technologies. The tooltip text file grows too:
1,748 `ENTRY` records against 1,710 in the original. This mod ships two
builds, "Original" and "Balanced" — they diverge across all four rule files,
and the balanced one prints roughly a third fewer lines to disk than the
plain one.

**Nations v4** turns campaign sub-nations into playable ones, adding 130 new
units, among them `Acerbus, the Mystic`, `Marwan, the Dark Alim`,
`Desert Mystic`, `Doge Guard`. It also drops exactly thirteen
`Summon Army Part *` ability records compared with the original game files —
a clean removal, not a rename: the army-summoning ability is rebuilt from
scratch in this mod, and the old parts have no further use.

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
three, it additionally edits 19 `.bhs` files: not only campaign map scenario
scripts (`campaigns\<nation>\Maps\*.bhs`), but the shared script library
itself — `rules\scripts\vinci.bhs`, `cuotl.bhs`, `alim.bhs`, and their
`*_spellai.bhs` ability-AI counterparts. This is the only one of the four
mods that changes how the computer opponent behaves, not just what entities
exist — and so the only one for which deleting `scripts.big` doesn't merely
unlock the edits, it actually delivers new AI behavior rather than just
letting the game read `rules\` off disk (what that archive is and why it
overrides `rules\` on disk is covered in
[rule-layers.en.md](rule-layers.en.md)).

**Both nations mods** take a different route entirely — through layer two and
three at once: the nation itself already exists in the `data\tribes\ctw\`
sets, and the mod never touches the shared pool, it just wires the right set
into a map's `<TRIBES>` block — exactly the route
[rule-layers.en.md](rule-layers.en.md) calls the "other" way to hand an
entity to the player. What separates the two nations mods isn't the method,
it's how much of the legwork is already done: v4 leaves the scenario-editor
step to the player for every map they want it on, while the large bundle
ships five maps where that step is already done, plus a separate set of
rewritten faction files for the sub-nations that aren't on any of those five.

All four share one thing done four different ways: an optional extra, bolted
on rather than baked in. The heroes mod packages its `Extras` folder: an AI
change to let it hire more than two heroes at once (with the author's own
warning about the performance hit), plus a `Fallen Refuge` for the Cuotl and
a `Clockwork Scrapyard` for the Vinci, each switched on separately. The large
nations bundle applies the same idea to a single hero: `Acerbus, the Mystic`
sits outside the main package, reachable only by opening the rule file by
hand and flipping one digit in `TRIBE_MASK` — the author's own choice not to
make casually available something they judged too strong.

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
