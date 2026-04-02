## Heap and Stack memory allocation
Memory in C programs is divided into different segments. The two most important for programmers are the stack and the heap.

```text
Memory Layout:
┌─────────────┐  High addresses
│   Stack     │  ↓ grows downward
├─────────────┤
│     ↓       │
│             │
│     ↑       │
├─────────────┤
│    Heap     │  ↑ grows upward
├─────────────┤
│   Data      │  (static/global variables)
├─────────────┤
│   Text      │  (code)
└─────────────┘  Low addresses
```

<img width="683" height="702" alt="image" src="https://github.com/user-attachments/assets/b4f2d52f-f78d-4366-950b-11a894760060" />


## Stack Memory
### Characteristics
* **Automatic allocation/deallocation**
* **LIFO (Last In, First Out)** structure
* **Fast** allocation (just move stack pointer)
* **Fixed size** (typically 1-8 MB)
* **Thread-safe** (each thread has its own stack)

### Stack Allocation Examples
```c
#include <stdio.h>

void function(int param) {  // 'param' on stack
    int local_var = 42;      // Stack allocation
    char buffer[100];        // Array on stack
    int *ptr;                // Pointer on stack (points to ?)
    
    printf("Stack addresses:\n");
    printf("  param:     %p\n", (void*)&param);
    printf("  local_var: %p\n", (void*)&local_var);
    printf("  buffer:    %p\n", (void*)buffer);
    printf("  ptr:       %p\n", (void*)&ptr);
}

int main() {
    int main_var = 10;       // Stack
    function(main_var);
    return 0;
}
```

### Stack Frame Layout
```c
#include <stdio.h>

void demonstrate_stack_frame() {
    int a = 1;
    int b = 2;
    int c = 3;
    
    // Notice how addresses are typically decreasing
    printf("Stack frame addresses (typically decreasing):\n");
    printf("  a at %p\n", (void*)&a);
    printf("  b at %p\n", (void*)&b);
    printf("  c at %p\n", (void*)&c);
}

void nested_calls() {
    int level1 = 1;
    printf("Level 1 at %p\n", (void*)&level1);
    
    {
        int level2 = 2;
        printf("Level 2 at %p\n", (void*)&level2);
        
        {
            int level3 = 3;
            printf("Level 3 at %p\n", (void*)&level3);
        }
    }
}

int main() {
    demonstrate_stack_frame();
    nested_calls();
    return 0;
}
```

## Heap Memory
### Characteristics
* **Manual allocation/deallocation** (malloc/free)
* **Arbitrary allocation order**
* **Slower** than stack (requires heap management)
* **Large size** (limited by available RAM)
* **Shared** across threads
* **Persists** until explicitly freed

### Heap Allocation Examples
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Basic heap allocation
    int *ptr = (int*)malloc(sizeof(int));
    if (ptr) {
        *ptr = 42;
        printf("Heap value: %d\n", *ptr);
        free(ptr);
    }
    
    // Array allocation
    int *array = (int*)malloc(10 * sizeof(int));
    if (array) {
        for (int i = 0; i < 10; i++) {
            array[i] = i * i;
        }
        free(array);
    }
    
    // Calloc - zero-initialized
    int *zeroed = (int*)calloc(10, sizeof(int));
    if (zeroed) {
        printf("First element: %d\n", zeroed[0]);  // 0
        free(zeroed);
    }
    
    // Realloc - resize
    int *dynamic = (int*)malloc(5 * sizeof(int));
    if (dynamic) {
        // Resize to 10 elements
        int *resized = (int*)realloc(dynamic, 10 * sizeof(int));
        if (resized) {
            dynamic = resized;
        }
        free(dynamic);
    }
    
    return 0;
}
```

