## Debugging
Debugging is the systematic process of finding and fixing errors—known as "bugs"—within a computer program or system

## Preprocessor Debugging
Debugging with the preprocessor involves using preprocessor directives and techniques to aid in debugging, such as conditional compilation, debug macros, and assertion mechanisms.

### Basic Debug Macro
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


