## Bitmask
is a pattern of bits used to select, set, clear, or test specific bits within a value. 
It's a fundamental technique for efficient data storage and manipulation, especially useful in embedded systems, hardware programming, and performance-critical applications.

## Basic Bitmask Operations
### 1. Defining Bitmasks
```c
#include <stdio.h>
#include <stdint.h>

// Method 1: Hexadecimal constants
#define MASK_BIT0   0x01  // 00000001
#define MASK_BIT1   0x02  // 00000010
#define MASK_BIT2   0x04  // 00000100
#define MASK_BIT3   0x08  // 00001000
#define MASK_BIT4   0x10  // 00010000
#define MASK_BIT5   0x20  // 00100000
#define MASK_BIT6   0x40  // 01000000
#define MASK_BIT7   0x80  // 10000000

// Method 2: Bit shift (more readable for positions)
#define BIT(n)      (1 << (n))
#define MASK_FLAG1  BIT(0)  // 00000001
#define MASK_FLAG2  BIT(1)  // 00000010
#define MASK_FLAG3  BIT(2)  // 00000100

// Method 3: Combined masks
#define MASK_LOW_NIBBLE  0x0F  // 00001111
#define MASK_HIGH_NIBBLE 0xF0  // 11110000
#define MASK_BYTE        0xFF  // 11111111

int main() {
    printf("BIT(0) = 0x%02X\n", BIT(0));
    printf("BIT(3) = 0x%02X\n", BIT(3));
    printf("BIT(7) = 0x%02X\n", BIT(7));
    return 0;
}
```

## Core Bitmask Operations
### 1. Setting Bits (OR)
```c
#include <stdio.h>
#include <stdint.h>

#define BIT0 0x01
#define BIT1 0x02
#define BIT2 0x04
#define BIT3 0x08

int main() {
    uint8_t flags = 0x00;  // 00000000
    
    // Set individual bits
    flags |= BIT0;          // 00000001
    flags |= BIT2;          // 00000101
    
    printf("After setting bits 0 and 2: 0x%02X\n", flags);
    
    // Set multiple bits at once
    flags |= (BIT1 | BIT3); // 00001111
    printf("After setting bits 1 and 3: 0x%02X\n", flags);
    
    // Set bit using position
    uint8_t pos = 4;
    flags |= (1 << pos);    // 00011111
    printf("After setting bit 4: 0x%02X\n", flags);
    
    return 0;
}
```

### 2. Clearing Bits (AND with NOT)
```c
#include <stdio.h>
#include <stdint.h>

#define BIT0 0x01
#define BIT1 0x02
#define BIT2 0x04
#define BIT3 0x08

int main() {
    uint8_t flags = 0xFF;  // 11111111
    
    // Clear individual bits
    flags &= ~BIT0;         // 11111110
    flags &= ~BIT2;         // 11111010
    
    printf("After clearing bits 0 and 2: 0x%02X\n", flags);
    
    // Clear multiple bits
    flags &= ~(BIT1 | BIT3); // 11110000
    printf("After clearing bits 1 and 3: 0x%02X\n", flags);
    
    // Clear bit using position
    uint8_t pos = 4;
    flags &= ~(1 << pos);    // 11101111
    printf("After clearing bit 4: 0x%02X\n", flags);
    
    return 0;
}
```

### 3. Toggling Bits (XOR)
```c
#include <stdio.h>
#include <stdint.h>

#define BIT0 0x01
#define BIT1 0x02
#define BIT2 0x04
#define LED_ON  0x01
#define LED_OFF 0x00

int main() {
    uint8_t flags = 0x0F;  // 00001111
    
    // Toggle individual bits
    flags ^= BIT0;          // 00001110
    flags ^= BIT2;          // 00001010
    
    printf("After toggling bits 0 and 2: 0x%02X\n", flags);
    
    // Toggle multiple bits
    flags ^= (BIT1 | BIT3); // 00000000
    printf("After toggling bits 1 and 3: 0x%02X\n", flags);
    
    // Practical example: toggle LED state
    uint8_t led_state = LED_ON;  // 00000001
    led_state ^= BIT0;           // 00000000 (OFF)
    led_state ^= BIT0;           // 00000001 (ON)
    printf("LED toggled twice: %d\n", led_state);
    
    return 0;
}
```

### 4. Testing Bits (AND)
```c
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>

#define BIT0 0x01
#define BIT1 0x02
#define BIT2 0x04
#define BIT3 0x08

bool is_bit_set(uint8_t value, uint8_t mask) {
    return (value & mask) != 0;
}

int main() {
    uint8_t flags = 0x0A;  // 00001010
    
    // Test individual bits
    printf("Bit 0: %s\n", is_bit_set(flags, BIT0) ? "set" : "clear");
    printf("Bit 1: %s\n", is_bit_set(flags, BIT1) ? "set" : "clear");
    printf("Bit 3: %s\n", is_bit_set(flags, BIT3) ? "set" : "clear");
    
    // Test multiple bits at once
    if ((flags & (BIT1 | BIT3)) == (BIT1 | BIT3)) {
        printf("Both bits 1 and 3 are set\n");
    }
    
    // Test if ANY of multiple bits are set
    if (flags & (BIT0 | BIT1)) {
        printf("At least one of bits 0 or 1 is set\n");
    }
    
    return 0;
}
```
