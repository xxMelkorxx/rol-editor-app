# What's driven by data, and what's hardwired into the engine

Russian: [mechanics.md](mechanics.md)

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

[game-data.en.md](game-data.en.md) already establishes the headline fact:
developer comments inside the rule files diverged from what the game actually
does three separate times, and each time the data turned out right, not the
comment's text. This document needs the same caution in two more places that
work differently. First, a field's own value can diverge from what the game
shows — not just its written description: further down is a price that never
changes in the file yet climbs with every purchase. Second, a plausible-looking
theory about what a field means can simply be wrong, with no comment
disagreement involved at all — that's how one candidate for "the player
presses this ability" gets rejected below. The only way to tell a working
theory about a mechanic from a misleading one is to reproduce it in the game,
not to re-read the text next to the field one more time.

## Technologies: 141 records, plus 322 more next to them

`data\techrules.xml` is not one list but two containers under a shared root:
`<TECHS>` (141 `<TECH>` records) and, separately, `<BONUSES>`. That second
container is not a small add-on — it holds **322** `<BONUS>` records, each
with its `DATA0` field filled in. A pass over the file that only walks blocks
inside `<TECHS>` doesn't miss a handful of records, it misses more than two
thirds of the total — Vinci bonus grants, hero-tied bonuses, borders, taxes,
healing, production. Both numbers, 141 and 322, are also given in
[rule-layers.en.md](rule-layers.en.md).

By the `WHERE` field, the 141 technologies in `<TECHS>` split without
remainder into three groups:

| Group | What marks it | Records |
|---|---|---|
| Leader research | `WHERE = leader` | 56 |
| Building-tied technology | `WHERE` = a building's name | 60 |
| City and unattached | `WHERE = none`/empty | 25 |
| **Total `<TECH>`** | | **141** |

56 + 60 + 25 = 141, with nothing left over. Next to it sits a separate block
of 322 `<BONUS>` records, with their own field set — they don't show up in
[reference/techs.en.md](reference/techs.en.md) (that table is built from
`<TECHS>` only). The full list of technologies with nation and how each is
obtained is in that same reference.

**One mechanic hides across two of those groups at once.** Vinci's "prototype
factory" is a building whose internal name is `Trade Mission` — the player
sees it labeled `Prototype Factory` — and its technologies share that same
internal name: `Trade Mission Tech Level N`. The mechanic itself is 31
`<TECH>` records: seven service records (`Trade Mission Tech Level 1..7`, cost
0, `WHERE = none`), plus **seven rings of three alternatives each**
(`EXCLUSIVE` wires the three records of one tier into a cycle — taking one
locks out the next one around the ring, not "both neighbors at once"), for
7 × 3 = 21 records with `WHERE = Trade Mission`, plus three top-tier records
with `WHERE = None`. Of those 31: the 21 are already counted under
"building-tied," and the 7 + 3 = 10 under "city and unattached." So `WHERE`
alone does not carve this mechanic out — what ties it together is only the
naming pattern (`Trade Mission Tech Level N`) and the `PREQ0`/`PREQ1` chain,
not one field's shared value. The same trap waits for anyone trying to
isolate a mechanic with a single column filter: check first whether `WHERE`
has cut it in half.

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

What you cannot get by editing `techrules.xml`: repeat purchases of the
top-tier prototype (it carries no `EXCLUSIVE`, so it can be bought again and
again) should cost more each time — yet the `COST` field stays a flat `2v` on
every repeat. Measuring in the game shows the third purchase really costs 3
prototypes, while the file still reads `2v`. This step even has a name —
`data\rules.xml` holds named constants `ramp_final_prototypes` and
`ramp_final_prototypes_for_all`, commented "use ramping for final prototype
techs?". So the mechanic is described in data after all — just in the one
file that, as the section below shows, ignores edits entirely, so the
practical takeaway doesn't change: changing or removing that ramp by editing
files isn't possible.

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
tiers** — `Glass Shards` (no cost — the starting tier) → `Glass Daggers`
(75w) → `Glass Swords` (125w) → `Glass Scimitars` (175w), three links chained
by `FROM`. The first tier's
`TYPENAME` in the data is the same string as the whole chain's usual label
(`Glass Scimitars`) — cross-reference by `TYPENAME` instead of `NAME` and it's
easy to mistake this chain for a ring, which it isn't. An ordinary unit's
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
  version to a nation-specific one;
- **the free-unit grant for a prototype is data after all — just not in
  `techrules.xml`.** `FreeMiner2` ("2 clockwork miners") and its siblings are
  only a trigger; they don't grant anything by themselves. The grant is
  carried by a `<BONUS>` record inside the same file's `<BONUSES>`: its
  `PREQ0` points at the trigger technology, and its `DATA0` names a craft from
  `craftrules.xml` that actually spawns the units:

  ```xml
  <BONUS><TYPENAME>Bonus Clockwork Miners 1</TYPENAME>
    <PREQ0>FreeMiner2</PREQ0>        <!-- trigger technology -->
    <DATA0>Trade Miners 2</DATA0>    <!-- craft that grants the units -->
  </BONUS>
  ```

  `Free Miner 1`, `Free Scout 1`, `Bonus Clockwork Foreman` (which also uses
  `DATA1`/`DATA2`), `Bonus Juggernauts`, and `Lead into Gold` all work the
  same way; the crafts `Trade Miners 1..6`, `Trade Foreman 1`, and
  `Trade Jugger 1` are visible in
  [reference/crafts.en.md](reference/crafts.en.md). There's even a built-in
  off switch: `Bonus Clockwork Miners 4` uses the same pattern with
  `PREQ0 = disable`. Since this runs through `PREQ0`/`DATA0`, wiring the grant
  to a different technology or a different craft is an ordinary data edit, not
  an engine change.

## What's hardwired — don't spend time here

Confirmed the same way, by measurement rather than by guessing:

- **Edits to `data\rules.xml` don't do anything** — [rule-layers.en.md](rule-layers.en.md)
  covers why (836 constants, six experiments, no visible change in any of
  them); here it's enough to remember the conclusion and not go looking for
  levers in that file.
- **The price-ramp step on repeat purchases** of Vinci's top-tier prototypes —
  the `COST` field never changes, yet the real price climbs by 1 prototype
  each purchase; the step itself is even named in `rules.xml`
  (`ramp_final_prototypes`), but that's the same file as above — you can read
  the constant, you just can't change what it does.
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
field has been found that would sort them by purpose. It's also unresolved how
to approach all 322 `<BONUS>` records as a whole: sorting through them has
already turned up dominance conditions, prototype unit grants, borders, taxes,
healing, and production — several different mechanics sharing one container,
not one mechanic, and a class-by-class breakdown of them is still to come. The
groups above make no claim to completeness: they're what measurement has
managed to pull apart so far, not a final registry of every mechanic in the
game.
