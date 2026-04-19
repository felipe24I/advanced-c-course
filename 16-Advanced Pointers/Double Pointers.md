## Double Pointers
* **Definition:** When a pointer holds the address of another pointer, it is called a pointer-to-pointer or double pointer.
* **Basic Pointer Concept:** A pointer stores the address of a variable.

<img width="332" height="102" alt="image" src="https://github.com/user-attachments/assets/16bd617c-27fc-4aee-af92-f4dfd60c72b7" />

* **Double Pointer Concept:** The first pointer contains the address of the second pointer. The second pointer points to the location containing the actual value.

<img width="553" height="111" alt="image" src="https://github.com/user-attachments/assets/edbb7d9f-9a44-4c4b-b393-bc9f3bad829e" />

### Syntax:
```c
int **var; // a pointer to a pointer of type int
```

* **Accessing Value:** To access a value indirectly pointed to by a pointer-to-pointer, the asterisk operator (*) must be applied twice.

### Example Code:
```c
// Declaring a double pointer
int **ipp;

// Declaring some ints
int i = 5, j = 6, k = 7;

// Initializing some pointers
int *ipp1 = &i, *ipp2 = &j;

// Assigning our double pointer
ipp = &ipp1;
```

### Double Pointers in Functions
A double pointer (**) is a pointer that points to another pointer. It's commonly used in functions when you need to modify the original pointer passed to the function.

### Modifying a Pointer Inside a Function
When you want to allocate memory or change where a pointer points, you need a double pointer:

```c
#include <stdio.h>
#include <malloc.h>

void allocateMemoryCORRECT(int **ptr, int size) {
    *ptr = (int*)malloc(size * sizeof(int));  // Modifies original pointer
}

int main() {
    int *arr = NULL;     // arr is a pointer to int
    allocateMemoryCORRECT(&arr, 5);  // Pass ADDRESS of the pointer

    arr[0] = 10;  // Works fine! arr now points to allocated memory
    printf("%d", arr[0]);
}
```

### What happens with single pointer (WRONG):
```c
void allocateMemoryWRONG(int *ptr, int size) {
    ptr = (int*)malloc(size * sizeof(int));  // Changes LOCAL copy only
    // ptr is a copy of the original pointer
}

int main() {
    int *arr = NULL;
    allocateMemoryWRONG(arr, 5);
    // arr is STILL NULL! The function couldn't change it
    
    arr[0] = 10;  // CRASH! arr is still NULL
}
```

### Visual Memory Layout
```c
Single pointer attempt (WRONG):
main:     arr [NULL] ──┐
                       │ (can't reach)
function: ptr [NULL] ──┘ (local copy)

Double pointer (CORRECT):
main:     arr [NULL] ◄──┐
                        │
function: ptr [&arr] ───┘ (can modify arr through *ptr)
```

### Note:
```c
int **c = **b;
```

The declaration "int **c = **b;" is wrong because it tries dereference a double pointer **b, which results in a value of type int, while is expected a double pointer int **c.
This can result in an undefined behaviour or run-time error 
