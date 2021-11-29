# Regex

Great for debugging and whatnot: https://regexr.com/ 

## Notations

`/re/` --> RegEx goes inside the two slashes

`/re/g` --> `g` here means `global` mode, so look for the RegEx throughout the entire document/string.

Other modes:
- `i` --> case insensitive
- `m` --> multiline. By default, RegEx can **only** span **one** line. 

## Characters
### Literal characters
`/car` matches the first three characters of "carnival". 
- Note: case-sensitive by default. 

**Standard (non-global) matching**: `/zz/` matches the first two z's of 'pizzazz'. However, with **global matching**, `/zz/g` matches both sets of z's in pizzazz.

The way that RegEx works is it reads left to right, kind of brute-force-y. Does the current character match? If yes, does the next one? So on and so forth until the entire RegEx pattern is matched... else, move on to the character *after* the first character in the sub-string we were just looking at. 

### Wildcard metacharacters
`.` represents *any* character **except newline**. 

Note: this only matches **once**, and is not intended to be a "repeats one or more time". Any metacharacter given is treated as appearing once in the pattern unless told otherwise. 

### Escaping metacharacter
The `/` character says that the symbol to the right should be treated as a literal character. 

For example, if we wanted to find a period (`.`), we couldn't just put a period in our pattern. That would be treated as a wildcard! 
- Instead, we'd need to put: `/myregex\.` --> this would find the first match of "myregex."
- NOTE: only metacharacters should be escaped; there's new meaning with a backslash + a literal character (more on that later)

### Other special characters
- Spaces are represented in the regex by a literal space. 
- Tabs: `\t`
- Line returns: `\r`, `\n`, `\r\n`

## Character sets
### Defining a character set
Use square brackets for this definition. Matches **one** of the characters within the character set. 

Example: we want to match both **grey** and **gray**. Regex: `/gr[ae]y/g`
- But, `/gr[ea]t/` does NOT match "great" (not two vowel matches, just ONE). 

We can have any literal characters defined in the character set plus the literal definition of metacharacters, so a period is just `[.]`

### Character ranges
Denoted by a `-` inside of a character set: note that the **dash becomes a metacharacter inside of a character set**, but **outside of a character set, it is a literal dash**. 
- Character set for all numbers: `[0-9]`
- All uppercase and lowercase letters: `[A-Za-z]`

### Negative character sets
Denoted by `^`. Specifies characters that can NOT appear in the pattern... but can be any *other* character. For example, `/[^aeiou]/g` matches any non-vowel character in the text. 

Another example: `/see[^mn]/` would match `seek` and `sees`, but would not match `seem` or `seen`. **Note**: it would also not match `see` (because there needs to be another character after the second `e` that is not `m` or `n`). It *could* match `see ` (see with a space after it) or `see.` --> there just needs to be another character after the second E, as long as it isn't M or N!

### Metacharacters inside character sets
Characters that need to be escaped inside of a character set: `]`, `-`, `^`, and `\`. 

For example, what if I want to find all instances of `var[X]` or `var(X)`, where X is any digit 0-9?
- Solution: `/var[[(][0-9][\])]/g`

### Shorthand character sets
These are shorthand for character sets: 
- `\d` --> digit, equivalent: `[0-9]`
- `\w` --> word character, equivalent: `[a-zA-Z0-9_]`
- `\s` --> whitespace, equivalent: `[ \t\r\n]`

The negation of those sets are just the same, but *capital* letters, so for instance to match any character that is not a digit, use `\D`.

**Caution**: these two shorthands are NOT the same: `/[^\d\s]/` and `/[\D\S]/`
- The first is looking for a character that is not a digit or a space character (restrictive)
    - Is this character a digit? No? Well, if it's a space character, include it!
- The second is looking for EITHER NOT digit OR NOT space character (this will match a lot of things!)
    - Is this character a digit? No? Well, if it's not a space character, include it! 

## Repetition
### Repetition metacharacters
- `*` = Preceding item, zero or more times
- `+` = Preceding item, one or more times
- `?` = Preceding item, zero or one time 

For example:
- `/apples*/g` matches "apple", "apples", and "applessssss"
- `/apples+/g` matches "apples" and "applessss", but not "apple"
- `/apples?/g` does NOT match "applessss", but does match "apple" and "apples"

### Quantified Repetition
For when we want to match something exactly N times. Denoted by curly braces { } and usually in the form of `{min, max(optional)}`

Three different syntaxes to use:
- `\d{4,8}` matches four to eight digits
- `\d{4}` matches *exactly* four digits
- `\d{4,}` matches four or more digits (max arg is infinity here)

### Greedy Expressions
Standard repetition quantifiers are greedy. They will try to find a match for the longest possible string that could still be valid from the pattern. 

What actually happens: the std repetition quantifiers match *as much as is valid* and then "give back" as little as possible to the next portion of the pattern. 

Example: `/.*[0-9]+/` with `Page 266` --> the `[0-9]+` pattern only matches the last '6' character. Everything else is matched by the `.` wildcard.

### Lazy Expressions
Instructs a quantifier to use a "lazy strategy" for making choices. **Opposite strategy for matching as greedy**: tells the preceding quantifier to match *as little as possible* before giving control to the next part of the pattern. 

Example: `/.*?[0-9]+/` with `Page 266` --> the `.*` has a question mark after it, so it's lazy. Once we reach the character '2', the `.` metacharacter gives up control and the rest of the string ('266') is matched by the `[0-9]+` pattern. 

## Grouping and Alternation
### Grouping metacharacters
Use parentheses - this allows us to apply a wildcard to a group of characters (note, this is NOT a character set). Examples: 
- `/(abc)+/g` matches "abc" and "abcabcabc"
- `/(in)?dependent/` matches "independent" and "dependent"

Go to "replace" in regexr... groups are given as variables `$1`, `$2`, etc. Play around with the find+replace functionality! Seems useful. 

### Alternation metacharacter
Use the pipe: `|` for alternation. Meaning: **match previous or next expression**. Kind of like OR: matches either expression on left OR on right. 

Note: left choice is given precedence. 

Examples:
- `/apple|orange/` matches "apple" and "orange"
- `/apple(juice|sauce)/` matches "applejuice" and "applesauce"