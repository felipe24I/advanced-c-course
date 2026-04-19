## Challenge 1
Write a program that creates, assigns, and accesses double pointers in C/C++. Follow these steps:

1. Create a normal integer variable (non-pointer) and assign it a random value.
2. Create a single integer pointer variable.
3. Create a double integer pointer variable.
4. Assign the address of the normal integer variable (step 1) to the single pointer (step 2).
5. Assign the address of the single pointer (step 2) to the double pointer variable (step 3).

Then, display the following output using all possible syntax:

- All possible ways to find the **value** of the normal integer variable (step 1)
- All possible ways to find the **address** of the normal integer variable (step 1)
- All possible ways to find the **value** of the single pointer variable (step 2)
- All possible ways to find the **address** of the single pointer variable (step 2)
- All possible ways to print the **double pointer value** and **address** (step 3)

### Example Output
```text
Value of num is: 123
Value of num using singlePointer is: 123
Value of num using doublePointer is: 123

Address of num is: 0xffffcbcc
Address of num using singlePointer is: 0xffffcbcc
Address of num using doublePointer is: 0xffffcbcc

Value of Pointer singlePointer is: 0xffffcbcc
Value of Pointer singlePointer using doublePointer is: 0xffffcbcc

Address of Pointer singlePointer is: 0xffffcbcc
Address of Pointer singlePointer using doublePointer is: 0xffffcbcc

Value of Pointer doublePointer is: 0xffffcbcc
Address of Pointer doublePointer is: 0xffffcb8
```

**Note:** Actual memory addresses will vary on each run. The values shown above are just examples.

## Solution
```c
#include <stdio.h>

int main()
{
    int a = 1;
    int *b = &a;
    int **c = &b;

    printf("\nAll possible ways to find value of the normal integer variable\n");
    printf("value of num is: %d\n", a);
    printf("value of num using singlePointer is: %d\n", *b);
    printf("value of num using doublePointer is: %d\n", **c);

    printf("\nAll possible ways to find address of the normal integer variable\n");
    printf("address of num is: %p\n", &a);
    printf("address of num using singlePointer is: %p\n", b);
    printf("address of num using doublePointer is: %p\n", *c); // c stores address of b, *c prints value of b (is different because value of b is address of a)

    printf("\nAll possible ways to find the value of the single pointer variable\n");
    printf("value of Pointer singlePointer is: %p\n", b); // value of b is the address of a
    printf("value of Pointer singlePointer using doublePointer is: %p\n", *c);

    printf("\nAll possible ways to find the address of the single pointer variable\n");
    printf("address of Pointer singlePointer is: %p\n", &b);
    printf("address of Pointer singlePointer using doublePointer is: %p\n", c); // c stores the address of b

    printf("\nAll possible ways to find double pointer value\n");
    printf("value of  Pointer doublePointer is: %p\n", c);

    printf("\nAll possible ways to find double pointer address\n");
    printf("address of Pointer doublePointer is: %p\n", &c);
    return 0;
}
```
