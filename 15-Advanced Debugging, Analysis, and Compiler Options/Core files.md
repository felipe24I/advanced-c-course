## Core Dump
A core dump (or core file) is a snapshot of program memory at the moment it crashed. It contains:
* Memory contents
* Register values
* Call stack
* Program state

Think of it as a crash scene photograph you can analyze later.

### Example program
```c
#include <stdio.h>

int main() {
    int *p;
    printf("%d", *p);
    return 0;
}
```

**ls -lrt:** This command lists files in reverse time order (oldest last/most recent at bottom).

**a.exe.stackdump:** In Cygwin, when a program crashes, it creates a .stackdump file instead of a traditional Unix core dump.

```text
a.exe.stackdump
```
This file contains:
* Stack trace at crash
* Register values
* Crash information

**Loading a Stack Dump in GDB**

```bash
gdb a.exe a.exe.stackdump
```

This tells GDB to: 
1. Load the executable a.exe (with debug symbols)
2. Load the stack dump file for analysis

In mi case, a.exe.stackdump was not generate automatically

### How to Get a Stack Dump Manually
If you want to save the crash state for later:

#### Method 1: Generate core dump from within GDB
```bash
(gdb) generate-core-file
Saved corefile core.12345
(gdb) quit

# Later analyze it:
gdb a.exe core.12345
```

