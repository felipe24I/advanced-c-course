## #warning directive
#warning is a preprocessor directive that issues a warning message during compilation but does not stop compilation (unlike #error). 
It's a compiler extension (not part of standard C) supported by GCC, Clang, and some other compilers.

```c
#include <stdio.h>

#warning "This feature is experimental"

int main() {
    printf("Program still compiles despite warning\n");
    return 0;
}
```

## Basic Syntax
```c
#warning message_text
#warning "message in quotes"
#warning message without quotes (GCC/Clang)
```

## Common Use Cases
### 1. Deprecation Warnings
```c
#include <stdio.h>

// Mark old functions as deprecated
#warning "Old API will be removed in next version"

void old_function(void) {
    printf("Using deprecated function\n");
}

// Conditional deprecation
#define VERSION 1

#if VERSION < 2
    #warning "Support for version 1 will be dropped soon"
#endif

int main() {
    old_function();
    return 0;
}
```

### 2. Incomplete Code Reminders
```c
#include <stdio.h>

// TODO reminders
#warning "TODO: Implement error handling"
#warning "TODO: Add bounds checking"
#warning "FIXME: Memory leak possible"

// Function with incomplete implementation
void process_data(int *data, int size) {
    #warning "Bounds checking not implemented yet"
    for (int i = 0; i < size; i++) {
        data[i] *= 2;  // No bounds check!
    }
}

int main() {
    int arr[] = {1, 2, 3};
    process_data(arr, 3);
    return 0;
}
```

### 3. Configuration Warnings
```c
#include <stdio.h>

// Warning about configuration
#ifndef CONFIG_FILE
    #warning "CONFIG_FILE not defined, using defaults"
    #define CONFIG_FILE "default.conf"
#endif

// Warning about suboptimal settings
#define BUFFER_SIZE 128

#if BUFFER_SIZE < 1024
    #warning "Small buffer size may affect performance"
#endif

// Warning about debug mode in production
#ifdef DEBUG
    #warning "Debug mode enabled - may impact performance"
#endif

int main() {
    printf("Config file: %s\n", CONFIG_FILE);
    printf("Buffer size: %d\n", BUFFER_SIZE);
    return 0;
}
```

### difference between #warning and #pragma GCC warning
#warning is a non-standard but widely supported directive (GCC, Clang) that directly issues a warning. 
#pragma GCC warning is a GCC/Clang-specific pragma that does the same thing but is part of the pragma system, allowing for push/pop control and integration with other diagnostic pragmas.
