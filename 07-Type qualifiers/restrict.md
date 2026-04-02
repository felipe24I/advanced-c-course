## Restrict
is a pointer qualifier (like const or volatile) used to help the compiler optimize code. 
it tells the compiler that this pointer is the only way to access the memory it points to within this scope. 
No other pointer will reference the same data (no aliasing).

### Without restrict:
The compiler assumes pointers might overlap.
* Generates slower, safer code.
* Reloads values from memory often.
* Cannot keep values in registers.
* Cannot remove redundant operations.

With restrict:
* Enables aggressive optimizations like loop unrolling and SIMD.

### Example:
```c
void copy(int x[], int y[]) {
    for (size_t i = 0; i < length; i++) {
        y[i] = x[i];
    }
}
```

* Adding restrict tells the compiler that x and y do not overlap.
* This enables further loop optimizations.

### Important:
If you break the promise (pointers do overlap), behavior is undefined because the compiler trusted you and optimized accordingly.

### Note:
restrict does not exist in C++ (C++ uses __restrict or __restrict__ as an extension).

### When to Use restrict:
* Use restrict when you are certain pointers do not share memory.
* This allows the compiler to perform optimizations that would otherwise be blocked.

### Syntax:
```c
int *restrict intPtrA;
int *restrict intPtrB;
```

restrict tells the compiler that for the duration of the scope in which the pointer is defined:
* They will never access the same value
* Pointers used to access integers inside an array are mutually exclusive in terms of memory access.

### Why restrict is Needed
Without restrict, the compiler must assume the worst case:
```c
#include <stdio.h>

// Compiler must assume a and b might point to overlapping memory
void vector_add(int *a, int *b, int n) {
    for (int i = 0; i < n; i++) {
        a[i] += b[i];
        // Compiler must reload a[i] each iteration because b could be a
        // If a and b overlap, writing to a[i] might affect b[i]
    }
}

// With restrict, compiler knows they don't overlap
void vector_add_restrict(int *restrict a, int *restrict b, int n) {
    for (int i = 0; i < n; i++) {
        a[i] += b[i];
        // Compiler can optimize aggressively:
        // - Load b[i] once
        // - Use SIMD instructions
        // - Reorder operations
    }
}

int main() {
    int array[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // These are safe - no overlap
    vector_add_restrict(array, array + 5, 5);
    
    // This would be undefined behavior - overlapping arrays with restrict
    // vector_add_restrict(array, array, 10);  // WRONG!
    
    return 0;
}
```
