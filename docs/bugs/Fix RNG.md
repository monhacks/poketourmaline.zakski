# Fix Broken Emerald RNG
Instead of choosing a different seed each time the game boots up (and by extension, a different sequence of 
pseudo-random numbers), Emerald always sets its seed as 0. This means that every time the game starts, the same numbers 
are always output in the same order.

change the following in config.h
```diff
-//#define BUGFIX
+#define BUGFIX
```