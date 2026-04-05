## Challenge 1
You need to write a "Hello, World!" program and then manually compile it from the command line using various options 
(e.g., changing the output name, adding debugging info, enabling warnings, treating warnings as errors). 
You also need to explore similar build options in an IDE like Code::Blocks.

## Solution
```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

### Manual Compilation with Various GCC Options
### Basic Compilation and Adding Debugging Information
```bash
gcc -g main.c -o main.exe
```
