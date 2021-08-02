# Fix Snow Weather
Credit to daniilS for the binary implementation this is based off of

In vanilla emerald (and Fire Red!) the WEATHER_SNOW is broken and will only emit a few snowflakes before stopping. Let's fix it!

For starters, open [src/field_weather_effect.c](../blob/master/src/field_weather_effect.c)

### Increase the number of snowflakes (optional)
If you want to have the snow be heavier (or lighter), find `Snow_InitVars` and edit the 
line `gWeatherPtr->targetSnowflakeSpriteCount = 16;` to a value of your choosing.

### Stop the snow from disappearing
Find the function `WaitSnowflakeSprite` and alter it as follows:
```diff
 static void WaitSnowflakeSprite(struct Sprite *sprite)
 {
-    if (gWeatherPtr->snowflakeTimer > 18)
+    if (++gWeatherPtr->snowflakeTimer > 18)
     {
         sprite->invisible = FALSE;
         sprite->callback = UpdateSnowflakeSprite;
         sprite->pos1.y = 250 - (gSpriteCoordOffsetY + sprite->centerToCornerVecY);
         sprite->tPosY = sprite->pos1.y * 128;
         gWeatherPtr->snowflakeTimer = 0;
     }
 }
```

### Fix snow spawning upon returning to the overworld
The snow is spawned based on gSpriteCoordOffsetX/Y, which is not updated until after the weather is initialized. To fix 
this, open [src/field_weather.c](../blob/master/src/field_weather.c).

1. First, add `#include "field_camera.h"` to the top of the file somewhere.
2. Next, add `UpdateCameraPanning();` before `sWeatherFuncs[gWeatherPtr->currWeather].initAll();` in `Task_WeatherInit`

### Hail on Overworld
Credit to Samu, To allow hail on when snow is present, go to **src/battle_util.c**
Search for `switch (GetCurrentWeather())` (Around line 2908) and input these code below the `WEATHER_DROUGHT` code.
```c
                case WEATHER_SNOW:
                    if (!(gBattleWeather & WEATHER_HAIL_ANY))
                    {
                        gBattleWeather = WEATHER_HAIL_ANY;
                        gBattleScripting.animArg1 = B_ANIM_HAIL_CONTINUES;
                        gBattleScripting.battler = battler;
                        effect++;
                    }
                    break;
```

The only thing left to do is to change the text string assigned to the snowy weather when it's casted in battle.

Head over to `src/battle_message.c` and search for `const u16 gWeatherStartsStringIds`.

Once there, make the following change:
```diff
const u16 gWeatherStartsStringIds[] =
{
    [WEATHER_NONE]               = STRINGID_ITISRAINING,
    [WEATHER_SUNNY_CLOUDS]       = STRINGID_ITISRAINING,
    [WEATHER_SUNNY]              = STRINGID_ITISRAINING,
    [WEATHER_RAIN]               = STRINGID_ITISRAINING,
-   [WEATHER_SNOW]               = STRINGID_ITISRAINING,
+   [WEATHER_SNOW]               = STRINGID_STARTEDHAIL,
    [WEATHER_RAIN_THUNDERSTORM]  = STRINGID_ITISRAINING,
    [WEATHER_FOG_HORIZONTAL]     = STRINGID_ITISRAINING,
    [WEATHER_VOLCANIC_ASH]       = STRINGID_ITISRAINING,
    [WEATHER_SANDSTORM]          = STRINGID_SANDSTORMISRAGING,
    [WEATHER_FOG_DIAGONAL]       = STRINGID_ITISRAINING,
    [WEATHER_UNDERWATER]         = STRINGID_ITISRAINING,
    [WEATHER_SHADE]              = STRINGID_ITISRAINING,
    [WEATHER_DROUGHT]            = STRINGID_SUNLIGHTSTRONG,
    [WEATHER_SNOW]               = STRINGID_STARTEDHAIL,
    [WEATHER_DOWNPOUR]           = STRINGID_ITISRAINING,
    [WEATHER_UNDERWATER_BUBBLES] = STRINGID_ITISRAINING,
    [WEATHER_ABNORMAL]           = STRINGID_ITISRAINING
};
```