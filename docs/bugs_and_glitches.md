
# Bugs and Glitches

These are known bugs and glitches in the original Pokémon Emerald game: code that clearly does not work as intended, or that only works in limited circumstances but has the possibility to fail or crash. Defining the `BUGFIX` and `UBFIX` preprocessor variables will fix some of these automatically. `UBFIX` will already be defined for MODERN builds.

Fixes are written in the `diff` format. If you've used Git before, this should look familiar:

```diff
 this is some code
-delete red - lines
+add green + lines
```

## Contents
- [Scrolling through items in the bag causes the image to flicker](/docs/bugs/Items Flicker When Scrolling Through Bag.md)
- [Snow Doesn't Fall Properly](/docs/bugs/Snow Doesn't Fall.md)
- [FIX RNG](/docs/bugs/Fix RNG.md)