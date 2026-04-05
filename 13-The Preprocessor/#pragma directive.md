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

## #pragmas (available in the gcc compiler)
### #pragma GCC dependency
**Allows you to check file timestamps** – makes the current file depend on another file. If the other file is newer than the current file, a warning is issued.
This pragma is useful if the current file is derived from the other file, and should be regenerated

```c
#pragma GCC dependency "headers.h"
#pragma GCC dependency "config.h" "Please update this file first"
```
### #pragma GCC poison
**Prevents specific identifiers from being used** – if any poisoned identifier appears in the code (even in comments or macros), compilation fails.

```c
#pragma GCC poison printf malloc free
// printf("Hello"); // ERROR: use of poisoned identifier
```

### #pragma GCC system_header
**Treats the current file as a system header** – disables most compiler warnings for the rest of this file (useful for third-party or generated code).

```c
#pragma GCC system_header
// No warnings from here onward
```

### #pragma once
**Ensures a header is included only once** – non-standard but widely supported alternative to include guards (#ifndef HEADER_H)

```c
#pragma once  // This file will be included only once per translation unit
```

### #pragma GCC warning
**Prints a warning message** during compilation without stopping.

```c
#pragma GCC warning "This feature is deprecated"
```

### #pragma GCC error
**Prints an error message** and stops compilation.

```c
#pragma GCC error "Unsupported architecture"
```

### #pragma message
**Prints an informational message** (like #warning but more generic; may be supported by other compilers).

```c
#pragma message "Building with debug flags"
```

### Example: Message and Information Pragmas
```c
#include <stdio.h>

// Display message during compilation
#pragma message("Compiling with optimizations")
#pragma message("Version: 2.0")

// Conditional messages
#ifdef DEBUG
    #pragma message("Debug mode enabled")
#endif

// TODO and NOTE markers (GCC/Clang)
#pragma message "TODO: Implement error handling"
#pragma message "NOTE: This function is deprecated"

// Error and warning (GCC)
#ifdef __GNUC__
    #pragma GCC warning "This feature is experimental"
    #pragma GCC error "Configuration incomplete"
#endif

int main() {
    printf("Check compiler output for messages\n");
    return 0;
}
```
