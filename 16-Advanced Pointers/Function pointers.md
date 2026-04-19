## Function pointers
functions contain addresses and thus, we can use pointers to point to functions

### pointers to functions can be:
#### passed to functions (callbacks)
passing a function as an argument so it can be called later

```c
#include <stdio.h>

// Function to be passed
void greetEnglish(void) {
    printf("Hello!\n");
}

void greetSpanish(void) {
    printf("Hola!\n");
}

// Function that accepts a function pointer parameter
void processGreeting(void (*greetFunc)(void)) {
    printf("Starting...\n");
    greetFunc();  // Call the passed function
    printf("Ending...\n");
}

int main() {
    processGreeting(greetEnglish);  // Pass English greeting
    processGreeting(greetSpanish);  // Pass Spanish greeting
    return 0;
}
```

#### Output:
```text
Starting...
Hello!
Ending...
Starting...
Hola!
Ending...
```

#### returned from functions
Functions can return pointers to other functions

```c
#include <stdio.h>

// Some operations
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }

// Define a function pointer type
typedef int (*MathOp)(int, int);

// Function that RETURNS a function pointer based on operator
MathOp getOperation(char op) {
    switch(op) {
        case '+': return add;
        case '-': return subtract;
        case '*': return multiply;
        default: return NULL;
    }
}

int main() {
    // Get function pointer from factory
    int (*operation)(int, int) = getOperation('+');
    printf("10 + 5 = %d\n", operation(10, 5));  // 15
    
    operation = getOperation('*');
    printf("10 * 5 = %d\n", operation(10, 5));  // 50
    
    return 0;
}
```

#### stored in arrays
An array of function pointers is a powerful data structure in C and C++ that stores the memory addresses of multiple functions in a single table. This allows you to call different functions dynamically by using an array index, often replacing long switch or if-else blocks. 

```c
#include <stdio.h>

// Menu functions
void newFile(void) { printf("Creating new file\n"); }
void openFile(void) { printf("Opening file\n"); }
void saveFile(void) { printf("Saving file\n"); }
void saveAs(void) { printf("Save as...\n"); }
void exitApp(void) { printf("Exiting\n"); }

int main() {
    // Array of function pointers
    void (*menu[5])(void) = {newFile, openFile, saveFile, saveAs, exitApp};
    
    // Simulate user choosing option 2 (Save)
    int choice = 2;
    menu[choice]();  // Calls saveFile()
    
    // Loop through all menu items
    for(int i = 0; i < 5; i++) {
        printf("Menu %d: ", i);
        menu[i]();
    }
    
    return 0;
}
```

#### Assigned to Other Function Pointers
Function pointers are variables - they can be copied, reassigned, and swapped.

```c
#include <stdio.h>

int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }

int main() {
    // Declare multiple function pointers
    int (*op1)(int, int) = add;
    int (*op2)(int, int) = subtract;
    int (*op3)(int, int);
    
    // Assign one pointer to another
    op3 = op1;  // op3 now points to add
    printf("op3: %d\n", op3(10, 5));  // 15
    
    // Swap pointers
    int (*temp)(int, int) = op1;
    op1 = op2;
    op2 = temp;
    
    printf("op1 now subtract: %d\n", op1(10, 5));  // 5
    printf("op2 now add: %d\n", op2(10, 5));       // 15
    
    // Reassign directly
    op1 = multiply;
    printf("op1 now multiply: %d\n", op1(10, 5));  // 50
    
    return 0;
}
```

### Example 1:
```c
#include <stdio.h>

// Regular function
int add(int a, int b) {
    return a + b;
}

int main() {
    // Declare function pointer
    int (*ptr)(int, int);
    
    // Assign function address
    ptr = &add;  // or just "ptr = add;" (function name decays to pointer)
    
    // Call through pointer
    int result = ptr(5, 3);     // result = 8
    // Or: int result = (*ptr)(5, 3);
    
    printf("%d\n", result);
    return 0;
}
```

### Example 2:
```c
#include <stdio.h>

int somedisplay(void);  // Use void for no parameters

int main(void)
{
    int (*func_ptr)(void);  // Match the function signature
    func_ptr = somedisplay;
    
    printf("\n Address of Function somedisplay is %p\n", (void*)func_ptr);
    
    // Either way works:
    (*func_ptr)();  // Explicit dereference
    // func_ptr();  // Simpler - also works
    
    return 0;
}

int somedisplay(void)
{
    printf("\n - Displaying some texts -\n");
    return 0;
}
```

### qsort() Example
```c

#include <stdio.h>
#include <stdlib.h>

// Comparison function for integers
int compareInts(const void *a, const void *b) {
    int int_a = *(int*)a;
    int int_b = *(int*)b;
    
    if (int_a < int_b) return -1;
    if (int_a > int_b) return 1;
    return 0;
    
    // Or simply: return (*(int*)a - *(int*)b);
}

int main() {
    int numbers[] = {45, 12, 78, 23, 89, 5, 67, 34};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    
    printf("Original array: ");
    for(int i = 0; i < size; i++)
        printf("%d ", numbers[i]);
    
    // qsort(array, number_of_elements, size_of_each_element, comparison_function)
    qsort(numbers, size, sizeof(int), compareInts);
    
    printf("\nSorted array:   ");
    for(int i = 0; i < size; i++)
        printf("%d ", numbers[i]);
    
    return 0;
}
```

### Explanation of the qsort() code
#### 1. Header files
```c
#include <stdio.h>   // For printf()
#include <stdlib.h>  // For qsort()
```

#### 2. The Comparison Function (Most Important Part)
```c
int compareInts(const void *a, const void *b) {
    int int_a = *(int*)a;
    int int_b = *(int*)b;
    
    if (int_a < int_b) return -1;
    if (int_a > int_b) return 1;
    return 0;
}
```

##### Why const void*?
* qsort() works with ANY data type, so it uses generic void* pointers
* const means the function promises not to modify the data
* We must cast to the correct type inside

##### Step-by-step of what happens:
```c
// Step 1: a and b are pointers to unknown type (void*)
// Step 2: (int*)a - Cast to pointer to int
// Step 3: *(int*)a - Dereference to get the actual int value
// Step 4: Store in int_a variable
```

##### Visual Representation:
```text
Memory:    [45] [12] [78] [23] ...
            ↑    ↑
            a    b

When qsort() calls compareInts(&numbers[0], &numbers[1]):

a = address of 45  →  (int*)a  →  *a = 45
b = address of 12  →  (int*)b  →  *b = 12

compareInts(45, 12) returns 1 (because 45 > 12)
```

##### Return Values:
```c
return -1;  // Means: a should come BEFORE b
return 1;   // Means: a should come AFTER b  
return 0;   // Means: a and b are equal (order doesn't matter)
```

#### 3. The Main Function
```c
int main() {
    // Create an array of integers
    int numbers[] = {45, 12, 78, 23, 89, 5, 67, 34};
    
    // Calculate how many elements are in the array
    int size = sizeof(numbers) / sizeof(numbers[0]);
```

##### sizeof() Explanation:
```c
sizeof(numbers)        = 32 bytes (8 integers × 4 bytes each)
sizeof(numbers[0])     = 4 bytes (one integer)
size = 32 / 4 = 8 elements
```

#### 4. The qsort() Function Call
```c
qsort(numbers, size, sizeof(int), compareInts);
```

##### Parameters explained:
* numbers (array name):	Pointer to first element (where to start sorting)
* size:	Number of elements to sort
* sizeof(int): Size of each element in bytes
* compareInts (function name): Function that tells qsort how to compare

#### 5. Complete Execution Walkthrough
Let's trace the first few comparisons:

```c
Initial array: [45, 12, 78, 23, 89, 5, 67, 34]

qsort() starts comparing elements:

Compare 45 and 12:
compareInts(&45, &12) returns 1 (45 > 12)
→ 45 should go AFTER 12
→ Swap them: [12, 45, 78, 23, 89, 5, 67, 34]

Compare 45 and 78:
compareInts(&45, &78) returns -1 (45 < 78)
→ Keep as is: [12, 45, 78, 23, 89, 5, 67, 34]

Compare 78 and 23:
compareInts(&78, &23) returns 1 (78 > 23)
→ Swap: [12, 45, 23, 78, 89, 5, 67, 34]

... and so on until fully sorted ...

Final array: [5, 12, 23, 34, 45, 67, 78, 89]
```

### Note: Pointer to a function vs. function returning a pointer
```c
/* function returning pointer to int */
int *func(int a, float b);

/* pointer to function returning int */
int (*func)(int a, float b);
```

* the difference between the above two declarations is only in the parentheses
* be very careful about placing the parentheses in the right place

#### Explanation of the Difference:
* int *func(int a, float b);	Function returning pointer to int, (is a Function)
* int (*func)(int a, float b);	Pointer to function returning int, (is a Pointer)

#### Note:
For the following array declaration:

double (*myArray[4])(double, double) = {&w, &x, &y, &z};

Show two ways using the array to invoke the second function with arguments of 10.0 and 2.5.

```c
myArray[1](10.0, 2.5); //first notation
(*myArray[1])(10.0, 2.5); // equivalent notation
```
