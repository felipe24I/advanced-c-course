## Const
The const qualifier specifies that a variable's value cannot be modified after initialization. 
It's a compile-time constraint that helps write safer, more maintainable code by preventing accidental modifications.

### Basic Usage
```c
#include <stdio.h>

int main() {
    // Basic const variables
    const int MAX_SIZE = 100;
    // MAX_SIZE = 200;  // ERROR: cannot modify const variable
    
    // Must initialize const variables
    const int VALUE;  // WARNING: uninitialized const (some compilers allow)
    
    // const with different types
    const float PI = 3.14159f;
    const char NEWLINE = '\n';
    
    printf("MAX_SIZE: %d\n", MAX_SIZE);
    printf("PI: %f\n", PI);
    
    return 0;
}
```

## Const with Pointers
Pointer declarations with const can be confusing. The placement of const matters significantly.

### 1. Pointer to Constant Data
```c
#include <stdio.h>

int main() {
    int value = 42;
    int another = 100;
    
    // Pointer to constant int: data cannot be changed, pointer can
    const int *ptr1 = &value;
    // *ptr1 = 50;     // ERROR: cannot modify the pointed-to value
    ptr1 = &another;    // OK: pointer itself can be changed
    
    printf("ptr1 points to: %d\n", *ptr1);
    
    // Equivalent to above (const before type)
    int const *ptr2 = &value;
    // *ptr2 = 50;     // ERROR: same as above
    ptr2 = &another;    // OK
    
    return 0;
}
```

### 2. Constant Pointer to Data
```c
#include <stdio.h>

int main() {
    int value = 42;
    int another = 100;
    
    // Constant pointer to int: pointer cannot change, data can
    int *const ptr3 = &value;
    *ptr3 = 50;         // OK: can modify the pointed-to value
    // ptr3 = &another; // ERROR: cannot change the pointer itself
    
    printf("Value: %d\n", value);  // Now 50
    
    return 0;
}
```

### 3. Constant Pointer to Constant Data
```c
#include <stdio.h>

int main() {
    int value = 42;
    int another = 100;
    
    // Constant pointer to constant int: neither can change
    const int *const ptr4 = &value;
    // *ptr4 = 50;     // ERROR: cannot modify data
    // ptr4 = &another; // ERROR: cannot modify pointer
    
    printf("Value: %d\n", *ptr4);
    
    return 0;
}
```

### 4. Reading Const Pointer Declarations
```c
#include <stdio.h>

int main() {
    int value = 42;
    
    // Read from right to left:
    const int *p1;        // p1 is a pointer to a constant int
    int const *p2;        // p2 is a pointer to a constant int (same)
    int *const p3 = &value; // p3 is a constant pointer to int
    const int *const p4 = &value; // p4 is a constant pointer to constant int
    
    // Helper: "const applies to what's immediately to its left,
    // unless it's at the beginning, then it applies to the right"
    
    return 0;
}
```

### Const with Arrays
```c
#include <stdio.h>
#include <string.h>

int main() {
    // Const array elements cannot be modified
    const int numbers[] = {1, 2, 3, 4, 5};
    // numbers[0] = 10;  // ERROR: cannot modify const array
    
    // Const array of pointers
    const char *colors[] = {"Red", "Green", "Blue"};
    // colors[0][0] = 'r';  // ERROR: cannot modify string literal
    colors[0] = "Yellow";    // OK: pointer can change (array not const)
    
    // Constant array of pointers to constant characters
    const char *const fixed_colors[] = {"Red", "Green", "Blue"};
    // fixed_colors[0] = "Yellow";  // ERROR: pointer is const
    // fixed_colors[0][0] = 'r';    // ERROR: data is const
    
    printf("Colors: %s, %s\n", colors[0], fixed_colors[0]);
    
    return 0;
}
```

### Const with Function Parameters
```c
#include <stdio.h>
#include <string.h>

// 1. Const parameter: protects the parameter inside function
void print_value(const int value) {
    // value = 100;  // ERROR: cannot modify const parameter
    printf("Value: %d\n", value);
}

// 2. Const pointer parameter: protects the data
void print_string(const char *str) {
    // str[0] = 'a';  // ERROR: cannot modify const data
    printf("String: %s\n", str);
}

// 3. Const pointer to const data: maximum protection
void process_data(const int *const data, const size_t size) {
    // data[0] = 10;  // ERROR: cannot modify data
    // data = NULL;   // ERROR: cannot modify pointer
    for (size_t i = 0; i < size; i++) {
        printf("%d ", data[i]);
    }
    printf("\n");
}

// 4. Const with arrays (decays to pointer)
void print_array(const int arr[], size_t size) {
    // arr[0] = 100;  // ERROR: arr is const pointer
    for (size_t i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// 5. Const with double pointers (common mistake)
void bad_const_example(const char **str) {
    // str[0] = "Hello";  // Actually allowed? Let's see...
}

void good_const_example(const char * const *str) {
    // Both levels are const
}

int main() {
    int x = 42;
    print_value(x);
    
    char text[] = "Hello";
    print_string(text);
    
    int nums[] = {1, 2, 3, 4, 5};
    process_data(nums, 5);
    print_array(nums, 5);
    
    return 0;
}
```

### Const with Return Values
```c
#include <stdio.h>

// Returning const value (often useless, but can prevent misuse)
const int get_value(void) {
    return 42;
}

// Returning const pointer to internal data
static int internal_value = 100;

const int* get_internal_value(void) {
    return &internal_value;  // Caller cannot modify internal data
}

// Returning const pointer to const data
const int* const get_fixed_value(void) {
    static const int fixed = 999;
    return &fixed;
}

int main() {
    int val = get_value();  // OK: copy made
    // get_value() = 50;    // ERROR: cannot assign to const
    
    const int *ptr = get_internal_value();
    // *ptr = 200;          // ERROR: cannot modify through const pointer
    
    printf("Value: %d\n", *ptr);
    
    return 0;
}
```

### Const with Structures
```c
#include <stdio.h>
#include <string.h>

typedef struct {
    int id;
    char name[50];
    float score;
} Student;

// Const structure parameter
void print_student(const Student *s) {
    // s->id = 100;        // ERROR: cannot modify const struct
    printf("ID: %d, Name: %s, Score: %.2f\n", 
           s->id, s->name, s->score);
}

// Const structure with mutable members
typedef struct {
    int id;
    char name[50];
    mutable int access_count;  // Note: C doesn't have 'mutable' (C++ only)
} Counter;

// Workaround: use pointer for modifiable members
typedef struct {
    int id;
    char name[50];
    int *access_count;  // Pointer to modifiable counter
} CounterC;

int main() {
    Student s1 = {123, "Alice", 95.5};
    print_student(&s1);
    
    // Const structure instance
    const Student s2 = {456, "Bob", 87.0};
    // s2.id = 789;        // ERROR: cannot modify const struct
    // s2.score = 90.0;    // ERROR: cannot modify const struct
    
    printf("Const student: %s\n", s2.name);
    
    return 0;
}
```

## Practical Examples
### 1. Lookup Tables
```c
#include <stdio.h>

// Const arrays for lookup tables (stored in ROM/Flash)
const int DAYS_IN_MONTH[] = {31, 28, 31, 30, 31, 30, 
                              31, 31, 30, 31, 30, 31};

const char* const MONTH_NAMES[] = {
    "January", "February", "March", "April", "May", "June",
    "July", "August", "September", "October", "November", "December"
};

// Const string literals (typically stored in read-only memory)
const char* get_month_name(int month) {
    if (month >= 1 && month <= 12) {
        return MONTH_NAMES[month - 1];
    }
    return "Invalid";
}

int main() {
    printf("February has %d days\n", DAYS_IN_MONTH[1]);
    printf("Month 3: %s\n", get_month_name(3));
    
    // DAYS_IN_MONTH[1] = 29;  // ERROR: cannot modify const array
    
    return 0;
}
```

### 2. Configuration Structures
```c
#include <stdio.h>

// Application configuration (const to prevent runtime changes)
typedef struct {
    const char* app_name;
    const int version_major;
    const int version_minor;
    const int max_connections;
    const int timeout_seconds;
} AppConfig;

// Global configuration (const)
const AppConfig CONFIG = {
    .app_name = "MyApp",
    .version_major = 1,
    .version_minor = 0,
    .max_connections = 100,
    .timeout_seconds = 30
};

// But sometimes we need mutable config for runtime
typedef struct {
    char* app_name;      // Mutable
    int version_major;   // Mutable
    int version_minor;
    int max_connections;
    int timeout_seconds;
} MutableConfig;

// Const pointer to mutable config (pointer const, data mutable)
MutableConfig *const pConfig = &(MutableConfig){
    .app_name = "MyApp",
    .version_major = 1,
    .version_minor = 0,
    .max_connections = 100,
    .timeout_seconds = 30
};

int main() {
    printf("App: %s v%d.%d\n", 
           CONFIG.app_name, CONFIG.version_major, CONFIG.version_minor);
    
    // Can modify through const pointer
    pConfig->max_connections = 200;  // OK: data not const
    // pConfig = NULL;                // ERROR: pointer is const
    
    printf("Max connections: %d\n", pConfig->max_connections);
    
    return 0;
}
```

### 3. String Handling
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Const-correct string functions
size_t string_length(const char *str) {
    const char *s = str;
    while (*s) s++;
    return s - str;
}

char* string_copy(char *dest, const char *src) {
    char *d = dest;
    const char *s = src;
    while ((*d++ = *s++));
    return dest;
}

// String comparison with const correctness
int string_compare(const char *s1, const char *s2) {
    while (*s1 && (*s1 == *s2)) {
        s1++;
        s2++;
    }
    return *(const unsigned char*)s1 - *(const unsigned char*)s2;
}

// Convert to uppercase (modifies in place)
char* string_toupper(char *str) {
    char *s = str;
    while (*s) {
        *s = toupper(*s);
        s++;
    }
    return str;
}

int main() {
    const char *greeting = "Hello, World!";  // String literal (const)
    char buffer[50];
    
    string_copy(buffer, greeting);
    printf("Copied: %s\n", buffer);
    printf("Length: %zu\n", string_length(greeting));
    
    string_toupper(buffer);
    printf("Uppercase: %s\n", buffer);
    
    // greeting[0] = 'h';  // ERROR: cannot modify string literal
    
    return 0;
}
```

### 4. Embedded System Register Mapping
```c
#include <stdio.h>
#include <stdint.h>

// Hardware register definitions (const for read-only registers)
typedef struct {
    volatile uint32_t CR;     // Control register (read/write)
    volatile const uint32_t SR;   // Status register (read-only)
    volatile uint32_t DR;     // Data register (read/write)
} UART_TypeDef;

// Peripheral base address (const pointer to volatile)
#define UART1_BASE 0x40004000
#define UART1 ((UART_TypeDef *)UART1_BASE)

// Read-only configuration parameters
typedef struct {
    const uint32_t baudrate;
    const uint8_t data_bits;
    const uint8_t stop_bits;
    const uint8_t parity;
} UART_Config;

// Default configuration (const)
const UART_Config DEFAULT_UART_CFG = {
    .baudrate = 115200,
    .data_bits = 8,
    .stop_bits = 1,
    .parity = 0
};

// Function with const parameters
void uart_init(const UART_Config *cfg) {
    // Use cfg to configure UART
    printf("Initializing UART: %d baud, %d data bits\n",
           cfg->baudrate, cfg->data_bits);
    
    // cfg->baudrate = 9600;  // ERROR: cannot modify const config
}

int main() {
    // Use const configuration
    uart_init(&DEFAULT_UART_CFG);
    
    // Read status register (const ensures we don't accidentally write)
    uint32_t status = UART1->SR;
    printf("UART Status: 0x%08X\n", status);
    
    // UART1->SR = 0;  // ERROR: SR is const, cannot write
    
    return 0;
}
```


