## Static
The static keyword has different meanings depending on where it's used:
1. **Inside functions** - Variable retains its value between function calls
2. **At file scope (outside functions)** - Variable/function is private to the file
3. **Inside blocks** - Variable has static storage duration but block scope

## Static Local Variables
### 1. Basic Concept - Persistent Values
```c
#include <stdio.h>

void counter() {
    static int count = 0;  // Initialized only once
    count++;
    printf("Function called %d times\n", count);
}

int main() {
    counter();  // count = 1
    counter();  // count = 2
    counter();  // count = 3
    counter();  // count = 4
    
    return 0;
}
```

### 2. Static vs Automatic Local Variables
```c
#include <stdio.h>

void automatic_demo() {
    int auto_var = 0;  // Created and initialized each call
    auto_var++;
    printf("automatic: %d\n", auto_var);
}

void static_demo() {
    static int static_var = 0;  // Initialized once
    static_var++;
    printf("static: %d\n", static_var);
}

int main() {
    printf("Automatic variable (recreated each call):\n");
    automatic_demo();  // 1
    automatic_demo();  // 1 (again!)
    automatic_demo();  // 1
    
    printf("\nStatic variable (persists across calls):\n");
    static_demo();  // 1
    static_demo();  // 2
    static_demo();  // 3
    
    return 0;
}
```

### 3. Initialization Rules
```c
#include <stdio.h>

void static_initialization() {
    // Static variables are initialized to zero by default
    static int count;      // Implicitly 0
    static float value;    // Implicitly 0.0
    static char *ptr;      // Implicitly NULL
    
    // Can only be initialized with constant expressions
    static int x = 10;     // OK
    static int y = 5 + 3;  // OK (constant expression)
    // static int z = x;   // ERROR: x is not constant
    
    printf("count=%d, value=%f, ptr=%p, x=%d, y=%d\n",
           count, value, (void*)ptr, x, y);
}

int main() {
    static_initialization();
    static_initialization();  // Initialization doesn't happen again
    
    return 0;
}
```

## Static Global Variables (File Scope)
### 1. File-Private Variables
```
// file1.c
#include <stdio.h>

static int file_private = 42;  // Only visible in this file
int global_visible = 100;       // Visible to other files

static void private_function() {
    printf("This function is private to file1.c\n");
}

void public_function() {
    printf("Public function can access private: %d\n", file_private);
    private_function();
}

// file2.c
#include <stdio.h>

extern int global_visible;  // OK - can access
// extern int file_private; // ERROR: file_private is static

int main() {
    printf("global_visible = %d\n", global_visible);
    // printf("file_private = %d\n", file_private); // ERROR
    
    public_function();  // OK - public function is accessible
    
    return 0;
}
```

### 2. Encapsulation Example
```c
// counter_module.h
#ifndef COUNTER_MODULE_H
#define COUNTER_MODULE_H

// Public interface
void increment_counter(void);
void decrement_counter(void);
int get_counter(void);
void reset_counter(void);

#endif

// counter_module.c
#include "counter_module.h"

// Private data (hidden from other files)
static int counter = 0;        // Only accessible within this file
static int max_value = 100;    // Private constant
static int min_value = -100;   // Private constant

// Private helper function
static int clamp(int value) {
    if (value > max_value) return max_value;
    if (value < min_value) return min_value;
    return value;
}

// Public functions (interface)
void increment_counter(void) {
    counter = clamp(counter + 1);
}

void decrement_counter(void) {
    counter = clamp(counter - 1);
}

int get_counter(void) {
    return counter;
}

void reset_counter(void) {
    counter = 0;
}

// main.c
#include <stdio.h>
#include "counter_module.h"

int main() {
    increment_counter();
    increment_counter();
    increment_counter();
    printf("Counter: %d\n", get_counter());
    
    decrement_counter();
    printf("Counter: %d\n", get_counter());
    
    // counter = 100;  // ERROR: cannot access private variable
    // max_value = 200; // ERROR: cannot access private variable
    
    return 0;
}
```

## Static Functions
### 1. File-Private Functions
```c
// math_utils.h
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

// Public functions
int add(int a, int b);
int multiply(int a, int b);

#endif

// math_utils.c
#include "math_utils.h"
#include <stdio.h>

// Private helper functions (not exposed in header)
static int validate_input(int a, int b) {
    printf("Validating: %d, %d\n", a, b);
    return 1;
}

static int log_operation(const char *op, int result) {
    printf("Operation: %s, Result: %d\n", op, result);
    return result;
}

// Public functions
int add(int a, int b) {
    if (!validate_input(a, b)) return 0;
    int result = a + b;
    return log_operation("add", result);
}

int multiply(int a, int b) {
    if (!validate_input(a, b)) return 0;
    int result = a * b;
    return log_operation("multiply", result);
}

// main.c
#include <stdio.h>
#include "math_utils.h"

int main() {
    printf("5 + 3 = %d\n", add(5, 3));
    printf("5 * 3 = %d\n", multiply(5, 3));
    
    // validate_input(5, 3);  // ERROR: function not visible
    // log_operation("test", 42);  // ERROR: function not visible
    
    return 0;
}
```

## Static with Arrays
```c
#include <stdio.h>
#include <string.h>

// Static array at file scope (private to file)
static char buffer[1024];
static int lookup_table[256];

void init_lookup() {
    for (int i = 0; i < 256; i++) {
        lookup_table[i] = i * i;
    }
}

int get_lookup(int index) {
    if (index >= 0 && index < 256) {
        return lookup_table[index];
    }
    return -1;
}

char* get_buffer() {
    return buffer;
}

int main() {
    init_lookup();
    printf("lookup_table[10] = %d\n", get_lookup(10));
    
    char *buf = get_buffer();
    strcpy(buf, "Hello, Static Buffer!");
    printf("Buffer: %s\n", buf);
    
    // buffer[0] = 'A';  // ERROR: cannot access directly (if static at file scope)
    // But can access through pointer returned by get_buffer()
    
    return 0;
}
```

## Practical Application
### 1. Function Call Counter with Reset
```c
#include <stdio.h>

int tracked_function() {
    static int call_count = 0;
    static int total_calls = 0;
    
    call_count++;
    total_calls++;
    
    printf("This call: %d, Total: %d\n", call_count, total_calls);
    return call_count;
}

void reset_call_count() {
    // Can't reset call_count directly - it's private
    // Need a reset mechanism
}

// Better: Provide reset capability
int resettable_function(int reset) {
    static int count = 0;
    
    if (reset) {
        count = 0;
        return 0;
    }
    
    return ++count;
}

int main() {
    for (int i = 0; i < 5; i++) {
        tracked_function();
    }
    
    printf("\nWith reset capability:\n");
    for (int i = 0; i < 3; i++) {
        printf("Count: %d\n", resettable_function(0));
    }
    resettable_function(1);  // Reset
    printf("After reset: %d\n", resettable_function(0));
    
    return 0;
}
```
