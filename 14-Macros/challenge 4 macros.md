## challenge 4 macros
Write a C program to check whether a character is uppercase or lowercase using macros, you should create two macros that accept a single argument (character)
* IS_UPPER
* IS_LOWER
* both of the macros should return a boolean
* true (1) or false (0) based on whether they are upper or lower case

* Your macro will need to include a conditional and can use logical operators
* Your program should have the user enter any character (getchar())
* Your program should then display whether the character is upper or lower case as output by invoking the above macros

### Example output:
```text
Enter any character: C
'C' is uppercase

Enter any character: 8
Entered character is not in the alphabet
```

## Solution
```c
#include <stdio.h>

#define IS_UPPER(c) (((c) >= 'A' && (c) <= 'Z') ? 1 : 0)
#define IS_LOWER(c) (((c) >= 'a' && (c) <= 'z') ? 1 : 0)

int main()
{
    char c;

    printf("Enter any character: ");
    c = getchar();

    if (IS_UPPER(c))
    {
        printf("'%c' is uppercase\n", c);
    }
    else if (IS_LOWER(c))
    {
        printf("'%c' is lowercase\n", c);
    }
    else
    {
        printf("Entered character is not in the alphabet\n");
    }

    return 0;
}
```

## Solution 2
```c
#include <stdio.h>

#define IS_UPPER(c) (c >= 'A' && c <= 'Z')
#define IS_LOWER(c) (c >= 'a' && c <= 'z')

int main()
{
    char c;

    printf("Enter any character: ");
    c = getchar();

    if (IS_UPPER(c))
    {
        printf("'%c' is uppercase\n", c);
    }
    else if (IS_LOWER(c))
    {
        printf("'%c' is lowercase\n", c);
    }
    else
    {
        printf("Entered character is not in the alphabet\n");
    }

    return 0;
}
```
