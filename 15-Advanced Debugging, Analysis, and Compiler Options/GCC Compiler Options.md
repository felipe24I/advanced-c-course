## Compilation steps
<img width="536" height="565" alt="image" src="https://github.com/user-attachments/assets/2929d7e1-e082-4c8b-99c5-dfbc56938623" />

## GCC Compiler Options
GCC (GNU Compiler Collection) options are command-line flags that control how the compiler processes source code, including optimization, warnings, debugging, and output generation.

## Basic Compilation Options
### Input/Output Options
```bash
# Basic compilation
gcc program.c                    # Creates a.out
gcc program.c -o program         # Names output 'program'
gcc -g program.c -o program.exe  # with -g flag (debugging information) includes debug symbols (variable names, line numbers, function names)
gcc -c program.c -o program.o    # Compile only, no linking
gcc program.o -o program         # Link object file

# Multiple files
gcc main.c helper.c -o program   # Compile and link multiple files
gcc -c main.c -o main.o          # Compile to object
gcc -c helper.c -o helper.o      # Compile to object
gcc main.o helper.o -o program   # Link objects

# Verbose output
gcc -v program.c                  # Show compilation details
gcc -### program.c                # Show commands without executing
```

### Windows CMD/PowerShell:
```bash
# These all create program.exe
gcc main.c -o program       # Creates program.exe
gcc main.c -o program.exe   # Creates program.exe
gcc main.c                  # Creates a.exe

# Running them:
program                     # Runs program.exe
program.exe                 # Also runs program.exe
.\program                   # Works
.\program.exe               # Works
```

### Linux Terminal:
```bash
# Different results
gcc main.c -o program       # Creates "program" (no extension)
gcc main.c -o program.exe   # Creates "program.exe" (literal name)

# Running them:
./program                   # Runs the no-extension version
./program.exe               # Runs the .exe version if it exists

# .exe means nothing special:
file program.exe            # May show "ELF executable" (not Windows exe)
```

### Language Standard Options
```bash
# C standards
gcc -std=c89 program.c            # ANSI C (C89/C90)
gcc -std=c99 program.c            # C99 standard
gcc -std=c11 program.c            # C11 standard
gcc -std=c17 program.c            # C17 standard
gcc -std=c23 program.c            # C23 standard (experimental)
gcc -std=gnu11 program.c          # C11 with GNU extensions

# Check standard
gcc -std=c99 -pedantic program.c  # Strict compliance
gcc -std=c99 -pedantic-errors     # Warnings become errors
```

## Warning Options
### Basic Warnings
```bash
# Essential warnings (USE THESE!)
gcc -Wall program.c                # All common warnings
gcc -Wextra program.c              # Extra warnings
gcc -Werror program.c              # Treat warnings as errors
gcc -Wshadow program.c             # Warn about variable shadowing
gcc -Wconversion program.c         # Warn about type conversions

# All warnings together
gcc -Wall -Wextra -Werror -Wshadow -Wconversion program.c
```

### Specific Warning Options
```bash
# Unused items
gcc -Wunused-variable program.c    # Unused variables
gcc -Wunused-function program.c    # Unused functions
gcc -Wunused-parameter program.c   # Unused parameters
gcc -Wunused-value program.c       # Unused return values

# Initialization
gcc -Wuninitialized program.c      # Uninitialized variables
gcc -Wmaybe-uninitialized program.c # Possibly uninitialized

# Format strings
gcc -Wformat program.c             # Check printf/scanf format
gcc -Wformat-security program.c    # Format string security

# Type issues
gcc -Wsign-compare program.c       # Signed/unsigned comparison
gcc -Wstrict-prototypes program.c  # Non-prototype declarations
gcc -Wmissing-prototypes program.c # Missing function prototypes

# Switch statements
gcc -Wswitch program.c             # Missing enum cases
gcc -Wswitch-default program.c     # No default case
gcc -Wswitch-enum program.c        # Missing enum cases (all)

# Pointers
gcc -Wpointer-arith program.c      # Pointer arithmetic on void*
gcc -Wcast-align program.c         # Cast increases alignment

# Other useful warnings
gcc -Wfloat-equal program.c        # Floating point equality
gcc -Wparentheses program.c        # Suggest parentheses
gcc -Wsequence-point program.c     # Sequence point violations
```

## Optimization Options
### Optimization Options
```bash
# No optimization (default, best for debugging)
gcc -O0 program.c -o program

# Basic optimization (good balance for debugging)
gcc -O1 program.c -o program
gcc -O program.c -o program        # Same as -O1

# Moderate optimization (recommended for release)
gcc -O2 program.c -o program

# Aggressive optimization (may increase code size)
gcc -O3 program.c -o program

# Optimize for size (good for embedded)
gcc -Os program.c -o program

# Fast math (may break strict compliance)
gcc -Ofast program.c -o program

# Optimize for specific architecture
gcc -march=native -O2 program.c    # Optimize for current CPU
```

