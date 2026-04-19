## Challenge function pointers
Write a C program that takes two integer arrays of equal size and allows the user to choose an arithmetic operation (add, subtract, multiply, or divide) to perform element-wise on the arrays. 
The program must use an array of function pointers to store the four operation functions, and a separate function that accepts two arrays, their size, and a function pointer as arguments, then dynamically allocates memory for and returns a result array. 
Finally, display the results and free the allocated memory.

## Solution
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int array1[] = {10,20,30,40,50,60,70,80,90,100};
int array2[] = {38,27,87,63,59,223,132,1,19,7};

int add(int a, int b) { return a + b;}
int sub(int a, int b) { return a - b;}
int mult(int a, int b) { return a * b;}
int divide(int a, int b) {return a / b;}

int* performOp(int *a, int *b, int size, int (*mathOp)(int, int));
void display(int *x, int size);

int main()
{
    int choice = 0;

    int (*menu[4])(int, int) = {add, sub, mult, divide};

    //size of the array
    unsigned int size = 0;

    int *result = NULL;

    // Calculate minimum size correctly
    int size1 = sizeof(array1) / sizeof(array1[0]);
    int size2 = sizeof(array2) / sizeof(array2[0]);
    size = (size1 < size2) ? size1 : size2;

    while(choice != 5)
    {
        printf("\n\nWhich operation do you want to perform?\n1.Add\n2.Subtract\n3.Multipliy\n4.Divide\n5.None ...\nEnter choice: ");
        scanf("%d", &choice);

        if(choice == 5) break;
        if(choice < 1 || choice > 5) continue;

        // Pass just the function pointer, not the result of calling it
        result = performOp(array1, array2, size, menu[choice - 1]);

        printf("\n\nThe results are ...\n");
        display(result, size);

        if(result != NULL)
            free(result);

    }

    return 0;
}

int* performOp(int *a, int *b, int size, int (*mathOp)(int, int))
{
    //Allocate memory for result
    int *result = (int*)malloc(size * sizeof(int));

    if (result == NULL) {
        printf("Memory allocation failed!\n");
        return NULL;
    }

    for (int i = 0; i < size; i++)
    {
       result[i] = mathOp(a[i], b[i]);
    }

    return result; // Return the allocated array
}

void display(int *x, int size)
{
    for (int i = 0; i < size; i++)
    {
       printf("%d ", x[i]);
    }
}

```

### Note
#### Your Code:
```c
int *result = (int*)malloc(size * sizeof(int));  // result is int*
```

#### What Each Expression Means:
```c
result     // pointer to the first integer (address)
result[0]  // the first integer VALUE
result[1]  // the second integer VALUE
result[i]  // the i-th integer VALUE

// If you tried *result[i]:
*result[0]  // result[0] is an integer (like 48)
            // *48 tries to dereference address 48 → CRASH
```

#### Visual Memory Layout:
```text
After malloc:
result → [48][47][117][93]...
          ↑
          result[0] = 48 (value, not address)

*result[0] would try to treat 48 as an address:
Address 48 → ??? (garbage or crash)
```

#### When Would You Use *result[i]?
Only if result is a pointer to a pointer:

```c
int **result = (int**)malloc(size * sizeof(int*));  // Array of pointers

// Then:
result[i]    // gives a POINTER to an integer
*result[i]   // gives the integer VALUE
```
