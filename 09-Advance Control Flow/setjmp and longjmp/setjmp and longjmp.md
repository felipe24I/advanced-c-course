## setjmp and longjmp
setjmp and longjmp provide a way to perform non-local jumps, allowing you to transfer control from one function to another without following the normal call-return sequence. 
They're like a "super goto" that can jump across function boundaries.

### Header File
```c
#include <setjmp.h>
```

### Core Functions
```c
int setjmp(jmp_buf env);
void longjmp(jmp_buf env, int val);
```

* **jmp_buf:** An array type that stores the environment (stack pointer, program counter, registers)
* **setjmp:** Saves the current environment and returns 0
* **longjmp:** Restores the saved environment, causing setjmp to return again with value val

### Basic Example
```c
#include <stdio.h>
#include <setjmp.h>

jmp_buf jump_buffer;

void function_b() {
    printf("In function_b\n");
    longjmp(jump_buffer, 1);  // Jump back to setjmp
    printf("This line never executes\n");
}

void function_a() {
    printf("In function_a\n");
    function_b();
    printf("This line never executes\n");
}

int main() {
    if (setjmp(jump_buffer) == 0) {
        printf("First time through\n");
        function_a();
    } else {
        printf("Jumped back to main\n");
    }
    return 0;
}
```

### Output
```text
First time through
In function_a
In function_b
Jumped back to main
```

### How It Works
1. setjmp saves the current execution context (registers, stack pointer, program counter) into jmp_buf
2. On first call, setjmp returns 0
3. Later, longjmp restores that saved context
4. Execution resumes as if setjmp had just returned, but with the value specified in longjmp

### Example 2: Error Handling and Recovery
```c
#include <stdio.h>
#include <setjmp.h>
#include <stdlib.h>

jmp_buf error_env;

void handle_error(const char *msg) {
    printf("ERROR: %s\n", msg);
    longjmp(error_env, 1);  // Jump to recovery point
}

void risky_operation(int depth) {
    if (depth > 5) {
        handle_error("Depth exceeded");
    }
    
    char *buffer = malloc(1024);
    if (!buffer) {
        handle_error("Memory allocation failed");
    }
    
    // Do risky work
    free(buffer);
}

int main() {
    if (setjmp(error_env) == 0) {
        // Normal execution
        printf("Starting operations\n");
        risky_operation(10);
        printf("Operation completed\n");
    } else {
        // Error recovery
        printf("Recovering from error\n");
        // Clean up, retry, or exit gracefully
    }
    return 0;
}
```
