## Challenge 1 macros
Write a program to print the values of the following predefined symbolic constants:
* __LINE__
* __FILE__
* __DATE__
* __TIME__
* __STDC__

### Example output
```text
__LINE__ = 34
__FILE__ = fedin.c
__DATE__ = Jul  16 2019
__TIME__ = 11:12:23
__STDC__ = 1
```

## Solution
```c
#include <stdio.h>

int main() {
    printf("=== Standard Predefined Macros ===\n\n");

    // File information
    printf("__FILE__: %s\n", __FILE__); // __FILE__: C:\Users\ASUS\Documents\Intermediate_C\14-Macros\Challenge1_macros\main.c
    printf("__LINE__: %d\n", __LINE__); // __LINE__: 8

    // Compilation time
    printf("__DATE__: %s\n", __DATE__); // __DATE__: Apr  4 2026
    printf("__TIME__: %s\n", __TIME__); // __TIME__: 22:08:40

    // Standard conformance
    printf("__STDC__: %d\n", __STDC__); // __STDC__: 1

    return 0;
}
```

