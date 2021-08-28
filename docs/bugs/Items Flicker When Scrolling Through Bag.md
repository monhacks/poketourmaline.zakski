# Scrolling through items in the bag causes the image to flicker

**Fix:** Add the following function to [src/item_menu_icons.c](https://github.com/pret/pokeemerald/blob/master/src/item_menu_icons.c):
```diff
+void HideBagItemIconSprite(u8 id)
+{
+	u8 *spriteId = &gBagMenu->spriteIds[10];
+	if (spriteIds[id] != 0xFF)
+	{
+		gSprites[spriteIds[id]].invisible = TRUE;
+	}
+}

```

and its corresponding declaration in [include/item_menu_icons.h](https://github.com/pret/pokeemerald/blob/master/include/item_menu_icons.h):

```diff
+void HideBagItemIconSprite(u8 id);

```

Then edit `BagMenu_MoveCursorCallback` in [src/item_menu.c](https://github.com/pret/pokeemerald/blob/master/src/item_menu.c):

```diff
	...
{
-	RemoveBagItemIconSprite(1 ^ gBagMenu->itemIconSlot);
+	HideBagItemIconSprite(gBagMenu->itemIconSlot ^ 1);
+	RemoveBagItemIconSprite(gBagMenu->itemIconSlot);
	if (itemIndex != LIST_CANCEL)
	...
```
