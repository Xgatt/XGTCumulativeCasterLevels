# XGTCumulativeCasterLevels

## Description
This is a WeiDU mod for the Infinity Engine Enhanced Edition games (BGEE, BG2EE, IWD:EE, EET) that allows casting level bonuses from various sources to accumulate. **Requires EEEx**

By default, Casting Level bonuses in the IE games do not stack -- the last-applied effect always wins. This mod fixes that. So if you had a class kit that had a -2 malus to Arcane Casting Level, wore a robe that gave +3 to Arcane Casting Level, and picked up a proficiency that gave +1 to Arcane Casting Level:

* **Your expectation:** -2 + 3 + 1 = +2
* **Realily in Vanilla IE games:** +1 if you picked up the proficiency last, +3 if you equipped the robe last, and -2 if you added the class kit last.
* **With this mod:** -2 + 3 + 1 = +2 (everything from all sources accumulates).

This also works with on-the-fly changes like Wild Mage casting level changes or temporary buffs like the Kappelmeister Bardsong from TheArtisanBG's Bardic Wonders mod.

## Compatibility and Installation Order
Install this **very late**. And I mean it's probably great if this was the absolute last mod in your install order. It can safely go after EET_End, it MUST go after Tactics Remix (which adds +1 CL to all enemy mages), Skills & Abilities (which adds a proficiency that grants +1 CL), A7-Multikits (which can generate new kits with CL changes), and NPC EE (which can respec a character). It patches every instance of the vanilla Casting Level Bonus effect (opcode 191) across all items, spells, and creature files with a new EEEx based effect (opcode 402, custom Lua fn), that accumulates the CL bonuses.

So to guarantee that no other op191 effects remain that can muck up your stats, install this very late. **Needs a new game for it to work**

## Technical Details (Under the Hood)
Vanilla Casting level bonus uses Opcode 191, which is hard coded at the engine level to be an override effect to the CASTINGLEVELBONUSMAGE or CASTINGLEVELBONUSCLERIC stat on a given Creature. Two sources of op191 cannot coexist -- the later one always overrides.

EEEx exposes a new Opcode, op402, which allows the invocation of a custom Lua function. All this mod does is to add that simple Lua function that looks at the configured bonus value (e.g. +2, -1, +3, etc) and ADD it to the existing CASTINGLEVELBONUSMAGE or CASTINGLEVELBONUSCLERIC stat instead of overriding it. 

At install time, it swaps out every instance of op191 with op402 invoking the aforementioned function. There should be zero impact to performance from this operation.

## Credits
Bubb, for basically handing me the solution after I'd been struggling for a while.
