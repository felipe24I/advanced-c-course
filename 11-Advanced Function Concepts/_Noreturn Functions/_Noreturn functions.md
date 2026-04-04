## _Noreturn functions
_Noreturn is a function specifier (introduced in C11) that indicates a function does not return to its caller. These functions either:
* Loop forever
* Terminate the program (exit, abort)
* Throw an exception (C++)
* Jump to another location (longjmp)

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>

// Using _Noreturn directly
_Noreturn void fatal_error(const char *msg) {
    printf("FATAL: %s\n", msg);
    exit(1);  // Never returns
}

// Using noreturn macro (from stdnoreturn.h)
#include <stdnoreturn.h>

noreturn void panic(const char *msg) {
    fprintf(stderr, "PANIC: %s\n", msg);
    abort();  // Never returns
}

int main() {
    fatal_error("Something went wrong");
    // This line never executes
    return 0;
}
```

## Why Use _Noreturn?
### Benefits
1. **Compiler optimizations** - The compiler can optimize better knowing the function never returns
2. **Better warnings** - Helps detect unreachable code
3. **Documentation** - Clearly indicates function behavior
4. **Static analysis** - Tools can analyze code flow more accurately

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>

noreturn void die(const char *msg) {
    printf("Error: %s\n", msg);
    exit(1);
}

int divide(int a, int b) {
    if (b == 0) {
        die("Division by zero");  // Never returns
    }
    return a / b;
    // No need for return after die()
}

int main() {
    int result = divide(10, 0);
    // Compiler knows this line is unreachable
    printf("Result: %d\n", result);  // Warning: unreachable code
    return 0;
}
```

## Common _Noreturn Functions
### 1. Standard Library Functions
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>
#include <setjmp.h>

// Standard library functions that never return
void exit(int status);           // Terminates program
void abort(void);                // Terminates abnormally
void _Exit(int status);          // Terminates immediately
void quick_exit(int status);     // Quick termination

// Example: Custom error handler
noreturn void error_handler(int code) {
    printf("Fatal error %d\n", code);
    exit(code);
}

int main() {
    if (some_condition()) {
        error_handler(1);  // Program ends here
    }
    return 0;
}
```

### 2. Infinite Loop Functions
```c
#include <stdio.h>
#include <stdnoreturn.h>
#include <unistd.h>

// Event loop that never returns
noreturn void event_loop(void) {
    while (1) {
        // Process events
        printf("Processing...\n");
        sleep(1);
    }
}

// Server main loop
noreturn void run_server(int port) {
    printf("Server starting on port %d\n", port);
    
    while (1) {
        // Accept connections
        // Handle requests
        // Never exits
    }
}

### 
// Task scheduler
noreturn void scheduler(void) {
    for (;;) {
        // Run tasks
        // Never returns
    }
}

int main() {
    printf("Starting event loop...\n");
    event_loop();  // Never returns
    printf("This will never be printed\n");
    return 0;
}
```

### 3. Using with longjmp
```c
#include <stdio.h>
#include <setjmp.h>
#include <stdnoreturn.h>

jmp_buf env;

noreturn void error_recovery(void) {
    printf("Error occurred, jumping back...\n");
    longjmp(env, 1);  // Never returns
}

int main() {
    if (setjmp(env) == 0) {
        printf("Normal execution\n");
        error_recovery();  // Never returns
        printf("This won't be printed\n");
    } else {
        printf("Recovered from error\n");
    }
    return 0;
}
```

## Practical Examples
### 1. Assertion Handler
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>
#include <stdarg.h>

// Custom assertion handler
noreturn void assertion_failed(const char *file, int line, 
                                const char *func, const char *expr) {
    fprintf(stderr, "Assertion failed: %s\n", expr);
    fprintf(stderr, "  File: %s, Line: %d\n", file, line);
    fprintf(stderr, "  Function: %s\n", func);
    abort();
}

// Assert macro
#define ASSERT(expr) \
    do { \
        if (!(expr)) { \
            assertion_failed(__FILE__, __LINE__, __func__, #expr); \
        } \
    } while(0)

int divide(int a, int b) {
    ASSERT(b != 0);
    return a / b;
}

int main() {
    int result = divide(10, 0);  // Will trigger assertion
    printf("Result: %d\n", result);
    return 0;
}
```

### 2. Logging and Exit System
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdnoreturn.h>
#include <stdarg.h>
#include <time.h>

typedef enum {
    LOG_INFO,
    LOG_WARNING,
    LOG_ERROR,
    LOG_FATAL
} LogLevel;

noreturn void log_fatal(const char *format, ...) {
    time_t now = time(NULL);
    fprintf(stderr, "[%s] FATAL: ", ctime(&now));
    
    va_list args;
    va_start(args, format);
    vfprintf(stderr, format, args);
    va_end(args);
    
    fprintf(stderr, "\n");
    exit(1);  // Never returns
}

// Function that may return or not
int risky_operation(int value) {
    if (value < 0) {
        log_fatal("Negative value %d not allowed", value);
    }
    return value * 2;  // Only reached if value >= 0
}

int main() {
    printf("Starting...\n");
    int result = risky_operation(-5);  // Will trigger fatal log
    printf("Result: %d\n", result);    // Never reached
    return 0;
}
```

