In Pokémon Let's Go Pikachu and Eevee, buying Poké Balls in bulk will give the player one Premier Ball for every ten balls purchased (e.g. buying 30 balls at once gives you three free Premier Balls). This also applies to all types of ball, instead of just regular Poké Balls. This tutorial will add this functionality to Emerald.

First, edit [src/shop.c](../blob/master/src/shop.c):
```diff
static void Task_ReturnToItemListAfterItemPurchase(u8 taskId)
{
    s16 *data = gTasks[taskId].data;

    if (gMain.newKeys & (A_BUTTON | B_BUTTON))
    {
        PlaySE(SE_SELECT);
-       if (tItemId == ITEM_POKE_BALL && tItemCount > 9 && AddBagItem(ITEM_PREMIER_BALL, 1) == TRUE)
+       if ((ItemId_GetPocket(tItemId) == POCKET_POKE_BALLS) && tItemCount > 9 && AddBagItem(ITEM_PREMIER_BALL, tItemCount / 10) == TRUE)
        {
-           BuyMenuDisplayMessage(taskId, gText_ThrowInPremierBall, BuyMenuReturnToItemList);
+           if (tItemCount > 19)
+           {
+               BuyMenuDisplayMessage(taskId, gText_ThrowInPremierBalls, BuyMenuReturnToItemList);
+           }
+           else
+           {
+               BuyMenuDisplayMessage(taskId, gText_ThrowInPremierBall, BuyMenuReturnToItemList);
+           }
        }
        else if((ItemId_GetPocket(tItemId) == POCKET_TM_HM))
        {
            RedrawListMenu(tListTaskId);
            BuyMenuReturnToItemList(taskId);
        }
        else
        {
            BuyMenuReturnToItemList(taskId);
        }
    }
}
```
With that done, you'll need a new string so that the clerk says "Premier Balls" instead of "Premier Ball" when giving the player more than one. Add a new string to [src/strings.c](../blob/master/src/strings.c), like so:

`const u8 gText_ThrowInPremierBalls[] = _("I'll throw in some Premier Balls, too.{PAUSE_UNTIL_PRESS}");`

Finally, don't forget to declare it in [include/strings.h](../blob/master/include/strings.h):

`extern const u8 gText_ThrowInPremierBalls[];`