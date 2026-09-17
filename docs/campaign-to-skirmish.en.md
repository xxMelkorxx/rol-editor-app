# Carrying a campaign hero into a skirmish battle

Russian: [campaign-to-skirmish.md](campaign-to-skirmish.md)

[rule-layers.en.md](rule-layers.en.md) explains why a campaign hero never
shows up in a skirmish: its rules exist, but they sit in a layer-2 set that
skirmish maps never wire in. This document is not the theory — it is the
transplant itself: the three files that get edited, and seven pitfalls, each
of which cost a separate round of "edit → launch → read the error window".
The recipe was built exactly that way, round by round, not derived ahead of
time from reading the files — so every pitfall below carries a number and a
symptom you can recognize in your own attempt, not just a description in
general terms.

The running example is Battaglion, the Siege General
(`Battaglion, the Siege General`) — a campaign hero who, on 2026-08-24/25, was
carried into a skirmish battle as a fully working fourth Vinci mercenary:
hired for 100 timonium and 100 wealth, levels up, and uses all four branches
of his abilities. For the rest of the heroes and other campaign entities that
could in principle be carried the same way, see the
[ctw](reference/ctw.en.md) reference.

The method described here goes through layer 1: it works on any skirmish
map, but it means moving the blocks by hand. The other method — wiring the
campaign rule set into layer 2 and 3 directly for a given map — never
touches the base files, but only works on the map where it was wired in.
Both methods, and the line between them, are covered in
[rule-layers.en.md](rule-layers.en.md).

## What to move

A single unit file is not enough: a campaign hero is described across three
files of the shared rule pool at once.

| File | What goes in it | Count for Battaglion |
|---|---|---|
| `data\unitrules.xml` | the hero itself, its level-2..5 forms, and any temporary transformation forms | 335 → 342 blocks |
| `data\craftrules.xml` | the hero's abilities (the `WHERE` field names its `TYPENAME`) | 509 → 521 |
| `data\techrules.xml` | the bonus records that the hero's passive abilities reference | 322 → 325 (the 141 `<TECH>` records in the same file are left alone) |

The source of the blocks being moved is the campaign files under
`data\tribes\ctw\`, `ctwheroes.xml` above all; some bonuses may sit in
`ctwbonuses.xml` instead. Reading that source and then writing the moved
blocks into both archives, `mod_data.big` and `multiplayer_data.big`, is a
question of format: Latin-1 encoding, mandatory zlib compression, and the
fact that all 29 campaign files sit in the archive in two forms at once,
plain text and compiled `bxml`, are all covered in
[game-data.en.md](game-data.en.md) — work with the text form, or the edit
simply never reaches the game.

## Checking against the base game

Before moving anything, it helps to see how the heroes already present in
skirmish are built: the transplanted hero is easiest to check by contrast
with them, rather than against an abstract rule.

By the stock ability records in `craftrules.xml`:

| Heroes | Records | Branches | Purchasable ones |
|---|---|---|---|
| Giacomo, the Doge, Lenora, Sawu, Damanhur | 12 | 4 | 11 |
| Dakhla | 15 | 4 | 11 |
| Xil, Czin, Shok (Cuotl) | 4 | 4 | 0 |

The usual layout is four ability branches by grid column, twelve records per
hero, with the first tier of the first branch free. Dakhla has fifteen
records, but three of them are extra: `Sand Building 1/2/3` priced at `0` —
the base game itself uses the "two records, one button" trick that pitfall 6
below has to unpick in reverse. Cuotl heroes never have purchasable
abilities at all — they cannot be used as a model when it comes to the
research row.

The stock Vinci hero's prices and experience by branch: `0 / 75w / 125w /
175w` in the first, `150 / 225 / 300` in the second, `300 / 400 / 800` in the
third, `1000 / 2000` in the fourth; experience stays constant within one
branch — 10, 20, 30, 40.

## Seven pitfalls

**1. The engine treats a blank `CODETAG` as a value.** In campaign files it
is often written out as a line break plus three tabs — not empty to the eye,
but not real text either. Battaglion has seven such blocks, and all seven end
up with the same non-empty codetag, and the game stops on load with "Two
types have the same codetag! This is illegal" (that window, like every other
error-message quote in this document, is engine text in English; neither the
message nor a translation of it turned up in `legends.exe` or in the game's
installed libraries, so for an interface running in another language this is
reproduced from what actually appeared on screen, not pulled from a string
found anywhere in the files). In the base `unitrules.xml`, 129 blocks carry a
`CODETAG` field: 93 hold distinct non-empty values, and 36 are written as
`<CODETAG/>` — that empty tag is the correct form; a tabs-and-newline one
needs to be replaced with it or dropped entirely.

**2. A campaign hero's `TRIBE_MASK` is `0000` — "belongs to no one".** It
needs the mask of the target nation set explicitly: `1000` Vinci, `0100`
Cuotl, `0001` Alin (`0010` is reserved for the Kahans, a nation that was
never implemented). The mask has to go on **transformation forms too**: for
Lenora, the `Lenora Boosted` form carries `1000`, and without it the
transformation simply never shows on screen.

**3. References to other entities hide inside the `DATA0` field.** That is
where an ability names, by identifier, the bonus or unit it works with, and
a mistake there produces a load-time window saying "Invalid key X of type
BONUS/UNIT". Battaglion has eight such references: six to bonuses and two to
his boosted forms. The field has to be checked by whether the recorded name
actually resolves in one of the base files, not by what it looks like: the
`Siege Shot` ability has `4 number to lower the storm number by` in that same
`DATA0` field — a plain number with an author's comment attached, not a
reference at all.

**4. `techrules.xml` needs the right container.** The fact itself, and the
numbers — 141 `<TECH>` inside `<TECHS>`, 322 `<BONUS>` inside `<BONUSES>` —
are already covered in [rule-layers.en.md](rule-layers.en.md); what matters
here is the practical move. A record inserted just before the closing root
tag produces outwardly valid XML and even a correct count under a naive tag
scan, but the game never sees it and raises the same "invalid key" window.
The reliable move is to insert a new record right after the last record of
the same type: that puts it inside whichever container the file is actually
using.

**5. Grid coordinates copied from the campaign are almost always wrong.**
`GRID_X`/`GRID_Y` place the button's column and row on the panel, while
`GRID_X2`/`GRID_Y2` place it in that branch's research row. Count only among
abilities whose `SPELL_FLAGS` contains `a` — the file's own comment for that
letter reads "Craft/Ability must be researched", meaning exactly the
abilities that are actually bought through research. There are 87 of those in
the base game, and 80 of them have `GRID_Y2 = 1`; zero shows up on the
remaining seven, and all seven of those have `WHERE = None`. Battaglion's
passive branch, carried over with a secondary coordinate of `(2, 0)`, never
appeared in the research row at all: the column was empty on screen and the
ability could not be bought. The correct value turned out to be `(2, 1)`.

**6. Button artwork can live on the ability's paired record, not on the
ability itself.** Campaign abilities are frequently built as two records
working together, and the fields `TEX_ID`/`TEX_COL`/`TEX_ROW`/`TEX_CLIP` (the
icon's address inside its atlas — how those atlases work is covered in
[game-data.en.md](game-data.en.md)) live on the secondary, support record.
Moving both records separately produces a button with no picture in the
game, plus a stray extra button in someone else's grid column. The right
move is to **merge** the two records into one; what that looks like on a
concrete example is in the section below.

**7. Campaign economy does not travel with the mechanic.** Campaign
abilities typically carry `RESEARCH_COST = 0` and `PREQ0 = None` — the
hero's progression in the campaign runs on the map's story, not on the
player's resources. For a skirmish, prices and requirements (such as the
large-city and great-city technologies) need to be set to match the stock
hero from the comparison section above, or every branch opens for free the
moment the match starts. The experience field is not a price but a reward —
how much experience the hero earns for the upgrade; in campaign records the
values run noticeably higher and climb faster along the chain, so carrying
them over as-is lets the hero reach level five after only two purchases.

## What a merge looks like

Battaglion's grapeshot ability in the campaign is exactly the pitfall-6 pair
— two records working together:

| | `Grapeshot` | `Grapeshot Slow` |
|---|---|---|
| damage field | `75y,30,100,100,7` | `0` |
| radius | — | `6 radius` |
| effect | — | slow, via `DATA0 = Grapeshot Bonus` |
| artwork | none | `TEX_ID = TEX_ICONS_PROTO_L3`, `TEX_COL = 0`, `TEX_ROW = 5` |
| tooltip | `GRAPESHOT` | `GRAPESHOT` — the same one |

The merged record takes the flags of both (`a12` + `gmaj` → `ja12g`), the
union of both targeting sets (`dbfyp` + `adbfyn` → `adbfyp`), and takes the
radius and artwork from the second record, while the slow effect itself
moves straight into the damage string: `75y,30,…` becomes `75yr75,30,…`. The
`r<number>` modifier inside the damage field is not something invented for
this transplant — it is a stock trick of the base game itself: it appears on
exactly eight abilities, and all eight of them immobilize their target
(`Web`, `Web 3`, `Web 4`, `LWeb`, `BurrowWeb`, `Web Shot`,
`Strip Mine Knockdown`, `Deadly Corruption`). After a merge like this, the
second record that carried the slow effect is no longer needed, and nothing
needs to be added to `techrules.xml` for it.

## A ready-made third-party solution

A third-party mod already does this at scale:
`Rise_of_Legends_-_Motters_Campaign_Heroes_In_Skirmish_Mod_-_version_3` (by
Otter) carries every campaign hero into skirmish at once, adding 414 unit
records, 680 craft records and 393 bonus records against 335 / 509 / 322 in
the original game. Inside the mod's archive sits `Modding Guide\Modding
Heroes.txt` — an author's own guide, which independently confirms pitfalls
1, 2, 5, 6 and 7 from the list above. The Battaglion blocks behind this
document's example were taken straight from it: 12 abilities instead of 15
(the mod author had already merged three "ability plus support record"
pairs), a `1111` nation mask on every ability against `1000` on the unit
itself (access to the ability is already decided by the `WHERE` field, which
names its owner — the nation mask itself is redundant here), and the artwork
sitting on the record it belongs on instead of hidden on a second one.

**Installing that mod as-is is not the way to do this.** It is built to have
its `Data\` and `Rules\` folders copied straight into the game's install
root, and the game never reads those folders — it only reads the `.big`
archives inside `BIGS\`. The working path is to pull the needed blocks out
of it and write them into the archives yourself.

One thing this recipe never covers at all is AI scripting. Without it, a
transplanted hero is fully playable by a human, but the computer opponent
will neither hire it on its own nor research its abilities.

## Order of work

1. Make a copy of `mod_data.big` and `multiplayer_data.big` from the game's
   install folder before changing anything in them — the archives may
   already carry earlier edits that are worth not losing on a rollback.
2. Gather the needed blocks from the campaign files: the unit and its forms,
   its abilities, its bonuses. The campaign files read with the same format
   caveats as any other archive content — Latin-1 encoding, and the fact that
   all 29 campaign files sit in the archive in two forms at once, plain text
   and compiled `bxml`, with the text form being the one that's needed — all
   covered in [game-data.en.md](game-data.en.md).
3. Check whether the hero's own slot on the recruit panel is free. The
   `<UNIT>` block carries its own `GRID_X`/`GRID_Y` — the coordinate of its
   button among the nation's mercenaries, a field entirely separate from the
   ability-grid coordinates of pitfall 5 above. For Battaglion, the copied
   `GRID_X = 2` collided head-on with Lenora, who already occupies Vinci's
   second mercenary slot — a free column had to be picked separately.
4. Go through all seven pitfalls above — all of them, not a selection: they
   are not mutually exclusive, and the Battaglion example ran into every one
   of them at once.
5. Check before writing anything to the archive: the XML parses in full; no
   duplicate `CODETAG` values; no whitespace-only `CODETAG` values; every
   name inside `DATA0` resolves in the base files; every block sits inside
   the right container.
6. Write the checked blocks into both archives — with the same format caveat
   that applies to any other archive write: Latin-1 for text and mandatory
   zlib compression, or the game refuses to read the file at all
   ([game-data.en.md](game-data.en.md)) — and launch the game.
7. Read every load-time error window literally: it names the entity and its
   type, which is usually enough to find the exact description still left
   unmoved in the campaign files.
