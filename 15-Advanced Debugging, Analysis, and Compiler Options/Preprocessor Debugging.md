## Debugging
Debugging is the systematic process of finding and fixing errors—known as "bugs"—within a computer program or system

## Preprocessor Debugging
Debugging with the preprocessor involves using preprocessor directives and techniques to aid in debugging, such as conditional compilation, debug macros, and assertion mechanisms.

### Basic Debug Macros
### Example 1
```c
#include <stdio.h>

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

#### Output example:
```bash
# Compile with DEBUG to enable debug
gcc -D DEBUG program.c -o program
```

```bash
./program
5 6 e
Number of integers read = 2
i = 5, j = 6, k = -2144863424
-2144863413
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

#### Output example:
```bash
# Compile with DEBUG to enable debug
gcc -D DEBUG program.c -o program
```

```bash
./program 5 10
processed 2 arguments
arg1 = 5, arg2 = 10
process(5, 10)
return 50
50
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

### Debug levels
Debug levels are a systematic way to control the verbosity of debug output, allowing you to enable more detailed logging as needed without changing code. Higher levels typically provide more detailed information.

### Example 1
```c
#include <stdio.h>
#include <stdlib.h>

int Debug = 0;

#ifdef DEBON
    #define DEBUG(level, fmt, ...)\
        if (Debug >= level)\
            fprintf(stderr, fmt, __VA_ARGS__)
#else
    #define DEBUG(level, fmt, ...)
#endif

int process (int i, int j)
{
    int val = 0;

    DEBUG(1, "process(%d, %d)\n", i, j);

    val = i * j;

    DEBUG(3, "return %d\n", val);

    return val;
}

int main(int argc, char* argv[])
{
    int arg1 = 0, arg2 = 0;

    if (argc > 2)
    {
        Debug = atoi(argv[1]);
        arg1 = atoi (argv[2]);
    }

    if (argc == 4)
        arg2 = atoi (argv[3]);

    DEBUG(2, "processed %i arguments\n", argc - 1);
    DEBUG(3, "arg1 = %d, arg2 = %d\n", arg1, arg2);

    printf("%d\n", process (arg1, arg2));

    return 0;
}
```

## Code Analysis
### The Debug Macro
```c
#ifdef DEBON
    #define DEBUG(level, fmt, ...)\
        if (Debug >= level)\
            fprintf(stderr, fmt, __VA_ARGS__)
#else
    #define DEBUG(level, fmt, ...)
#endif
```

#### What this does:
* If DEBON is defined (via compiler flag -DDEBON), the DEBUG macro is active
* If DEBON is NOT defined, the macro becomes empty (no debug output)
* The macro checks if the global Debug variable is >= the requested level

#### The Global Debug Variable
```c
int Debug = 0;
```

This variable controls the debug level at **runtime** (not compile time). Higher values = more verbose output.

#### How Debug Levels Work in This Example
* **Level 1:** Function entry/exit, **output:** process(10, 20)
* **Level 2:** Major operations, **output:** processed 2 arguments
* **Level 3:** Detailed info, **output:** arg1 = 10, arg2 = 20, return 200

#### Compilation and Execution Examples
#### 1. Debug Disabled (Default)
```bash
# Compile without DEBON - DEBUG macro does nothing
gcc program.c -o program

# Run
./program 2 10 20
```

#### Output:
```text
200
```

No debug output at all because the macro expands to nothing.

#### 2. Debug Enabled with Different Levels
```bash
# Compile with DEBON to enable debug
gcc -D DEBON program.c -o program
```

#### Run with different debug levels:
**Level 1:**

```bash
# Level 1 - Shows function entry only
./program 1 10 20
```

#### Output
```text
process(10, 20)
200
```

**Level 2:**

```bash
# Level 2 - Shows level 1 + level 2 messages
./program 2 10 20
```

#### Output
```text
process(10, 20)
processed 2 arguments
200
```

**Level 3:**

```bash
# Level 3 - Shows everything
./program 3 10 20
```

#### Output
```text
process(10, 20)
processed 2 arguments
arg1 = 10, arg2 = 20
return 200
200
```

### Key Features of This Design
#### 1. Compile-time Toggle (DEBON)
```bash
# Debug enabled
gcc -D DEBON program.c -o program

# Debug disabled (production build)
gcc program.c -o program
```

#### 2. Runtime Level Control
```bash
./program 0 ...  # No debug output
./program 1 ...  # Function calls only
./program 2 ...  # + major operations
./program 3 ...  # + detailed info
```

#### 3. Selective Output Based on Level
```c
DEBUG(1, "process(%d, %d)\n", i, j);  // Level 1 - function entry
DEBUG(2, "processed %i arguments\n", argc - 1);  // Level 2 - major ops
DEBUG(3, "arg1 = %d, arg2 = %d\n", arg1, arg2);  // Level 3 - details
DEBUG(3, "return %d\n", val);  // Level 3 - return value
```
