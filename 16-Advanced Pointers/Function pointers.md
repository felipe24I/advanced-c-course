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
Arrays of function pointers create efficient dispatch mechanisms.

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

