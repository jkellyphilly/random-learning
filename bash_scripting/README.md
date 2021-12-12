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


### Working with variables


### Working with numbers


### Comparing values with test


### Comparing values with extended test


### Formatting and stylng text output


### Formatting output with printf


### Working with arrays



## Bash control structures

## Interacting with the user

## Bash in the real world