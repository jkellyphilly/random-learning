# Bash scripting
## Using Bash
### What's bash?
It's a shell ("bourne again shell"), a program that allows us to interact with many things on our computer. Works best on Linux systems, though not required to be on Linux. 

Type `bash` to enter an interactive bash shell. `bash --version` gives the current bash version. 

### Pipes and redirections
Pipes (`|`): Result/output of one command/process gets immediately fed into the next. 
- Example: `ls | wc -l` --> takes the result of list and does a word-count. 

#### Output redirections: `>` and `>>`. Puts the contents of one command into a file. 
- Example: `ls > list.txt`
- Example: `ls >> list.txt` (add the result of `ls` on to the end of `list.txt` file)

**Note**: we have multiple options for redirections! Can use `1>` (for standard output) or `2>` (for error output)

#### Input redirections and here documents
- `<` : input redirection
    - Example: `cat < list.txt` --> send the contents of `list.txt` to `cat`
- `<<` : here document (read up on that later, no idea what this means)
    - Example: `cat << EndOfText`
        - Reads up until the delimiter (in this case `EndOfText`) and passes in to `cat`... ?

### Bash builtins and other commands
Most commands we run in shell aren't part of bash, but rather other programs on the computer that we run *in* bash. However, there are some built-in bash commands. 
- `echo` --> "echoes" back text/output to the console, **includes a newline char at the end!**
- `printf` --> prints back to console, **does not include a newline char at the end of the line**

We can check to see if a command is builtin or a separate program: `command -V <name-of-command>`. 

**Note**: if there exists a command (such as `echo`) that is *both* a built-in AND a program, the built-in is always given precedence for running. 
- What if we want to use the command version instead?
- `enable -n echo` --> now we use the command `echo` instead of the builtin `echo`

### Bash expansions and substitutions
`~` --> represents the user's `$HOME` environment variable. Test it out: `echo ~`
- `~-` --> old directory PWD: gives the next-level-down directory from home you were just working in.

#### Brace expansion
Allows for multiple options in the command we're running. 
- Example: `echo c{a,o,u}t`
- Console output: `cat cot cut`

Instead of just comma-separated values, we can also use two dots to represent a list. 
- Example: `echo /tmp/{1..3}/file.txt`
- Console output: `/tmp/1/file.txt /tmp/2/file.txt /tmp/3/file.txt`

Can use multiple expansion sets, too. 
- Example: `echo file_{01..12}{a..d}.txt`
- First prints the `file_01a.txt file_01b.txt` etc., *then* moves on to `file_02a.txt`, etc.

### Parameter expansion
Parameters denoted by `$`. Example: `greeting="hello there!"`

Running `echo $greeting` prints "hello there!" to the console (without quotes, of course). We can also get individual portions of the variable(s) (like getting a substring) like this:
- `echo ${greeting:6}` --> `there!` (starts at the sixth character and prints onward from that)

What if we wanted to **replace** or substitute text in a parameter?
- `echo ${greeting/there/everybody}` --> `hello everybody!`
- **NOTE**: with one slash, this only replaces the *first* instance of a match. 
- If we wanted ALL instances of `there`, we'd write `${greeting//there/everybody}`

### Command substitution
Puts the output of one command into another command. 

Example: `echo "The kernel is $(uname -r)."`

#### Arithmetic expansion
Only in older BASH? Looks like normal expansion but with **two** parentheses (i.e. `echo $(( 4 / 5))`). 

Note: the example above returns zero. This only works with integers (i.e. only integer division supported).

## Programming with bash
### Understanding Bash Script Syntax
**One-liners**: a bash script on one line, separated by semicolons/pipes, etc.

Bash scripts are text files that contain a series of commands. 

1. **Important**: for an *executable* bash script file, we have to put a shebang line at the top of the file to tell the OS how to run this program. I.e. is this python? BASH? Ruby? Etc.
    - `#!/usr/bin/env bash`
2. Make the file *executable* by running `chmod +x <file>`

### Displaying text with echo
Can either wrap text in quotes OR we have to **escape** special characters (like parentheses). 

**Note**: each echo command includes a newline character at the end. Turn this off with `-n` flag. 

### Working with variables
When assigning a variable in bash, important to remember that **there should be no spaces on either side of the equals sign**.
- `mygreeting=Hello`

What if we want to make something **read-only**, that is, set the value and make it unable to be changed?
- `declare -r myname="Joel"`

Uppercase/lowercase some text: `declare -l mytext="THIS WILL ALL GET LOWERCASED"`
- `-u` flag for uppercase

### Working with numbers
Pretty simple stuff. Use arithmetic operators to do some math. 
- `echo $((4**2))`

Using variables: let's say we set `a` equal to 3. 
- `((a+=3))` and then `echo $a` results in printing `6` to console.
- **Note**: can't use the dollar sign within this parameter expansion (i.e. `(($a++))` will yield an error) because the variable is already being expanded

**Note**: important to *declare* variables we want to treat as numbers as integers so BASH doesn't interpret as a string.
- `declare -i b=3`

**`bc`** --> tells bash how many decimal points to display (?). Consider the following:
```
declare -i c=1
declare -i d=3
e=$(echo "scale=3; $c/$d" | bc)
echo $e

CONSOLE OUTPUT: .333
```

### Comparing values with test
First of all, to get the full list of commands available, run `help test` from CLI.

`test` is used by square brackets. 
- Ex: what if we wanted to know if our home directory is a directory?
- `[ -d ~ ]`
- Returns (internally) 0/1 indicating success or failure. If failure, error printed to console (else looks like nothing)

Another example:
- Is 4 less than 5?
- `[ 4 -lt 5 ]; echo $?`
- Prints `0` to the console (the `echo $?` command is `echo`ing the previous command's result)

**Note**: remember to keep spaces between the brackets and arguments (i.e. `[4-lt5]` won't work)

### Comparing values with extended test
Extended test: notation with two brackets (i.e. `[[ ... ]]`)

Example: Is my home directory a directory and does the bash executable file exist? 
- `[[ -d ~ && -a /bin/bash ]]; echo $?`
- prints `0` b/c home directory is a directory AND the bash file exists at `/bin/bash`

Note: logical or is `||`

**Regex matching**:
- `[[ "cat" =~ c.* ]]; echo $?`
- prints `0` b/c `cat` matches the provided regex

**Note**: extended test isn't always available outside of bash. If working with just bash, can use extended test since there are more features. But if potentially going outside of bash... maybe use test instead?

### Formatting and stylng text output
For `echo`ing something back to terminal, pass `-e` flag for including special characters for formatting purposes (such as tabs/newlines). I.e. `echo -e "Name\t\tNumber"` will print Name (2 tabs) Number. Then moving forward we can correspondingly use two tabs between names and numbers to have a pretty output. 

**The bell**: `/a` makes a *sound* to the user. Can be used to notify the user when something errors (how fun!)

Also options to make colored text. Look into this separately. 

### Formatting output with printf
Kind of similar to `echo` except that there is more options for FORMATTING. 

Here's how to achieve the same print-outs with `echo` and `printf`:
- `echo "The results are: $(( 2 + 2 )) and $(( 3 / 1 ))`
- `printf "The results are: %d and %d\n" $(( 2 + 2 )) $(( 3 / 1 ))`

Google how to print the current date. Fairly straightforward with `printf`. 

### Working with arrays
Declare an array: 
- `declare -a snacks=("apple" "banana" "orange")`
- `echo ${snacks[2]}` --> `orange`

Appending to the end of an array:
- `snacks+=("mango")`

Inserting into a fixed location:
- `snacks[5]="mango"`

Show me the entire array:
- `echo ${snacks[@]}`
- **Note**: does NOT show empty values in the array.

**Associated array**: Array with keys instead of indices (kind of like a dict):
- `declare -A <array-dict-name-here>`

## Bash control structures
See `if_example_1` and `while_example_1` for examples of if/else and while. 

Same with `until_example_1` and `for_examples`

### Using functions

Denoted like: `fn(){ ... }`

More exampmles in the `function_examples` script. 

### Reading and writing text files
Writing to: `>` overwrites content, `>>` appends content. 

## Interacting with the user
### Working with arguments
`$0` argument is the name of the script. 

### Working with options


### Getting input during execution



## Bash in the real world