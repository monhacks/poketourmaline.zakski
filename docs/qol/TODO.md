# Planned Changes
- Difficulty
    - **_Basic_**: These tutorials are simple changes that those with basic programming knowledge should be able to do.
    - **_Intermediate_**: These tutorials require more work and programming knowledge.
    - **_Advanced_**: These tutorials overhaul systems, and may require advanced knowledge.

## Basic
- **[Change Time-Based Evolution Times](https://github.com/pret/pokeemerald/wiki/Change-Time-Based-Evolution-Times)**
- **[Trainer Backsprite Editing](https://github.com/pret/pokeemerald/wiki/Trainer-Backsprite-Editing)**
- **[Faster HP Drain](https://github.com/pret/pokeemerald/wiki/Faster-HP-Drain)**
- **[FRLG/DP+ White Out Money Calculation](https://github.com/pret/pokeemerald/wiki/Better-White-Out-Money-Calculation)**
- **[Allow running indoors](https://github.com/pret/pokeemerald/wiki/Allow-running-indoors)**
- **[Increase money limit](https://github.com/pret/pokeemerald/wiki/Increase-money-limit)**
- **[Infinite TM usage](https://github.com/pret/pokeemerald/wiki/Infinite-TM-usage)**
- **[Trainer Class-Based Poké Balls](https://github.com/pret/pokeemerald/wiki/Trainer-Class-Based-Poké-Balls)**
- **[Learn moves upon evolution](https://github.com/pret/pokeemerald/wiki/Learn-moves-upon-evolution)**
- **[Prompt for reusing Repels](https://github.com/pret/pokeemerald/wiki/Prompt-for-reusing-Repels)**
- **[Pokecenters disregard eggs](https://github.com/pret/pokeemerald/wiki/Pokecenters-Disregard-Eggs)**
- **[Repeated Medicine Use](https://github.com/pret/pokeemerald/wiki/Repeated-Field-Medicine-Use)**
- **[Chain Fishing](https://github.com/pret/pokeemerald/wiki/Chain-Fishing)**
- **[Update Sitrus Berry's effect to Gen 4 standard](Update-Sitrus-Berry's-effect-to-Gen-4-standard)**
- **[Shop Items by Badge Count](https://github.com/pret/pokeemerald/wiki/Shop-Items-By-Badge-Count)**
- **[Shuckle makes Berry Juice](https://github.com/pret/pokeemerald/wiki/Shuckle-makes-Berry-Juice)**
- **[Remove the functionally redundant move grammar tables](https://github.com/pret/pokeemerald/wiki/Remove-the-functionally-redundant-move-grammar-tables)**
- **[Disable Bag use In Battle](https://github.com/pret/pokeemerald/wiki/Disable-Bag-Use-In-Battle)**
- **[Disable Catching Pokemon](https://github.com/pret/pokeemerald/wiki/Disable-Catching-Pokemon)**
- **[Dynamic Trade Names](https://github.com/pret/pokeemerald/wiki/Dynamic-Trade-Names)**
- **[Enable trade with FRLG without beating the game](https://github.com/pret/pokeemerald/wiki/Enable-trade-with-FRLG-without-beating-the-game)**
- **[Omnidirectional Jump Behavior](https://github.com/pret/pokeemerald/wiki/Omnidirectional-Jump)**
- **[Uniquely Shuffle Arrays](Uniquely-Shuffle-Array)**

### Intermediate
- **[Implementing the “textcolor” script command from FRLG and give object events their own text colour](https://github.com/pret/pokeemerald/wiki/Implementing-the-“textcolor”-script-command-from-FRLG-and-give-object-events-their-own-text-colour)**
- **[Implementing ipatix's High Quality Audio Mixer](https://github.com/pret/pokeemerald/wiki/Implementing-ipatix's-High-Quality-Audio-Mixer)**
### Advanced
- **[Triple-layer metatiles](https://github.com/pret/pokeemerald/wiki/Triple-layer-metatiles)**
- **[Stair Warps](https://github.com/pret/pokeemerald/wiki/Stair-Warps)**

## Branches

### [battle_engine](/https://github.com/rh-hideout/pokeemerald-expansion/tree/battle_engine)
An overhaul and upgrade of pokeemerald's battle engine with newer gen features and mechanics, such as:

    Physical/Special Split
    New and Updated Move Effects, Abilities and Item Effects
    Fairy Type
    Mega Evolution
    Double Wild Battles
    Smarter AI
    Ability pop-up messages
    A wealth of configuration options
    Mid-battle trainer messages
    Custom multi-battles
    And more!

For more information see [pokeemerald Expansion's wiki](https://github.com/rh-hideout/pokeemerald-expansion/wiki/About-the-Project).

#### Maintainers: 
ROM Hacking Hideout

### [pokemon_expansion](https://github.com/rh-hideout/pokeemerald-expansion/tree/pokemon_expansion)
Pokemon from future games backported to pokeemerald. Features include:

    Most Pokemon have two frames of animation
    6 colors in icon palettes instead of 3
    Base stats and evolution data, including new evolution methods
    Hidden abilities
    Updated Pokedex and entries
    Cries for Pokemon
    Gen 7 movesets
    TM/HM and Move Tutor compatibility
    Some Forms
    Configuration options

For more information see [pokeemerald Expansion's wiki](https://github.com/rh-hideout/pokeemerald-expansion/wiki/About-the-Project).

#### Maintainers:
ROM Hacking Hideout

### [item_expansion](https://github.com/rh-hideout/pokeemerald-expansion/tree/item_expansion)
Items from future games backported to pokeemerald. Features include:

    Most items from future games
    All new berries
    All new Poke Balls, including throw animations, descriptions, pics, catch rates, etc.
    Battle item data (their effect is implemented in battle_engine)
    Evolution stones and Gen 7 potions
    Configuration options

For more information see [pokeemerald Expansion's wiki](https://github.com/rh-hideout/pokeemerald-expansion/wiki/About-the-Project).

#### Maintainers:
ROM Hacking Hideout

### [BetterBag](https://github.com/AsparagusEduardo/pokeemerald/tree/BetterBag)
Adds the following new pockets to the bag:

    Medicine (HP, PP and status recovery items)
    Power-Up (Vitamins, Rare Candy and evolution items)
    Battle Items (X items, Pokédoll/FluffyTail and hold items with battle effects)

For more info see [this Pokecommunity post](https://www.pokecommunity.com/showthread.php?t=424360).

#### Maintainers: 
AsparagusEduardo

### [overworld-expansion](https://github.com/ghoulslash/pokeemerald/tree/overworld-expansion)
Increases the number of maximum object events in the game from 255 to 65535. This is done by converting the ID from a u8 
to a u16.

#### Maintainers: 
ghoulslash

### [dexnav](https://github.com/ghoulslash/pokeemerald/tree/dexnav)
Adds a simplified DexNav, complete with Search and Detector modes, creeping up to wild Pokemon, and chaining.

For more info, see [this Pokecommunity post](https://www.pokecommunity.com/showthread.php?t=440571).

#### Maintainers:
ghoulslash

### [dynamic-ow-pals](https://github.com/ExpoSeed/pokeemerald/tree/dynamic-ow-pals)
Adds a system to load and unload overworld palettes based on what is on screen, instead of keeping all overworld 
palettes loaded at once. This enables you to be much more flexible about overworld palettes.

#### Maintainers: 
ExpoSeed

## To do
- transfer the rest of the simple modifications from the [Pokecommunity thread](https://www.pokecommunity.com/showthread.php?p=9986048)