# tiny regex intro an receipts

`Some people, when confronted with a problem, think "I know, I'll use regular expressions." Now they have two problems.`

However ...
- `.` (a dot) stands for any character
- `*` matches the previous item zero or more times
- `+` mateches the previous item one or more times
- `?` matches the previous item zero or one time
- `\` escapes special character. `\.` to match an atual period, `\\` for an actual backslash

- `\d` any digit [1,2,3,4,5,6,7,8,9,0]
- `D` any character that is NOT a digit
- `\s` any whitespace character
- `\w` any word character (letter, word, underscore)
- `[...]` anything in the bractes, eg [ab3] matches anything that contains an a or a b or a 3
    -`[0-9]` matches any digit, just like \d, but `[1-3]` matches only 1 or 2 or 3
    - `[A-Z]` matches any capital letter
    - `[a-Z]` matches any letter
- `^` beginning of a row
- `$` end of row
- `|` is or
- `()` the full string in brackets, used for grouping, we'll combine that later

and so forth ... can google for more, it's powerful.


## some receipts

we want to match the item_id "0": `^0$`
- `^` beginning of the word
- `0` the string to look for
- `$` end of word
- just searching for "0" would give any id that contains a zero
- `^0$|^64$|^128$` matches the item_id "0", "64" and "128"

assume we want to search for gen1 and gen2 items in the collection column  
many roads lead to rome, but we can do: `^Gen[12]`
- `^` marks the beginning of the row (actually in this case it's not neccessary
- `Gen` for obvious reasons
- `[12] either one or two

we want to search for E2 and E3 items in rarity: `E[23]`

we want to search for "Voice" or "Mouth" in the type column: `Voice|Mouth`

we want to search for "Skin type" and "Skin color": `^Skin (type|color)`
- `^` beginning of the string, not so important in our case
- `Skin ` the whitespace is important
- `(type|color)` either "type" or "color"
- `Skin type|Skin color` does the same

we want to search for the labels "Green" AND "Orange": `(Green.*Orange)|(Orange.*Green)`
- `(Green.*Orange)` finds entries there Green is before orange with any amount of characters (including comma and whitespace) inbetween
- `|` logical OR
- `(Orange.*Green) same as above, but the other way
- of course there are more "better" ways to do it ;-)


i havn't figured how to really have the filter do sensible number filtering, but by regex we could do:
- `[1-9][0-9]0?` filters for any number from 10 to 100
   - `[1-9]` first digit is 1 to 9
   - `[0-9]` the second digit, anything from 0 to 9
      - by now we see numbers from ten to 99
   - `0?` third digit is a zero, matched zero or one time


last we can use this to filter for itemnames with inconsistend spelling.
let's assume we are looking for a "headband" ... easy, but the we realise that some items are "head band"
- `head ?band` or `(head)( ?)(band)` for easyer readability
