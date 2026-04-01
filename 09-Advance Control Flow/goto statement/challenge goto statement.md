## Challenge goto statement
Create a program that uses only goto statements (no looping constructs like for, while, or do-while) to print a triangular pattern of asterisks. 
The program must simulate nested looping behavior using goto to control both row and column iteration, resulting in an output that forms a triangle shape

<img width="107" height="145" alt="image" src="https://github.com/user-attachments/assets/5f9e7469-c203-4429-991c-9ab090aaba45" />

## Solution
```c
#include <stdio.h>

int main()
{
    char str[9];
    int i = 0;
    int j;

    rows_loop:

        if (i >= 5) goto end;

        j = 0;

    columns_loop:

        if (j >= 9) goto next_i;
        if (i == 4)
        {
            str[j] = '*';
        }
        else if ((j == 4+i) | (j == 4-i))
        {
            str[j] = '*';
        }
        else
        {
            str[j] = ' ';
        }
        printf("%c", str[j]);
        j++;

        goto columns_loop;

    next_i:
        i++;
        printf("\n");
        goto rows_loop;


    end:
        return 0;
}
```
