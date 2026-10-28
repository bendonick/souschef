# Decisions

## 1. Every purchase is its own row

The pantry has two tables: one row per food, and one row per time I bought it, with the quantity, the date and the price. When I use something, the oldest purchase goes first.

The first version had one table with a quantity and a date. I replaced it because "use soon" has to mean the oldest carton of milk, not milk in general, and because spending and waste both need to know about the individual purchase.

The cost is that every pantry read joins two tables, and the app has to understand both.

## 2. The scanner proposes, I confirm

Scanning a receipt returns a list of lines for me to check. Nothing is written until I press confirm. Only then does the pantry update and the shopping list get ticked off, in one step.

The simpler option was to write straight after the scan and let me fix mistakes afterwards. But a model misreads receipts, and a misread that lands in the pantry first has to be found and undone. On a review screen it gets fixed before it exists. The same pattern now covers recipe photos.

The cost is one extra screen on every scan.

## 3. No guessing when two names are the same food

When I add "chicken breasts" and later "Chicken-Breast", they should be one product. The first matcher guessed with a similarity score, and it sometimes merged two foods that were not the same. The quantity and dates on something I trusted changed without any sign.

Now the matcher cleans the name and looks it up in a fixed list of known foods. If it does not find one, it only merges on an exact name match. Recipe suggestions, where a near miss is only a suggestion, still use a looser match.

The cost is that some foods are not in the list yet and end up as their own product. A duplicate is visible, though. A wrong merge is not.

## 4. "You can make this" is never saved

When Cook shows that I have three of a recipe's four ingredients, that number is worked out fresh every time from what is in the pantry right now.

I could have stored a score per recipe and updated it when the pantry changed. But a stale "ready tonight" is the worst thing that screen can show, and at one user recomputing is cheap.

The cost comes later: with many users this would need a cache, and I would rather add that when it hurts than guess at it now.

## 5. Nutrition comes from a local copy, not a live call

The macro numbers come from the USDA food database, but the app never calls USDA while I use it. A script maps my known foods to USDA entries once and stores the values in my own database.

USDA is rate limited and sometimes down, and the pantry summary should work either way. When I correct an estimate for a food, the correction is attached to the food, not to the pantry row, so it survives when I eat the item and buy it again.

The cost is a copy I have to rebuild when USDA changes, and a coverage gap the app shows rather than hides: "7 of 20 items counted".
