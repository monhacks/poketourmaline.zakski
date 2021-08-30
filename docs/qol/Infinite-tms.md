# Infinite-TMS
This tutorial will make all TMs infinitely reusable, unholdable by Pokemon, unsellable to shops, and also removes the 
number from appearing in the Bag.

## Contents
1. [Don't consume TMs on use](#1-dont-consumes-tms-on-use)
2. [Treat TMs as HMs in the bag](#2-treat-tms-as-hms-in-the-bag)
3. [Don't allow TM selling and rebuying](#3-dont-allow-tm-selling-and-rebuying)
4. [Don't replenish PP when TMs are used](#4-dont-replenish-pp-when-tms-are-used)

## 1. Don't consume TMs on use
Normally, the game checks if the item used was a TM or an HM, and consumes the item if it was a TM. We can just simply 
remove this check.

Edit [src/party_menu.c](../blob/master/src/party_menu.c):
```diff
static void Task_LearnedMove(u8 taskId)
{
    struct Pokemon *mon = &gPlayerParty[gPartyMenu.slotId];
    s16 *move = &gPartyMenu.data1;
    u16 item = gSpecialVar_ItemId;

    if (move[1] == 0)
    {
        AdjustFriendship(mon, FRIENDSHIP_EVENT_LEARN_TMHM);
-       if (item < ITEM_HM01_CUT)
-           RemoveBagItem(item, 1);
    }
    GetMonNickname(mon, gStringVar1);
    StringCopy(gStringVar2, gMoveNames[move[0]]);
    StringExpandPlaceholders(gStringVar4, gText_PkmnLearnedMove3);
    DisplayPartyMenuMessage(gStringVar4, TRUE);
    ScheduleBgCopyTilemapToVram(2);
    gTasks[taskId].func = Task_DoLearnedMoveFanfareAfterText;
}
```
If you were to stop here, TMs can still be given to Pokemon and you can still see the number in the bag. The next step 
will address this.

## 2. Treat TMs as HMs in the bag
`struct Item` has a field `importance`. If this field has a nonzero value, then the item is treated like a key item, and 
cannot be held, tossed, or deposited in the PC, and it will not display an item count in the bag.

Edit [src/data/items.h](../blob/master/src/data/items.h):
```diff
    [ITEM_TM01_FOCUS_PUNCH] =
    {
        .name = _("TM01"),
        .itemId = ITEM_TM01_FOCUS_PUNCH,
        .price = 3000,
        .description = sTM01Desc,
+       .importance = 1,
        .pocket = POCKET_TM_HM,
        .type = 1,
        .fieldUseFunc = ItemUseOutOfBattle_TMHM,
        .secondaryId = 0,
    },
```
You will need to repeat the above for every TM.

But there is still an issue: TMs can still be sold. This will be addressed in the next step.

## 3. Don't allow TM selling and rebuying

If you were to see what determines if an item can be sold, you will find that any item with a price of 0 cannot be 
sold. So it's as simple as setting the TM prices to 0, right? Wrong. Setting the item price to 0 also means that the
buying price will also be 0!

In order to avoid this, we will need to modify the logic that checks for whether an item can be sold. All we need to do 
is check if the item ID is a TM. Edit the `Task_ItemContext_Sell` function of 
[src/item_menu.c](../blob/master/src/item_menu.c):

```diff
-    if (ItemId_GetPrice(gSpecialVar_ItemId) == 0)
+    if (ItemId_GetPrice(gSpecialVar_ItemId) == 0 || ItemId_GetPocket(gSpecialVar_ItemId) == POCKET_TM_HM)
     {
         CopyItemName(gSpecialVar_ItemId, gStringVar2);
         StringExpandPlaceholders(gStringVar4, gText_CantBuyKeyItem);
         DisplayItemMessage(taskId, 1, gStringVar4, BagMenu_InitListsMenu);
     }
```
The last issue to take care of is that TMs that the player already has can be bought again for no effect. This step is 
technically optional, but it is nice to have that bit of polish.

We will first need to add a new string. Edit [include/strings.h](../blob/master/include/strings.h):
```diff
 extern const u8 gText_InBagVar1[];
 extern const u8 gText_Var1AndYouWantedVar2[];
 extern const u8 gText_HereYouGoThankYou[];
 extern const u8 gText_NoMoreRoomForThis[];
+extern const u8 gText_YouAlreadyHaveThis[];
 extern const u8 gText_ThankYouIllSendItHome[];
 extern const u8 gText_ThanksIllSendItHome[];
 extern const u8 gText_SpaceForVar1Full[];
 extern const u8 gText_ThrowInPremierBall[];
```
Edit [src/strings.c](../blob/master/src/strings.c):
```diff
 const u8 gText_ThanksIllSendItHome[] = _("Thanks!\nI'll send it to your PC at home.");
 const u8 gText_YouDontHaveMoney[] = _("You don't have enough money.{PAUSE_UNTIL_PRESS}");
 const u8 gText_NoMoreRoomForThis[] = _("You have no more room for this\nitem.{PAUSE_UNTIL_PRESS}");
+const u8 gText_YouAlreadyHaveThis[] = _("You already have this item.{PAUSE_UNTIL_PRESS}");
 const u8 gText_SpaceForVar1Full[] = _("The space for {STR_VAR_1} is full.{PAUSE_UNTIL_PRESS}");
 const u8 gText_AnythingElseICanHelp[] = _("Is there anything else I can help\nyou with?");
 const u8 gText_CanIHelpWithAnythingElse[] = _("Can I help you with anything else?");
```
Now to add the code that prevents the rebuying. Edit the `Task_BuyMenu` function of [src/shop.c](../blob/master/src/shop.c):
```diff
             if (!IsEnoughMoney(&gSaveBlock1Ptr->money, gShopDataPtr->totalCost))
             {
                 BuyMenuDisplayMessage(taskId, gText_YouDontHaveMoney, BuyMenuReturnToItemList);
             }
+            else if (ItemId_GetPocket(itemId) == POCKET_TM_HM && CheckBagHasItem(itemId, 1))
+            {
+                BuyMenuDisplayMessage(taskId, gText_YouAlreadyHaveThis, BuyMenuReturnToItemList);
+            }
             else
             {
```

This prevents rebuying from shops, but there's still the Mauville Game Corner. Let's go to 
[data/maps/MauvilleCity_GameCorner/scripts.inc](../blob/master/data/maps/MauvilleCity_GameCorner/scripts.inc).

First, let's replace the `MauvilleCity_GameCorner_EventScript_NoRoomForTM` function with 
`MauvilleCity_GameCorner_EventScript_YouAlreadyHaveThis`. The bag will no longer be full, so let's instead use the same
string we used for the shops.

```diff
        goto MauvilleCity_GameCorner_EventScript_ReturnToChooseTMPrize
        end
 
-MauvilleCity_GameCorner_EventScript_NoRoomForTM::
-       call Common_EventScript_BagIsFull
+MauvilleCity_GameCorner_EventScript_YouAlreadyHaveThis::
+       msgbox gText_YouAlreadyHaveThis, MSGBOX_DEFAULT
        goto MauvilleCity_GameCorner_EventScript_ReturnToChooseTMPrize
        end
```

Next, we need to replace all of the checks to see if there is room with checks for if you have the item. These currently 
use `checkitemspace`. We can replace them with `checkitem` and exiting the function if the result is true.
```diff
        compare VAR_TEMP_2, TM13_COINS
        goto_if_lt MauvilleCity_GameCorner_EventScript_NotEnoughCoinsForTM
-       checkitemspace ITEM_TM13, 1
-       compare VAR_RESULT, FALSE
-       goto_if_eq MauvilleCity_GameCorner_EventScript_NoRoomForTM
+       checkitem ITEM_TM13, 1
+       compare VAR_RESULT, TRUE
+       goto_if_eq MauvilleCity_GameCorner_EventScript_YouAlreadyHaveThis
        removecoins TM13_COINS
        additem ITEM_TM13
        updatecoinsbox 1, 1
```


Repeat for each TM.

We are almost done, but there is one final issue to deal with: TMs will replenish PP. It is time to address that.

## 4. Don't replenish PP when TMs are used
This step is also technically optional (and accurate to Gen 6+ behavior!), but right now, if you overwrite a TM move 
with another TM and then reteach the old TM, the move will get all its PP back! This is an exploit that can be used to 
retain infinite PP on that move. Gen 5's solution was to assign the lower of the new PP or the old PP, meaning that TMs 
cannot be used to gain PP.

To replicate this, open up `Task_PartyMenuReplaceMove` of [src/party_menu.c](../blob/master/src/party_menu.c):
```diff
 static void Task_PartyMenuReplaceMove(u8 taskId)
 {
     struct Pokemon *mon;
     u16 move;
+    u8 oldPP;
 
     if (IsPartyMenuTextPrinterActive() != TRUE)
     {
         mon = &gPlayerParty[gPartyMenu.slotId];
         RemoveMonPPBonus(mon, GetMoveSlotToReplace());
+        oldPP = GetMonData(mon, MON_DATA_PP1 + GetMoveSlotToReplace(), NULL);
         move = gPartyMenu.data1;
         SetMonMoveSlot(mon, move, GetMoveSlotToReplace());
+        if (GetMonData(mon, MON_DATA_PP1 + GetMoveSlotToReplace(), NULL) > oldPP)
+            SetMonData(mon, MON_DATA_PP1 + GetMoveSlotToReplace(), &oldPP);
         Task_LearnedMove(taskId);
     }
 }
```
We remember the PP before replacing the move, and if it's lower than the new PP, we put the old PP back in the move slot.

And that's it! TMs are now reusable just like in Gen 5!

![](https://i.imgur.com/KwzzUSm.png)
![](https://i.imgur.com/tFDpY8B.png)
![](https://i.imgur.com/zA0lj2E.png)

Alternatively, see [this commit](https://github.com/LOuroboros/pokeemerald/commit/6f69765770a52c1a7d6608a117112b78a2afcc22) 
for an implementation by Karathan that turns TMs and HMs into a bitfield, freeing up some saveblock space. This saves 
space, but is slightly more complex.
