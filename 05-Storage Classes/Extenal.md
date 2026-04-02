## External
extern is a storage class specifier that declares a variable or function without defining it. 
It tells the compiler that the actual definition exists elsewhere (in another file or later in the same file), allowing multiple files to share the same variable or function.

### Basic Concept
```c
// file1.c - Definition
#include <stdio.h>

int global_counter = 0;  // Definition (allocates memory)

void increment_counter() {
    global_counter++;
}

// file2.c - Declaration with extern
#include <stdio.h>

extern int global_counter;  // Declaration (no memory allocation)
extern void increment_counter();  // Function declaration

int main() {
    printf("Initial: %d\n", global_counter);
    increment_counter();
    printf("After increment: %d\n", global_counter);
    return 0;
}
```

### Variable Declaration vs Definition
```c
#include <stdio.h>

// DEFINITION: allocates memory
int defined_var = 42;

// DECLARATION: no memory allocation, just tells compiler it exists
extern int declared_var;

// Tentative definition (C)
int tentative_var;  // Treated as definition if no other definition

int main() {
    printf("defined_var = %d\n", defined_var);
    // printf("declared_var = %d\n", declared_var);  // Error: undefined reference
    
    return 0;
}

// Later in the same file (or another file)
int declared_var = 100;  // Actual definition
```

### Extern with Functions
```c
// Functions have external linkage by default
// These three declarations are equivalent:

// 1. Implicit extern (default)
void function1(void);

// 2. Explicit extern
extern void function2(void);

// 3. With parameters
extern int add(int a, int b);

// Function definition (also external by default)
int add(int a, int b) {
    return a + b;
}
```

## Multi-file Example
### header.h
```c
#ifndef HEADER_H
#define HEADER_H

// Declarations (extern is optional for functions)
extern int shared_counter;
extern int shared_array[];

void increment_counter(void);
int get_counter(void);
void init_array(int size);

#endif
```

### shared.c
```c
#include "header.h"

// Definitions
int shared_counter = 0;
int shared_array[100];  // Size known here

void increment_counter(void) {
    shared_counter++;
}

int get_counter(void) {
    return shared_counter;
}

void init_array(int size) {
    for (int i = 0; i < size && i < 100; i++) {
        shared_array[i] = i * i;
    }
}
```

### main.c
```c
#include <stdio.h>
#include "header.h"

int main() {
    printf("Initial counter: %d\n", get_counter());
    
    increment_counter();
    increment_counter();
    
    printf("After increments: %d\n", get_counter());
    
    init_array(10);
    printf("shared_array[5] = %d\n", shared_array[5]);
    
    return 0;
}
```

## Extern with Arrays
```c
// file1.c
int big_array[1000];  // Definition with size

// file2.c
extern int big_array[];  // Size not needed for declaration
// Can access big_array[0] to big_array[999]

// file3.c
extern int big_array[1000];  // Size can be specified (ignored)

// CORRECT: Accessing array
void process_array(void) {
    extern int big_array[];
    big_array[0] = 42;  // OK
}
```

## Extern with Constants
```c
// file1.c
#include <stdio.h>

// Constants have internal linkage by default in C
const int local_const = 100;  // static (file scope only)

// To make const external, use extern
extern const int shared_const = 200;  // External linkage

// file2.c
extern const int shared_const;  // OK - can access
// extern const int local_const;  // Error - not visible

int main() {
    printf("shared_const = %d\n", shared_const);
    return 0;
}
```

## Extern with Pointers
```c
// file1.c
int value = 42;
int *ptr = &value;
int **pptr = &ptr;

// file2.c
#include <stdio.h>

extern int value;
extern int *ptr;
extern int **pptr;

int main() {
    printf("value = %d\n", value);
    printf("*ptr = %d\n", *ptr);
    printf("**pptr = %d\n", **pptr);
    
    // Modify through pointer
    *ptr = 100;
    printf("After modification: value = %d\n", value);
    
    return 0;
}
```

## Extern in Header Files
### config.h
```c
#ifndef CONFIG_H
#define CONFIG_H

// Global configuration variables
extern int debug_level;
extern char log_file[256];
extern int max_connections;

// Configuration functions
void load_config(void);
void save_config(void);

#endif
```

### config.c
```c
#include "config.h"
#include <stdio.h>
#include <string.h>

// Definitions
int debug_level = 1;
char log_file[256] = "app.log";
int max_connections = 100;

void load_config(void) {
    // Load from file
    printf("Loading configuration...\n");
    printf("  debug_level = %d\n", debug_level);
    printf("  log_file = %s\n", log_file);
    printf("  max_connections = %d\n", max_connections);
}

void save_config(void) {
    // Save to file
    printf("Saving configuration...\n");
}
```

### main.c
```c
#include <stdio.h>
#include "config.h"

int main() {
    // Use global configuration
    printf("Current debug level: %d\n", debug_level);
    
    // Modify configuration
    debug_level = 2;
    strcpy(log_file, "debug.log");
    
    load_config();  // Shows modified values
    save_config();
    
    return 0;
}
```

## Common Use Case
### Application State
```c
// app_state.h
#ifndef APP_STATE_H
#define APP_STATE_H

typedef struct {
    int initialized;
    char version[32];
    int user_count;
    int is_running;
} AppState;

extern AppState app;

void app_init(void);
void app_shutdown(void);

#endif

// app_state.c
#include "app_state.h"
#include <string.h>

AppState app = {
    .initialized = 0,
    .version = "1.0.0",
    .user_count = 0,
    .is_running = 0
};

void app_init(void) {
    app.initialized = 1;
    app.is_running = 1;
}

void app_shutdown(void) {
    app.is_running = 0;
}
```
