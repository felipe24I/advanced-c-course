## Challenge 3 macros
Write a C program to find the square and cube of a number using macros, You should create two macros
* a SQUARE macro
* a CUBE macro

* You need to figure out how many parameters there should be
* Your program should have the user enter any number
* Your program should then display the square and cube of the number as output by invoking the above macros

### Example output
```text
Enter any number to find square and cube: 10
SQUARE(10) = 100
CUBE(10) = 1000
```

## Solution
```c
# include <stdio.h>

#define SQUARE(a) printf("SQUARE(%d) = %d\n", a, a*a);
#define CUBE(a) printf("CUBE(%d) = %d\n", a, a*a*a);

int main()
{
    int a;

    printf("Enter any number to find suare and cube: ");
    scanf("%d", &a);

    SQUARE(a);
    CUBE(a);

    return 0;

}
```
