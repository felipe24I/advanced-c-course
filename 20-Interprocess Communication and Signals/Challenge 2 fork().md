## Challenge 2 fork()
The problem consists of creating a C program that uses fork() to generate a process hierarchy with one parent process and three child processes. 
The program must identify each process as the parent, first child, second child, or third child, and print its process ID using getpid() 
and its parent process ID using getppid(). Since process scheduling is controlled by the operating system, the output order may change each time the program runs.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main(void)
{
    pid_t pid1, pid2, pid3;

    pid1 = fork();

    if (pid1 == 0)
    {
        printf("\nfirst child\n");
        printf("my id is %d\n", getpid());
        printf("my parent id is %d\n", getppid());
    }
    else
    {
        pid2 = fork();

        if (pid2 == 0)
        {
            printf("\nsecond child\n");
            printf("my id is %d\n", getpid());
            printf("my parent id is %d\n", getppid());
        }
        else
        {
            pid3 = fork();

            if (pid3 == 0)
            {
                printf("\nthird child\n");
                printf("my id is %d\n", getpid());
                printf("my parent id is %d\n", getppid());
            }
            else
            {
                printf("\nparent\n");
                printf("my id is %d\n", getpid());
                printf("my parent id is %d\n", getppid());

                wait(NULL);
                wait(NULL);
                wait(NULL);
            }
        }
    }

    return 0;
}
```

Run it on Linux/WSL, because fork() does not work in normal Windows MinGW.
