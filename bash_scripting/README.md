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

We can check to see if a command is builtin or a separate program: `command -V <name-of-command>`

### Brackets and braces in bash

### Bash expansions and substitutions

### Brace expansion

### Parameter expansion

### Command substitution

### Arithmetic expansion

## Programming with bash

## Bash control structures

## Interacting with the user

## Bash in the real world