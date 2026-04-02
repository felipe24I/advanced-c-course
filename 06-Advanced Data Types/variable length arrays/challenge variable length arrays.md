## Challenge variable length arrays
You need to write a program that uses a variable length array (VLA) to store user-input elements and then calculates and displays the sum of those elements.

### Output Example
```text
Input elements: 10, 20, 30, 40, 50
Sum of all elements = 150
```

## Solution
```c
#include <stdio.h>

void Fill_array(size_t tam, int array[tam])
{
    for ( size_t i = 0; i < tam; i++)
    {
        printf("\nEnter number in [%d]: ", i);
        scanf("%d", &array[i]);
    }
}

int sum (size_t tam, int array[tam])
{
    int sum = 0;

    for ( size_t i = 0; i < tam; i++)
    {
        sum += array[i];
    }

    return sum;
}

int main()
{
    size_t tam;

    printf("Enter the size of the array: ");
    scanf("%zd", &tam);

    int array[tam];

    Fill_array(tam, array);

    int result = sum(tam, array);

    printf("\nSum= %d", result);
}
```
