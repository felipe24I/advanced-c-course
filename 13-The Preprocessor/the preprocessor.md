The process of creating a C program involves many different steps
* preprocessor
* compilation
* assembler
* linker
* loader

## The Preprocesor
The preprocessor is the first stage of compilation that processes source code before the compiler sees it. It handles directives that begin with # (hash symbol).

The C Preprocessor is essentially a text substitution tool

The C preprocessor mainly performs three tasks on your code

### 1. removes all the comments
* A comment is written only for other engineers who need to understand your code (it is of no use to a machine)
* the preprocessor removes all comments as they are not required in the execution of the program

### 2. includes all of the files from various libraries that the program needs to compile
* #include directive (includes the contents of the library file specified)

### 3. expansion of macro definitions
* small functions that do not contain as much overhead to process as a regular function
  
```text
Source Code → PREPROCESSOR → Modified Code → COMPILER → Object Code
                ↓
         #include, #define, #ifdef, etc.
```

## Preprocessor Directives Overview
* **#include:** Insert file contents
* **#define:** Define macro
* **#undef:** Undefine macro
* **#if / #ifdef / #ifndef:** Conditional compilation
* **#else / #elif:** Alternative conditions
* **#endif:** End conditional block
* **#error:** Generate error message
* **#warning:** Generate warning message
* **#pragma:** Compiler-specific instructions
* **#line:** Change line number/filename
* **#** (stringizing): Convert macro parameter to string
* **##** (token pasting): Concatenate tokens

## 1. File Inclusion (#include)
```c
// Include system header (searches system paths)
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

// Include user header (searches current directory first)
#include "myheader.h"
#include "../utils/helper.h"

// Include with path
#include "/usr/local/include/custom.h"

// Include can be nested
// file1.h includes file2.h which includes file3.h
```

## 2. Macro Definition (#define)
### Object-like Macros
```c
// Simple constants
#define PI 3.14159
#define MAX_BUFFER 1024
#define PROGRAM_NAME "MyApp"

// Expression macros
#define AREA_CIRCLE(r) (PI * (r) * (r))

// Multiple lines (use backslash)
#define COMPLEX_MACRO(x, y) \
    do { \
        int temp = (x); \
        (x) = (y); \
        (y) = temp; \
    } while(0)

#include <stdio.h>

#define SQUARE(x) ((x) * (x))

int main() {
    int a = 5;
    int result = SQUARE(a + 1);  // Expands to: ((a + 1) * (a + 1))
    printf("Result: %d\n", result);
    
    return 0;
}
```

### Function-like Macros
```c
#include <stdio.h>

// Basic function macro
#define MAX(a, b) ((a) > (b) ? (a) : (b))
#define MIN(a, b) ((a) < (b) ? (a) : (b))
#define ABS(x) ((x) < 0 ? -(x) : (x))

// Macro with multiple statements
#define SWAP(a, b) do { \
    typeof(a) _temp = (a); \
    (a) = (b); \
    (b) = _temp; \
} while(0)

// Macro with return-like behavior (GCC)
#define MAX_SAFE(a, b) ({ \
    typeof(a) _a = (a); \
    typeof(b) _b = (b); \
    _a > _b ? _a : _b; \
})

int main() {
    int x = 10, y = 20;
    
    printf("Max: %d\n", MAX(x, y));
    printf("Min: %d\n", MIN(x, y));
    printf("ABS(-5): %d\n", ABS(-5));
    
    SWAP(x, y);
    printf("After swap: x=%d, y=%d\n", x, y);
    
    return 0;
}
```

## 3. Conditional Compilation
### Basic Conditionals
```c
#include <stdio.h>

#define DEBUG 1
#define VERSION 2

int main() {
    #if DEBUG
        printf("Debug mode enabled\n");
    #endif
    
    #if VERSION >= 2
        printf("Version 2 features active\n");
    #endif
    
    #if defined(DEBUG) && VERSION > 1
        printf("Debug with version > 1\n");
    #endif
    
    return 0;
}
```

### #ifdef / #ifndef
```c
#include <stdio.h>

#define FEATURE_X 1
// #define FEATURE_Y  // Commented out

int main() {
    #ifdef FEATURE_X
        printf("Feature X is enabled\n");
    #endif
    
    #ifndef FEATURE_Y
        printf("Feature Y is NOT enabled\n");
    #endif
    
    // Common pattern: header guards
    // #ifndef HEADER_H
    // #define HEADER_H
    // ... header content ...
    // #endif
    
    return 0;
}
```

### #if, #elif, #else
```c
#include <stdio.h>

#define PLATFORM 2  // 1=Windows, 2=Linux, 3=Mac

int main() {
    #if PLATFORM == 1
        printf("Running on Windows\n");
        #define PATH_SEPARATOR '\\'
    #elif PLATFORM == 2
        printf("Running on Linux\n");
        #define PATH_SEPARATOR '/'
    #elif PLATFORM == 3
        printf("Running on macOS\n");
        #define PATH_SEPARATOR '/'
    #else
        printf("Unknown platform\n");
        #define PATH_SEPARATOR '/'
    #endif
    
    printf("Path separator: %c\n", PATH_SEPARATOR);
    
    return 0;
}
```

## 4. Predefined Macros
```c
#include <stdio.h>

int main() {
    printf("File: %s\n", __FILE__);           // Current filename
    printf("Line: %d\n", __LINE__);           // Current line number
    printf("Date: %s\n", __DATE__);           // Compilation date
    printf("Time: %s\n", __TIME__);           // Compilation time
    printf("Function: %s\n", __func__);       // Current function (C99)
    printf("STDC: %d\n", __STDC__);           // ANSI C compliance
    
    #ifdef __STDC_VERSION__
        printf("STDC Version: %ld\n", __STDC_VERSION__);  // e.g., 201112L for C11
    #endif
    
    #ifdef __GNUC__
        printf("GCC version: %d.%d\n", __GNUC__, __GNUC_MINOR__);
    #endif
    
    #ifdef _WIN32
        printf("Windows platform\n");
    #elif __linux__
        printf("Linux platform\n");
    #elif __APPLE__
        printf("macOS platform\n");
    #endif
    
    return 0;
}
```

## 5. Stringizing Operator (#)
```c
#include <stdio.h>

// Convert macro parameter to string
#define STRINGIFY(x) #x
#define TO_STRING(x) STRINGIFY(x)

// Print variable name and value
#define PRINT_VAR(x) printf("%s = %d\n", #x, x)

// Print expression
#define PRINT_EXPR(x) printf("%s = %d\n", #x, (x))

int main() {
    int count = 42;
    int value = 100;
    
    printf("STRINGIFY(hello): %s\n", STRINGIFY(hello));
    printf("TO_STRING(123): %s\n", TO_STRING(123));
    
    PRINT_VAR(count);
    PRINT_VAR(value);
    
    PRINT_EXPR(count + value);
    PRINT_EXPR(5 * 3);
    
    return 0;
}
```

## 6. Token Pasting Operator (##)
```c
#include <stdio.h>

// Concatenate tokens
#define CONCAT(a, b) a ## b
#define VAR(name, num) name ## num

// Generate function names
#define FUNC_NAME(prefix, suffix) prefix ## _ ## suffix

// Create variable names dynamically
#define MAKE_VAR(type, name) type var_ ## name

int main() {
    int var1 = 10;
    int var2 = 20;
    int var3 = 30;
    
    // CONCAT(var, 1) becomes var1
    printf("var1 = %d\n", CONCAT(var, 1));
    printf("var2 = %d\n", CONCAT(var, 2));
    printf("var3 = %d\n", CONCAT(var, 3));
    
    // Create variable with name my_value
    int MAKE_VAR(int, my_value) = 100;
    printf("var_my_value = %d\n", var_my_value);
    
    return 0;
}
```


