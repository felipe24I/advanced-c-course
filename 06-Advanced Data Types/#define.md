## #define
#define is a preprocessor directive that creates macros - symbolic names or expressions that are replaced by the preprocessor before compilation. 
It's a text substitution mechanism that happens before the actual compilation begins.

### Basic Syntax
```c
// Object-like macros
#define MACRO_NAME replacement_text

// Function-like macros
#define MACRO_NAME(parameters) replacement_text
```

## Object-like Macros
### 1. Simple Constants
```c
#include <stdio.h>

#define PI 3.14159
#define MAX_STUDENTS 100
#define COURSE_NAME "Programming in C"
#define NEWLINE '\n'

int main() {
    // PI, MAX_STUDENTS, etc. are replaced with their values
    double area = PI * 10 * 10;
    int students = MAX_STUDENTS;
    
    printf("Course: %s\n", COURSE_NAME);
    printf("Area: %.2f\n", area);
    printf("Students: %d%c", students, NEWLINE);
    
    return 0;
}
```

### 2. Symbolic Constants vs const
```c
// #define: preprocessor, no type checking, no memory allocation
#define MAX_SIZE 100

// const: actual variable, type checking, memory allocated
const int MAX_SIZE_CONST = 100;

// When to use which:
// #define - for true constants, array sizes, simple values
// const - when type safety matters, debugging (visible to debugger)

int array[MAX_SIZE];  // OK with #define
// int array[MAX_SIZE_CONST];  // Might not work in C89 (VLA in C99)
```

## Function-like Macros
### 1. Simple Function Macros
```c
#include <stdio.h>

#define SQUARE(x) ((x) * (x))
#define MAX(a, b) ((a) > (b) ? (a) : (b))
#define MIN(a, b) ((a) < (b) ? (a) : (b))
#define ABS(x) ((x) < 0 ? -(x) : (x))

int main() {
    int a = 5, b = 3;
    
    printf("Square of %d: %d\n", a, SQUARE(a));
    printf("Max of %d and %d: %d\n", a, b, MAX(a, b));
    printf("Min of %d and %d: %d\n", a, b, MIN(a, b));
    printf("ABS of -10: %d\n", ABS(-10));
    
    // WARNING: SQUARE(a + b) expands to ((a + b) * (a + b))
    printf("Square of sum: %d\n", SQUARE(a + b));
    
    return 0;
}
```

### 2. Important: Macro Parameters Need Parentheses
```c
#include <stdio.h>

// BAD: No parentheses around parameters
#define SQUARE_BAD(x) x * x
// SQUARE_BAD(5+3) expands to: 5 + 3 * 5 + 3 = 5 + 15 + 3 = 23 (WRONG!)

// GOOD: Parentheses around parameters and whole expression
#define SQUARE_GOOD(x) ((x) * (x))
// SQUARE_GOOD(5+3) expands to: ((5+3) * (5+3)) = 64 (CORRECT)

int main() {
    int x = 5;
    
    printf("SQUARE_BAD(5+3): %d\n", SQUARE_BAD(5 + 3));    // 23 - WRONG!
    printf("SQUARE_GOOD(5+3): %d\n", SQUARE_GOOD(5 + 3));  // 64 - CORRECT
    
    return 0;
}
```

### 3. Multi-statement Macros
```c
#include <stdio.h>

// Multiple statements - use do-while(0) trick
#define SWAP(a, b) do { \
    typeof(a) temp = a; \
    a = b; \
    b = temp; \
} while(0)

// Or without do-while (risky)
#define SWAP_RISKY(a, b) { typeof(a) temp = a; a = b; b = temp; }

// Debug print macro
#define DEBUG_PRINT(msg) do { \
    printf("DEBUG [%s:%d]: %s\n", __FILE__, __LINE__, msg); \
} while(0)

// Logging macro with levels
#define LOG_ERROR(msg) fprintf(stderr, "ERROR: %s\n", msg)
#define LOG_WARNING(msg) fprintf(stderr, "WARNING: %s\n", msg)
#define LOG_INFO(msg) printf("INFO: %s\n", msg)

int main() {
    int x = 10, y = 20;
    
    SWAP(x, y);
    printf("After swap: x=%d, y=%d\n", x, y);
    
    DEBUG_PRINT("Program started");
    LOG_INFO("Processing data");
    
    return 0;
}
```

### Predefined Macros
```c
#include <stdio.h>

int main() {
    printf("File: %s\n", __FILE__);           // Current file name
    printf("Line: %d\n", __LINE__);           // Current line number
    printf("Date: %s\n", __DATE__);           // Compilation date
    printf("Time: %s\n", __TIME__);           // Compilation time
    printf("Function: %s\n", __func__);       // Current function (C99)
    printf("STDC: %d\n", __STDC__);           // ANSI C compliance
    
    #ifdef __cplusplus
        printf("Compiled as C++\n");
    #else
        printf("Compiled as C\n");
    #endif
    
    return 0;
}
```

## Conditional Compilation
### 1. #if, #ifdef, #ifndef
```c
#include <stdio.h>

#define DEBUG 1
#define VERSION 2

int main() {
    #ifdef DEBUG
        printf("Debug mode enabled\n");
    #endif
    
    #ifndef RELEASE
        printf("Not in release mode\n");
    #endif
    
    #if VERSION >= 2
        printf("Version 2 or higher\n");
    #endif
    
    #if defined(DEBUG) && VERSION > 1
        printf("Debug mode with version > 1\n");
    #endif
    
    return 0;
}
```


