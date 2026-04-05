## Macros
Macros are preprocessor directives that perform text substitution before compilation. They can be constants, expressions, or function-like constructs.

## Syntax
```c
#define MACRO macro_body
```

```c
#include <stdio.h>

// Object-like macro
#define PI 3.14159

// Function-like macro
#define SQUARE(x) ((x) * (x))

int main() {
    double area = PI * SQUARE(5);  // Expands to: 3.14159 * ((5) * (5))
    printf("Area: %f\n", area);
    return 0;
}
```

## Types of Macros
### 1. Object-like Macros (Constants)
```c
// Simple constants
#define MAX_SIZE 100
#define BUFFER_SIZE 1024
#define PROGRAM_NAME "MyApp"
#define NEWLINE '\n'

// Mathematical constants
#define PI 3.14159265359
#define E 2.71828182846

// Expression constants
#define AREA_CIRCLE(r) (PI * (r) * (r))
#define DEG_TO_RAD(deg) ((deg) * PI / 180.0)

#include <stdio.h>

int main() {
    char buffer[MAX_SIZE];
    printf("Program: %s\n", PROGRAM_NAME);
    printf("Buffer size: %d\n", BUFFER_SIZE);
    printf("Area of circle radius 5: %f\n", AREA_CIRCLE(5));
    return 0;
}
```

### 2. Function-like Macros
```c
#include <stdio.h>

// Basic function macros
#define MAX(a, b) ((a) > (b) ? (a) : (b))
#define MIN(a, b) ((a) < (b) ? (a) : (b))
#define ABS(x) ((x) < 0 ? -(x) : (x))

// Multiple statements macro
#define SWAP(a, b) do { \
    typeof(a) _temp = (a); \
    (a) = (b); \
    (b) = _temp; \
} while(0)

// Macros with return-like behavior (GCC extension)
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

### Predefined Macros
```c
#include <stdio.h>

int main() {
    // Standard predefined macros
    printf("File: %s\n", __FILE__);
    printf("Line: %d\n", __LINE__);
    printf("Date: %s\n", __DATE__);
    printf("Time: %s\n", __TIME__);
    printf("Function: %s\n", __func__);  // C99
    printf("STDC: %d\n", __STDC__);
    
    #ifdef __STDC_VERSION__
        printf("C version: %ld\n", __STDC_VERSION__);
    #endif
    
    // Compiler-specific
    #ifdef __GNUC__
        printf("GCC version: %d.%d\n", __GNUC__, __GNUC_MINOR__);
    #endif
    
    #ifdef _WIN32
        printf("Windows platform\n");
    #elif __linux__
        printf("Linux platform\n");
    #endif
    
    return 0;
}
```

### Macros vs Functions
#### Speed
* A macro is faster than a function

#### Space
* When you call a function, it has to allocate some data ( a newly allocated stack frame), macros do not have this overhead, macros insert code directly into the program (textual substitution)
* if you use the same macro 20 tiems, you get 20 lines of code inserted into your program
* if you use a function 20 times, you have just one copy of the function statements in your program (less space is used)
* functios are preferred over macros when writing large chunks of code

#### Other considerations
* Macros have an advantage in that they do not have to worry about variable types, just substitute the argument you pass them
* Functions give you type checking
* It is much harder to debug a macro than when you use a function

#### Alternatives
* Inline functions are the best alternative to macros
* Inline functions can debugged and also have type checking
