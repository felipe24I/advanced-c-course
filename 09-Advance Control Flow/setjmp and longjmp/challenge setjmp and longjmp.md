## Challenge setjmp and longjmp
Write a C program that uses setjmp and longjmp to handle unrecoverable errors. 
Create a function error_recovery that prints an error and uses longjmp to return control to the main loop. 
In main, use an infinite loop with setjmp at the top. When setjmp returns 0, simulate an error by calling error_recovery. 
The program should restart the loop cleanly after each error.

```c
#include <stdio.h>
#include <setjmp.h>

jmp_buf buf;

void error_recovery()
{
    printf("detected an undrecoverable error\n");
    longjmp(buf, 1);
}

int main()
{

  while (1)
    {
        if (setjmp(buf))
        {
            printf("back in main\n");
            break;
        }
        else
        {
            error_recovery();
        }
    }
}
```
