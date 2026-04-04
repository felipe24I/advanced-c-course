## #pragma
#pragma (pragmatic information) is a compiler-specific directive that provides additional instructions to the compiler

### Syntax
```c
#pragma token_name
```

* **token_name:** represents a command for the compiler to obey

### Three important points
* There are only a limited list of supported tokens for each standard/compiler
* The set of commands that can appear in #pragma directives is different for each compiler
* A pragma not recognized by the implementation is ignored

### #pragmas (available in the gcc compiler)
