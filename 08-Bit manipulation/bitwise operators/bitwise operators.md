## Bitwise Operators
### 1. AND (&) - Bitwise AND
```c
#include <stdio.h>

int main() {
    unsigned char a = 0b10101010;  // 170
    unsigned char b = 0b11001100;  // 204
    
    unsigned char result = a & b;  // 10001000 (136)
    
    printf("a & b = %u\n", result);
    
    // Common use: Bit masking
    unsigned char value = 0b10110101;
    unsigned char mask = 0b00001111;  // Lower 4 bits
    unsigned char lower_nibble = value & mask;  // 00000101
    
    printf("Lower nibble: %u\n", lower_nibble);  // 5
    
    return 0;
}
```

### OR (|) - Bitwise OR
```c
#include <stdio.h>

int main() {
    unsigned char a = 0b10101010;  // 170
    unsigned char b = 0b11001100;  // 204
    
    unsigned char result = a | b;  // 11101110 (238)
    
    printf("a | b = %u\n", result);
    
    // Common use: Setting bits
    unsigned char flags = 0b00000000;
    flags = flags | 0b00000001;  // Set bit 0
    flags |= 0b00000010;         // Set bit 1
    
    printf("Flags: %u\n", flags);  // 3 (00000011)
    
    return 0;
}
```

### 3. XOR (^) - Bitwise Exclusive OR
```c
#include <stdio.h>

int main() {
    unsigned char a = 0b10101010;  // 170
    unsigned char b = 0b11001100;  // 204
    
    unsigned char result = a ^ b;  // 01100110 (102)
    
    printf("a ^ b = %u\n", result);
    
    // Common use: Toggling bits
    unsigned char flags = 0b00001111;
    flags ^= 0b00000101;  // Toggle bits 0 and 2
    // Result: 00001010
    
    // XOR swap (no temporary variable)
    int x = 5, y = 10;
    x ^= y;
    y ^= x;
    x ^= y;
    printf("x=%d, y=%d\n", x, y);  // x=10, y=5
    
    return 0;
}
```

### 4. NOT (~) - Bitwise Complement
```c
#include <stdio.h>

int main() {
    unsigned char a = 0b10101010;  // 170
    unsigned char result = ~a;     // 01010101 (85)
    
    printf("~a = %u\n", result);
    
    // For signed integers, ~x = -x - 1
    int b = 42;
    printf("~42 = %d\n", ~b);  // -43
    
    return 0;
}
```

### 5. Left Shift (<<)
```c
#include <stdio.h>

int main() {
    unsigned char a = 0b00001010;  // 10
    
    unsigned char result1 = a << 1;  // 00010100 (20) - multiply by 2
    unsigned char result2 = a << 2;  // 00101000 (40) - multiply by 4
    unsigned char result3 = a << 3;  // 01010000 (80) - multiply by 8
    
    printf("10 << 1 = %u\n", result1);
    printf("10 << 2 = %u\n", result2);
    printf("10 << 3 = %u\n", result3);
    
    // Building values from bits
    unsigned char byte = (1 << 7) | (1 << 3) | (1 << 0);
    // 10001001 binary = 137 decimal
    
    return 0;
}
```

### 6. Right Shift (>>)
```c
#include <stdio.h>

int main() {
    unsigned char a = 0b10101000;  // 168
    
    unsigned char result1 = a >> 1;  // 01010100 (84) - divide by 2
    unsigned char result2 = a >> 2;  // 00101010 (42) - divide by 4
    
    printf("168 >> 1 = %u\n", result1);
    printf("168 >> 2 = %u\n", result2);
    
    // Logical vs Arithmetic shift
    signed char b = -16;  // 11110000 in two's complement
    signed char arith_shift = b >> 1;  // 11111000 (-8) - sign extended
    unsigned char logical_shift = (unsigned char)b >> 1;  // 01111000 (120)
    
    printf("Arithmetic shift: %d\n", arith_shift);
    printf("Logical shift: %u\n", logical_shift);
    
    return 0;
}
```

## Common Bit Manipulation Techniques
### 1. Setting, Clearing, and Toggling Bits
```c
#include <stdio.h>
#include <stdint.h>

typedef struct {
    uint8_t flags;
} Status;

// Macros for bit operations
#define SET_BIT(reg, bit)   ((reg) |= (1 << (bit)))
#define CLEAR_BIT(reg, bit) ((reg) &= ~(1 << (bit)))
#define TOGGLE_BIT(reg, bit) ((reg) ^= (1 << (bit)))
#define CHECK_BIT(reg, bit) (((reg) >> (bit)) & 1)

int main() {
    uint8_t reg = 0b00000000;
    
    // Set bits 0 and 3
    SET_BIT(reg, 0);  // 00000001
    SET_BIT(reg, 3);  // 00001001
    printf("After setting: 0x%02X\n", reg);
    
    // Clear bit 3
    CLEAR_BIT(reg, 3);  // 00000001
    printf("After clearing: 0x%02X\n", reg);
    
    // Toggle bit 0
    TOGGLE_BIT(reg, 0);  // 00000000
    printf("After toggling: 0x%02X\n", reg);
    
    // Check bit 0
    printf("Bit 0 is: %d\n", CHECK_BIT(reg, 0));
    
    return 0;
}
```

### 2. Extracting and Inserting Bits
```c
#include <stdio.h>
#include <stdint.h>

// Extract bits from position 'start' with 'count' bits
uint32_t extract_bits(uint32_t value, int start, int count) {
    return (value >> start) & ((1 << count) - 1);
}

// Insert 'bits' into 'value' at position 'start'
uint32_t insert_bits(uint32_t value, uint32_t bits, int start, int count) {
    uint32_t mask = ((1 << count) - 1) << start;
    return (value & ~mask) | ((bits << start) & mask);
}

int main() {
    uint32_t value = 0b1101011010101101;
    
    // Extract bits 4-7 (4 bits)
    uint32_t extracted = extract_bits(value, 4, 4);
    printf("Extracted: 0x%X\n", extracted);
    
    // Insert new bits at position 4
    value = insert_bits(value, 0b1010, 4, 4);
    printf("After insert: 0x%X\n", value);
    
    return 0;
}
```

### 3. Bit Fields in Structures
```c
#include <stdio.h>

// Bit fields (compiler-dependent, but useful)
struct DeviceStatus {
    unsigned int power_on   : 1;  // Bit 0
    unsigned int error      : 1;  // Bit 1
    unsigned int mode       : 2;  // Bits 2-3
    unsigned int temperature: 4;  // Bits 4-7
    unsigned int reserved   : 8;  // Bits 8-15
};

int main() {
    struct DeviceStatus status = {0};
    
    status.power_on = 1;
    status.error = 0;
    status.mode = 3;  // 11 binary
    status.temperature = 10;  // 1010 binary
    
    printf("Size: %zu bytes\n", sizeof(status));
    printf("Power: %u\n", status.power_on);
    printf("Mode: %u\n", status.mode);
    printf("Temp: %u\n", status.temperature);
    
    // Access as integer (caution: endianness)
    unsigned int* ptr = (unsigned int*)&status;
    printf("Raw value: 0x%X\n", *ptr);
    
    return 0;
}
```

