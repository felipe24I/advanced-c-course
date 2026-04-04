## #Undef
#undef removes a macro definition that was previously created with #define. 
It "undefines" the macro, making it no longer available for the rest of the compilation.

```c
#include <stdio.h>

#define VALUE 100

int main() {
    printf("VALUE = %d\n", VALUE);  // 100
    
    #undef VALUE  // Remove the definition
    
    // printf("VALUE = %d\n", VALUE);  // ERROR: VALUE is no longer defined!
    
    return 0;
}
```

## Basic Syntax
```c
#undef MACRO_NAME

// Note: No equal sign, no value, just the macro name
```

## Simple Examples
### Example 1: Basic Undef
```c
#include <stdio.h>

#define DEBUG 1

int main() {
    #ifdef DEBUG
        printf("Debug mode ON\n");
    #endif
    
    #undef DEBUG  // Turn off debug
    
    #ifdef DEBUG
        printf("This won't print\n");
    #else
        printf("Debug mode OFF\n");
    #endif
    
    return 0;
}
```

### Example 2: Redefining Macros
```c
#include <stdio.h>

#define BUFFER_SIZE 100

int main() {
    printf("Buffer size: %d\n", BUFFER_SIZE);
    
    #undef BUFFER_SIZE
    #define BUFFER_SIZE 200  // Redefine with new value
    
    printf("New buffer size: %d\n", BUFFER_SIZE);
    
    return 0;
}
```

## Practical Use Cases
### 1. Temporarily Disabling Debug Code
```c
#include <stdio.h>

#define DEBUG

void process_data(int value) {
    #ifdef DEBUG
        printf("DEBUG: Processing value %d\n", value);
    #endif
    
    // Process the data
    int result = value * 2;
    
    #ifdef DEBUG
        printf("DEBUG: Result = %d\n", result);
    #endif
}

int main() {
    printf("With debug:\n");
    process_data(10);
    
    #undef DEBUG
    printf("\nWithout debug:\n");
    process_data(20);
    
    return 0;
}
```




