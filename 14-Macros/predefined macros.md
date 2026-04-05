## Predefined macros
Predefined macros are built-in macros automatically defined by the compiler that provide information about the compilation process, such as file name, line number, date, time, and compiler version.

## Standard Predefined Macros (C89/C99/C11)
### 1. __FILE__ - Current File Name
Expands to a string literal containing the name of the current source file.

```c
#include <stdio.h>

int main() {
    printf("This file is: %s\n", __FILE__);
    return 0;
}
// Output: This file is: program.c (or full path depending on compiler)
```

### 2. __LINE__ - Current Line Number
Expands to an integer constant representing the current line number in the source file.

```c
#include <stdio.h>

int main() {
    printf("This is line %d\n", __LINE__);  // Line 5
    printf("This is line %d\n", __LINE__);  // Line 6
    printf("This is line %d\n", __LINE__);  // Line 7
    return 0;
}
```

### 3. __DATE__ - Compilation Date
Expands to a string literal containing the date of compilation in the format "MMM DD YYYY".

```c
#include <stdio.h>

int main() {
    printf("Compiled on: %s\n", __DATE__);
    // Output: Compiled on: Apr 4 2024
    return 0;
}
```

### 4. __TIME__ - Compilation Time
Expands to a string literal containing the time of compilation in the format "HH:MM:SS".

```c
#include <stdio.h>

int main() {
    printf("Compiled at: %s\n", __TIME__);
    // Output: Compiled at: 14:30:25
    return 0;
}
```

### 5. __STDC__ - Standard Conformance
Expands to 1 if the compiler conforms to the ANSI C standard (ISO/IEC 9899).

```c
#include <stdio.h>

int main() {
    #ifdef __STDC__
        printf("Compiler conforms to ANSI C standard\n");
        printf("__STDC__ = %d\n", __STDC__);
    #else
        printf("Compiler does not conform to ANSI C\n");
    #endif
    return 0;
}
```

### 6. __STDC_VERSION__ - C Standard Version (C99+)
Expands to a long integer constant representing the C standard version.

```c
#include <stdio.h>

int main() {
    #ifdef __STDC_VERSION__
        printf("C standard version: %ld\n", __STDC_VERSION__);
        // 199901L = C99
        // 201112L = C11
        // 201710L = C17
        // 202311L = C23
    #else
        printf("C89/C90 (no __STDC_VERSION__)\n");
    #endif
    return 0;
}
```

### 7. __STDC_HOSTED__ - Hosted vs Freestanding (C99+)
Expands to 1 for hosted implementation (full standard library), 0 for freestanding (embedded).

```c
#include <stdio.h>

int main() {
    #ifdef __STDC_HOSTED__
        if (__STDC_HOSTED__) {
            printf("Hosted implementation (full standard library)\n");
        } else {
            printf("Freestanding implementation (embedded)\n");
        }
    #endif
    return 0;
}
```

### 8. __func__ - Current Function Name (C99)
Expands to a string literal containing the name of the current function.

```c
#include <stdio.h>

void my_function(void) {
    printf("Inside function: %s\n", __func__);
}

int main() {
    printf("Inside function: %s\n", __func__);
    my_function();
    return 0;
}
// Output:
// Inside function: main
// Inside function: my_function
```

## Complete Example with All Standard Macros
```c
#include <stdio.h>

int main() {
    printf("=== Standard Predefined Macros ===\n\n");
    
    // File information
    printf("__FILE__: %s\n", __FILE__);
    printf("__LINE__: %d\n", __LINE__);
    
    // Compilation time
    printf("__DATE__: %s\n", __DATE__);
    printf("__TIME__: %s\n", __TIME__);
    
    // Function name (C99)
    printf("__func__: %s\n", __func__);
    
    // Standard conformance
    printf("__STDC__: %d\n", __STDC__);
    
    #ifdef __STDC_VERSION__
        printf("__STDC_VERSION__: %ld\n", __STDC_VERSION__);
    #else
        printf("__STDC_VERSION__: not defined (C89/C90)\n");
    #endif
    
    #ifdef __STDC_HOSTED__
        printf("__STDC_HOSTED__: %d\n", __STDC_HOSTED__);
    #endif
    
    return 0;
}
```
