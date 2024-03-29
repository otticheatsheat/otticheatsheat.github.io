# tiny regex intro and receipts

`Some people, when confronted with a problem, think "I know, I'll use regular expressions." Now they have two problems.`


## However ...
- `.` (a dot) stands for any character
- `*` matches the previous item zero or more times
- `+` matches the previous item one or more times
- `?` matches the previous item zero or one time
- `\` escapes special character. `\.` to match an actual period, `\\` for an actual backslash
- `\d` any digit [1,2,3,4,5,6,7,8,9,0]
- `\D` any character that is NOT a digit
- `\s` any whitespace character, `\S` anything not a whitespace character
- `\w` any word character (letter, word, underscore), `\W` anything not a word character
- `[...]` anything in the brackets, eg [ab3] matches anything that contains   a or b or 3
    - `[0-9]` matches any digit, just like \d, but `[1-3]` matches only 1 or 2 or 3
    - `[A-Z]` matches any capital letter
    - `[a-Z]` matches any letter
    - `[a-z]` lowercase letters only
- `^` beginning of a string (in which to search)
- `$` end of the string
- `|`  logical OR
- `()` the full string in brackets, used for grouping, we'll combine that later

and so forth ... can google for more, it's powerful.


## some receipts

### match a 0
- we want to match the item_id "0": `^0$`
   - `^` beginning of the string to match
   - `0` the item to look for
   - `$` end of string
   - just searching for "0" would give any id that contains a zero
   - `^0$|^64$|^128$` matches the item_id "0" or "64" or "128"


### match Gen1 or Gen2, E2 or E3, Voice or Mouth
- assume we want to search for Gen1 and Gen2 items in the collection column.
we can do: `^Gen[12]`
   - `^` marks the beginning of the row (in our case not that neccessary)
   - `Gen` for obvious reasons
   - `[12]` either one or two
   - could also just match for `[12]` or `1|2`

- we want to search for E2 and E3 items in rarity: `E[23]`

- we want to search for "Voice" or "Mouth" in the type column: `Voice|Mouth`


### Skin type or Skin color
- we want to search for "Skin type" and "Skin color": `^Skin (type|color)`
   - `^` beginning of the string, not so important in our case
   - `Skin ` the whitespace is important
   - `(type|color)` either "type" or "color"
   - `Skin type|Skin color` does the same
   - and because there aren't any other possibilities with Skin, `type|color` would do too


### Labels containing Green AND Orange
- `(Green.*Orange)|(Orange.*Green)`
   - `(Green.*Orange)` finds entries where Green is before Orange with any amount of characters (including comma and whitespace) inbetween
   - `|` logical OR
   - `(Orange.*Green)` same as above, but the other way
   - of course there are more "better" ways to do it ;-)


### Red - Rabbit - Lunar New Year
- we can use `?=`, a positive lookahead, to achieve the same,
   - `(?=.*rabbit)(?=.*red)(?=.*lunar new year)` matches red rabbits in the lunar
	 new year


### filter for numbers from 10 to 100
- i haven't figured how to have the filter do sensible number filtering, but by regex we could do:
   - `[1-9][0-9]0?` filters for any number from 10 to 100
      - `[1-9]` first digit is 1 to 9
      - `[0-9]` the second digit, anything from 0 to 9
         - by now we see numbers from ten to 99
      - `0?` third digit is a zero, matched zero or one time

### greater or equal 24
- `([2][4-9])|([3-9][0-9])|(\d{3})`
   - we divide into three logical group
      - `([2][4-9])` 24-29
	  - `([3-9][0-9])` 30-99
	  - `(\d{3})` any number of 3 digits length


### match different writing of the same thing, headband and head band
- `head ?band` or `(head)( ?)(band)` for easyer readability
   - whitespace followed by a `?` matches the whitespace zero or one time
