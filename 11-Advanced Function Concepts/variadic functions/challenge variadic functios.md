## Challenge variadic functios
You need to write a variadic function that takes a variable number of integer arguments. 
The first argument specifies how many numbers follow, and the function returns their sum. 
The program should demonstrate this by calling the function with different counts of arguments.

### Output example
```text
addingNumbers(2, 10, 20) → 30
addingNumbers(3, 10, 20, 30) → 60
addingNumbers(4, 10, 20, 30, 40) → 100
```

## Solution
```c
#include <stdio.h>
#include <stdarg.h>

int addingNumbers (int count, ...)
{
    int result = 0;
    int number;
    va_list args;
    va_start(args, count);

    for (int i = 0; i < count; i++)
    {
        number = va_arg(args, int);
        result += number;
    }

    va_end(args);

    return result;

}

int main()
{
    int sum = addingNumbers(2, 10, 20);
    int sum2 = addingNumbers(3, 10, 20, 30);
    int sum3 = addingNumbers(4, 10, 20, 30, 40);

    printf("sum= %d\n", sum);
    printf("sum2= %d\n", sum2);
    printf("sum3= %d\n", sum3);

    return 0;
}
```
