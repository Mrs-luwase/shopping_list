# Shopping List+

My Week 2 Mobile Programming assignment (Session 3). It is a shopping list app built with Flutter and Dart. I started from the finished Shopping List app (guide steps 1 to 8) and added upgrades one part at a time, with one commit per part.

I am a first-year BSc IT student and this is my first Flutter app, so I wrote this README in my own words to show what I understand.

## Status

- Parts 1 to 4 are done and working.
- Parts 5 (filters) and 6 (item details screen) are **not finished yet**. I ran out of time because I lost a lot of it to a blank-page bug (see "Problems I met").

## Features

The base app (from the guide) already lets me add items, tap an item to mark it as bought (crossed out), delete an item, and remove all bought items with the top bar button. The title shows how many items are left.

On top of that I added:

1. **Empty state:** when the list has no items, the page shows "Your list is empty" in the middle instead of a blank page. When I add an item, the list comes back.
2. **Quantities:** each item has a quantity that starts at 1. Each row shows a `-` button, the number, a `+` button and the bin. `+` adds one. `-` removes one but never goes below 1.
3. **No duplicates, just more:** if I type an item that is already on the list (any capitals or extra spaces, like ` mILK `), no new row is added. The quantity goes up by 1 and a message says, for example, "Milk is already on the list: now x2".
4. **Undo:** deleting an item shows "Bread removed" with an **Undo** button. Undo puts the item back in the same position, with its quantity and bought status.

## Screenshots

**Part 2: quantities** (Milk has quantity 4)

![Quantities](screenshots/part2-quantities.png)

**Part 4: Undo message after deleting Bread**

![Undo message](screenshots/part4-undo-message.png)

**Part 4: Bread back in the same place after tapping Undo**

![After undo](screenshots/part4-undo-restored.png)

## How to run

```
flutter pub get
flutter run -d chrome
```

## Problems I met

- **Blank white page after adding quantity.** My code did not compile because I had pasted only part of the file and many closing brackets `)` `]` `}` were missing. Every widget that opens must close, in reverse order. After I fixed the end of the file and ran the app again (Stop then Run), it worked.
- **Undo button not showing.** My app was still using the old `removeItem` that only showed a message. After I replaced it and hot reloaded, the Undo button appeared.
- **The list goes back to Milk, Bread, Rice when I refresh Chrome.** Refreshing restarts the app, and the list only lives in memory.

## Think

**1. In part 5, why does deleting by index remove the wrong item when a filter is on? How did you fix it?**

I have not built Part 5 yet, but I understand the problem from the instructions. With a filter on, the screen shows a shorter list (`shown`) made from `items`. The `index` that the row gives me is a position in `shown`, not in `items`. So `items.removeAt(index)` removes whatever sits at that position in the full list, which can be a different item, and there is no error message. The fix is to remove the item itself with `items.remove(item)`, which finds the correct one whatever the filter is.

**2. Why did you have to remove `final` from `name`? Why is `final` still a good choice for most fields?**

I have not built Part 6 yet. `final` means a value is set once and can never change. On the details screen the user can rename an item, so `name` must be able to change and `final` has to go. For most fields `final` is still good because it protects my data from being changed by accident somewhere else in the code, so there are fewer bugs, and the code is easier to read because I know that value stays the same.

**3. Why did you need a hot restart after adding quantity to `Item`, when a save was enough for everything else?**

Hot reload keeps the data the app already has in memory. My `items` list was already created from the old `Item` class, so changing the class did not rebuild it. Hot restart starts the app from zero, so the list is built again with the new `Item` that has `quantity`. When I change what kind of data I store, I need a hot restart, but when I only change how things look, a save is enough.

**4. The details screen changed the item, but the list only showed it after `setState` in `.then`. Why? What does this tell you about how Flutter decides when to redraw?**

I have not built Part 6 yet, but my understanding is that Flutter does not watch my data. Changing a value, like `item.name`, does not redraw the screen by itself. Flutter only calls `build` again when I call `setState`. So after coming back from the details screen, the list still shows the old name until `setState` tells Flutter to redraw. This is the same lesson as step 5 of the guide: the data can change while the screen stays old.

## What I learned

- Everything on screen is a widget, and screens are trees of widgets inside each other.
- Data that changes lives in the `State`, and `setState` tells Flutter to redraw.
- A rule that refuses (like "quantity never below 1") is the same idea as the early-return checks from my labs.
- I should save my data in the model class (`Item`), not in the screen.
- I should test the app after every small change and commit each time it works.
