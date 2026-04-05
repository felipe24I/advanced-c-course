## GCC Environment Variables
Environment variables control GCC's behavior, affecting how it finds headers, libraries, and tools. 
They can override default paths and add search directories without modifying command-line options.

## Core GCC Environment Variables
### 1. PATH - Executable Search Path
Controls where GCC looks for other executables (assembler, linker, etc.).

```bash
# Add custom bin directory to PATH
export PATH=/usr/local/custom/bin:$PATH

# Check current PATH
echo $PATH

# Add multiple directories
export PATH=/opt/gcc/bin:/usr/local/bin:$PATH
```

### 2. CPATH
is an environment variable that specifies additional directories to search for header files (.h files) when compiling C and C++ programs. 
It's a convenient alternative to using -I flags on the command line.

```bash
# Basic usage
export CPATH=/custom/include:/opt/libs/include

# Compile without -I flags
gcc program.c  # Automatically searches CPATH directories
```

### 3. LIBRARY_PATH - Linker Search Path
Specifies directories where the linker searches for libraries (both static and shared).

```bash
# Add library directories
export LIBRARY_PATH=/usr/local/lib:/opt/libs/lib

# Multiple paths (colon-separated)
export LIBRARY_PATH=/custom/lib1:/custom/lib2:$LIBRARY_PATH

# Use for compilation
gcc program.c -lmyLib  # Searches in LIBRARY_PATH
```
