## Variable Length Arrays
Variable Length Arrays (VLAs) are arrays whose size is determined at runtime, not at compile time. 
Introduced in C99, they allow you to create arrays with sizes specified by variables.

### Basic Syntax and Usage
```c
#include <stdio.h>

int main() {
    int n;
    printf("Enter array size: ");
    scanf("%d", &n);
    
    // VLA - size determined at runtime
    int arr[n];
    
    // Initialize and use
    for (int i = 0; i < n; i++) {
        arr[i] = i * i;
    }
    
    // Print array
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    
    printf("Size of array: %zu bytes\n", sizeof(arr));
    
    return 0;
}
```

## VLAs in Functions
### 1. VLA as Function Parameters
```c
#include <stdio.h>

// VLA as function parameter (size comes before array)
void print_matrix(int rows, int cols, int matrix[rows][cols]) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%4d ", matrix[i][j]);
        }
        printf("\n");
    }
}

// Alternative syntax with *
void print_matrix2(int rows, int cols, int matrix[][cols]) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%4d ", matrix[i][j]);
        }
        printf("\n");
    }
}

// VLA with size specified after array (less common)
void process_array(int n, int arr[n]) {
    for (int i = 0; i < n; i++) {
        arr[i] *= 2;
    }
}

int main() {
    int rows = 3, cols = 4;
    int matrix[rows][cols];
    
    // Initialize matrix
    int counter = 1;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            matrix[i][j] = counter++;
        }
    }
    
    print_matrix(rows, cols, matrix);
    
    return 0;
}
```

###  2. Returning VLAs (Not Allowed)
```c
// WRONG: Cannot return VLA
int* create_vla(int n) {
    int arr[n];  // Local VLA
    // ...
    return arr;  // BUG: Returning pointer to local array
}

// CORRECT: Use dynamic allocation
int* create_array(int n) {
    int *arr = malloc(n * sizeof(int));
    if (arr) {
        for (int i = 0; i < n; i++) {
            arr[i] = i;
        }
    }
    return arr;  // OK: heap-allocated
}

// CORRECT: Pass VLA as parameter and fill
void fill_array(int n, int arr[n]) {
    for (int i = 0; i < n; i++) {
        arr[i] = i;
    }
}
```

## What Do Empty Brackets Mean
Empty brackets [] indicate that the array size is not specified at declaration.
The size can be determined from initialization, function parameters, or external definitions.

### 1. Array Declaration with Initialization
When you declare an array with empty brackets and initialize it, the compiler automatically determines the size.

```c
#include <stdio.h>

int main() {
    // Compiler counts the number of elements
    int numbers[] = {1, 2, 3, 4, 5};
    // Same as: int numbers[5] = {1, 2, 3, 4, 5};
    
    char message[] = "Hello";
    // Same as: char message[6] = {'H','e','l','l','o','\0'};
    
    float values[] = {1.1, 2.2, 3.3};
    // Same as: float values[3] = {1.1, 2.2, 3.3};
    
    printf("numbers has %zu elements\n", sizeof(numbers) / sizeof(numbers[0]));
    printf("message has %zu elements\n", sizeof(message) / sizeof(message[0]));
    printf("values has %zu elements\n", sizeof(values) / sizeof(values[0]));
    
    return 0;
}
```

### 2. Multi-dimensional Arrays with Empty Brackets
For multi-dimensional arrays, only the first dimension can be empty.

```c
#include <stdio.h>

int main() {
    // 2D array - first dimension can be empty
    int matrix[][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };
    // Compiler determines rows = 3, columns fixed at 3
    
    // 3D array - only first dimension can be empty
    int cube[][2][3] = {
        {{1,2,3}, {4,5,6}},
        {{7,8,9}, {10,11,12}}
    };
    // Compiler determines first dimension = 2
    
    // Print matrix
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

### 3. Function Parameters
In function parameters, empty brackets indicate that the parameter is an array (actually a pointer).

```c
#include <stdio.h>

// These three declarations are equivalent:
void process1(int arr[]) {          // Empty brackets
    printf("Size of parameter: %zu\n", sizeof(arr));  // Prints pointer size!
}

void process2(int arr[10]) {        // With size (ignored)
    printf("Size of parameter: %zu\n", sizeof(arr));  // Also pointer size
}

void process3(int *arr) {           // Pointer syntax
    printf("Size of parameter: %zu\n", sizeof(arr));  // Same: pointer size
}

// For multi-dimensional arrays
void process_2d(int arr[][5]) {     // First dimension empty, second fixed
    // arr is pointer to array of 5 ints
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 5; j++) {
            printf("%d ", arr[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int numbers[] = {10, 20, 30, 40, 50};
    int matrix[][5] = {
        {1,2,3,4,5},
        {6,7,8,9,10},
        {11,12,13,14,15}
    };
    
    printf("In main, numbers size: %zu\n", sizeof(numbers));  // 20 bytes
    process1(numbers);  // In function, prints pointer size (8 bytes)
    
    process_2d(matrix);
    
    return 0;
}
```

### 4. External Declarations
Empty brackets are used when declaring arrays that are defined elsewhere.

```c
// In header.h
extern int global_array[];  // Declaration: size unknown
extern char message[];      // Defined in another file

// In source1.c
int global_array[100] = {0};  // Definition: size known
char message[] = "Hello, World!";

// In source2.c
#include "header.h"

void use_external() {
    global_array[0] = 42;     // OK: array defined elsewhere
    printf("%s\n", message);  // OK
}
```
