## What is Building/Compiling?
Building/compiling is the process of converting human-readable C source code into machine-executable binary code. This involves multiple stages:

```text
Source Code (.c) → Preprocessor → Compiler → Assembler → Linker → Executable
```

## The Complete Build Process
### Stage 1: Preprocessing
```bash
# Generate preprocessed output (.i file)
gcc -E main.c -o main.i
```

```c
// main.c (source)
#include <stdio.h>
#define PI 3.14159

int main() {
    printf("PI = %f\n", PI);
    return 0;
}
```

```c
// main.i (preprocessed - expands includes, macros, etc.)
// ... thousands of lines from stdio.h ...
int main() {
    printf("PI = %f\n", 3.14159);
    return 0;
}
```

### Stage 2: Compilation to Assembly
```bash
# Generate assembly code (.s file)
gcc -S main.i -o main.s
```

```assembly
; main.s (assembly code)
main:
    pushq   %rbp
    movq    %rsp, %rbp
    movl    $.LC0, %edi
    movsd   .LC1(%rip), %xmm0
    movq    %rax, %rdi
    call    printf
    movl    $0, %eax
    popq    %rbp
    ret
```

### Stage 3: Assembly to Object Code
```bash
# Generate object file (.o or .obj)
gcc -c main.s -o main.o
# or directly from source
gcc -c main.c -o main.o
```

### Stage 4: Linking
```bash
# Generate executable
gcc main.o -o program
# or all at once
gcc main.c -o program
```

## Complete Build Example
### Multi-file Project
```c
// math.h
#ifndef MATH_H
#define MATH_H
int add(int a, int b);
int subtract(int a, int b);
#endif

// math.c
#include "math.h"
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }

// main.c
#include <stdio.h>
#include "math.h"
int main() {
    printf("10 + 5 = %d\n", add(10, 5));
    printf("10 - 5 = %d\n", subtract(10, 5));
    return 0;
}
```

### Manual Build Steps
```bash
# Step 1: Compile each source file to object code
gcc -c main.c -o main.o
gcc -c math.c -o math.o

# Step 2: Link object files into executable
gcc main.o math.o -o program

# Step 3: Run the program
./program
```

### One-Step Build
```bash
# Compile and link in one command
gcc main.c math.c -o program
```

## Compiler Flags and Options
### Essential GCC Flags
```bash
# Basic compilation
gcc program.c                    # Creates a.out
gcc program.c -o program         # Names output 'program'

# Warning flags (always use these!)
gcc -Wall program.c              # All warnings
gcc -Wextra program.c            # Extra warnings
gcc -Werror program.c            # Treat warnings as errors
gcc -pedantic program.c          # Strict ISO C compliance

# Optimization levels
gcc -O0 program.c                # No optimization (default)
gcc -O1 program.c                # Basic optimization
gcc -O2 program.c                # Good optimization
gcc -O3 program.c                # Aggressive optimization
gcc -Os program.c                # Optimize for size
gcc -Ofast program.c             # Fast but may break standards

# Debugging
gcc -g program.c                 # Include debug symbols
gcc -ggdb program.c              # GDB-specific debug info

# Standard version
gcc -std=c89 program.c           # ANSI C standard
gcc -std=c99 program.c           # C99 standard
gcc -std=c11 program.c           # C11 standard
gcc -std=c17 program.c           # C17 standard
gcc -std=gnu17 program.c         # GNU extensions

# Preprocessor definitions
gcc -DDEBUG program.c            # #define DEBUG 1
gcc -DVERSION=2 program.c        # #define VERSION 2

# Include paths
gcc -I./include program.c        # Add include directory
gcc -I/usr/local/include program.c

# Library paths
gcc -L./lib program.c -lmylib    # Add library path
gcc -lm program.c                # Link math library

# Output control
gcc -v program.c                 # Verbose output
gcc -save-temps program.c        # Save intermediate files
```

## Common Flag Combinations
```bash
# Development build (fast compilation, debug info)
gcc -g -O0 -Wall -Wextra program.c -o program

# Release build (optimized, no debug)
gcc -O3 -DNDEBUG -Wall program.c -o program

# Strict C99 compliance
gcc -std=c99 -pedantic -Wall -Wextra -Werror program.c -o program

# Small executable size
gcc -Os -s program.c -o program

# Profile-guided optimization
gcc -fprofile-generate program.c -o program
./program  # run to generate profile
gcc -fprofile-use program.c -o program
```

## Build Process Visualization
```c
// Example showing each stage's output
#include <stdio.h>

#define HELLO "World"

int main() {
    printf("Hello, %s!\n", HELLO);
    return 0;
}
```

```bash
# 1. Preprocess (see expanded code)
gcc -E hello.c
# Output: Expanded code with stdio.h and macro replaced

# 2. Compile to assembly
gcc -S hello.c
# Creates hello.s

# 3. Compile to object (binary)
gcc -c hello.c
# Creates hello.o

# 4. Display object file info
file hello.o
# hello.o: ELF 64-bit LSB relocatable, x86-64

# 5. Link to executable
gcc hello.o -o hello
# Creates executable 'hello'

# 6. Inspect executable
file hello
# hello: ELF 64-bit LSB executable, x86-64
size hello
# text    data     bss     dec     hex filename
# 1324     552       8    1884     75c hello

# 7. Run
./hello
# Hello, World!

# 8. Check dependencies
ldd hello
# linux-vdso.so.1 (0x00007ffe...)
# libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6
```

## Using Makefiles for Building
### Simple Makefile
```makefile
# Makefile
CC = gcc
CFLAGS = -Wall -Wextra -O2 -g
TARGET = program
SOURCES = main.c math.c io.c
OBJECTS = $(SOURCES:.c=.o)

all: $(TARGET)

$(TARGET): $(OBJECTS)
	$(CC) $(OBJECTS) -o $@

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJECTS) $(TARGET)

.PHONY: all clean
```

```bash
# Using Makefile
make          # Build program
make clean    # Remove object files
make -j4      # Parallel build with 4 jobs
make -n       # Dry run (show commands)
make -B       # Force rebuild
```
