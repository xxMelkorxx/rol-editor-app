# The three layers of the game's rules

Russian: [rule-layers.md](rule-layers.md)

A unit exists in the rule files but never shows up in battle. A stat edit
takes effect in the campaign but does nothing in a skirmish. A nation exists
in the archives, yet there is no way to pick it in the lobby. None of this is
a bug or an accident — the same entity is described in not one place but
three layers, and each layer is able to change the one before it. Here is how
those layers are built, and how to read a mismatch between them.

This document continues [game-data.en.md](game-data.en.md): that one maps the
archives and the four rule files, this one explains why a record sitting in
those files is not yet a guarantee it will ever appear on screen.

## The three layers at a glance

| Layer | Where it lives | What it decides |
|---|---|---|
| 1. Base rules | `data\unitrules.xml`, `buildingrules`, `techrules`, `craftrules` | the shared pool of entities and their stats |
| 2. Faction rule sets | `data\tribes\*.xml`, `data\tribes\ctw\*.xml` | which entities from the pool a given faction gets, and with what changes |
| 3. The map | a `.map` file | which rule sets are wired in, and which nations are playable on this map |

## Layer 1 — the shared pool

The four rule files are the entire set of entities the game knows about at
all: 335 `<UNIT>`, 142 `<BUILDING>`, 141 `<TECH>` plus another 322 `<BONUS>`
(they live in the same `techrules.xml`, as a separate block), and 509
`<CRAFT>`.

Availability inside this layer is set by the `TRIBE_MASK` field — four bits,
left to right Vinci, Cuotl, Kahan, Alin. But that mask only speaks for the
three main nations: it answers "can Vinci build this", and knows nothing
about the Pirates, the Mianans, or any other campaign sub-nation — for those,
the mask is a flat `0000`, and access is granted entirely by the second
layer.

The key of a record here is `TYPENAME`, not `NAME`; the fact itself, along
with the example of three identically-named units, is covered in
[game-data.en.md](game-data.en.md). What matters for this document: `NAME`
being non-unique is a property of layer 1 specifically, not something that
appears separately at the faction-set or map level. Among the 335 units,
`NAME` repeats 53 times; in Russian, the three "Miner" units from that
example are just as indistinguishable by ear — that is not a translation
slip, it is the same layer-1 property showing up in another language.

**A trap built into the layer.** The files are not structured alike. `<UNIT>`
and `<CRAFT>` sit right at the root of their files, while `techrules.xml`
nests `<TECH>` inside a `<TECHS>` container and `<BONUS>` inside a
`<BONUSES>` one. A block dropped into the wrong container still produces
perfectly valid XML, and a naive tag count still comes out right — but the
engine never sees it.

## Layer 2 — faction rule sets

`data\tribes\` holds the four main-nation files (`alim`, `cuotl`, `vinci`,
`empty`) plus a `ctw\` folder with 29 campaign files. A faction file is not a
list of entities — it is a list of operations on the layer-1 pool. The
`reference_style` attribute names the operation; across all 29 campaign
files there are 3356 of them:

| Operation | Blocks | What it does |
|---|---|---|
| `remove` | 1728 | take an entity away from this faction |
| `replace` | 523 | change individual fields |
| `own` | 459 | hand an entity over to the faction |
| `copy` | 364 | clone someone else's record under its own `TYPENAME` |
| `new` | 282 | describe a brand-new entity from scratch |

A direct consequence follows: a nation with no mask of its own has no path
into layer 1 at all — it is assembled entirely out of `own` and `remove`
operations. `pirata.xml` is 4 `own` and 23 `remove` plus a list of leaders
and cities; `mianans.xml` is 82 `own` and 303 `remove`. Such a nation is
defined by what it was granted and what was taken away, not by a block of
its own in the base files. A full breakdown of the new and cloned campaign
entities, translations included and with a note on where each one ends up,
is in the [ctw](reference/ctw.en.md) reference: 646 `new`-and-`copy` records,
87 of them untranslated.

The main-nation files are built differently: `vinci.xml` describes not
operations but the whole makeup of a nation — `<LEADERS>`, `<CITIES>`,
`<STARTS>` (starting sets for a normal game, a big-city start, and a
defense start) — and it carries a `playable="1"` attribute. Campaign files
have a different root element, `<ROOT name=…>`, with no `playable` attribute
at all.

## Layer 3 — the map

A map file is XML despite its `.map` extension. Inside it: a header carrying
the match settings, terrain geometry, pre-placed units, and the block that
matters for rules:

```xml
<TRIBES>
  <TRIBE name="CTWRules" playable="false"/>
</TRIBES>
```

Each line wires in a layer-2 rule set by file name and declares whether that
faction can be played. **The map is what turns layer 2 on**; with no line in
`<TRIBES>`, a rule set simply does not exist for that map.

Measured on the installed game (62 skirmish maps, 36 campaign maps):

| Maps | `<TRIBES>` |
|---|---|
| 61 skirmish maps | empty — no rule sets wired in |
| 1 skirmish map | one set, `DMRules`, but with `playable="false"` — the same map whose description says "Special Deathmatch Rules in Effect" |
| Campaign maps | `CTWHeroes` on 21 maps, `CTWVinci` on 19, `CTWAlim` on 17, `CTWCuotlRules` on 7, then story factions: `moon god` 6, `sun god` 5, `death god` 5, `dark alim` 4 |

Which answers the question from the top of this document: a campaign hero
does not show up in a skirmish not because the game "doesn't know" about it
— it knows perfectly well, the hero's rules sit in `ctwheroes.xml`. The
reason is that 61 of the 62 skirmish maps simply never wire the `CTWHeroes`
set into their `<TRIBES>` block, so for those maps layer 2 is empty for that
hero.

A map also changes settings for the match itself, through header attributes
(`force_rush_rules_disable`, `does_override_teamstyle`, `enable_dominance`
and similar ones). What it never does is override an entity's stats: `HITS`,
`COST` and `TRIBE_MASK` never once appear in a map file. Any stat change has
to come through layer 1 or layer 2.

## Two ways to hand an entity to the player

**Through layer 1.** Put the entity in one of the four `data\*rules.xml`
files and give it the `TRIBE_MASK` of the nation you want. This works on any
map, including all 62 skirmish maps, because layer 1 is always wired in,
with no `<TRIBES>` block needed. This is how a campaign hero gets carried
into a skirmish battle — the recipe and its seven pitfalls are in
[campaign-to-skirmish.en.md](campaign-to-skirmish.en.md).

**Through layers 2 and 3 together.** Leave the entity inside its faction's
rule set as is, and wire that set into the map you want from the game's own
scenario editor: the switch lives in the map's properties, under the game
settings section, among the nation-management controls. The exact menu path
(`Map Properties → Game Settings → Manage Nations`, a `Playable` flag) comes
from a third-party modding guide, not from running the editor ourselves —
the labels in your copy of the game may differ, but the section is the same
one. This only works on the map where it was wired in, but it never touches
the base rule files at all. This is how the existing new-nation mods are
built — covered in [mods.en.md](mods.en.md).

## Where things physically live

Layer-1 rules and layer-2 rule sets live inside the `mod_data.big` and
`multiplayer_data.big` archives ([game-data.en.md](game-data.en.md) explains
why an edit belongs in `mod_data.big` specifically). Maps, by contrast, sit
on disk as ordinary files, not inside any archive: `maps\` holds the 62
skirmish maps, `campaigns\<nation>\MAPS\` holds the 36 campaign maps, and one
more map lives in `mapfiles\`.

AI scripts live separately, in the `scripts.big` archive, and follow a rule
of their own: as long as that archive is present, the engine reads scripts
only from it and simply ignores any edits made in the `rules\` folder on disk
(the scripts sit inside it, at paths like `rules\scripts\prodai_lib.bhs`).
Authors of AI mods have to delete `scripts.big` entirely for their own
scripts to take effect at all — otherwise the archive and the folder on disk
carry the same content, and the archive wins.
