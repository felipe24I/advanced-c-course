## Binary Number Basics
### Number Systems
```c
#include <stdio.h>

int main() {
    // Decimal (base 10): 0-9
    int decimal = 42;

    // Binary (base 2): 0-1
    int binary = 0b101010;  // Leading 0b 

    // Octal (base 8): 0-7
    int octal = 052;  // Leading 0

    // Hexadecimal (base 16): 0-9, A-F
    int hex = 0x2A;  // Leading 0x

    printf("Decimal: %d\n", decimal);     // 42
    printf("Binary: %d\n", binary);       // 42
    printf("Octal: %d\n", octal);         // 42
    printf("Hex: %d\n", hex);             // 42

    return 0;
}

```
