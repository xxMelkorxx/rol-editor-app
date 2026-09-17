# Global rules: `rules.xml`

Russian: [rules-constants.md](rules-constants.md)

836 parameters in 61 sections — numbers the engine looks up by name: economy, combat, healing, borders, victory, hero and faction bonuses. Each entry has been checked against whether its name occurs among the strings of the game's executable: parameter names are compiled into the code, and a missing name means nothing ever reads that entry.

## How an entry is built

```xml
<ECONOMY>
  <GATHER_AND_COMMERCE name="starting_resources">
    <COMMENT>Starting resources at beginning of game</COMMENT>
    <VALUE0>100 timonium</VALUE0>
    <VALUE1>50 wealth</VALUE1>
    ...
    <VALUE11/>
</ECONOMY>
```

The section tag and the subsection tag together form the path the code asks
for, as in `economy/gather_and_commerce`; `name` is the parameter's name
within that path.

**`VALUE0`..`VALUE11` are not a fixed axis but an array of up to twelve
slots.** What a slot means is decided by the code and hinted at by
`COMMENT`; at least five axes occur:

| Axis | Example | What the slots mean |
|---|---|---|
| resource | `timonium_buy` = `135 wealth`, `0`, `0`, `0 energy`, `0` | timonium, wealth, research points, energy, fifth slot unlabeled |
| upgrade level | `unit_heal_rate` = 1, 2, 4, 7, 10, 15, 20, 30 | eight upgrade tiers |
| hero level | `giacomo_research_bonus` = 1..5 | five levels |
| city size | `base_trade_value` = 5, 10, 15, 20, 25 | Site, Small City, City, Large City, Great City |
| counter | `diamond_los_bonus` = 0..6, then 6 | number of controlled diamonds |

Slot occupancy:

| Slots filled | Entries |
|---:|---:|
| 0 | 1 |
| 1 | 679 |
| 2 | 2 |
| 3 | 32 |
| 4 | 45 |
| 5 | 56 |
| 6 | 1 |
| 8 | 10 |
| 11 | 1 |
| 12 | 9 |

That means 679 of 836 parameters are plain scalars; the rest are arrays.

## Entries the code never asks for

11 entries have a name that never occurs in `legends.exe`. The value is
there in the file, but nothing ever reads it — editing such a line does
nothing.

| Section | `name` in the file | What is wrong |
|---|---|---|
| `rules/territory` | `territory_push_factor` | the name is not in the code at all |
| `rules/world` | `whatever` | the name is not in the code at all |
| `tribes/vinci` | `vinci_politics` | the name is not in the code at all |
| `tribes/cuotl` | `battery_sanctuary_extra_pushfactort` | typo; the code asks for `battery_sanctuary_extra_push_factor` |
| `spells/interdict` | `interdict_attrition_multiplier` | the name is not in the code at all |
| `spells/heartbeat_cooldown` | `hero_heartbeat_cooldown_magic ` | trailing space; the code asks for `hero_heartbeat_cooldown_magic` |
| `spells/heartbeat_cooldown` | `hero_heartbeat_cooldown_tech ` | trailing space; the code asks for `hero_heartbeat_cooldown_tech` |
| `spells/heartbeat_cooldown` | `unit_heartbeat_cooldown_magic ` | trailing space; the code asks for `unit_heartbeat_cooldown_magic` |
| `spells/heartbeat_cooldown` | `unit_heartbeat_cooldown_tech ` | trailing space; the code asks for `unit_heartbeat_cooldown_tech` |
| `spells/heartbeat_cooldown` | `dominance_heartbeat_cooldown ` | trailing space; the code asks for `dominance_heartbeat_cooldown` |
| `music/music` | `music_ambient_sensitivity_duration` | the code asks only for `music_peace_sensitivity_duration` |

The five trailing-space cases are candidates, not a verdict: if the parser
trims whitespace from `name`, they work. This is checked by reading the code
near `stringlookup.cpp`; not verified yet.

## Sections

| Path | Entries |
|---|---:|
| `ctw/generals` | 21 |
| `ctw/rules` | 74 |
| `economy/costs` | 14 |
| `economy/gather_and_commerce` | 63 |
| `economy/goodybox` | 4 |
| `economy/plunder` | 7 |
| `economy/research` | 11 |
| `economy/trade` | 17 |
| `heroes/alim` | 6 |
| `heroes/cuotl` | 3 |
| `heroes/heroes` | 3 |
| `heroes/vinci` | 4 |
| `match/match` | 2 |
| `music/music` | 11 |
| `physics/physics` | 4 |
| `pings/minimap` | 4 |
| `rules/aircraft` | 6 |
| `rules/attrition` | 12 |
| `rules/buildings` | 88 |
| `rules/cheats` | 7 |
| `rules/combat` | 57 |
| `rules/diplomacy` | 7 |
| `rules/districts` | 22 |
| `rules/dominance` | 20 |
| `rules/healing` | 20 |
| `rules/line_of_sight` | 15 |
| `rules/movement` | 28 |
| `rules/neutrals` | 51 |
| `rules/popcap` | 9 |
| `rules/spells` | 5 |
| `rules/strip` | 18 |
| `rules/territory` | 15 |
| `rules/units` | 57 |
| `rules/victory` | 19 |
| `rules/world` | 3 |
| `solodifficulty/moderate` | 1 |
| `solodifficulty/tough` | 5 |
| `solodifficulty/tougher` | 2 |
| `solodifficulty/toughest` | 2 |
| `spells/besiege` | 1 |
| `spells/burningcloud` | 1 |
| `spells/flamesuits` | 1 |
| `spells/harpoon` | 2 |
| `spells/heartbeat_cooldown` | 6 |
| `spells/initial_cooldown` | 8 |
| `spells/interdict` | 1 |
| `spells/slow` | 3 |
| `spells/slowdown` | 1 |
| `spells/stormcity` | 9 |
| `spells/suncloak` | 1 |
| `spells/sweepingcharge` | 1 |
| `spells/warflag` | 2 |
| `spells/whirlwind` | 1 |
| `tribes/alim` | 5 |
| `tribes/cuotl` | 13 |
| `tribes/vinci` | 23 |
| `ui/attackwarnings` | 7 |
| `ui/music` | 7 |
| `ui/ui` | 21 |
| `wonders/cryptofknowledge` | 3 |
| `wonders/greatfastness` | 2 |

The code knows two sections that are not in the file: `nations/korean` and
`nations/lakota` — leftovers from Rise of Nations, Big Huge Games' previous
game.

## All parameters

Empty slots are omitted. A `−` in the "code" column means the name is not in
`legends.exe`.


### `economy/gather_and_commerce`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `starting_resources` | 100 timonium, 50 wealth, 1 research points, 50 energy | Starting resources at beginning of game |
| + | `timonium_gather_size` | 4, 5, 6, 7, 8, 9, 10, 11, 0, 0, 0, 0 |  |
| + | `timonium_gather_slots` | 3, 4, 5, 6, 7, 8, 9, 10, 0, 0, 0, 0 |  |
| + | `mine_radius` | 6 | tiles |
| + | `gather_rate` | 900 | frames |
| + | `num_timonium_gather` | 8 |  |
| + | `mine_bonus_rate` | 0 | resources |
| + | `base_commerce_cap` | 100, 100, 100, 100, 100 | Base Commerce Cap for each good |
| + | `city_income` | 0, 0, 0, 0, 0 | Income per city |
| + | `baseline_income` | 10, 0, 0, 0, 0 | Baseline minimum income just for existing |
| + | `timonium_buy` | 135 wealth, 0, 0, 0 energy, 0 | Cost in wealth to buy 100 timonium |
| + | `timonium_sell` | 65 wealth, 0, 0, 0 energy, 0 | Wealth gained in return for selling 100 timonium |
| + | `timonium_buy_upgrade` | 125 wealth, 0, 0, 0 energy, 0 | Cost in wealth to buy 100 timonium |
| + | `timonium_sell_upgrade` | 75 wealth, 0, 0, 0 energy, 0 | Wealth gained in return for selling 100 timonium |
| + | `merchant_district_commerce_cap` | 50, 75, 100, 75 energy | amount, by level of city, that a guild merchant adds to your commerce cap (City, Large City, Great City) |
| + | `city_peasant_rate` | 10, 10, 10 | resources for gatherers in city radius (by city size) |
| + | `market_basement` | 10 | Lowest sell price the market likes to stay near, even when everyone is selling like gangbusters |
| + | `market_equilibrium` | 60 | The equilibrium sell price (equilibrium buy price is twice this) |
| + | `market_min_variance` | 2 |  |
| + | `market_min_trend` | 8 | cycles |
| + | `market_trend_range` | 16 | cycles |
| + | `market_cycle_rate` | 1 | frames |
| + | `market_supply_demand` | 3, 2, 1 | +/- to sell price by age (double to buy price) |
| + | `peasant_rate` | 10 | resources |
| + | `wealth_bonus` | 0, 5, 10, 20, 40 |  |
| + | `guild_peasant_rate` | 0 | resources for gatherers in city radius (by guild district) |
| + | `oil_rate` | 35 |  |
| + | `village_wealth_income` | (for Hamlet), (for Small City), (for Town) | amount of wealth income you get from villages (per Small City upgrade) |
| + | `woodcutter_radius` | 6 | tiles |
| + | `gem_income_costs` | 1 | 1: all gem costs relate to gem income, 0: gem costs relate to amount collected |
| + | `gem_income_debt` | 0, 0, 0, 0, 0 | 1: allow player to intentionally drive gem income negative, 0: spending capped at +0 gem income |
| + | `gem_income_debt_amount` | 2 Timonium, 2 wealth, 0 Research Points, 2 energy, 0 | If gem income is -1, you lose this much income from Timonium and wealth |
| + | `fishermen_bonus` | 0, 50, 100, 200, 200 |  |
| + | `granary_bonus` | 0, 0, 0, 0, 0 |  |
| + | `lumbermill_bonus` | 0, 0, 0, 0, 0 |  |
| + | `smelter_bonus` | 50, 100, 150, 200, 250 |  |
| + | `merchants_bonus` | 100, 120, 150, 200, 300 |  |
| + | `territory_taxes` | 0, 50, 100, 200, 300 |  |
| + | `lumber_commerce` | 10 | commerce cap |
| + | `farms_per_city_base` | 5 |  |
| + | `farms_per_city_level` | 0 |  |
| + | `refinery_bonus` | 33 | % per refinery |
| + | `num_mountain_gather` | 5 |  |
| + | `gems_rate` | 50 |  |
| + | `gem_grind_rate` | 75 frames (5 seconds) | In X frames, one +1 of gem income will be ground off of a gem site by units that do that |
| + | `gem_gather_slots` | 0 |  |
| + | `scholar_rate` | 20, 30, 40, 50, 60, 70 |  |
| + | `mountain_gather_size` | 100, 210, 275, 400, 1000, 0, 0, 0, 0, 0, 0, 0 |  |
| + | `mountain_gather_slots` | 5, 5, 5, 10, 10, 0, 0, 0, 0, 0, 0, 0 |  |
| + | `food_bonus_for_farm` | 20 |  |
| + | `oil_bonus_for_well` | 50 |  |
| + | `knowledge_bonus_for_university` | 25 |  |
| + | `timber_bonus_per_wood_slot` | 5 |  |
| + | `metal_bonus_per_mine_slot` | 5 |  |
| + | `river_resource_value` | 2 |  |
| + | `village_taxes` | 0 | bonus wealth |
| + | `building_taxes` | 0 | bonus wealth (per building in Small City/city) |
| + | `temple_taxes` | 0 | bonus wealth (+ amount above for just being a building) |
| + | `village_literacy` | 0 | bonus knowledge |
| + | `university_literacy` | 0 | bonus knowledge |
| + | `library_literacy` | 0 | bonus knowledge |
| + | `stripped_income` | 0 timonium, 0, 0, 0 energy, 0 | The amount of income you get per good for each percent of your territory that is stripped |
| + | `allow_gathersite_completion_bonus` | 0 | Wether or not a bonus is given for a gathersite being built |

### `economy/research`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `age_cities` | 2, 4, 6 | Total # of cities needed to age up |
| + | `tech_science_discount` | 10 | percent |
| + | `tech_science_speedup` | 10 | percent |
| + | `tech_age_behind_discount` | 0 | percent |
| + | `tech_age_behind_knowledge_discount` | 0 | percent |
| + | `tech_color_behind_discount` | 0 | percent |
| + | `tech_color_behind_knowledge_discount` | 0 | percent |
| + | `military_upgrade_discount` | 0 | % per level ahead |
| + | `military_unit_discount` | 0 | % per level ahead |
| + | `global_prosperity` | 25 | % |
| + | `research_points_score_value` | 200 | Value of research point cost of tech in scoring |

### `economy/costs`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `unit_cost_factor` | 1 | resources |
| + | `build_cost_factor` | 1 | resources |
| + | `tech_cost_factor` | 1 | resources |
| + | `spell_cost_factor` | 1 | resources |
| + | `unit_refit_max_cost` | 40 | delta per resource type per unit |
| + | `unit_scholar_ramp_max` | 2000 | % additional ramping cost max |
| + | `unit_worker_ramp_max` | 1000 | % additional ramping cost max |
| + | `unit_other_civilian_ramp_max` | 1000 | % additional ramping cost max |
| + | `unit_military_ramp_max` | 1000 | % additional ramping cost max |
| + | `ramp_final` | 50 | % additional knowledge cost per final tech already researched |
| + | `research_premium` | 1 | times base cost (Please don't use except on your own machine for testing; for main line game adjust times individually -- BR) |
| + | `research_tick_premium` | 1 | times base time (Please don't use except on your own machine for testing; for main line game adjust times individually -- BR) |
| + | `district_ramp_cost` | 0 | Additional ramp for each district you build. |
| + | `initial_prototype_visits` | 1 | number of starting visists |

### `economy/goodybox`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `goody_box` | 20, 0, 0, 0 energy | Base resources (Timonium, Wealth, Research) |
| + | `goody_box_magus` | 5, 0, 0, 0 energy | Resources per Guild District (Timonium, Wealth, Research) |
| + | `rare_bonus` | 0, 0, 0, 0 | Base resources (Timonium, Wealth, Research) |
| + | `rare_bonus_magus` | 0, 0, 0, 0 | Resources per Guild District (Timonium, Wealth, Research) |

### `economy/plunder`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `city_plunder_per_level` | 50 |  |
| + | `village_plunder` | 25 |  |
| + | `capital_plunder` | 100 |  |
| + | `capital_plunder_assassin` | 500 | (times number of players eliminated so far) |
| + | `plunder` | 100 | % of raze value |
| + | `worker_plunder` | 15 M, 0, 0, 0 | plunder for each type of resource (M, W, G) |
| + | `caravan_plunder` | 0, 50 W, 0, 50 e | plunder for each type of resource (M, W, G) |

### `economy/trade`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `caravans_reestablish_routes` | 1 | new caravans taking up already-established routes get income immediately |
| + | `caravans_need_not_establish_routes` | 1 | new caravans taking up ANY routes get income immediately |
| + | `caravan_arrives_bonus` | 20 | New caravan gets bonus |
| + | `caravan_arrives_merchant_bonus` | 0 | New caravan gets bonus based on merchant districts |
| + | `trade_multiplier` | 1/1 | base multiplier for all trade routes |
| + | `base_trade_value` | 5, 10, 15, 20, 25 | base trade value of a site/city (Site, Small City, City, Large City, Great City) |
| + | `district_trade_value` | 1, 1, 1 | base trade value of each district in a city (includes merchant and guild districts) |
| + | `merchant_district_global_trade_value` | 2 | additional trade value of each merchant district in the world |
| + | `merchant_district_trade_value` | 0 | additional trade value of each merchant district in a city |
| + | `guild_district_trade_value` | 0 | additional trade value of each guild district in a city |
| + | `foreign_trade_bonus` | 2/2 | multiplier for trade between different nations (applies to neutral villages) (note you only get half of foreign trade) |
| + | `energy_base` | 0, 0, 0, 0, 0 | base energy value of a site/city (Site, Small City, City, Large City, Great City) |
| + | `energy_merchant_district_local` | 0, 0, 25, 30, 35 | energy value of a location per local Merchant District (i.e. in same city, so must BE a city) |
| + | `energy_other_district_local` | 0, 0, 0, 0, 0 | energy value of a location per local non-merchant District (i.e. in same city, so must BE a city) |
| + | `energy_merchant_district_global` | 1, 1, 0, 0, 0 | energy value of a location per global Merchant District (i.e. anywhere on map) |
| + | `energy_other_district_global` | 0, 0, 0, 0, 0 | energy value of a location per global non-merchant District (i.e. anywhere on map) |
| + | `energy_merchant_district_global_allied` | 0, 0, 0, 0, 0 | energy value of an allied location per global Merchant District (i.e. anywhere on map) |

### `rules/cheats`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `unit_time_base` | 1/1 | fraction of base production time (Must be 1/1 except on your own machine for testing; for main line game adjust times individually - BR) |
| + | `unit_time_ramp` | 1/1 | fractional time ramp (Must be 1/1 except on your own machine for testing; for main line game adjust times individually - BR) |
| + | `accel_train` | 1 | fraction of base speed (Must be 1 except on your own machine for testing; for main line game adjust times individually - BR) |
| + | `accel_construct` | 1 | fractional speed (Please don't use except on your own machine for testing; for main line game adjust times individually -- BR) |
| + | `accel_research` | 1 | speed (use only on own machine for testing -- BR) |
| + | `mana_recovery` | 1/1 | fraction of normal mana recovery (Must be 1/1 except on your own machine for testing or demo; for main line game adjust times individually - BR) |
| + | `storm_time_modifier` | 1/1 | fraction of normal storm time (Please don't use except on own machine for testing, or for demos -- BR) |

### `rules/popcap`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `base_pop_cap` | 20 | Base pop cap |
| + | `ctw_base_pop_cap` | 25 | Base pop cap for CtW games |
| + | `city_pop_cap` | 0, 0, 0 | Pop cap by city size |
| + | `military_pop_cap_vinci` | 15 (for City), 25 (for large city), 35 (for great city) | Military District pop cap |
| + | `military_pop_cap_alim` | 15 (for City), 25 (for large city), 35 (for great city) | Military District pop cap |
| + | `military_pop_cap_cuotl` | 15 (for City), 25 (for large city), 35 (for great city) | Military District pop cap |
| + | `village_pop_cap` | 0 | Small City pop cap |
| + | `max_pop_cap` | 300 | Maximum possible Pop cap |
| + | `base_city_limit` | 2 | pop cap |

### `rules/territory`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `capital_territory_bonus` | 6 | tiles |
| + | `capital_territory_limit_bonus` | 8 | tiles |
| + | `territory_base` | 24 | tiles |
| + | `territory_den` | 5 | tiles |
| + | `territory_num` | 11 | (Numerator + BuildingFactor + BorderBonuses)/Denominator |
| − | `territory_push_factor` | - | **See BORDER_1 in TECHRULES.XML Bonus section for tech-based push** |
| + | `palace_district_push_limit` | 8 | Cities get this much extra push distance based on number of palace districts |
| + | `palace_district_push_factor` | 2 | Cities get this much extra push strength based on number of palace districts |
| + | `palace_district_global_push` | 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 | Total # of palace districts causes this much "global push" |
| + | `holy_district_push_factor_by_city_size` | 1, 1, 1 | Each holy district global push strength based on the size of the city it is in. |
| + | `holy_district_push_limit_by_city_size` | 1, 1, 1 | Each holy district global push distance based on the size of the city it is in. |
| + | `cuotl_noncity_holy_bonus` | 1 | Each holy district provides local push to noncities of this much |
| + | `upgrade_district_push` | 1 | If true, pushing districts (i.e. Holy Districts) have their push multiplied by city size |
| + | `lost_pusher_seconds` | 5 | After you lose a border pushing building, you can't build one for this many seconds |
| + | `lost_pusher_count_allies` | 1 | If true, the above calculation includes when allies lose buildings |

### `rules/victory`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `timer_refresh_ratio` | 5 | seconds with timer off removes 1 second from clock |
| + | `retake_capital` | 225 | frames |
| + | `wonder_timer` | 4500 | frames |
| + | `wonder_age` | 0 | frames |
| + | `popwin_timer` | 3600 | frames |
| + | `armageddon` | 4 | nukes |
| + | `armageddon_per_nation` | 1 | nukes |
| + | `armageddon_per_team` | 2 | nukes |
| + | `nuke_embargo_base` | 900 | frames |
| + | `nuke_embargo_nation` | 900 | frames |
| + | `nuke_embargo_world` | 0 | frames |
| + | `reassimilation` | 300 | % normal rate (of my own cities I've recaptured) |
| + | `assimilation_timer` | 1000 | frames |
| + | `ai_resignation_offer_wait` | 300 | seconds |
| + | `defeated_blow_up_towers` | 0 | Blow up defeated leader's defensive buildings? |
| + | `defeated_blow_up_units` | 0 | Blow up defeated leader's combat units? |
| + | `defeated_give_units_to_ally` | 1 | Give defeated leader's combat units to ally? |
| + | `defeated_give_cities_to_ally` | 1 | Give defeated leader's cities to ally? |
| + | `defeated_give_stuff_to_conqueror` | 0 | Give defeated leader's cities, etc, to conqueror? |

### `rules/attrition`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `siege_attrition` | 50 | % reduction |
| + | `militia_attrition` | 300 | % increase |
| + | `attrition_aged_up` | 25 | % increase |
| + | `peace_attrition` | 8 | frames: this is the special border-violation attrition when you're at peace |
| + | `building_safe_from_attrition_time` | 500 | frames |
| + | `assassin_attrition` | 8 | frames: this is the special assassin attrition |
| + | `attrition` | 60 | frames: this is the baseline level for "regular" attrition |
| + | `attrition_upgrade` | 0, 0, 0, 0, 0 | (this is for anti-attrition techs, not currently supported) |
| + | `attrition_improved` | 0, 1, 2, 4, 8, 10, 10, 10 | Attrition damage caused based on upgrade level |
| + | `interdict_attrition` | 4, 10, 15 | Interdict attrition bonus by age (age 1, 2, 3) |
| + | `building_attrition_use_percent` | 0 | (1 = building attrition rates are percentages hp/sec, 0 = building attrition rates are frames / 1 hp of damage) |
| + | `building_attrition` | 5 (3 per sec) | amount of attrition buildings take based on what "building_attrition_use_percent" is set to. |

### `rules/healing`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `supply_heal_rate` | 0 | frames (0 means don't heal at all) |
| + | `civilian_heal_rate` | 45 | frames (0 means don't heal at all) |
| + | `heal_delay_under_fire` | 75 frames | frames |
| + | `heal_delay_move` | 75 frames | frames |
| + | `heal_delay_attack` | 75 frames | frames |
| + | `unit_heal_rate_use_percent` | 0, 0, 0, 0, 0, 0, 0, 0 | (1 = heal rates are percentages/sec, 0 = heal rates are hit points / sec) |
| + | `unit_heal_rate` | 1, 2, 4, 7, 10, 15, 20, 30 | amount healed per second (see above) |
| + | `building_heal_rate_use_percent` | 1, 1, 1, 1, 1, 1, 1, 1 | (1 = heal rates are percentages/sec, 0 = heal rates are hit points / sec) |
| + | `building_heal_rate` | .5%, 1%, 2%, 4%, 8%, 16%, 32%, 64% | amount healed per second (see above) |
| + | `cuotl_city_heal_rate_use_percent` | 0, 0, 0, 0, 0, 0, 0, 0 | (1 = heal rates are percentages/sec, 0 = heal rates are hit points / sec) |
| + | `cuotl_city_heal_rate` | 1, 2, 3, 4, 5, 6, 7, 8 | amount healed per second (see above) |
| + | `vinci_build_heal_rate_use_percent` | 0, 0, 0, 0, 0, 0, 0, 0 | (1 = heal rates are percentages/sec, 0 = heal rates are hit points / sec) |
| + | `vinci_build_heal_rate` | 1, 2, 3, 4, 5, 6, 7, 8 | amount healed per second (see above) |
| + | `aircraft_heal_rate` | 60, 45, 30, 15 |  |
| + | `building_heal_level` | 0, 0, 0, 0 | An index into the building_heal_rate array above based on tribe. (Alin, Cuotl, Vinci) |
| + | `holy_district_heal_rate` | .25 | (global) amount of hit points healed per second |
| + | `holy_district_heal_by_level` | 1 | Do holy districts increase their healing by city size? |
| + | `ui_heal_display_rate` | 10 | x / heal_rate = frames skipped when showing healing (lower # == more frequent display) |
| + | `ui_heal_display_rate_limit` | 4 | The lowest number of frames skipped when diplaying healing. |
| + | `building_max_auto_heal_percent` | 10% | The maximum percent of a building's total health that a it will heal to. |

### `rules/diplomacy`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `base_tribute` | 50 | % of gift gets through |
| + | `tribute_per_city` | 5 | % additional gift gets through per city (beyond first city) |
| + | `tribute_per_district` | 5 | % additional gift gets through per district |
| + | `diplo_cooldown_frames` | 450 | How many frames before another diplomacy action with this leader can be taken |
| + | `ally_to_war_delay` | 45 sec | Period when we specifically warn you why you're taking special attrition |
| + | `ally_to_war_grace` | 30 sec | Grace period between ending alliance (or starting peace) and taking severe tire damage |
| + | `timed_peace_seconds` | 300 | How long a timed peace lasts in seconds |

### `rules/buildings`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `building_set_levels` | 2, 3, 4 | How many buildings of a certain type do you need to get levels of upgrade |
| + | `city_limit` | 2, 4, 6, 8 | Cities allowed per lore tower level |
| + | `city_center_radius` | 32 | tiles |
| + | `city_palace_radius` | 0 | tiles |
| + | `city_center_pop_radius` | 0 | tiles |
| + | `city_capture_radius` | 0 | tiles |
| + | `superiority_capture_radius` | 16 | tiles |
| + | `superiority_only_count_captains` | 1 | If set to 1, only count captains of units for superiority (grunts count as 1, not 9) |
| + | `city_spacing` | 30 | tiles |
| + | `village_upgrade_spacing` | 8 | tiles |
| + | `relax_city_spacing` | 10 | tiles |
| + | `first_city_near_coast` | 16 | tiles |
| + | `fort_spacing` | 0 | tiles |
| + | `stripmine_spacing` | 12 | tiles |
| + | `idol_spacing` | 12 | tiles |
| + | `obelisk_spacing` | 12 | tiles |
| + | `obelisk_power_up_radius` | 20 | tiles if 0 or smaller it will use the buildings max range |
| + | `obelisk_power_up_recharge_percent` | 100 | percentage of the building recharge the obelisk has when powering up another obelisk |
| + | `obelisk_power_up_percent` | 50 | percentage of the building attack to pass on for power up |
| + | `fort_to_enemy_city_spacing` | 0 | tiles |
| + | `building_to_city_spacing` | 3 tcoords, 2 tcoords, 1 tcoords | Extra space that should be kept around cities/districts so that other buildings can't get too close. |
| + | `city_to_building_spacing` | 1 tcoords | Extra space that should be kept around buildings so that cities/districts can't get too close. |
| + | `timonium_spacing` | 1 tcoords | Extra space that should be kept around timonium so that buildings (not mines) can't get too close. |
| + | `timonium_to_mine_spacing` | 1 tcoords | Extra space that should be kept around timonium so that mines can't get too close. |
| + | `construction_use_proportional_health` | 0 | (1 = Buildings under construction have proportional hit points; 0 = Buildings under construction have full hit points) |
| + | `construction_restore_to_full_health` | 0 | (1 = Completed buildings have their full health restored; 0 = completed buildings retain damage sustained during construction) |
| + | `construction_under_attack_slowdown` | 75% | If a building under construction gets attacked, slow down construction by X percent. |
| + | `refund_damage_scale` | 100% | Refunds from deleting buildings are scaled down by damage in this proportion to the percent of total damage (so if this is set to 100%, then causing 50% damage to a building will cause the refund to be scaled down by 50%). |
| + | `refund_damage_penalty` | 50% | Refunds from deleting buildings are scaled down by this much in addition to being scaled down by the amount of damage above. So if this is set to 50% then whatever amount of refund survives the "refund_damage_scale" calculation above is scaled down by a further 50%. |
| + | `city_districts` | 10, 10, 10 | Max districts in [City, Large City, Great City] - does not count palace districts |
| + | `city_buildings` | 3 | Districts to upgrade to Large City - does not count palace districts |
| + | `metro_buildings` | 6 | Districts to upgrade to Great City - does not count palace districts |
| + | `city_palaces` | 1 | Palace Districts to upgrade to Large City |
| + | `metro_palaces` | 2 | Palace Districts to upgrade to Great City |
| + | `building_hp_upgrade` | 25 | % per level |
| + | `build_support_factor` | 1 | resources |
| + | `disband_city_rate` | 400 | % normal raze time for a building |
| + | `disband_senate_rate` | 400 | % normal raze time for a building |
| + | `tower_garrison_upgrade` | 2 |  |
| + | `fort_garrison_upgrade` | 0 |  |
| + | `city_hp_bonus_to_buildings` | 0 | % HP bonus per level given by city to nearby buildings |
| + | `senate_armor_bonus` | 1 | armor per city level |
| + | `capital_build_time` | 300 | % of normal city build time (this is for nomad games only) |
| + | `military_district_build_speed` | 40% | % spawn-rate inefficiency adding military districts to city |
| + | `large_city_spawn_speed` | 0% | % Increase of grunt spawn based on being a large city |
| + | `major_city_spawn_speed` | 0% | % Increase of grunt spawn based on being a great city |
| + | `temple_upgrade_hp` | 25, 50, 100, 150, 200 |  |
| + | `temple_upgrade_range` | 1, 2, 3, 4, 5 |  |
| + | `fort_upgrade_range` | 0, 0, 0, 0, 0 |  |
| + | `fort_upgrade_los` | 0, 0, 0, 0, 0 |  |
| + | `tower_fort_range` | 0, 1, 2, 3 |  |
| + | `tower_fort_los` | 0, 2, 4, 6 |  |
| + | `vinci_shrine_preq_num` | 2 | For each building that has a shrine as a prerequisite, this is the number of buildings of that type I can build for eac shrine built. |
| + | `stripmine_build_radius` | 16 | The radius that Vinci buildings must be within of a stripmine upon building. |
| + | `magical_build_radius` | 24 | The radius(in Tiles) that a new building must be witnin of an already built unit or building. |
| + | `village_minimum_capture_hp` | 10 | Minimum HP left on Small City after capture |
| + | `city_minimum_capture_hp` | 10 | Minimum HP left on City after capture |
| + | `shrine_sacrifice_upgrade` | 5 | Grunts sacrificed to upgrade shrine |
| + | `tree_clearing_cost` | 0% extra | Percent extra cost for placing a building on top of a tree. |
| + | `repair_time_ratio` | 1 | Ratio that drives how long repair should take (1/2 means a full repair takes half as long as building construction does). |
| + | `buildtime_free_radius` | 12 | WCoord radius in which no extra build time is added |
| + | `buildtime_distance_unit` | 1 | Wcoord granularity of build time added |
| + | `buildtime_per_distance` | 0 | amount of extra build time added per extra distance |
| + | `buildtime_calibration` | 0 | (0 = linear, 1 = progressive 1/3/6/10/15/21/etc) |
| + | `physics_state_damage_divisor` | 1 | the total hit points of a building divided by this value = the maximum amout of damage a building can take |
| + | `crypt_of_knowledge_capture_research_points` | 2 | the total research points given when crypt is first captured |
| + | `crypt_of_knowledge_ownership_research_points` | 1 | number of reserch points upon holding of crypt for designated time period |
| + | `crypt_of_knowledge_ownership_interval` | 4500 frames (5 mins) | number of frames needed to hold crypt before research points are awarded |
| + | `construction_under_attack_damage_multiplier` | 4/1 | The amount that damage is multiplied by when a building is attacked that is under construction. |
| + | `construction_under_attack_air_damage_multiplier` | 1/1 | The amount that damage is multiplied by when a building is attacked that is under construction (attack by an air unit) |
| + | `city_health_recompute_caps` | 20, 40, 60, 80 | Percentage of city health to recompute Pop and Econ caps and Wealth value for trade |
| + | `city_health_cap_percent` | 20, 40, 60, 80 | Cooresponds to "city_health_recompute_caps". This is the percentage to lose at the recompute cap value |
| + | `city_damage_changes_trade_value` | 0 (1 = enabled, 0 = disabled) | if enabled then a caravans wealth income will be reduced as a city takes damage. |
| + | `city_damage_changes_attack_value` | 1 (1 = enabled, 0 = disabled) | if enabled then a the city attack will be reduced as a city takes damage. |
| + | `city_damage_discrete_values` | 0 (1 = enabled, 0 = disabled) | recompute caps when city reaches certain damage levels specified in city_health_recompute_caps |
| + | `city_damage_check_seconds` | 5 seconds | if city_damage_discrete_values is disabled then this is checked to recompute caps every X seconds |
| + | `calculator_reduction_dist` | 18 tiles | if a building is built within this radius it becomes cheaper based on "calculator_reduction" |
| + | `calculator_reduction` | 10% |  |
| + | `calculator_reduction_dist_upgrade` | 22 tiles | if a building is built within this radius it becomes cheaper based on "calculator_reduction" |
| + | `calculator_reduction_upgrade` | 25% |  |
| + | `flying_cancels_unit_construction` | 1 | 1 for yes, 0 for no |
| + | `flying_pauses_unit_construction` | 0 | 1 for yes, 0 for no |
| + | `take_damage_from_razing` | 0 | 1 for yes, 0 for no |
| + | `tower_superiority_value` | 2 | Value of a normal combat building in calculating military superiority |
| + | `fort_superiority_value` | 4 | Value of a fortress building in calculating military superiority |
| + | `unit_superiority_value` | 1, 2, 3, 4 | Value of a unit in calculating military superiority, by size |
| + | `building_under_attack_frames` | 150 | Minimum frames a building is considered "under attack" once it is attacked |
| + | `neutral_under_attack_frames` | 300 | Minimum frames a neutral building is considered "under attack" once it is attacked |

### `rules/districts`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `military_district_grunts_cuotl` | 0 (for City), 1 (for medium city), 2 (for large city) | A new military district will give X number of free grunts (Cuotl) |
| + | `military_district_grunts_vinci` | 1 (for City), 2 (for medium city), 3 (for large city) | A new military district will give X number of free grunts (Vinci) |
| + | `military_district_grunts_alim` | 2 (for City), 3 (for medium city), 4 (for large city) | A new military district will give X number of free grunts (Alim) |
| + | `merchant_district_caravans` | 0 | Free caravans per merchant district |
| + | `merchant_district_wealth` | 0 (for City), 0 (for medium city), 0 (for large city) | bonus wealth (+ amount for each merchant district) |
| + | `guild_district_extra_workers` | 0 free worker (City), 0 free workers (medium city), 0 free workers (large city) | Each gather site mining Timonium in city radius when a Guild District is built will get X free workers. |
| + | `guild_district_extra_heads` | 0 extra slot (City), 0 extra slots (medium city), 0 extra slots (large city) | Each gather site mining Timonium in city radius will get X free gather slots for each Guild District. |
| + | `guild_district_cheaper_districts` | 0 less timonium | Each Guild District in a city will make other districts in the same city cheaper. |
| + | `guild_district_global_prod_bonus` | 0 extra timonium per worker, 0 extra timonium per worker, 0 extra timonium per worker | Each Guild District in your empire will increase timonium slot value, by levlel of city |
| + | `sanctuary_district_attrition_levels` | 1 (for City), 2 (for medium city), 3 (for large city) | The level of attrition that each Sanctuary District will do based on city size. |
| + | `sanctuary_district_max_attrition_level` | 32 | The maximum level of attrition that all Sanctuary Districts can do in total. (Currently set to the "attrition" variable in rules.xml) |
| + | `industrial_district_unit_production_bonus` | 2% | Unit production conducted this much faster per Industrial District (and per level of city -- BR, 1/22/2006) |
| + | `industrial_district_unit_production_bonus_minus` | 1% | Unit production conducted this much faster per Industrial District (minus this much flat amount per district) |
| + | `industrial_district_building_construction_bonus` | 3% | Building construction conducted this much faster per Industrial District (and per level of city -- BR, 1/22/2006) |
| + | `industrial_district_building_construction_bonus_minus` | 2% | Building construction conducted this much faster per Industrial District (minus this much flat amount per district) |
| + | `industrial_district_prototype_visits` | 1 | Amount of prototype visits given per industrial district |
| + | `unfinished_district_placement` | 1 | Allow unfinished districts to count as part of a city when building new districts (1 for yes, 0 for no). |
| + | `sanctuary_district_arks` | 1 | Free holy arks per sanctuary district |
| + | `sanctuary_district_arks_cheaper` | 5% | Holy Arks this much cheaper per sanctuary district |
| + | `magus_district_mana` | 1% | Bonus hero mana per magus district |
| + | `magus_district_mana_recovery` | 1% | Bonus hero mana recovery per magus district |
| + | `magus_district_cooldown` | 1% | Bonus hero spell cooldown per magus district |

### `rules/neutrals`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `aggro_in_ctw` | 1 | Can guys aggro in ctw |
| + | `aggro_in_solo` | 1 | Can guys aggro in solo (non-ctw) |
| + | `aggro_in_multiplayer` | 1 | Can guys aggro in multiplayer |
| + | `aggro_minimum_troops` | 3 | A neutral can't ever self aggro without this many enemy troops nearby |
| + | `aggro_minimum_seconds` | 300 | A neutral can't ever self aggro before this number of seconds has passed |
| + | `aggro_interval_seconds` | 15 | Neutrals check this often for aggro (0 = disables entire concept of neutral aggro) |
| + | `aggro_base_tolerance` | 10 | if (Rnd(x) less-or-equal num-nearby-troops) then aggro |
| + | `aggro_territory_tolerance` | 10 | Extra tolerance if I'm now in the offender's territory |
| + | `aggro_radius` | 3 | WCoord radius for aggro |
| + | `aggro_prevention_radius` | 8 | WCoord radius for checking if enemies were nearby |
| + | `aggro_friend_boundary` | 40 | Has to be at least this friendly (0-100) for friendliness to count toward tolerance |
| + | `aggro_friend_denominator` | 5 | For every this-many points of friendliness above the boundary, add to tolerance |
| + | `aggro_only_if_any_town_attacked` | 1 | If true, neutrals only aggro against players who have attacked (or subjugated) at least one town. |
| + | `aggro_min_health` | .8f | Min health of a unit to cause aggro |
| + | `neutral_building_health` | 50% | Buildings with the 's' flag have this percentage of full health when they are neutral-owned |
| + | `neutral_target_unit_radius` | 24 tiles | Maximum radius in tiles from the site that defenders will attack a unit |
| + | `neutral_target_building_radius` | 20 tiles | Maximum radius in tiles from the site that defenders will attack a building |
| + | `neutral_defender_move_radius` | 18 tiles | The radius(in TILES) that the defenders stay within of the site |
| + | `neutral_defender_obey_radius_attack` | 1 | Should the defender obey the defender radius when they are chasing someone to attack them (1Y, 0N) |
| + | `max_defender_distance` | 4 tiles | When a defender is not under attack, it will try to get closer to it's building if it is farther than this many tiles away. |
| + | `defender_type` | Barbarian | The type of unit that the defender will be. |
| + | `defender_spawn_rate` | 300 | The default defender spawn rate (in seconds). |
| + | `defender_number` | 2 | The number of defenders that lurk inside each Small City initially. |
| + | `defender_cost_ratio` | 100% | percentage defender costs applied to town cost |
| + | `cost_penalty_unfriendly_territory` | 50% | Purchase price penalty if in unfriendly territory |
| + | `purchase_very_near_enemy` | 300% | Purchase modifier if very near to enemy (and far from me) |
| + | `purchase_near_enemy` | 200% | Purchase modifier if very near to enemy (and medium from me) |
| + | `purchase_near_enemy_and_me` | 75% | Purchase modifier if very near to enemy (and near to me) |
| + | `purchase_far_away` | 150% | Purchase modifier if far away |
| + | `purchase_nearer_enemy` | 50% | Purchase modifier if nearer to the enemy than to me (but not super close to him) |
| + | `purchase_medium_distance` | 25% | Purchase modifier if medium distance from me |
| + | `peace_after_aggro_discount` | 75% | Discount off peace price if we aggroed the neutral (as opposed to war from start of game) |
| + | `tribute_good_string` | 10m | The amount of tribute you will get from a Small City by default (currently supports only one type of good). |
| + | `tribute_terracotta_type` | BaseGrunt | The type of unit to be spawned. |
| + | `tribute_terracotta_number` | 1 | The amount of income you get of the specified good. |
| + | `tribute_terracotta_spawn_rate` | 90 | The spawn rate in seconds |
| + | `tribute_tethered_type` | Clockwork Man | The type of unit to be spawned. |
| + | `kill_defenders_combat_capture` | 0 | Kills all defenders when a storm is completed (0 = No, 1 = Yes) |
| + | `tribute_tethered_spawn_rate` | 30 | The time to wait to respawn after a teathered unit dies. |
| + | `defender_cost_ratio` | 100 | percentage defender costs applied to town cost |
| + | `trade_cost_to_friend_ratio` | 12 | This increases the friendliness level by (trade_cost_to_friend_ratio / base_town_cost)*100% |
| + | `max_friendliness_at_war` | 50% friendly max | Maximum faction value you can have with a town you are at war with |
| + | `tapped_reset_time` | 30 seconds | The time that must elapse after a player's last tap before a player's tapped stamp is reset on a neutral site. (in seconds) |
| + | `kill_faction_decrease` | 10% friendly decrease | Decrease to friendliness when you kill a town defender |
| + | `multiple_trade_penalty` | 40% decrease | Decrease to trade-to-friend bonus when multiple caravans "focus trading" on same site |
| + | `tapped_reset_time` | 30 seconds | The time that must elapse after a player's last tap before a player's tapped stamp is reset on a neutral site. (in seconds) |
| + | `subjugate_duration` | 1 | How long in 1/15 seconds it takes to subjugate a building (or -1 for as long as to storm; -2 for half as long, etc) |
| + | `cheap_nearest_cities` | 1 | This many cities, nearest the player, always yield the minimum buy price |
| + | `ctw_cheap_nearest_cities` | 1 | This many cities, nearest the player, always yield the minimum buy price |
| + | `cheap_nearest_sites` | 1 | This many sites, nearest the player, always yield the minimum buy price |
| + | `ctw_cheap_nearest_sites` | 1 | This many sites, nearest the player, always yield the minimum buy price |

### `rules/units`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `troops_upgrade_los` | 2 |  |
| + | `spy_upgrade_los` | 2 |  |
| + | `spy_bribe_upgrade_range` | 2 |  |
| + | `spy_informer_upgrade_range` | 3 |  |
| + | `general_upgrade_los` | 2 |  |
| + | `spy_general_cost` | 50 | % with bonus |
| + | `jam_unit_radar_prob` | 50 | % |
| + | `commando_min_damage` | 200 |  |
| + | `special_min_damage` | 400 |  |
| + | `elite_min_damage` | 800 |  |
| + | `forced_march_speed` | 50 | m |
| + | `general_speed_bonus` | 3 | m |
| + | `general_radius` | 15 | tiles (BR - 3/30/2005 - increased from 6 to 15) |
| + | `general_building_range` | 1 | tiles |
| + | `general_building_attack` | 1 |  |
| + | `general_rally_armor` | 2 | armor |
| + | `supply_radius` | 22 | tiles (BR - 6/29/2004 - increased from 14 to 20) (DK 8/8/05 20->22) |
| + | `supply_building_radius` | 25 | tiles |
| + | `decoy_time` | 2500 | frames |
| + | `caravan_attack_bonus` | 20 | attack |
| + | `siege_out_of_supply_reload` | 3/2 | fraction of normal delay |
| + | `artillery_out_of_supply_reload` | 2 | fraction of normal delay |
| + | `spy_upgrade_hp` | 15, 45, 90, 150 |  |
| + | `supply_hp_upgrade` | 0, 20, 40, 60 |  |
| + | `paradrop_range` | 64 | tiles |
| + | `artillery_under_attack_less_damage` | 50% of normal damage | Any unit with a RangedAttackSlow anim will do less damage when it is under attack |
| + | `resistance_to_magical` | 25% damage reduction | percentage resistance to magical damage if unit resists magical damage |
| + | `resistance_to_physical` | 25% | percentage resistance to physical damage if unit resists physical damage |
| + | `industrial_unit_prod_rate` | 1 | When you have Industrial Dominace, the following value is the multiplier for the speed at which queued units are produced. |
| + | `fly_speed` | 2 | Speed multiplier for Units under the fly spell |
| + | `fly_los` | 2 | LOS multiplier for Units under the fly spell |
| + | `cuotl_statue_proximity` | 10 | The proximity (in tiles) that the three cuotl temples must be within of one another to build the statue. |
| + | `hero_radius_upgrade` | 2 | The amount the hero's radius is upgraded (in tiles) for each hero upgrade. |
| + | `sandhorror_tether_radius` | 25 | The number of tiles that the Sand Horror can travel away from the building that trained it. |
| + | `melee_on_fire_damage` | 1 | When I hit a unit with a melee attack and I have the UNITTYPE_ON_FIRE flag they get this much damage. |
| + | `hero_levels` | 5 | The total number of levels that a hero can upgrade to. |
| + | `default_hero_spell_level_xp` | 10, 20, 30, 40 | The default amount of XP that each level of spell is set to for heroes |
| + | `hero_level_xp_normal` | 30, 70, 120, 180 | Level XP requirements for levels beyond the first |
| + | `hero_level_xp_spell` | 30, 70, 120, 180 | Level XP requirements for levels beyond the first (if using spellcast method) |
| + | `hero_level_xp_global` | 30, 70, 120, 180 | Level XP requirements for levels beyond the first (if using techtrack method) |
| + | `hero_xp_from_city` | 0 Alim, 0 x, 1 Cuotl, 0 Vinci | Do heroes get XP from cities (by nation) |
| + | `hero_xp_from_city_amount` | 20, 30 | XP gained for higher city levels |
| + | `hero_heal_level_upgrade` | 1 | The number of levels added to the researched heal level for a hero with the HEROSPELL |
| + | `master_heal_level_upgrade` | 1 | The number of levels added to the researched heal level for a master unit |
| + | `hero_holy_district_heal_multiplier` | 1.65 | Multiplier for heroes healed by holy districts |
| + | `hero_holy_ark_heal_multiplier` | 1.5 | Multiplier for heroes healed by holy arks |
| + | `hero_splash_damage_resist` | 50% | Percent of splash damage resisted by a hero with the HEROSPELL |
| + | `attrition_resistance_percent` | 25% | Percent of attrition resisted by units with the "Q" mask |
| + | `attrition_resistance_percent_buildings` | 75% | Percent of attrition resisted by units with the "Q" mask |
| + | `supply_heal_level_upgrade` | 0 | Number of levels a supply unit gets when in friendly territory |
| + | `min_attack_ground_radius` | 3/2 | A unit's attack ground radius (in tiles) must be at least this big to have the option to attack ground |
| + | `doge_guard_spell_fall_off_time` | 15 | The time (in frames) that all negative spells that are put on the Doge Guard will be reduced to. |
| + | `only_one_master` | 1 | 1 = Yes, 0 = No |
| + | `max_dist_to_use_melee_anim_at_close_range` | 120 | How close does the units have to be to it's target when it has the unit flag2 "e" (in Coords, 1 Tile = 192 Coords) |
| + | `glass_dragon_attack_frames` | 1 | number of frames it takes until the glass dragon attacks at 100% since last attack |
| + | `target_transport_moves_to_pickup` | 1 | targeted transport moves to pick up units |
| + | `target_transport_clear_orders` | 1 | targeted transport clears orders to move |

### `rules/movement`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `unit_pack_turn_bonus` | 2 | X normal speed (units turn faster when packed) |
| + | `unit_block_radius` | 48 | Coord (1 UCoord) (master calibration for unit blocking) |
| + | `dock_radius` | 12 | tiles |
| + | `build_move_speed` | 1 | Coords per frame (will be multiplied by building speed) |
| + | `unit_formation_spacing` | 1/16 | fraction of tile (calibration for unit spacing in formations) |
| + | `unit_move_speed` | 1/192 | fraction of tile (granularity for unit movement speeds) |
| + | `unit_speed_hack` | 1/1 | Global adjuster for unit speeds |
| + | `unit_turn_speed` | 1 | fractional rate (master control for unit turn speed) |
| + | `no_turn_anim_penalty` | 50% of normal turn speed | Turn speed modifier when a unit that needs a turn anim hasn't started the turn anim yet. (NOT CURRENTLY USED) |
| + | `no_turn_anim_heavy_penalty` | 25% of normal turn speed | Additional turn speed modifier the turning unit is flying, and turning more than 90 degrees. (NOT CURRENTLY USED) |
| + | `unit_guy_spacing` | 1/16 | fraction of tile (calibration for spacing within a foot unit) |
| + | `unit_train_distance` | 5/2 | fraction of tile |
| + | `unit_train_max_distance` | 7/2 | fraction of tile |
| + | `boat_train_distance` | 3/2 | fraction of tile |
| + | `boat_train_max_distance` | 8 | tiles |
| + | `boat_garrison_max_distance` | 3 | tiles |
| + | `unit_board_distance` | 3 | fraction of tile |
| + | `unit_disembark_distance` | 3 | fraction of tile |
| + | `attention_distance` | 5 | distance from an attentive unit to trigger it |
| + | `building_float_height` | 800 | distance above terrain that buildings will fly at |
| + | `formation_aspect_low` | 0.125 (8 to 1) | Formation aspect ratio low end (really wide formation) |
| + | `formation_aspect_mid` | 0.4 (2.5 to 1) | Formation aspect ratio midpoint (default formation) |
| + | `formation_aspect_high` | 5 (1 to 5) | Formation aspect ratio high end (really narrow formation) |
| + | `formation_default_setting` | 5 | Default setting (currently from 0 to 8) for formations |
| + | `melee_damage_slow_frames` | 30 | # 1/15 sec frames unit slowed when sustains melee damage |
| + | `melee_damage_slow_immune` | 30 | Speed below this level immune to melee damage slowdown |
| + | `melee_damage_slow_percent` | 50% | Remaining speed reduced by this percentage while slowed by melee |
| + | `grunt_catch_up_modifier` | 7/4 | speed multiplier for grunts to get back into their formation |

### `rules/combat`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `unit_respond_range` | 12 | tiles: distance units normally look for enemies to attack, or whatever. (BR 3/1/2006 - please discuss any changes with me) |
| + | `unit_defensive_respond_range` | 12 | tiles (BR - please discuss any changes with me) |
| + | `unit_sentry_respond_range` | 8 | tiles (BR - please discuss any changes with me) |
| + | `unit_guard_respond_range` | 12 | tiles (BR - please discuss any changes with me) |
| + | `unit_shout_range` | 24 | tiles: distance units will respond to enemies that nearby friendly units are attacking (should be larger than unit_respond_range) |
| + | `unit_guard_max_range` | 20 | (in tiles) The maximum distance we can be away from our guarded unit, while attacking, before moving closer to them. This shoul be a bit bigger than unit_guard_respond_range |
| + | `unit_build_respond_range` | 12 | tiles |
| + | `unit_gather_respond_range` | 32 | tiles |
| + | `aircraft_respond_range` | 8 | tiles (BR 1/16/2003 -- made this smaller so that planes don't go wandering all over the place into air defenses) |
| + | `bomber_respond_range` | 8 | tiles |
| + | `ship_defensive_respond_range` | 8 | tiles (Ships need a better respond range so fire ships won't just sit there) |
| + | `transport_death_penalty` | 50% | amount of damage sustained by units forcibly ejected from dying transport |
| + | `height_increment` | 200 | z-value (whatever that means, I don't think it's in tiles) |
| + | `height_bonus` | 10 | % per increment |
| + | `flank_bonus` | 50 | % per level of flank (max bonus is twice this number) |
| + | `cavalry_flank_bonus` | 40 | % (of base flank bonus) |
| + | `vehicle_flank_bonus` | 33 | % (of base flank bonus) |
| + | `focus_fire_frames` | 30 | (Was overkill_frames) Unit hit twice or more w/in this time period receives attenuated damage from second and subsequent hits. Does not apply to damage from buildings. |
| + | `focus_fire_damage` | 2/3 | (Was overkill_damage) Damage attenuation from focus fire |
| + | `focus_fire_damage_squad` | 1/2 | Damage attenuation from focus fire by squad |
| + | `focus_fire_damage_hero` | 9/12, 8/12, 7/12, 6/12, 5/12 | Damage attenuation from focus fire to heroes, by level |
| + | `ai_break_off_chance` | 50 | If the AI is trying to chase a target that was in range, what percent chance does it have to give up? |
| + | `target_radius` | 1/2 | fraction of tile (calibration for target sizes) |
| + | `range_inaccuracy` | 1/128 | percent per # tiles |
| + | `rocky_modifier` | 2/3 | (light infantry in rocks) |
| + | `entrenchment_modifier` | 2/3 |  |
| + | `river_modifier` | 2 | fractional modifier (units take more damage in rivers) |
| + | `recapture_city_modifier` | 2 | fractional modifier |
| + | `storm_time_neutrals` | 1/4 | fraction of normal storm time for neutrals |
| + | `storm_time_no_cities_left` | 1/4 | fraction of normal storm time for someone with no unstormed cities left |
| + | `storm_time_own_city` | 1/4 | fraction of normal storm time for recapturing own cities |
| + | `speed_when_wounded` | 20% | percentage modifier |
| + | `attack_when_wounded` | 50% | percentage modifier |
| + | `ground_vs_air_buildings` | 50% less damage | When ground units attack flying buildings, they do this much less damage |
| + | `knockdown_size_values` | 0, 999, 999, 999 | Amount of knockdown needed to knock over a particular size of unit (small, medium, large, building) |
| + | `skill_size_modifier` | 100%, 75%, 50%, 50% | Skilled weapons damage against size types (small, medium, large, building) |
| + | `pound_size_modifier` | 50%, 75%, 100%, 100% | Pounding weapons damage against size types (small, medium, large, building) |
| + | `siege_size_modifier` | 50%, 75%, 100%, 200% | Siege weapons damage against size types (small, medium, large, building) |
| + | `accurate_size_modifier` | 100%, 100%, 100%, 100% | Accurate weapons damage against size types (small, medium, large, building) |
| + | `piercing_size_modifier` | 100%, 100%, 100%, 100% | Piercing weapons damage against size types (small, medium, large, building) |
| + | `anti_tech_size_modifier` | 10%, 10%, 10% | Adittional damage that anti-tech weapons do to mechanical targets weapons damage against size types (small, medium, large) |
| + | `skill_armor_modifier` | 100%, 100%, 100% | Skilled weapons damage against armor types (light, medium, heavy) |
| + | `pound_armor_modifier` | 100%, 100%, 100% | Pounding weapons damage against armor types (light, medium, heavy) |
| + | `siege_armor_modifier` | 100%, 100%, 100% | Siege weapons damage against armor types (light, medium, heavy) |
| + | `accurate_armor_modifier` | 100%, 75%, 50% | Accurate weapons damage against armor types (light, medium, heavy) |
| + | `piercing_armor_modifier` | 100%, 90%, 80% | Piercing weapons damage against armor types (light, medium, heavy) |
| + | `splash_range_granularity` | 4 | splash damage radius granularity. TILE/x |
| + | `weapons_attack_bonus` | 5% | Attack bonus added for each Weapons Power level researched |
| + | `trample_slowdown_percent` | 1% | Every point of trample slowdown on a unit will lower speed by this percent |
| + | `trample_slowdown_bleed` | 1 | One point of trample slowdown will be removed from a unit after this many frames |
| + | `trample_slowdown_points` | 1, 3, 5, 15 | Trampling a unit will give you this many trample slowdown points (by unit size) |
| + | `trample_no_knockdown_divisor` | 45 | If we trample someone but we can't knock them over, then the trample damage will be divided by this value to prevent fast killing. (Currently set to the frame rate, "15", so full trample will be done every second if we continually run into the unit.) |
| + | `trample_max_slowdown_percent` | 50% | The maximum percent of slowdown that our unit can have from trampling |
| + | `neutral_site_aggro_radius` | 10 | Radius that hostile neutrals will attack buildings. |
| + | `force_melee_combat` | 0 | If you want melee units to force range units into melee combat, put a 1 here. |
| + | `peace_city_jack_timer` | 30 | If you are at peace with another player, you are not capable of city jacking that player for this many seconds. |
| + | `melee_splash_vs_multifigure_damage` | 36% |  |

### `rules/aircraft`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `air_unit_mana_recharge` | 1 | craft per frame |
| + | `max_aircraft_per_carrier` | 7 |  |
| + | `max_aircraft_per_airbase` | 10 |  |
| + | `bombing_mana_cost` | 0 | mana |
| + | `sam_site_reload_chance` | 10 | % |
| + | `sam_site_reload_time` | 50 |  |

### `rules/world`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `min_continent_spacing` | 3 | wcoords including coasts |
| + | `min_num_tree_tiles_in_wcoord_for_forest` | 4 | The min number of tree tcoords to designate a forest |
| − | `whatever` | 0 | Extra item so it will make a grid. |

### `rules/line_of_sight`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `only_show_nearest_cities` | 2 | If 0 then all cities on map revealed at start; otherwise only closest X cities to your starting location are revealed. |
| + | `ctw_only_show_nearest_cities` | 0 | If 0 then all cities on map revealed at start; otherwise only closest X cities to your starting location are revealed. |
| + | `see_all_terrain` | 1 | Number of Diamonds to have this ability |
| + | `see_all_gems` | 3 | Number of Diamonds to have this ability |
| + | `see_all_goodies` | 6 | Number of Diamonds to have this ability |
| + | `see_all_villages` | 4 | Number of Diamonds to have this ability |
| + | `see_my_gems` | 2 | Number of Diamonds to have this ability |
| + | `see_my_goodies` | 2 | Number of Diamonds to have this ability |
| + | `see_my_villages` | 2 | Number of Diamonds to have this ability |
| + | `see_all_friendly` | 3 | Number of Diamonds to have this ability |
| + | `see_all_buildings` | 4 | Number of Diamonds to have this ability |
| + | `see_all_heroes` | 5 | Number of Diamonds to have this ability |
| + | `see_all_units` | 6 | Number of Diamonds to have this ability |
| + | `forest_los_percent` | 50 | The percent of the line of sight radius that is effected by forests (ie 33 == last 33 percent of los) |
| + | `diamond_los_bonus` | 0, 1, 2, 3, 4, 5, 6, 6, 6, 6, 6, 6 | Bonus to unit line of sight indexed by the number of diamonds under control |

### `rules/dominance`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `min_craft_title` | 8, 9, 10, 11, 12, 13, 14, 15, 16, 16, 16, 16 | Minimum techs to get Craft dominance |
| + | `min_tactical_title` | 10, 15, 20, 25, 30, 40, 50, 60, 80, 100, 150, 300 | Minimum kill points for Tactical dominance |
| + | `min_army_title` | 8, 10, 12, 15, 20, 30, 40, 50, 75, 100, 200, 400 | Number of units purchased for Army Dominance |
| + | `min_resource_title` | 750, 1000, 1250, 1500, 2000, 2500, 3500, 5000, 7000, 10000, 20000, 40000 | Minimum Timonium (+ Wealth/Energy?) to get Resource dominance |
| + | `count_wealth_for_resource_title` | 1 | if this is "1" then the numbers in min_resource_title include wealth and energy as well as timonium |
| + | `use_income_for_resource_title` | 0 | if this is "1" then Resource Dominance is based on income rather than treasury |
| + | `tie_for_resource_title` | 0 | Need to beat your opponents by this much to have clear dominance |
| + | `tie_for_craft_title` | 0 | Need to beat your opponents by this much to have clear dominance |
| + | `tie_for_tactical_title` | 0 | Need to beat your opponents by this much to have clear dominance |
| + | `tie_for_army_title` | 0 | Need to beat your opponents by this much to have clear dominance |
| + | `resource_title_stickiness` | 0 | If your opponent has the title, this is subtracted from the tie value |
| + | `craft_title_stickiness` | 0 | If your opponent has the title, this is subtracted from the tie value |
| + | `tactical_title_stickiness` | 0 | If your opponent has the title, this is subtracted from the tie value |
| + | `army_title_stickiness` | 0 | If your opponent has the title, this is subtracted from the tie value |
| + | `kill_value_mine` | 3 | Mine/Wagon kill value |
| + | `kill_value_caravan` | 2 | Caravan kill value |
| + | `kill_value_miner` | 1 | Miner kill value |
| + | `dominances_permanent` | 0 | If "1" the dominances are permanent and never change hands |
| + | `dominance_level_minutes` | 10 | Game minutes before dominances upgrade |
| + | `max_dominance_levels` | 4 | Total possible upgrade levels for dominances |

### `rules/strip`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `strip_city_base_size` | 300 tiles | A city will strip X terrain tiles when it starts |
| + | `strip_city_end_size` | 600 tiles | A city will strip a total of X terrain tiles, then stop |
| + | `strip_city_level_size` | 600 tiles | Extra tiles stripped per level of city |
| + | `strip_city_growth_rate` | 5 | City strip area will grow every X frames (15th of second) |
| + | `strip_city_growth_size` | 15 tiles | Number of tiles stripped when a city grows |
| + | `strip_mine_base_size` | 100 tiles | A mine will strip X terrain tiles when it starts |
| + | `strip_mine_end_size` | 600 tiles | A mine will strip a total of X terrain tiles, then stop |
| + | `strip_mine_growth_rate` | 15 | Mine strip area will grow every X frames (15th of second) |
| + | `strip_mine_growth_size` | 15 tiles | Number of tiles stripped when a mine grows |
| + | `company_mine_base_size` | 300 tiles | A mine will strip X terrain tiles when it starts |
| + | `company_mine_end_size` | 300 tiles | A mine will strip a total of X terrain tiles, then stop |
| + | `company_mine_growth_rate` | 15 | Mine strip area will grow every X frames (15th of second) |
| + | `company_mine_growth_size` | 5 tiles | Number of tiles stripped when a mine grows |
| + | `industrial_mine_base_size` | 600 tiles | A mine will strip X terrain tiles when it starts |
| + | `industrial_mine_end_size` | 600 tiles | A mine will strip a total of X terrain tiles, then stop |
| + | `industrial_mine_growth_rate` | 15 | Mine strip area will grow every X frames (15th of second) |
| + | `industrial_mine_growth_size` | 5 tiles | Number of tiles stripped when a mine grows |
| + | `strip_height_crawl` | 100 | stripped terrain won't spread up a cliff this high |

### `rules/spells`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `spell_summon_percent_area` | 75% | How much of the area within a summon radius that must be a valid tile? |
| + | `flash_cease_fire_seconds` | 5 | How many seconds to flash the cease fire spell tinting |
| + | `spell_chop_range` | 24 | The range that spell ranges start being chopped at when trying to get close enough to cast (in tiles) |
| + | `spell_chop_range_divisor` | 20 | If distance to the spell targe is grater than spell_chop_range then the range divided by the spell_chop_range divisor will be subtract from the test range to find a closer spot. This is used to account for errors in the distance approximation functions. |
| + | `spell_valid_cast_location_search_range` | 4 | The maximum range in Tiles that a spell will search out for a valid cast location when you are targeting an area. |

### `tribes/vinci`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `banking_trade_value` | 1 | a vinci city gets +X trade value for each level of banking researched |
| + | `clockwork_death_heal_radius` | 12 tiles | when a clockwork man dies, it will heal any clockwork men within this radius |
| + | `clockwork_death_heal_amount` | 45 hps | when a clockwork man dies, it will heal nearby clockwork men this much |
| + | `clockwork_rebuild_count` | 2 dead clockwork guys | when this number of clockwork men die near each other, a new guy is created |
| + | `clockwork_man_attack_bonus` | 1 | a clockwork man gets +X attack for each guy on his left, right, forward, and rear |
| + | `strip_mine_extra_heads` | 0 | when a strip mine is on a gather site, it gets this many extra heads |
| + | `strip_mine_build_anywhere` | 0 | Set to non-zero if strip mines can be built anywhere (if so, it's the number of heads you get) |
| + | `industrial_mine_bonus` | 50% | Bonus heads at industrial mine |
| + | `vinci_mil_terrain` | 1 | number of tiles destroyed when a vinci military building makes a unit |
| + | `material_bonus_for_strip_mine` | 25 | how many Timonium do you get for building a strip mine (now unused) |
| + | `vinci_tech_ramp_cost` | 0, 25, 0, 0, 0 | ramping costs for vinci techs (for each good) |
| + | `vinci_banking` | 5% | Vinci banking tech value towards all production |
| + | `vinci_industry` | 10% | Vinci industry tech value towards unit production speed |
| + | `vinci_invention` | 5% | Vinci invention tech value towards technology research costs |
| − | `vinci_politics` | 0, 1, 2, 4, 6, 8, 11, 0 | Vinci politics value toward national borders |
| + | `vinci_build_terrain` | 16 | number of tiles destroyed when a new vinci non-stripmine building is created |
| + | `vinci_bonus_worker_slots` | 0 | the number of extra clockwork miner slots at each Vinci mine. |
| + | `bonus_worker_extra_gather` | 3 | the number of extra resources gathered by clockwork miners |
| + | `powerplant_unit_production_bonus` | 25% | unit and unit research conducted this much faster |
| + | `vinci_storm_time_bonus` | 0% | % reduction in storm times |
| + | `vinci_storm_damage_bonus` | 0% | % reduction in storm damage |
| + | `ramp_final_prototypes` | 1 | use ramping for final prototype techs? |
| + | `ramp_final_prototypes_for_all` | 1 | use ramping across all final prototype techs |

### `tribes/cuotl`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `battery_mill_income` | 1 extra income, 1 extra income, 2 extra income, 2 extra income, 3 extra income | +X extra income per head at a Mill with a battery |
| + | `battery_obelisk_attack_bonus` | 5% extra attack value, 15% extra attack value, 30% extra attack value, 45% extra attack value, 60% extra attack value | X% extra combat value for Obelisk with a battery |
| + | `battery_cheaper_fane` | 0%, 0%, 0%, 0%, 0% | Non-gem costs of units built at fane with a battery are this much cheaper |
| + | `battery_faster_fane` | 10%, 20%, 35%, 50%, 65% | Units built at the fane with a battery are built this much faster |
| + | `battery_cheaper_aircraft` | 0%, 0, 0, 0, 0 | Non-gem costs of aircraft built at a building with a battery are this much cheaper |
| + | `battery_faster_aircraft` | 10%, 20%, 35%, 50%, 65% | Aircraft built at a building with a battery are built this much faster DFK Actually appear to MOVE faster (BR - NOT true, they just think Quetzals move too fast in general and are confused). |
| + | `battery_sanctuary_extra_push_limit` | 5 extra limit, 10 extra limit, 20 extra limit, 30 extra limit, 40 extra limit | Sanctuary with a batter gets extra border push |
| − | `battery_sanctuary_extra_pushfactort` | 1 extra factor, 2 extra factor, 4 extra factor, 6 extra factor, 9 extra factor | Sanctuary with a batter gets extra border push |
| + | `battery_faster_sanctuary` | 10%, 20%, 35%, 50%, 65% | Units built at the sanctuary with a battery are built this much faster |
| + | `battery_energy_rate_bonus` | 0% extra attack value, 0% extra attack value, 0% extra attack value, 0% extra attack value, 0% extra attack value | X% extra gather rate for city with a battery |
| + | `battery_idol_attack_bonus` | 0% extra attack value, 0% extra attack value, 0% extra attack value, 0% extra attack value, 0% extra attack value | X% extra combat value for Idol with a battery |
| + | `cuotl_tech_ramp_cost` | 25, 0, 0, 10 gem level, 0 | ramping costs for cuotl techs (for each good) |
| + | `cuotl_reactor_mana_bonus` | 200 | Number of mana added for each Reactor Power level researched |

### `tribes/alim`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `alim_build_rate` | 0% | Alin building rate increase (100% == twice as fast) |
| + | `alim_base_push` | 0 push_limit, 0 push_factor | The base push factor for Alin buildings (push_limit, push_factor) |
| + | `alim_cost_neutral` | 50% | Additional cost to build in neutral territory |
| + | `alim_cost_enemy` | 100% | Additional cost to build in enemy territory |
| + | `alim_cost_apply_to_ramp` | 0 | Alim additional costs apply to ramping cost as well |

### `heroes/heroes`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `heroes_level_by_order` | 1 | If true, non-cuotl heroes gain levels based on the order they come in (second hero at second level, etc). |
| + | `heroes_summon_at_full_health` | 1 | If 1, heroes will show up at full health. If 0, they will show up at the same health as the summon point. |
| + | `cast_xp_score_bonus` | 50, 100, 150, 200 | Heroes that level up by casting spells don't get a score bonus for researching spells. So they get bonus points for leveling up a hero. |

### `heroes/vinci`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `giacomo_research_bonus` | 1, 2, 3, 4, 5 | bonus research points by level |
| + | `giacomo_lab_bonus` | 0%, 0%, 0%, 0%, 0% | lab building price reduction by level |
| + | `doge_unit_rate_bonus` | 5%, 10%, 15%, 25%, 30% | % unit production speed bonus by level |
| + | `lenora_wealth_income` | 10, 20, 30, 40, 50 | bonus lenora wealth by level |

### `heroes/cuotl`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `czin_attrition` | 1, 2, 3, 4, 5 | attrition by level |
| + | `xil_cooldown` | 5%, 10%, 15%, 20%, 25% | cooldown reduction by level |
| + | `shok_shields` | 5%, 10%, 15%, 20%, 25% | shield improvement by level |

### `heroes/alim`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `sawu_spiders` | 0, 1, 2, 3, 4 | free spiders by level |
| + | `sawu_timonium_income` | 10, 20, 30, 40, 50 | sawu timonium by level |
| + | `dakhla_scorpions` | 0, 1, 2, 3, 4 | free scorpions by level |
| + | `dakhla_commerce_cap` | 10, 25, 50, 75, 100 | bonus commerce cap by level |
| + | `damanhur_afreets` | 0, 1, 2, 3, 4 | free afreets by level |
| + | `damanhur_popcap` | 10, 25, 50, 75, 100 | bonus pop cap by level |

### `wonders/greatfastness`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `greatfastness_heal` | 300 | % that units are healed faster when units are when garrisonned inside |
| + | `greatfastness_hp_bonus` | 50 | % HP increase for all other builds when the leader has built the wonder |

### `wonders/cryptofknowledge`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `cryptofknowledge_attack_bonus` | 50 | % that hero's damage to the enemy is increased |
| + | `cryptofknowledge_research_bonus` | 25 | % that research times are reduced |
| + | `cryptofknowledge_cooldown_bonus` | 25 | % that cooldown times are reduced |

### `ctw/rules`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `ctw_ramp_qb_neutral_rate` | 3 |  |
| + | `ctw_ramp_qb_neutral_max` | 4 |  |
| + | `ctw_starting_tribute` | 25 |  |
| + | `ctw_no_attack_bonus` | 25 |  |
| + | `ctw_no_attack_bonus_cap` | 100 |  |
| + | `ctw_trib_no_attack` | 3 |  |
| + | `ctw_continent_bonus` | 10 |  |
| + | `ctw_attrition` | 50 | % bonus |
| + | `ctw_base_war_cost` | 20 | tribute |
| + | `ctw_war_cost_stance_time_x` | 1 | tribute |
| + | `ctw_base_peace_cost` | 20 | tribute |
| + | `ctw_peace_cost_att_x` | 1 | tribute |
| + | `ctw_base_build_cost` | 20 | tribute |
| + | `ctw_build_cost_time_held_x` | 10 | x |
| + | `ctw_base_card_cost` | 30 | tribute |
| + | `ctw_card_cost_num_purchased_x` | 40 | x |
| + | `ctw_card_cost_ramp_cap` | 110 |  |
| + | `ctw_base_territory_cost` | 75 | tribute |
| + | `ctw_territory_cost_att_x` | 2 |  |
| + | `ctw_territory_cost_str_x` | 25 |  |
| + | `ctw_territory_min_emp_size` | 4 |  |
| + | `ctw_territory_max_att` | 17 |  |
| + | `ctw_max_attitude` | 20 |  |
| + | `ctw_start_attitude` | 10 |  |
| + | `ctw_num_opp_for_larger_map` | 2 |  |
| + | `ctw_min_keep_alliance` | 4 |  |
| + | `ctw_break_alliance_prob_x_att` | 1 |  |
| + | `ctw_capital_starting_infra` | 2 |  |
| + | `ctw_nomad_starting_res_x` | 3 |  |
| + | `ctw_max_infra_special_scen` | 3 |  |
| + | `ctw_max_treachery_infra` | 3 |  |
| + | `ctw_age_cost` | 50 |  |
| + | `ctw_eureka_techs` | 2 |  |
| + | `ctw_eureka_all_techs` | 1 |  |
| + | `ctw_max_bribe_att` | 15 |  |
| + | `ctw_cit_per_extra_city` | 10 |  |
| + | `ctw_ai_barb_chance` | 95 |  |
| + | `ctw_ai_stronger_chance` | 80 |  |
| + | `ctw_additional_ally_inc` | 100 |  |
| + | `ctw_break_bribe_x` | 3 |  |
| + | `ctw_ally_war_cost_x` | 2 |  |
| + | `ctw_ai_all_cards_num` | 4 |  |
| + | `ctw_ai_all_cards_prob` | 25 |  |
| + | `ctw_ai_tac_card_prob` | 25 |  |
| + | `ctw_ai_tac_card_ally_prob` | 50 |  |
| + | `ctw_bribe_trib_min` | 10 |  |
| + | `ctw_bribe_tough_hate_acc_prob` | 35 |  |
| + | `ctw_bribe_tough_acc_prob` | 50 |  |
| + | `ctw_bribe_acc_prob` | 100 |  |
| + | `ctw_demand_trib_tough_prob` | 50 |  |
| + | `ctw_demand_trib_prob` | 35 |  |
| + | `ctw_overrun_peace_prob` | 50 |  |
| + | `ctw_overrun_easiest_prob` | 0 |  |
| + | `ctw_overrun_easy_prob` | 10 |  |
| + | `ctw_overrun_moderate_prob` | 20 |  |
| + | `ctw_overrun_tough_prob` | 100 |  |
| + | `ctw_def_victory_bonus` | 50 |  |
| + | `ctw_wonder_min_civics` | 4 |  |
| + | `ctw_conquest_time_limit` | 90 |  |
| + | `ctw_great_thinker_discount` | 25 |  |
| + | `ctw_great_thinker_time` | 25 |  |
| + | `ctw_market_bonus_buy` | 25 |  |
| + | `ctw_market_bonus_sell` | 20 |  |
| + | `ctw_heal_bonus` | 100 |  |
| + | `ctw_prod_rate_bonus` | 5 |  |
| + | `ctw_max_wonder_cards` | 1 |  |
| + | `ctw_num_bonus_elephants` | 6 |  |
| + | `ctw_parmenio_cav` | 5 |  |
| + | `ctw_memnon_infra` | 4 |  |
| + | `ctw_spitamenes_archers` | 5 |  |
| + | `ctw_construction_bonus` | 30 | building contruction time bonus. percentage This only applies to non-ctw quick battles. |
| + | `ctw_train_bonus` | 0 | unit train time bonus. percentage This only applies to non-ctw quick battles. |
| + | `ctw_research_bonus` | 0 | research time bonus. percentage This only applies to non-ctw quick battles. |
| + | `ctw_unit_ramp_reduction` | 50 | unit ramp time reduction. percentage This only applies to non-ctw quick battles. |

### `ctw/generals`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `alexander_attack` | 1 |  |
| + | `alexander_heavy_inf_attack` | 1 |  |
| + | `parmenio_heavy_cav_attack` | 1 |  |
| + | `ptolemy_range_bonus` | 1 |  |
| + | `ptolemy_los_bonus` | 2 |  |
| + | `ptolemy_siege_attack` | 1 |  |
| + | `ptolemy_armor` | 1 |  |
| + | `darius_armor` | 1 |  |
| + | `spitamenes_horse_archer_attack` | 1 |  |
| + | `antipater_heal_rate` | 20 |  |
| + | `antipater_garrison_attack_bonus` | 3 |  |
| + | `alexander_forced_march_speed` | 3/2 |  |
| + | `parmenio_radius_adjust` | 3/2 |  |
| + | `parmenio_ambush_time` | 3 |  |
| + | `parmenio_ambush_cost` | 1 |  |
| + | `spitamenes_cav_speed` | 281/256 |  |
| + | `porus_elephant_speed` | 147/128 |  |
| + | `porus_decoys` | 2 |  |
| + | `memnon_regen_rate` | 2 |  |
| + | `antipater_entrench_bonus` | 51/64 |  |
| + | `antipater_entrench_rate` | 1/2 |  |

### `spells/stormcity`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `stormcity_grunts_per_city_size` | 1, 2, 4, 6 | Number of grunts it costs per city size (Small City, City, Large City, Great City) |
| + | `stormcity_health_multiplier` | 128/256, 256/256, 512/256, 1024/256 | The multiplier for the current city health left: 0 - 24%, 25% - 49%, 50% - 74%, 75% - 100% |
| + | `stormcity_grunts_per_district` | 1 | Number of grunts it costs per district in a city |
| + | `stormcity_grunts_per_military_district` | 1 | Number of additional grunts it costs per military district in a city |
| + | `stormcity_extra_grunts_already_stormed` | 1, 2, 3, 4 | Numbers of extra grunts it costs if the city is already being stormed |
| + | `stormcity_original_race` | 1/2 | Grunt storming multiplier when you originally built city you are storming |
| + | `stormcity_minimum_grunts` | 0, 0, 0, 0 | The minimum number of grunts that it takes to storm after all modifiers have been applied. (Small City, City, Large City, Great City) |
| + | `stormcity_grunt_damage` | 85 | The amount of damage given to each stormer when they storm a building |
| + | `stormcity_min_restorm_wait` | 75 | The time(in frames) that a player must wait to storm again after a storm cycle ends. |

### `spells/besiege`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `besiege_stormcity_multiplier` | 192/256 | Multiplier to reduce the number of grunts it takes to storm a city |

### `spells/flamesuits`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `flamesuits_damage` | 5 | The amount of damage that an enemey unit recieves when he attacks a unit that has the flamesuit spell |

### `spells/slow`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `slow_amount` | 66% slower, 33% slower, 10% slower, 0% slower | percent that unit will be slowed (by unit size (small, medium, large)) |
| + | `slow_chance` | 100%, 100%, 100%, 0% | percent chance that slow spell will be cast (by unit size (small, medium, large)) |
| + | `slow_duration` | 15, 10, 5, 0 | seconds that unit will be slowed (by unit size (small, medium, large)) |

### `spells/whirlwind`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `whirlwind_damage` | 5, 10, 15, 20 | Damage done every second based on the size of the object we are damaging (small, medium, large, building) |

### `spells/burningcloud`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `burning_cloud_damage` | 5, 10, 15, 20 | Damage done every second based on the size of the object we are damaging (small, medium, large, building) |

### `spells/suncloak`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `sun_cloak_damage` | 5, 10, 15, 20 | Damage done every second based on the size of the object we are damaging (small, medium, large, building) |

### `spells/interdict`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| − | `interdict_attrition_multiplier` | 2, 4, 8 | The multiplier for attrition by age (age 1, 2, 3) |

### `spells/harpoon`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `harpoon_attack_bonus` | 10, 10, 10, 10 | attack bonus that that the harpooning unit receives (by unit size (small, medium, large)) |
| + | `harpoon_damage_reduction` | 50%, 50%, 50%, 50% | the percentage that the harpooned unit's attack will be reduced (by unit size (small, medium, large)) |

### `spells/slowdown`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `slowdown_amount` | 20% slower, 20% slower, 20% slower, 20% slower | percent that unit will be slowed (by unit size (small, medium, large)) |

### `spells/warflag`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `warflag_attack_bonus` | 25%, 50%, 100% | attack bonus that units withing the war_flags radius receive (by age) |
| + | `warflag_armor_bonus` | 1, 3, 5 | armor bonus that units withing the war_flags radius receive (by age) |

### `spells/initial_cooldown`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `initial_dominance_cooldown` | 0 | Should the initial cooldown for dominance spells be in effect (percentage) |
| + | `initial_national_cooldown` | 50 | Should the initial cooldown for national spells be in effect (percentage) |
| + | `initial_hero_cooldown` | 100 | Should the initial cooldown for hero spells be in effect (percentage) |
| + | `initial_object_cooldown` | 100 | Should the initial cooldown for normal unit and building spells be in effect (percentage) |
| + | `initial_dominance_cooldown_ctw` | 100 | Should the initial cooldown for dominance spells be in effect in CTW (percentage) |
| + | `initial_national_cooldown_ctw` | 100 | Should the initial cooldown for national spells be in effect in CTW (percentage) |
| + | `initial_hero_cooldown_ctw` | 0 | Should the initial cooldown for hero spells be in effect in CTW (percentage) |
| + | `initial_object_cooldown_ctw` | 0 | Should the initial cooldown for normal unit and building spells be in effect in CTW (percentage) |

### `spells/heartbeat_cooldown`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| − | `hero_heartbeat_cooldown_magic ` | 5 | Heartbeat cooldown for magic heroes (in seconds) |
| − | `hero_heartbeat_cooldown_tech ` | 10 | Heartbeat cooldown for tech heroes (in seconds) |
| + | `units_have_heartbeat_cooldown` | 0 | Use Heartbeat cooldown on units (0 = Don't use) |
| − | `unit_heartbeat_cooldown_magic ` | 5 | Heartbeat cooldown for magic units (in seconds) |
| − | `unit_heartbeat_cooldown_tech ` | 10 | Heartbeat cooldown for tech units (in seconds) |
| − | `dominance_heartbeat_cooldown ` | 15 | Heartbeat cooldown for dominance and national spells (in seconds) |

### `spells/sweepingcharge`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `sweeping_charge_upgrade` | 100% additional attack strength, 3 seconds longer, 0, 0 | Upgrades the sweeing charge spell when the berserker has it. |

### `ui/ui`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `ui_num_districts_no_barracks` | 3 | The number of districs it takes to flash the build button when you have no barracks built. |
| + | `ui_show_building_completion` | 1 | Shows a message when buildings completed |
| + | `ui_cursor_hero_summon_radius` | 3 | The radius of the hero summon cursor in tiles |
| + | `ui_hero_in_danger_percent` | 0.40 | What % of health the hero has left before it is considered in danger |
| + | `ui_hero_under_attack_frames` | 30 | The hero button will flash if the last time it was attacked is within this number of frames |
| + | `ui_control_group_gap` | 10 | gap between the hero/master unit list and the control group display |
| + | `ui_tutorial_arrow_feedback_sound_interval` | 20 | interval in seconds between feedback sound for "tutorial arrow visible" being played |
| + | `text_alert_short_length` | 2000 | Length in milliseconds of short center-screen text messages (UI feedback/preferences) |
| + | `text_alert_normal_length` | 3000 | Length in milliseconds of normal center-screen text messages (Minor alerts) |
| + | `text_alert_long_length` | 5000 | Length in milliseconds of long center-screen text messages (Major alerts) |
| + | `text_alert_extra_long_length` | 8000 | Length in milliseconds of extra long center-screen text messages (Quest Updates) |
| + | `zeke_messages` | 1 | Wanna know about Zeke? |
| + | `ui_quest_update_interval_easy` | 120 | How long the quest button waits to blink at you on easy difficutly. |
| + | `ui_quest_update_interval_medium` | 180 | How long the quest button waHow long the quest button waits to blink at you on medium difficutly.its to update |
| + | `ui_quest_update_interval_hard` | 300 | How long How long the quest button waits to blink at you on hard difficutly.the quest button waits to update |
| + | `ui_quest_update_voiceover_timeout` | 30000 | How long to wait on a voiceover to finish before forcing ui quest update to stop waiting on it (time in ms) |
| + | `power_alert_frequency_in_seconds` | 15 | The number of seconds between a power alert on a laptop with a low battery. |
| + | `concise_chat` | 1 | Default use concise chat display in non-scenarios (sets "ConciseChat" in Personal.INI) |
| + | `start_countdown_seconds` | 10 | Seconds to count down at beginning of multiplayer game |
| + | `timonium_selection_circle_height` | 300 | Extra height for Timonium selection circles |
| + | `world_pre_show_pause` | 1500 | Millisecond delay for in-world help text |

### `ui/music`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `phrase_when_hero_arrives` | 0 |  |
| + | `phrase_when_hero_levels` | 0 |  |
| + | `phrase_when_hero_levels_other_player` | 0 |  |
| + | `phrase_when_hero_in_danger` | 1 |  |
| + | `phrase_when_others_summon_hero` | 1 |  |
| + | `phrase_on_attack_timing` | 2000 | Use a phrase if attacked after not being attacked for this long (1/15 second) |
| + | `min_track_time_before_ambient_interruption` | 60000 | Milliseconds a military track must play before an ambient can interrupt it |

### `ui/attackwarnings`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `raid_message_frames` | 900 | Minimum time in frames between raid warnings |
| + | `raid_warning_frames` | 300 | Minimum time in frames between raid warnings (sound) |
| + | `raid_warning_reset_frames` | -1 | Minimum non-raid quiet time before we think this is a separate raid. |
| + | `attack_message_frames` | 900 | Minimum time in frames between attack warnings |
| + | `attack_warning_frames` | 300 | Minimum time in frames between attack warnings (sound) |
| + | `attack_warning_reset_frames` | -1 | Minimum non-attack quiet time before we think this is a separate raid. |
| + | `warning_spacing_frames` | 100 | Minimum time between raid/attack warnings if both want to happen at once |

### `solodifficulty/tough`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `gather_handicap_tough` | 0 | gather handicap on tough |
| + | `gather_handicap_tough_ctw` | 0 | gather handicap on tough (CTW) |
| + | `general_handicap_tough` | 0 | General handicap on toughest (0-20), affecting build speeds and cooldowns. |
| + | `general_handicap_tough_ctw` | 0 | General handicap on toughest (0-20), affecting build speeds and cooldowns. |
| + | `cuotl_handicap_tough_ctw_qb` | 10% | gather handicap for the cuotl in tough ctw quick battles |

### `solodifficulty/moderate`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `cuotl_handicap_moderate_ctw_qb` | 0 | gather handicap for the cuotl in moderate ctw quick battles |

### `solodifficulty/tougher`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `gather_handicap_tougher` | 25% | gather handicap on tougher |
| + | `general_handicap_tougher` | 0 | General handicap on toughest (0-20), affecting build speeds and cooldowns. |

### `solodifficulty/toughest`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `gather_handicap_toughest` | 50% | gather handicap on toughest level (for an insanely hard opponent use about 50%, for more of a tough-ER than a tough-EST, use about 25%) |
| + | `general_handicap_toughest` | 0 | General handicap on toughest (0-20), affecting build speeds and cooldowns. For tough-ER use about 10, for tough-EST use about 16. (BR... 3/20/2006 discovered this value isn't actually used for solo games, it would only activate in multiplayer comp stomps, which kind of freaked me out so I'm setting this to 0 for consistency of AI behavior). |

### `match/match`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `online_tournament_default` | 1 | use tournament rules for matches |
| + | `online_no_capital_default` | 1 | use no-capital-rules for matches |

### `pings/minimap`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `minimap_default_num_pings` | 5 | default number of pings shown |
| + | `minimap_waypoint_ping_divisor` | 7 | divide minimap width by this number to get the pings end width |
| + | `minimap_ping_divisor` | 7 | divide minimap width by this number to get the pings end width |
| + | `minimap_big_ping_divisor` | 1 | divide minimap width by this number to get the pings starting width. |

### `music/music`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `music_combat_threshold` | 1600 | hit-to-damage rate that we consider combat music time |
| + | `music_combat_force` | 3500 | hit-to-damage rate that forces combat music |
| + | `music_ambient_threshold` | 800 | hit-to-damage rate that we consider ambient music time if we drop back down to this level |
| + | `music_ambient_force` | 100 | hit-to-damage rate that forces ambient music if we drop to this level |
| + | `music_winning_value` | 500 | Extra bonus factor in combat for if we're winning in score |
| + | `music_combat_sensitivity_duration` | 500 | Seconds of never hearing combat music before we lower the bar on combat music |
| − | `music_ambient_sensitivity_duration` | 500 | Seconds of never hearing ambient music before we lower the bar on combat music |
| + | `music_combat_threshold_sensitive` | 1250 | (Sensitive) hit-to-damage rate that we consider combat music time |
| + | `music_combat_force_sensitive` | 2000 | (Sensitive) hit-to-damage rate that forces combat music |
| + | `music_ambient_threshold_sensitive` | 1250 | (Sensitive) (hit-to-damage rate that we consider ambient music time if we drop back down to this level |
| + | `music_ambient_force_sensitive` | 400 | (Sensitive) hit-to-damage rate that forces ambient music if we drop to this level |

### `physics/physics`

| code | `name` | Values | Comment |
|:---:|---|---|---|
| + | `tumble_dist_divisor` | 10 | A number that we divide the tumble path dist by when we want to get a semi-accurate impulse. |
| + | `splash_angle_offset` | 192 | Offset towards the creater of the splash damage(in coords 192 = 1 Tile) |
| + | `physics_impulse_multiplier` | 1.0f | The amount that a weapon's physics impulse will be multiplied by (float) |
| + | `physics_default_impulse_strength` | 50 | The strength of the impulse that will be used to throw a ragdoll when it gets attacked. |
