## #error
#error is a preprocessor directive that forces the compiler to stop compilation and display an error message. 
It's used to detect invalid configurations, missing requirements, or unsupported platforms at compile time.

```c
#include <stdio.h>

#ifndef VERSION
    #error "VERSION macro must be defined!"
#endif

int main() {
    printf("Version: %d\n", VERSION);
    return 0;
}
```

## Basic Syntax
```c
#error error_message

// Note: No quotes required, but the message is typically in quotes
#error "This is an error message"
#error This also works but less common
```

## Common Use Case
### 1. Checking Required Macros
```c
#include <stdio.h>

// Check if required macros are defined
#ifndef CONFIG_FILE
    #error "CONFIG_FILE must be defined before including this header"
#endif

#ifndef MAX_BUFFER_SIZE
    #error "MAX_BUFFER_SIZE must be defined"
#endif

#ifndef DEBUG_LEVEL
    #error "DEBUG_LEVEL must be defined (0-3)"
#endif

int main() {
    printf("Config: %s\n", CONFIG_FILE);
    printf("Buffer size: %d\n", MAX_BUFFER_SIZE);
    return 0;
}
```

### 2. Incomplete Code
```c
// incomplete.c
#define IMPLEMENTED 0  // Change to 1 when ready

#if !IMPLEMENTED
    #error "Jason - Function incomplete. Fix before using"
#endif

int main() {
    // Function implementation
    return 0;
}
```

### 3. Compiler-Dependent Code
```c
// platform_check.c
#include <stdio.h>
#include <limits.h>

// Check for 16-bit platform
#if INT_MAX == 32767
    #define PLATFORM_16BIT 1
#elif INT_MAX == 2147483647
    #define PLATFORM_32BIT 1
#else
    #error "Unsupported integer size! This program requires 16-bit or 32-bit int"
#endif

int main() {
    #ifdef PLATFORM_16BIT
        printf("Running on 16-bit platform\n");
        printf("INT_MAX = %d\n", INT_MAX);
        // 16-bit specific code
    #else
        printf("Running on 32-bit platform\n");
        printf("INT_MAX = %d\n", INT_MAX);
        // 32-bit specific code
    #endif
    return 0;
}
```

### 4. Conditionally-Compiled Code
```c
// conditional_options_better.c
#include <stdio.h>

// Define one of these (uncomment to test)
// #define OPT_1
// #define OPT_2

// Check if exactly one option is defined
#if defined(OPT_1) && defined(OPT_2)
    #error "Cannot define both OPT_1 and OPT_2. Please define only one."
#elif !defined(OPT_1) && !defined(OPT_2)
    #error "*** You must define one of OPT_1 or OPT_2 ***"
#endif

#ifdef OPT_1
    void process(void) {
        printf("Processing with OPTION 1\n");
    }
#endif

#ifdef OPT_2
    void process(void) {
        printf("Processing with OPTION 2\n");
    }
#endif

int main() {
    process();
    return 0;
}
```
