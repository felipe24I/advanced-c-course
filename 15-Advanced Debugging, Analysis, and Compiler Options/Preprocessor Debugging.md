## Debugging
Debugging is the systematic process of finding and fixing errors—known as "bugs"—within a computer program or system

## Preprocessor Debugging
Debugging with the preprocessor involves using preprocessor directives and techniques to aid in debugging, such as conditional compilation, debug macros, and assertion mechanisms.

### Basic Debug Macros
### Example 1
```c
#include <stdio.h>

#define DEBUG

int process (int i, int j, int k)
{
    return i + j + k;
}

int main()
{
    int i, j, k, nread;

    nread = scanf("%d %d %d", &i, &j, &k);

    #ifdef DEBUG
        fprintf(stderr, "Number of integers read = %i\n", nread);
        fprintf(stderr, "i = %d, j = %d, k = %d\n", i, j, k);
    #endif // DEBUG

    printf("sum= %d\n", process(i, j, k));

    return 0;
}
```

### Example 2
```c
#include <stdio.h>
#include <stdlib.h>

int process (int i, int j)
{
    int val = 0;

    #ifdef DEBUG
        fprintf(stderr, "process(%d, %d)\n", i, j);
    #endif // DEBUG

    val = i * j;

    #ifdef DEBUG
        fprintf(stderr, "return %d\n", val);
    #endif // DEBUG

    return val;
}

int main(int argc, char* argv[])
{
    int arg1 = 0, arg2 = 0;

    if (argc > 1)
        arg1 = atoi (argv[1]);
    if (argc == 3)
        arg2 = atoi (argv[2]);

    #ifdef DEBUG
        fprintf(stderr, "processed %i arguments\n", argc - 1);
        fprintf(stderr, "arg1 = %d, arg2 = %d\n", arg1, arg2);
    #endif // DEBUG

    printf("%d\n", process (arg1, arg2));

    return 0;
}
```

### Example 3
```c
#include <stdio.h>
#include <stdlib.h>

#define DEBUG(fmt, ...) fprintf(stderr, fmt, __VA_ARGS__)

int process (int i, int j)
{
    int val = 0;

    DEBUG("process(%d, %d)\n", i, j);

    val = i * j;

    DEBUG("return %d\n", val);

    return val;
}

int main(int argc, char* argv[])
{
    int arg1 = 0, arg2 = 0;

    if (argc > 1)
        arg1 = atoi (argv[1]);
    if (argc == 3)
        arg2 = atoi (argv[2]);

    DEBUG("processed %i arguments\n", argc - 1);
    DEBUG("arg1 = %d, arg2 = %d\n", arg1, arg2);

    printf("%d\n", process (arg1, arg2));

    return 0;
}
```
