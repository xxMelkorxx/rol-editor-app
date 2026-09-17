# Changelog

Russian: [CHANGELOG.md](CHANGELOG.md)

Format: one section per version, newest first. The version here is the same
one shown in `RolRulesEditor.App.exe`'s properties and in the package's file
name.

## 0.0.1 — 2026-09-18

First release.

### Added

- An editor for the stats of units, heroes and buildings: edits pile up in a
  profile and go into the game's archives with one button.
- Revert to the original archives at any moment.
- Export and import your set of edits as a file.
- A bilingual interface: the shell language and the game's naming language
  are switched independently.
- A bilingual knowledge base on the game's data: six write-ups and an
  appendix of seven reference tables.

### Known limitations

- Technologies and crafts cannot be opened in the interface at all. Research
  covers the leader's development branches only: 14 branches, 56 records. The
  other 85 technologies and all 509 crafts are there in the game's rule
  files, but have no section of their own yet.
- Weapon properties are shown but cannot be edited one by one.
- Numeric fields have text boxes, no sliders.
- Some labels only pick up a theme change after a restart.
