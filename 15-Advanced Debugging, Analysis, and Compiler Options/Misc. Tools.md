## nm - List Symbols from Object Files
nm lists symbols (functions, variables) from object files, executables, or libraries.

### Basic Usage
```bash
# List all symbols
nm program.o
nm program
nm libmylib.a

# List symbols in a shared library
nm libmylib.so

# List all symbols (including debug)
nm -a program
```

## Symbol Types (Key Letters)
### T (Text Section) - Defined Functions
A T in the second column indicates a function that is defined

```c
// example.c
void defined_function(void) {  // This will show as T
    // function body
}

int global_var = 42;  // This will show as D (data)

int main() {  // Will show as T
    defined_function();
    return 0;
}
```

```bash
# Compile
gcc -c example.c -o example.o

# Check symbols
nm example.o
# Output:
# 0000000000000000 T defined_function
# 0000000000000000 D global_var
# 0000000000000014 T main
```

### U (Undefined) - External Symbols
a U indicates a function which is undefined and should be resolved by the linker

```c
// main.c
extern int external_var;  // Will show as U
extern void external_func(void);  // Will show as U

int main() {
    external_var = 42;
    external_func();
    return 0;
}
```

```bash
# Compile
gcc -c main.c -o main.o

# Check undefined symbols
nm -u main.o
# Output:
#                  U external_func
#                  U external_var
```

### ldd (List Dynamic Dependencies)
examines an executable and displays a list of the shared libraries that it needs

```bash
# Check dependencies of any executable
ldd /bin/bash

# Check dependencies of a shared library itself
ldd /usr/lib/libc.so.6

# Check a custom program in the current directory
ldd ./my_program
```

**Output**
```text
linux-vdso.so.1 (0x00007ffe5e7b6000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f5d7e000000)
/lib64/ld-linux-x86-64.so.2 (0x00007f5d7e3e5000)
```

* linux-vdso.so.1 is a virtual shared library provided by the kernel.
* libc.so.6 is the standard C library. The path => shows where it was found on the system.
* The final line shows the dynamic linker itself.
