## Inline functions
Inline functions are a C99 feature that suggest to the compiler to replace the function call with the actual function code at the call site, eliminating function call overhead.

```c
#include <stdio.h>

// Regular function (function call overhead)
int square_normal(int x) {
    return x * x;
}

// Inline function (no function call overhead)
inline int square_inline(int x) {
    return x * x;
}

int main() {
    int a = square_normal(5);   // Function call
    int b = square_inline(5);   // Compiler replaces with: int b = 5 * 5;
    
    printf("%d %d\n", a, b);
    return 0;
}
```

## How Inline Functions Work
### Without Inline (Function Call)
```c
int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(5, 3);
    // Assembly:
    // push 3
    // push 5
    // call add    ← Function call overhead
    // mov result, eax
    return 0;
}
```

### With Inline (Code Substitution)
```c
inline int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(5, 3);
    // Assembly:
    // mov eax, 5
    // add eax, 3  ← Direct code, no function call
    // mov result, eax
    return 0;
}
```

## Syntax and Usage
### 1. Basic Inline Functions
```c
#include <stdio.h>

// Simple inline function
inline int max(int a, int b) {
    return a > b ? a : b;
}

// Inline with multiple statements
inline void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Inline function with loops
inline int sum_array(int arr[], int n) {
    int sum = 0;
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }
    return sum;
}

int main() {
    int x = 10, y = 20;
    printf("Max: %d\n", max(x, y));
    
    swap(&x, &y);
    printf("Swapped: x=%d, y=%d\n", x, y);
    
    int arr[] = {1, 2, 3, 4, 5};
    printf("Sum: %d\n", sum_array(arr, 5));
    
    return 0;
}
```

### 2. Static Inline Functions
```c
// Static inline - visible only in this file
static inline int square(int x) {
    return x * x;
}

// Each .c file gets its own copy
// Use when function is used only in one file
```

### 3. Extern Inline Functions (C99)
```c
// file: math.h
#ifndef MATH_H
#define MATH_H

// External inline declaration
extern inline int add(int a, int b);

#endif

// file: math.c
#include "math.h"

// Definition of external inline function
extern inline int add(int a, int b) {
    return a + b;
}

// file: main.c
#include "math.h"

int main() {
    int result = add(5, 3);  // Can be inlined or called
    return 0;
}
```

## Practical Examples
### 1. Vector Math Operations
```c
#include <stdio.h>
#include <math.h>

typedef struct {
    float x, y, z;
} Vector3;

// Inline vector operations (no function call overhead)
static inline Vector3 vector_add(Vector3 a, Vector3 b) {
    return (Vector3){a.x + b.x, a.y + b.y, a.z + b.z};
}

static inline Vector3 vector_sub(Vector3 a, Vector3 b) {
    return (Vector3){a.x - b.x, a.y - b.y, a.z - b.z};
}

static inline float vector_dot(Vector3 a, Vector3 b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

static inline Vector3 vector_scale(Vector3 v, float s) {
    return (Vector3){v.x * s, v.y * s, v.z * s};
}

static inline float vector_length(Vector3 v) {
    return sqrtf(vector_dot(v, v));
}

static inline Vector3 vector_normalize(Vector3 v) {
    float len = vector_length(v);
    if (len > 0) {
        return vector_scale(v, 1.0f / len);
    }
    return v;
}

int main() {
    Vector3 v1 = {1.0f, 2.0f, 3.0f};
    Vector3 v2 = {4.0f, 5.0f, 6.0f};
    
    Vector3 sum = vector_add(v1, v2);
    Vector3 diff = vector_sub(v1, v2);
    float dot = vector_dot(v1, v2);
    float len = vector_length(v1);
    
    printf("Sum: (%.1f, %.1f, %.1f)\n", sum.x, sum.y, sum.z);
    printf("Diff: (%.1f, %.1f, %.1f)\n", diff.x, diff.y, diff.z);
    printf("Dot: %.1f\n", dot);
    printf("Length: %.2f\n", len);
    
    return 0;
}
```

### 2. String Utilities
```c
#include <stdio.h>
#include <ctype.h>

// Inline string functions
static inline char to_upper(char c) {
    return (c >= 'a' && c <= 'z') ? c - 'a' + 'A' : c;
}

static inline char to_lower(char c) {
    return (c >= 'A' && c <= 'Z') ? c - 'A' + 'a' : c;
}

static inline int is_digit(char c) {
    return c >= '0' && c <= '9';
}

static inline int is_letter(char c) {
    return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z');
}

static inline int is_alnum(char c) {
    return is_digit(c) || is_letter(c);
}

static inline int char_to_int(char c) {
    return is_digit(c) ? c - '0' : -1;
}

int main() {
    char test[] = "Hello123";
    
    for (int i = 0; test[i]; i++) {
        printf("%c: upper=%c, lower=%c, digit=%d, letter=%d\n",
               test[i], to_upper(test[i]), to_lower(test[i]),
               is_digit(test[i]), is_letter(test[i]));
    }
    
    return 0;
}
```

### 3. Bit Manipulation Utilities
```c
#include <stdio.h>
#include <stdint.h>

// Inline bit operations
static inline uint32_t set_bit(uint32_t value, int bit) {
    return value | (1u << bit);
}

static inline uint32_t clear_bit(uint32_t value, int bit) {
    return value & ~(1u << bit);
}

static inline uint32_t toggle_bit(uint32_t value, int bit) {
    return value ^ (1u << bit);
}

static inline int test_bit(uint32_t value, int bit) {
    return (value >> bit) & 1;
}

static inline uint32_t set_bits(uint32_t value, uint32_t mask) {
    return value | mask;
}

static inline uint32_t clear_bits(uint32_t value, uint32_t mask) {
    return value & ~mask;
}

static inline int is_power_of_two(uint32_t x) {
    return x && !(x & (x - 1));
}

static inline int count_bits(uint32_t x) {
    // Brian Kernighan's algorithm
    int count = 0;
    while (x) {
        x &= (x - 1);
        count++;
    }
    return count;
}

int main() {
    uint32_t flags = 0;
    
    flags = set_bit(flags, 3);
    flags = set_bit(flags, 5);
    printf("Flags: 0x%08X\n", flags);
    printf("Bit 3: %d\n", test_bit(flags, 3));
    printf("Bit 4: %d\n", test_bit(flags, 4));
    
    flags = clear_bit(flags, 3);
    printf("After clearing bit 3: 0x%08X\n", flags);
    
    printf("Is 16 power of two? %d\n", is_power_of_two(16));
    printf("Bits in 0x55555555: %d\n", count_bits(0x55555555));
    
    return 0;
}
```

### 4. Ring Buffer Operations
```c
#include <stdio.h>
#include <stdbool.h>
#include <string.h>

#define BUFFER_SIZE 16

typedef struct {
    char buffer[BUFFER_SIZE];
    int head;
    int tail;
    int count;
} RingBuffer;

// Inline ring buffer operations
static inline void ring_init(RingBuffer *rb) {
    rb->head = 0;
    rb->tail = 0;
    rb->count = 0;
}

static inline bool ring_is_full(RingBuffer *rb) {
    return rb->count == BUFFER_SIZE;
}

static inline bool ring_is_empty(RingBuffer *rb) {
    return rb->count == 0;
}

static inline int ring_available(RingBuffer *rb) {
    return BUFFER_SIZE - rb->count;
}

static inline bool ring_push(RingBuffer *rb, char c) {
    if (ring_is_full(rb)) return false;
    rb->buffer[rb->head] = c;
    rb->head = (rb->head + 1) % BUFFER_SIZE;
    rb->count++;
    return true;
}

static inline bool ring_pop(RingBuffer *rb, char *c) {
    if (ring_is_empty(rb)) return false;
    *c = rb->buffer[rb->tail];
    rb->tail = (rb->tail + 1) % BUFFER_SIZE;
    rb->count--;
    return true;
}

int main() {
    RingBuffer rb;
    ring_init(&rb);
    
    // Push some characters
    const char *msg = "Hello";
    for (int i = 0; msg[i]; i++) {
        ring_push(&rb, msg[i]);
    }
    
    printf("Available: %d\n", ring_available(&rb));
    printf("Is empty: %d\n", ring_is_empty(&rb));
    
    // Pop and print
    char c;
    while (ring_pop(&rb, &c)) {
        printf("%c", c);
    }
    printf("\n");
    
    printf("Is empty: %d\n", ring_is_empty(&rb));
    
    return 0;
}
```

