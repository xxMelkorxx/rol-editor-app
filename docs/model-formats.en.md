# Models and animations: `.bh3` and `.bha`

Russian: [model-formats.md](model-formats.md)

This is a separate branch of the knowledge base — graphics rather than rules.
Models, animations and textures live in the same `.big` archives as everything
else: meshes `.bh3`, animations `.bha`, prefabs `.pfb` and textures `.tga` sit
in the per-faction archives (`vinci_units.big`, `alim_buildings.big` and so
on), while the tables that tie them to units are in `data\`. The archive map is
[game-data.en.md](game-data.en.md), which also explains why a loose file
dropped next to the game is never read. For models that rule holds without
exception: a modified `.bh3` reaches the game only by rebuilding the archive it
came from.

The format is Big Huge Engine, the same one Rise of Nations used, raised to
versions 10 and 11. Below: what it is made of, where the traps are, and what
cannot be done with it.

The numbers come from a sweep over the whole shipped data: **783 `.bh3` files,
1927 `.bha`, 1321 `.pfb`**, 293,198 chunks across meshes and animations.

## Where to start: somebody else's tool

There is a public library for the same format under Rise of Nations —
[ptasev/Rise-of-Nations](https://github.com/ptasev/Rise-of-Nations): a `.bh3`
parser, a glTF converter and a Blender add-on. That is somebody else's work
under its own terms; it is named here because it is a sensible starting point,
since the shape of the format is already described there.

On Rise of Legends files it stops on the very first one:

```
Expected chunk id 1 but was 1000.
```

It trips over a header chunk that Rise of Nations did not have. Nothing needs
rewriting from scratch — the differences are listed in their own table below.

## The chunk tree

Both formats are a tree of chunks of one shape, little-endian:

| Offset | Type | Field |
|---|---|---|
| 0 | `int32` | `size` — chunk size **including these eight bytes** |
| 4 | `uint16` | `id` |
| 6 | `uint16` | `numChildren` |
| 8 | | payload, then the children back to back to the end of the chunk |

The payload length is **written nowhere** in the file, and one measured
invariant is what lets you derive it:

> **A chunk with children has exactly zero payload; a leaf has a payload of
> `size − 8`.** True for all 293,198 chunks in the shipped data, without a
> single exception.

So parsing is a single forward pass, with no guessing about where the payload
ends and the children begin. The root chunk's `size` equals the file size —
which doubles as a first integrity check.

An array inside a leaf is laid out the same way everywhere: `int32 count`, then
`count × stride` bytes. Every leaf length matches the formulas below to the
byte, with no padding or alignment anywhere, so **byte-exact rewriting is
achievable**: replace one leaf's payload, fix the parents' sizes, and the rest
of the file survives the write untouched — including chunks whose meaning
nobody knows.

## Inside `.bh3`

| id | What | Layout | Instances |
|---|---|---|---|
| 0 | root | container | 783 |
| 1000 | header | container | 783 |
| 1001 | version | `int32` — 10 or 11 | 783 |
| 1002 | exporter stamp | ASCII, build time and export time | 694 |
| 1 | mesh | container | 785 |
| 2 | positions, **per skinning record** | array, stride 12 (three `float`) | 785 |
| 3 | normals, per skinning record | array, stride 12 | 785 |
| 8 | a vector per skinning record, purpose unknown | array, stride 12 | 768 |
| 4 | UV coordinates, **per draw vertex** | array, stride 8 (two `float`) | 785 |
| 5 | indices, addressing **draw vertices** | array, stride 2 (`uint16`); `count` is the number of indices, so a third as many triangles | 785 |
| 50 | weight of a skinning record | array, stride 4 (`float`) | 466 |
| 51 | draw vertex the record belongs to | array, stride 4 (`int32`) | 466 |
| 110 | vertex colours | array, stride 4 (RGBA) | 64 |
| 6 | skeleton node | container | 24,143 |
| 7 | bone | see below | 23,629 |
| 111 | bone with a joint description | bone + 44 bytes | 514 |
| 13 | bounds | container | 783 |
| 14, 15, 16 | three bounds slots | container, often empty | 783 / 783 / 775 |
| 17 | sphere | 16 bytes: centre (3 `float`) + radius | 978 |
| 18 | box | 24 bytes: corner (3 `float`) + size (3 `float`), **not** min/max | 104 |
| 70 | model flags | 4 bytes | 309 |
| 90 | not worked out | 4 bytes | 63 |
| 113 | collision header | 8 bytes | 94 |
| 114 | PhysX collision | NovodeX stream, signature `NXS\x01 CVXM` (convex mesh) | 94 |

There are two versions: **10 in 89 files, 11 in 694**. Chunk `1002` exists only
in version 11.

**The order and the number of the mesh's children vary.** Five different sets
occur, from `(2, 3, 4, 5)` to `(2, 110, 3, 8, 4, 5, 51, 50)`. Read by `id`, not
by position; a parser written as "expect 2, then 3, then 4, then 5" breaks on
most of the shipped files. Two files out of 783 carry two meshes and two
skeletons at once.

### The bone

| Offset | Type | Field |
|---|---|---|
| 0 | `int32` | index of the bone's first vertex |
| 4 | `int32` | how many vertices the bone owns |
| 8 | `int32` | name length |
| 12 | ASCII | name |
| 12 + len | `float × 4` | quaternion, **stored inverted** |
| 28 + len | `float × 3` | position |
| 40 + len | `float` | scale, always 1.0 |
| 44 + len | `int32` | flags, values 0…3 |

Chunk 111 adds 44 bytes of joint description on top of that; the numbers in it
are recognisably rotation limits (0.5236 = 30°, 0.7854 = 45°). Such bones occur
in 46 models, PhysX collision in eight, and those eight are a subset of the
forty-six.

**The name length does not always include the terminating zero.** Across the
shipped data 23,729 names end with a zero byte and 414 do not. Read all
`nameLength` bytes and trim trailing zeros; a blind `nameLength − 1` eats the
last letter of those 414, turning `DUMMY01` and `DUMMY02` into the same
`DUMMY0`. From there, any code that relies on bone names is working with false
matches.

The skeleton tree: node `6` holds the bone itself (`7` or `111`) as its first
child, then one node `6` per child bone.

## Inside `.bha`

| id | What | Layout | Instances |
|---|---|---|---|
| 0 | root | container | 1927 |
| 1000 / 1001 / 1002 | header / version / stamp | as in `.bh3` | 1915 / 1915 / 1658 |
| 8 | track node | container | 90,542 |
| 7 | keys | array, stride 36 | 90,542 |
| 130 | key flags | one byte per key | 42,915 |

A key is 36 bytes: `float` time, quaternion (4 `float`), position (3 `float`),
`float` scale. **Time is a delta from the previous key, not an absolute
value**; the typical value is 0.0333, one frame at 30 frames per second. The
duration comes from the same place: `clockworkman_walk.bha` has 37 tracks, the
first one holds 41 keys, and the deltas add up to 1.3333 seconds — exactly 40
frames.

The length of chunk 130 matched the key count of the neighbouring chunk 7 in
all 42,915 cases, so it is one flag per key; what its values mean (5, 3, 4, 1,
10, 12) has not been established. It exists only in version 11, and not in all
of those files: 1014 out of 1658.

Versions: **10 in 257 files, 11 in 1658, no header at all in 12**. Those twelve
are the Rise of Nations layout.

**`id = 8` means different things in the two formats:** in `.bh3` it is a leaf
holding an array of vectors, in `.bha` a container — a track node. The chunk
table has to be kept separate per format; a shared one will parse one of them
wrongly, and silently.

## Prefabs: `.pfb`

Buildings do not reference a mesh directly — they reference a `.pfb` prefab.
That is the same chunk container: all 1321 shipped files parse with the same
code without a single error, and inside are the same familiar ids in the same
roles — mesh, positions, UVs, indices, bones, bounds, collision.

The difference is that a `.pfb` is **an assembly, not one model**: a file
carries several roots, up to 119 of them, each with its own mesh, skeleton and
bounds — that is, several placed instances in a single file.

It also has chunks that appear in neither `.bh3` nor `.bha`: `112`, `200`…`203`
and `300`…`306`. Their layout has not been worked out — neither the field count
nor the meaning. All that is known is that `300`…`306` occur in practically
every shipped file, while `200`…`203` occur exactly once per file in 448 files
out of 1321.

## Skinning: two vertex spaces

This is the central trap of the format itself, and there is no way around it.

Vertices are bound to bones **by ranges**: a bone owns a contiguous slice of
the array starting at `VertexStart` and `VertexCount` long. The lengths add up
to the record count exactly.

The mesh arrays, though, live in **two different spaces**:

- **A skinning record** is "this point, pulled by this bone". Chunks 2
  (positions), 3 (normals) and 8 are indexed by it, and the bone ranges carve
  up precisely this array.
- **A draw vertex** is a point on the surface. Chunk 4 (UVs) and the **triangle
  indices** are indexed by that.

As long as one bone pulls a vertex, the two are the same thing. Where two or
more bones do, there are as many records per vertex as there are bones, and the
arrays diverge: `clockworkspider_lod_1` has 1352 records against 1348 draw
vertices.

Chunks 50 and 51 tie them together, one value per skinning record: **51 is the
draw vertex number**, 50 is the weight. The final point is assembled linearly:

```
position of vertex r = Σ weight(i) · (position(i) × world matrix of record i's bone)
```

over every record `i` where `chunk51[i] == r`. The weights per vertex sum to
one, no draw vertex is left without a record, and the largest triangle index is
always below the UV count. 464 models out of 783 carry these chunks; in the
rest the binding is rigid and the two spaces coincide.

**Reading 51 as "the index of a partner vertex" is a mistake**, and a hard one
to notice: a read-then-write-back round trip still matches, because the error
is symmetric, and the mesh only falls apart on screen in a third-party viewer.

The same split produces a trap that looks like data corruption but is not:
**in 280 meshes out of 785 the UV array is shorter than the position array.**

```
boar.bh3   skinning records=728  draw vertices=522  indices=1362  largest index=521
```

It is tempting to read that as "the trailing vertices just have no UVs", and
that is wrong: the extra records are scattered throughout the array, and the
indices address a different array altogether. A naive "one UV per vertex"
parser silently ruins the mesh here.

## The main pipeline trap: geometry cannot move without the skeleton

Skinning is rigid, and vertices are stored in the **local space of the bone
that owns them** — and in the game's own models they sit right up against that
bone. In `clockworkman_lod_3` a vertex is on average 54.6 units from the origin
of its bone, with a median of 33.9.

Hence a rule written down nowhere in the format, and the easiest one to break:

> **A rigged model cannot be edited without moving the skeleton.**

Here is what happens when you break it. Stretch only the mesh vertices in an
editor and leave the bones where they were: in the rest pose the model looks
perfect, while the average distance from a vertex to its bone grows several
times over. In game that turns into parts flying off — the engine rotates the
bone, and the part swings on an arc with a lever arm of several hundred units
instead of a few dozen. The correct move is to scale the armature and let the
mesh follow it: the part itself stretches but does not drift away from its
joint, and the lever arm grows only moderately.

You can check this without launching the game and without even opening an
editor: **if the average distance from a vertex to the origin of its bone has
grown several times over, the edit is wrong.** It is a cheap check, and it
catches the mistake before it reaches the archive.

A related limit: **the set of bones cannot be changed.** Chunk 111 describes
the joints of specific bones and 113/114 the collision built for them; an added
or removed bone would make that data a lie with nothing left to verify it
against. Leaving a bone with no geometry, on the other hand, is fine — the game
does that itself.

## Coordinate system

- `.bh3` is left-handed: X left, Y back, Z up.
- glTF is right-handed: X left, Y up, Z forward.
- The conversion is `X = X, Y = Z, Z = −Y`, a −90° rotation about X, and only
  for the root bone — the rest are already expressed relative to their parent.
- The bone quaternion is **inverted** on reading.
- **Triangle winding is not touched.** The temptation to flip it "to match" the
  axis change is strong, but the conversion is a rotation, and a rotation does
  not change orientation. Flipping gives you inside-out triangles, and that
  surfaces late: double-sided viewer materials hide it, and in game the two
  flips cancel each other out.

## Differences from Rise of Nations

This is exactly what breaks tools written for Rise of Nations:

| What | Rise of Nations | Rise of Legends |
|---|---|---|
| Header | none | chunk `1000` → `1001` version, `1002` stamp |
| Positions (2) | 16 bytes: `vec3` plus a junk `1.0f` | **12 bytes**, a clean `vec3` |
| Normals (3) | `vec3` plus a `count × uint32` tail | **`vec3` only**, no tail |
| Bone (7) | quaternion, position, `float` | the same **plus an `int32` of flags** |
| Mesh children (1) | strictly `2, 3, 4, 5` | 4…8 children, order varies |
| New | — | 8, 13…18, 50, 51, 70, 90, 110, 111, 113, 114, 130 |

## LOD: the numbering runs backwards, and the file drawn is not the obvious one

**`lod_0` is the coarsest model, not the most detailed.** For `clockworkman`:

| file | vertices | triangles | bones |
|---|---|---|---|
| `clockworkman_lod_0.bh3` | 828 | 457 | 32 |
| `clockworkman_lod_1.bh3` | 1327 | 823 | 35 |
| `clockworkman_lod_2.bh3` | 2731 | 1548 | 37 |
| `clockworkman_lod_3.bh3` | 3594 | 2103 | 37 |

Expect the opposite and you will render a stub and conclude that your tool is
lying.

It gets worse: **the level is picked by the graphics quality setting, not by
distance to the camera.** On "lowest" the level 0 file is drawn; on "low",
"high" and "highest" it is level 1. Zooming did not change the level; level 3
showed up exactly once — on "highest" with the camera right up close.

And finally, the level number in the table need not match the number in the
file name. For `CLOCKWORKMAN` the `<LOD level="2">` block in
`unit_graphics.xml` points at `clockworkman_LOD_1.BH3`, while
`clockworkman_LOD_2.BH3` is **never** mentioned in that table at all — the file
ships and is perfectly valid, but the game does not know about it.

**What this means for model work:** improve the file that actually gets loaded,
not the one with the larger number. In ordinary play that is `LOD_1`.

## Graphics tables are not edited where the text is

A trap worth an entire evening. The graphics tables — `unit_graphics.xml`,
`unit_materials.xml`, `unit_skeletons.xml`, `materials.xml`, `lod.xml`,
`team_color.xml` and their kin — sit in the archives **twice**: as text and as
compiled binary XML. The engine reads the binary form out of `data.big` and
**ignores** the text copy, so editing the text changes nothing in game.

The binary form is the same data with strings in UTF-16 and a table of unique
values: searching for `BH3` in a single-byte encoding finds nothing, in UTF-16
it finds hundreds. Editing in place works **as long as the string length does
not change**; a replacement of a different length would require rebuilding the
whole table.

**The rule files are not affected.** `unitrules.xml`, `buildingrules.xml`,
`techrules.xml` and `craftrules.xml` have no binary twin at all and exist only
as text — this trap is purely a graphics one.

## Textures

A `.bh3` has no text field in it except the exporter stamp, which means the
texture name is not in the model. All it has is UVs. The link is made from
outside, in three steps:

| step | file | what is there |
|---|---|---|
| 1 | `data\unit_graphics.xml` | `<UNIT type="...">`, with one block per level: `<MODEL file="...bh3"/>` and `<MATERIAL name="....fx"/>` |
| 2 | `data\unit_materials.xml` | `<MATERIAL material_name="....fx">` with `<TEXTURE role="diffusemap / normalmap / teamcolormap"/>` |
| 3 | `art\units\...` | the file itself: named `.tga`, DDS inside |

### Four traps in that chain

**Names do not work.** The obvious rule "the texture is named after the model"
fails for roughly half the pairs. The diffuse map of `clockworkman_lod_3.bh3`
is `clockwork_diffuse_hi.tga`, while the `clockworkman.tga` sitting right next
to it belongs to an entirely different material.

**One mesh is worn by several units, and they are dressed differently.**
`clockworkman_LOD_0.BH3` is `CLOCKWORKMAN`, `CLOCKWORKMAN_UPGRADE` and
`SCAVENGERGRUNT` all at once, and the last of those is dressed by a completely
different material, `Scavengergrunt.fx`. "The texture of this model" is a badly
posed question: a texture belongs to a **unit**, not to a mesh.

**The material is chosen per detail level.** For `CLOCKWORKMAN`, levels 0–2 use
`Clockworkman.fx` (`clockwork_diffuse.tga`) while level 3 uses
`Clockworkman_hi.fx` (`clockwork_diffuse_Hi.tga`), and those are different sets
of textures.

**The `type=` attribute on `<MATERIAL>` picks a table, it does not merely label
a kind.** The values are `units`, `terrain`, `buildings` and `rpg`. That
matters, because 21 material names are defined in two tables at once —
`unit_materials.xml` and `materials.xml` — with different textures, and
`clockworkman.fx` is one of them.

### Texture format

**The `.tga` extension is a lie: there is DDS inside.** Blender opens such
files as they are, because it looks at the content rather than the extension.
Other editors (GIMP, Paint.NET) need the file renamed to `.dds`.

Across all 3503 textures under `art\`: **DXT5 — 2830, uncompressed 32-bit —
346, DXT1 — 325, uncompressed 24-bit — 2**. For unit textures alone (933
files): 886 DXT5, 46 DXT1 and one uncompressed; sizes are 256×256 (438 files),
512×512 (323), 128×128 (135), and smaller only rarely. Exactly seven files out
of 3503 carry a mip chain — the rest have a single level.

Three roles and three suffixes:

| role | suffix | what it is |
|---|---|---|
| `diffusemap` | `_diffuse` | base colour |
| `normalmap` | `_dot3` | normal map, the recognisable "flat blue" |
| `teamcolormap` | `_team` | player colour mask |

**The player colour mask is a mask, not a paint job.** The red and the blue
unit wear the same texture: the engine paints by the mask itself. The mask
lives in the alpha channel and is binary — a small share of the surface is
painted. That share has to be counted against a threshold rather than against
"alpha is non-zero": in DXT5 the alpha is slightly above zero almost
everywhere, and a naive count reports 99 % instead of the real figure.

The team colours themselves are in `data\team_color.xml`, elements
`TEAM0`…`TEAM9`, attribute `unit_color`. There are ten teams: 0 red, 1 blue,
2 green, 3 yellow, 4 cyan, 5 orange, 6 purple, 7 pink, 8 white and 9 black —
the last two being the neutrals. The file holds several colour schemes,
including colour-blind variants.

Blender cannot write DDS, so the way back for a texture runs through an
external converter. A useful rule when writing: **replace only the surfaces and
take the header from the original file wholesale.** There is no reason to
change a compression format the game already accepted, and the cube maps (there
are ten in the shipped data — `clouds.dds`, `cubemap.dds`, `environment.dds`
and others) declare six faces in their header, so writing a single surface over
such a template produces a file six times smaller than the header claims.

One more DXT1 subtlety: unlike DXT5 it has no alpha block. Instead the fourth
palette colour becomes transparent when the reference point `color0` is not
greater than `color1` as a number. The game genuinely relies on this: the
foliage cut-out on trees rests on exactly that mode. An encoder that always
writes an opaque four-colour block turns such a texture solid.

## An animation lands on a skeleton by position, not by name

Tracks in a `.bha` **carry no names at all**. They are matched to bones by
position in the tree: child number `i` to child number `i`.

Hence the fitness rule for a "model + animation" pair: the pair works when **no
node of the rig has more children than the matching node of the skeleton**.
Extra bones with no track are harmless; extra tracks shift a whole branch onto
the wrong bones. The mesh stays intact through all of it, so to the eye it does
not look broken, only oddly posed — the figure tipped on its side, an arm
growing from the wrong place.

This is worth checking properly, because the animation rig and the model
skeleton are **different trees** and do not agree on node counts.
`clockworkman_walk.bha` has 37 tracks: levels 2 and 3 (37 bones) walk
correctly, level 1 (35 bones) already shifts at two nodes, and level 0 (32
bones) at five, which tips the figure over.

The extra skeleton branches that get no track are usually placeholders with a
`$$$` suffix, and attachment points that need no animation anyway. But the
format guarantees nothing here, so on an unfamiliar model it is worth checking
with your eyes.

## Smaller traps in the data

**Normals can be zero.** In 37 models out of 783 there are vertices whose
normal has effectively zero length — 1368 of them in total, and in
`kahanwalker_lod_0` 390 out of 850. That is the game's data, not a parsing
error; glTF exporters die on such a vertex, so the normal has to be rebuilt
from the adjacent triangles — and necessarily **in model space**, because
neighbouring vertices of a triangle may belong to different bones, and adding
vectors from different local spaces is meaningless.

**There are non-numeric coordinates in the data.** Five shipped models —
`kahanwalker_lod_0`, `kahan_peasant_lod_0`, `barbarian_sword`,
`barbarian_sword2` and `glassgolem_shard_left` — contain `NaN`. In the first
two a triangle **references** the broken vertex, so this is not junk sitting in
an unused tail of the array.

**Some models have every triangle degenerate.** These are small effects and
placeholders (`afreet_upgrade`, `ammo_blank`, `glass_shard`,
`fire_circle_mesh` and the like). There is no geometry in them at all; judging
by the names, whatever is visible is drawn by the effects system, not by a
mesh.

**Degeneracy cannot be decided by exact equality.** A triangle is degenerate
when two of its corners coincide, and the obvious thing to write is `a == b`.
Do not: in the original file the coordinates match bit for bit, but after a
trip through matrices they diverge in the low bits. The threshold has to be
geometric — a fraction of the model's size.

**Vertex colours live in two different spaces.** Of the 64 files carrying chunk
110, in some the length equals the skinning record count and in others the draw
vertex count. Chunks 3 and 8 know no such split: they always go by skinning
records.

## What to open all this with

The chunk format is simple, and your own parser is an evening's work. The
ready-made route into Blender is the `rol-model-converter` tool
([xxMelkorxx/rol-model-converter](https://github.com/xxMelkorxx/rol-model-converter)):
it reads `.bh3` and `.bha`, writes `.glb` that Blender opens with the stock
glTF 2.0 import, walks the chain to the textures itself, and can go back to
`.bh3`.

Three things to know when viewing in Blender, whatever opened the file:

1. **Set 30 frames per second before importing.** glTF stores time in seconds,
   Blender in frames, and the conversion uses whatever rate the scene has at
   the moment of import. At 30 the keys land on whole frames; at the default 24
   they land on fractions. Changing the rate **after** the import does not
   help: the frame numbers stay as they were and simply come to mean a
   different time.
2. **Set the end of the scene range by hand.** The glTF importer leaves it at
   the default 1…250, so past the end of the animation the player silently
   winds through nothing.
3. **Far clipping.** Rise of Legends models are hundreds of units tall and
   vanish as you pull back: `N` → the **View** tab → raise **Clip End**.

And a fourth point, a quirk rather than a trap: the model arrives rotated 90° —
it faces along X. In Blender that is the right view (Numpad 3), not the front
one (Numpad 1).

## What cannot be done

An honest list, with the reason rather than a "not yet".

- **Drop a `.bh3` or a `.tga` next to the game as a loose file.** The engine
  reads archives only; the one road is rebuilding the `.big` the file came from
  ([game-data.en.md](game-data.en.md)).
- **Edit a graphics table as text.** The text copy is ignored, the binary one
  in `data.big` is what gets read, and a string in it can only be replaced by
  one of the same length.
- **Change the set of bones.** Joint descriptions and collision are tied to
  specific bones; an extra or missing bone makes that data a lie.
- **Carry everything a model holds through glTF.** The chunk 8 vector, the
  second binding, vertex colours, PhysX collision and joint descriptions map to
  nothing in glTF: either you copy the chunk payloads across, or you lose them.
- **Recompute chunk 8 after a topology change.** Nobody knows what that vector
  is (see below), so there is nothing to compute it from.
- **Save DDS from Blender.** It cannot write the format; the way back for a
  texture is an external converter only.
- **Get byte-exact equality after a round trip through glTF.** The numbers pass
  through matrices and come back differing in the low bits — compare geometry,
  not bytes. Byte-exact equality is only achievable by editing the chunk tree
  in place, without parsing the geometry at all.
- **Find out which material dresses a building.** For units the material is
  named in the table explicitly; for buildings it is not — there are zero
  nested material tags in the building blocks, and the material name does not
  occur as a string inside the `.pfb` either. What defines that link has not
  been established.

## What remains unknown

- **Chunk 8** — one vector per skinning record, unit length in every case,
  depending on both the geometry and the UV layout. It turned out to be neither
  a tangent, nor a binormal, nor a normal in model space, nor the direction
  from bone to vertex — all four hypotheses were rejected by measurement. The
  game accepts and renders a model without it exactly as before, but that is
  the only thing known about it for certain.
- Chunk 90, and the extra eight bytes on some bones.
- The meaning of the flag values: bones (0…3), models, animation keys.
- Why there are three bounds slots (14/15/16) and how they differ.
- The contents of chunk 113 — the eight bytes before the PhysX stream.
- The prefab chunks that appear in neither meshes nor animations: 112,
  200…203, 300…306.
- Exactly how the graphics quality setting maps to a detail level: the
  thresholds in `lod.xml` do not describe the observed behaviour.
