# Short intro to Go
First of all, Go is a compiled, statically typed language. 
    - All variables have a `type` that is known at compilation time.

Compiled executables are OS-specific. So if you compile for Windows, it can only run on Windows. If you compile on Mac, it can only run on Mac. So on, and so forth.

## Basics
### OOP with Go
Go isn't *entirely* an object oriented language. In Go, we can:
    - Define custom interfaces
    - Define custom types that can implement one or more interface(s)
    - Custom types can have methods
    - Can define structs

What Go *can't* do: 
    - no method/operator overloading
    - no type inheritenace (like class inheritence)
    - no structured error handling (try-catch, for instance)
    - no implicit numeric conversions

### Essential syntax rules
Go is case sensitive. 

**Initial uppercase character means the symbol is exported**. This is the same as making the variable **public** in some languages... so a **lowercase character** would be the same as saying it's a *private* variable.

### Setup
(This)[https://go.dev/] is the main site for questions/documentation. I downloaded and then installed an extension for VSCode. 

### Executing
Only one source code file per application can have a main function.

- `go run main.go`
- `go run .` - equivalent to above

Build into an executable file (the compiled version)
- `go build main.go` (or `go build .`)

## Variables n' stuff
### Math
Go has most of the same math operators as other C-style languages, and bitwise operators that are in C/Java. 

Need to do operations together on **same** types. Example:
```
var anInt int = 5
var aFloat float64 = 42
sum := float64(anInt) + aFloat
fmt.Printf("Sum: %v, Type: %T\n", sum, sum)
// CONSOLE OUTPUT:
// Sum: 47, Type: float64
```

More functions and constants can be found with `import "math"`. 