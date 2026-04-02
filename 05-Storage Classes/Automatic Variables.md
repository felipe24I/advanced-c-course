## Automatic Variables
Automatic variables are variables that are automatically created when program execution enters a block and automatically destroyed when execution leaves that block. 
They are the default storage class for local variables.

### Basic Concept
```c
#include <stdio.h>

void function() {
    // These are all automatic variables
    int x = 10;           // Automatic
    float y = 3.14;       // Automatic
    char c = 'A';         // Automatic
    int numbers[5];       // Automatic array
    
    printf("Inside function: x = %d\n", x);
}

int main() {
    function();
    // x is not accessible here - it's been destroyed
    
    return 0;
}
```

### Storage Class Specifier: auto
The auto keyword explicitly declares an automatic variable, but it's almost never used because it's the default.
```c
#include <stdio.h>

int main() {
    // These two declarations are equivalent
    int x = 10;           // Automatic (default)
    auto int y = 20;      // Explicit automatic
    
    // Also valid
    auto float pi = 3.14159;
    auto char letter = 'A';
    
    printf("x = %d, y = %d\n", x, y);
    printf("pi = %.5f, letter = %c\n", pi, letter);
    
    return 0;
}
```

## Lifetime and Scope
### 1. Block Scope
```c
#include <stdio.h>

int global = 100;  // Not automatic - static storage duration

int main() {
    int a = 10;  // Automatic - exists in main
    
    {
        int b = 20;  // Automatic - exists only in this inner block
        printf("Inner block: a = %d, b = %d\n", a, b);
        // a is accessible (outer scope)
        // b is accessible (current scope)
    }
    
    // b is NOT accessible here - out of scope
    printf("Outer block: a = %d\n", a);
    // printf("b = %d\n", b);  // ERROR: 'b' undeclared
    
    return 0;
}
```

### 2. Function Parameters (Automatic)
```c
#include <stdio.h>

// Function parameters are automatic variables
void add(int x, int y) {  // x and y are automatic
    int result = x + y;    // result is automatic
    printf("%d + %d = %d\n", x, y, result);
    // x, y, result are destroyed when function returns
}

int main() {
    add(5, 3);
    // x, y, result no longer exist
    
    return 0;
}
```

### Multiple Blocks and Nesting
```c
#include <stdio.h>

int main() {
    int level1 = 1;
    printf("Level 1: %d\n", level1);
    
    {
        int level2 = 2;
        printf("  Level 2: %d, can see level1 = %d\n", level2, level1);
        
        {
            int level3 = 3;
            printf("    Level 3: %d, can see level2 = %d, level1 = %d\n", 
                   level3, level2, level1);
            
            // Variables from outer scopes are accessible
            level1 = 10;  // Can modify outer variables
        }
        
        // level3 is gone
        printf("  Back to level 2: level2 = %d, level1 = %d\n", level2, level1);
    }
    
    // level2 is gone
    printf("Back to level 1: level1 = %d\n", level1);
    
    return 0;
}
```
