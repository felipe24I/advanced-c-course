## Challenge flexible array members
You need to write a program that defines a structure with a flexible array member. 
The program should ask the user for the array size, allocate the appropriate amount of memory for the structure, store the size along with dummy data in the flexible array, and then print the array elements.

### Output example
```text
Enter the size of the array: 5
Array elements: 0, 1, 2, 3, 4
```

## Solution
```c
#include <stdio.h>
#include<stdlib.h>

typedef struct myArray
{
    int arraySize;
    int array[];
}arr;

void Fill_array (size_t tam, arr *array1)
{
    char buffer [20];
    for (size_t i = 0; i < tam; i++)
    {
        printf("Enter a number in [%zd]: ", i);
        fgets(buffer, 20, stdin);
        array1->array[i] = atoi(buffer);
    }
}

void print_array (size_t tam, arr *array1)
{
    for (size_t i = 0; i < tam; i++)
    {
        printf("%d ", array1->array[i]);
    }
}

int main()
{
    size_t desiredSize;

    printf("Enter a desired size: ");
    scanf("%zd", &desiredSize);

    getchar();

    arr *array1 = malloc(sizeof(arr) + desiredSize * sizeof(int));

    if (array1 == NULL)
    {
        printf("Memory allocation failed\n");
        return 1;
    }

    array1->arraySize = desiredSize;

    Fill_array(desiredSize, array1);

    print_array(desiredSize, array1);

    free(array1);

    return 0;




}
```
