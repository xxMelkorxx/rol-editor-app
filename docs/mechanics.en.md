# What's driven by data, and what's hardwired into the engine

Русский: [mechanics.md](mechanics.md)

Before you rewrite a unit's stat or a technology's cost, it helps to know what
a data edit can actually change. Part of the game's mechanics is fully
described in the four rule files from [game-data.en.md](game-data.en.md) — you
can reshape those almost freely. Part only looks configurable: the numbers sit
in a file, but the actual outcome is decided by `legends.exe`, and editing the
field changes nothing on screen. The line between the two is this document's
whole subject, and it saves weeks of guessing, because reading the file cannot
tell them apart — only measuring the game can.

This document continues [game-data.en.md](game-data.en.md) (where the rule
files live) and [rule-layers.en.md](rule-layers.en.md) (why a record in a file
still isn't a guarantee it shows up in battle). Here — of everything those
files contain, what is actually a lever.

## Verify by measurement, not by reading

The rule files are heavily commented, and the comments are usually reliable —
but not always. While sorting technologies and crafts into groups, checking
against the game's real behavior turned up a mismatch with a developer's
comment next to a field three separate times, and each time the measured
behavior turned out right, not the comment's text. Further down are a couple
of such cases: a price that never changes in the file yet climbs in the game,
and a field that looks like the right one but means something else entirely.
The only reliable way to test a theory about a mechanic is to reproduce it in
the game, not to re-read the comment one more time.

## Technologies: 145 records in one file, not 141

`data\techrules.xml` is not one list but two containers under a shared root:
`<TECHS>` (141 `<TECH>` records) and, separately, `<BONUSES>` (4 `<BONUS>`
records). Any pass over the file that only walks blocks inside `<TECHS>`
misses those four entirely — they're just as real, only living in a different
container with a different field set.

By the `WHERE` field, the 141 technologies split without remainder into four
groups:

| Group | What marks it | Records |
|---|---|---|
| Leader research | `WHERE = leader` | 56 |
| Building-tied technology | `WHERE` = a building's name | 60 |
| City and unattached | `WHERE = none`/empty | 25 |
| — | (total `<TECH>`) | **141** |
| Condition-style bonuses | `<BONUS>` inside `<BONUSES>` | 4 |

56 + 60 + 25 + 4 = 145 — exactly what the file holds. The full list with
nation and how each is obtained is in
[reference/techs.en.md](reference/techs.en.md).

**One mechanic hides across two of those groups at once.** Vinci's "prototype
factory" (a building the data itself names `Prototype Factory`, not the label
you see on the building list) accounts for 31 records: seven service records
(`Trade Mission Tech Level 1..7`, cost 0), plus three rings of seven
alternatives each (`EXCLUSIVE` wires a ring so taking one alternative locks
out its two neighbors), plus three top-tier records. Of those 31, 28 have a
`WHERE` pointing at a building (already counted under "building-tied"), and
the three top-tier ones have `WHERE = None` (already counted under "city and
unattached"). So `WHERE` alone does not carve this mechanic out — what ties it
together is only the naming pattern (`Trade Mission Tech Level N`) and the
`PREQ0`/`PREQ1` chain, not one field's shared value. The same trap waits for
anyone trying to isolate a mechanic with a single column filter: check first
whether `WHERE` has cut it in half.

What's clearly safe to change here: all 56 leader-research records follow a
strict cost ladder, `1r → 2r → 4r → 7r`, with zero exceptions — but that's a
convention of the shipped data, not an engine limit. The game accepts a mixed
cost of "resources plus research points" in the same `COST` field if you write
one in. A technology's grid position (`GRID_X`, `GRID_Y`), its predecessor
(`PREQ1`), its age requirement (`PREQ2`) are ordinary fields too, and change
without side effects. Inside a prototype ring you can reshuffle who excludes
whom (`EXCLUSIVE`) without breaking the mechanic — existing mods do exactly
that. Real building-upgrade chains through `PREQ1` (pairs and triples, e.g.
`Sandtough` → `Stonetough` → `Rocktough`) are ordinary data too — reordering or
adding a link works.

What you cannot get by editing these fields: repeat purchases of the top-tier
prototype (it carries no `EXCLUSIVE`, so it can be bought again and again)
should cost more each time — yet the `COST` field stays a flat `2v` on every
repeat. Measuring in the game shows the third purchase really costs 3
prototypes, while the file still reads `2v`. The price-ramp step is wired into
the engine and keyed to which record this is, not to any field value —
changing or removing that ramp by editing `techrules.xml` is not possible.

## Crafts: 509 records, split by purpose

`data\craftrules.xml` holds all 509 `<CRAFT>` records — active abilities,
spells, orders, and service effects — as one flat list. The `WHERE` field
splits it without remainder:

- **171** point at a unit — the ability belongs to a creature;
- **74** point at a building — the ability belongs to a structure;
- **264** point nowhere (`none`, empty, special words like `disable`).

The full list with translations and owners is
[reference/crafts.en.md](reference/crafts.en.md).

The first group isn't uniform: of the 171 records, **88** belong to heroes
(the unit's `FLAGS` contains `h`), and **83** to ordinary units — and these are
different mechanics, even though the `WHERE` field looks identical on both.
The measurable difference:

| Field | on heroes (88) | on ordinary units (83) |
|---|---|---|
| `RESEARCH_COST` set | 66 | 1 |
| `MANA` set | 39 | 4 |
| `COST_LEVEL` set | 12 | 0 |
| a step chained via `FROM` | 55 | 28 |
| `XP` set | 88 | 64 |

A hero's ability is typically **researched for resources and grows through
tiers** — `Glass Scimitars` → `Glass Daggers` (75w) → `Glass Swords` (125w) →
`Glass Scimitars` (175w), three links chained by `FROM`. An ordinary unit's
ability simply exists from birth. Editing a hero's cost, mana, and tiers is a
working lever; on an ordinary unit those fields are blank for a reason, not by
accident.

The remaining 264 records with no `WHERE` split further by who references
them: **72** are derivatives (something else's `FROM`, `CHAIN`, `GRAFT`, or
`ICONGRAFT` points at them — the next tier of a spell or the next link of a
chain, not a standalone entity), **9** are disabled (`WHERE = disable`), and
**183** remain independent and unclaimed — a mix of national dominance
effects, hit effects, states (`Land`, `Takeoff`), and pure service graphics
(`Attrition Graphic 1/2/3/End`).

Cross-cutting mechanisms that don't line up with this split: a spell's level
is almost always expressed as `FROM` pointing at another record — 152 such
links, some names spell it out directly (`Web (Level 3)`, `Plunder
(Level 4)`); what unlocks an ability is `PREQ0`, and across all 509 records it
points at a technology for 152 of them, at a unit for 6, at a building for 2,
and is empty for 349; a multi-stage effect's continuation is `CHAIN`, 37
records call the next part (`Summon Army` → `Summon Army Part 0`). An ability
unlocked by leader research is the common case, not a rare exception: that's
how Vinci's national power works (`Strip Mine`, tied to `Mining 1..4`), Alin's
(`Summon Army`, tied to `Evocation 1..4`), and Cuotl's (`Farsight`, tied to
`Reactor Power 1..4`).

**There is no field for "the player presses this" — that's measured, not
assumed.** Two obvious candidates were tested and rejected. Icon and
`CODETAG` don't work: both turn up mixed on clearly pressable abilities
(`Teleport`) and on purely internal states (`Physics Start`). `TARGET_FLAGS`
doesn't work either: it marks "this ability can have a target selected," not
"the player presses it" — the dominance ability `Cease Fire` is pressed but
has no target, while the hit effect `Hit by Glass` has a target but is never
pressed. The one thing `TARGET_FLAGS` splits honestly is records with a target
(59 of the 183 independent ones) against records without one (124) — meaning
this group is genuinely mixed, and no single field will sort the
player-facing lever out of it.

## What data genuinely controls

Confirmed either by measurement or by direct edit:

- cost, time, grid position and prerequisites of leader research — including a
  mixed "resources plus research points" cost, which never appears in the
  shipped data but is accepted by the engine;
- the composition of Vinci prototype exclusion rings (`EXCLUSIVE`) — mods
  freely reshuffle these between tiers;
- building upgrade chains and hero ability tiers — both go through `FROM`,
  both are ordinary fields;
- research-point production (`RESEARCH_PTS`) — the currency that pays for
  leader research; for Vinci it comes from the research lab turning into one
  of eight buildings (`FROM`, a 4×2 `GRID_X`×`GRID_Y` grid), for Cuotl and Alin
  it comes from districts; 14 buildings carry this field in total, each with
  its own value;
- a building's second grid row (empty in the shipped panel of the prototype
  factory) fills in by hiring a unit: `WHERE` pointing at the building, a
  `GRID_X`/`GRID_Y` coordinate, a `PREQ0` requirement. This isn't a guess —
  comparing four existing mods against the shipped game shows each one adds
  exactly four units this way, and none of them touch the game's code to do
  it;
- a district building's national variant — through `GRAFT`, tying the generic
  version to a nation-specific one.

## What's hardwired — don't spend time here

Confirmed the same way, by measurement rather than by guessing:

- **Edits to `data\rules.xml` don't do anything.** This is the most deceptive
  file of all: 836 named constants, each with its own comment, looking like
  the main dial for economy and combat. Six experiments in a row (both forms
  of the file, both archives, including filling every slot of one parameter at
  once) produced no visible change in the game. Why is covered in
  [rule-layers.en.md](rule-layers.en.md); treat these 836 parameters as a
  description of how the game works, not as a list of levers.
- **The price-ramp step on repeat purchases** of Vinci's top-tier prototypes —
  the `COST` field never changes, yet the real price climbs by 1 prototype
  each purchase.
- **Free-unit grants for a prototype** (`FreeMiner2` = "get 2 miners",
  `FreeJuggernauts` and the like) — these records have an empty `CODETAG`, and
  no field at all carries a count or list of granted units. The only thing the
  engine can be keying off is the record's own name (`TYPENAME`), meaning the
  effect is wired by name, not by a field's value.
- **The district-into-city transformation itself.** The rules describe only
  the building tiers (`SmallCity` → `City` → `Large City` → `Great City`,
  chained by `FROM`) and the technology markers the city grants itself
  (`City Tech` and its siblings, `COST = 0`, `WHERE = none`) — but none of the
  16 district records carries a field linking a specific built district to the
  city's growth. The markers can be used as a condition elsewhere (that's what
  the third and fourth leader-research tiers do), but triggering the growth
  itself by editing data is not possible.
- **The concept of "a player-activated ability"** itself — see the previous
  section: the data has no representation for it at all, so there can be no
  lever for it in the data either.

## What's still unsorted

The 183 independent, unclaimed crafts mix at least four different purposes
(dominance conditions, hit effects, states, service graphics), and no single
field has been found that would sort them by purpose. It's also unresolved
where the four `<BONUS>` records belong conceptually — a mechanic of their
own, or part of something broader. The groups above make no claim to
completeness: they're what measurement has managed to pull apart so far, not
a final registry of every mechanic in the game.
