# Short intro to Go
First of all, Go is a compiled, statically typed language. 
    - All variables have a `type` that is known at compilation time.

Compiled executables are OS-specific. So if you compile for Windows, it can only run on Windows. If you compile on Mac, it can only run on Mac. So on, and so forth.

## OOP with Go
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