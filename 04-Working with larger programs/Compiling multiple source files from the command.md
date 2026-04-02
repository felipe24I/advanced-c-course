## Compiling multiple source files from the command line
### Basic Concepts
When you have a program split across multiple .c files, you need to compile them together and link them into a single executable.
```bash
# Basic compilation of multiple files
gcc file1.c file2.c file3.c -o program

# Or compile separately then link
gcc -c file1.c -o file1.o
gcc -c file2.c -o file2.o
gcc -c file3.c -o file3.o
gcc file1.o file2.o file3.o -o program
```

## Simple Example
### File Structure
```c
// main.c
#include <stdio.h>
#include "math_utils.h"
#include "string_utils.h"

int main() {
    printf("5 + 3 = %d\n", add(5, 3));
    printf("10 - 4 = %d\n", subtract(10, 4));
    
    char str[] = "Hello";
    capitalize(str);
    printf("Capitalized: %s\n", str);
    
    return 0;
}
```

```c
// math_utils.h
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

int add(int a, int b);
int subtract(int a, int b);

#endif
```

```c
// math_utils.c
#include "math_utils.h"

int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}
```

```c
// string_utils.h
#ifndef STRING_UTILS_H
#define STRING_UTILS_H

void capitalize(char *str);

#endif
```

```c
// string_utils.c
#include "string_utils.h"
#include <ctype.h>

void capitalize(char *str) {
    if (str && *str) {
        *str = toupper(*str);
    }
}
```

### Compilation Commands
```bash
# One-step compilation (simplest)
gcc main.c math_utils.c string_utils.c -o program

# Run the program
./program

# Output:
# 5 + 3 = 8
# 10 - 4 = 6
# Capitalized: Hello
```

## Step-by-Step Compilation
### 1. Compile to Object Files
```bash
# Compile each .c file to .o (object) file
gcc -c main.c -o main.o
gcc -c math_utils.c -o math_utils.o
gcc -c string_utils.c -o string_utils.o

# The -c flag means: compile only, don't link
# This creates machine code but with unresolved symbols
```

### 2. Link Object Files
```bash
# Link all object files into executable
gcc main.o math_utils.o string_utils.o -o program

# Or with additional libraries
gcc main.o math_utils.o string_utils.o -lm -o program
```

### 3. Run the Program
```bash
# Execute
./program

# On Windows
program.exe
```
