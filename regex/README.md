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