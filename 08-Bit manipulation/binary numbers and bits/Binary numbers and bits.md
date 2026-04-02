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

## Binary Representation
### Bit Positions and Values
```text
Binary: 1 0 1 0 1 0 1 0
Bits:   7 6 5 4 3 2 1 0  (position from right)
Value:  128 64 32 16 8 4 2 1

Example: 42 decimal = 00101010 binary
          0*128 + 0*64 + 1*32 + 0*16 + 1*8 + 0*4 + 1*2 + 0*1 = 42
```

### Data Type Sizes
```c
#include <stdio.h>
#include <limits.h>

int main() {
    printf("Type sizes in bits:\n");
    printf("char:      %zu bits\n", sizeof(char) * 8);
    printf("short:     %zu bits\n", sizeof(short) * 8);
    printf("int:       %zu bits\n", sizeof(int) * 8);
    printf("long:      %zu bits\n", sizeof(long) * 8);
    printf("long long: %zu bits\n", sizeof(long long) * 8);
    
    // Range of unsigned char: 0 to 255
    unsigned char byte = 255;  // 11111111 binary
    
    // Range of signed char: -128 to 127
    signed char signed_byte = 127;  // 01111111 binary
    
    return 0;
}
```
