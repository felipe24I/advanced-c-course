## Challenge 2 macros
* Write a program that defines a macro that accepts two parameters and returns the sum of the given numbers

```text
#define MACRO_NAME(params) MACRO_BODY
```

* MACRO_NAME should be SUM
* params are the parameters passed to macro
* MACRO_BODY is the code for the actual logic of the macro
* Your program should have the user enter the two numbers
* Your program should then display the sum as output by invoking the above macro

## Solution
```c
#include <stdio.h>

# define SUM(a, b) printf("%d + %d = %d\n", a, b, a+b)

int main()
{
    int a, b;

    printf("Enter a: "); // 4
    scanf("%d", &a);

    printf("Enter b: "); // 5
    scanf("%d", &b);

    SUM(a, b); // 4 + 5 = 9

    return 0;
}
```
