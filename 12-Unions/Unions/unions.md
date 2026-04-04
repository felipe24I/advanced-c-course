## Union
A union is a special data type that allows storing different data types in the same memory location. 
Unlike structures where each member has its own memory, **all members of a union share the same memory space.**

```c
#include <stdio.h>

// Structure: each member has its own memory
struct StructExample {
    int i;      // 4 bytes
    float f;    // 4 bytes
    char c;     // 1 byte
    // Total: 9+ bytes (with padding)
};

// Union: all members share memory
union UnionExample {
    int i;      // 4 bytes
    float f;    // 4 bytes
    char c;     // 1 byte
    // Total: 4 bytes (largest member)
};

int main() {
    printf("Size of struct: %zu bytes\n", sizeof(struct StructExample)); // Size of struct: 12 bytes
    printf("Size of union: %zu bytes\n", sizeof(union UnionExample)); // Size of union: 4 bytes
    
    return 0;
}
```

## Memory Layout Visualization
```c
#include <stdio.h>
#include <string.h>

union Data {
    int i;      // 4 bytes
    float f;    // 4 bytes
    char str[20]; // 20 bytes
};

int main() {
    union Data data;

    printf("Union size: %zu bytes\n", sizeof(data)); // Union size: 20 bytes

    // All members share the same memory address
    printf("Address of i:   %p\n", (void*)&data.i); // Address of i:   000000ADD0BFFAF0
    printf("Address of f:   %p\n", (void*)&data.f); // Address of f:   000000ADD0BFFAF0
    printf("Address of str: %p\n", (void*)&data.str); // Address of str: 000000ADD0BFFAF0

    // Setting one member affects others
    data.i = 65;
    printf("\nAfter setting i = 65:\n"); // After setting i = 65:
    printf("  i = %d\n", data.i); // i = 65
    printf("  f = %f\n", data.f); // f = 0.000000
    printf("  str = %s\n", data.str); // str = A

    data.f = 3.14159f;
    printf("\nAfter setting f = 3.14159:\n"); // After setting f = 3.14159:
    printf("  i = %d\n", data.i); // i = 1078530000
    printf("  f = %f\n", data.f); // f = 3.141590

    strcpy(data.str, "Hello");
    printf("\nAfter setting str = 'Hello':\n"); // After setting str = 'Hello':
    printf("  i = %d\n", data.i); // i = 1819043144
    printf("  f = %f\n", data.f); // f = 1143139122437582505939828736.000000
    printf("  str = %s\n", data.str); // str = Hello

    return 0;
}
```

**Note:** Yes! 1078530000 is exactly the integer representation of the bits that make up 3.14159f in IEEE 754 format.

Think of it this way:

Memory contains 4 bytes: 0x40, 0x49, 0x0F, 0xD0

As a float, these bytes mean 3.14159

As an integer, these same bytes mean 1078530000

## Basic Union Operations
### 1. Declaration and Initialization
```c
#include <stdio.h>

// Union declaration
union Value {
    int integer;
    float decimal;
    char character;
};

int main() {
    // Initialize with first member (C99)
    union Value v1 = {42};  // Sets integer member

    // Designated initializer (C99)
    union Value v2 = {.decimal = 3.14};

    // Initialize with any member (GCC extension)
    union Value v3 = {.character = 'A'};

    // After declaration, assign values
    union Value v4;
    v4.integer = 100;

    printf("v1.integer = %d\n", v1.integer); // v1.integer = 42
    printf("v2.decimal = %.2f\n", v2.decimal); // v2.decimal = 3.14 
    printf("v3.character = %c\n", v3.character); // v3.character = A
    printf("v4.integer = %d\n", v4.integer); // v4.integer = 100

    return 0;
}
```

**Note:** When you create separate union variables (v1, v2, v3, v4), they are independent memory locations. Each has its own memory space, so they don't interfere with each other.

### visual representation
```text
Memory Layout:

v1: [integer=42]     ← Separate memory (4 bytes)
     ↑
     |
v2: [decimal=3.14]   ← Different memory (4 bytes)
     ↑
     |
v3: [character='A']  ← Different memory (4 bytes)
     ↑
     |
v4: [integer=100]    ← Different memory (4 bytes)
```

**Key Point: Interference Happens WITHIN a Union, NOT Between Unions**

### Real-World Analogy
Think of unions like parking spaces:
```c
// ONE union = ONE parking space that can hold different types of vehicles
union ParkingSpace {
    int car;      // Can hold a car
    float truck;  // Can hold a truck  
    char bicycle; // Can hold a bicycle
};

// Multiple unions = MULTIPLE parking spaces
union ParkingSpace space1;  // Parking space #1
union ParkingSpace space2;  // Parking space #2
union ParkingSpace space3;  // Parking space #3
```

**Within one space:** You can only park ONE vehicle at a time
* Park a car → space1.car
* Then park a truck → OVERWRITES the car!

**Different spaces:** No interference
* space1 has a car
* space2 has a truck
* space3 has a bicycle
* They don't affect each other!

### 2. Accessing Union Members
```c
#include <stdio.h>

union Packet {
    int int_value;
    float float_value;
    char bytes[4];
};

int main() {
    union Packet p;
    
    // Store integer
    p.int_value = 0x12345678;
    printf("As integer: 0x%08X\n", p.int_value);
    printf("As bytes: %02X %02X %02X %02X\n", 
           p.bytes[0], p.bytes[1], p.bytes[2], p.bytes[3]);
    
    // Store float
    p.float_value = 3.14159f;
    printf("\nAs float: %f\n", p.float_value);
    printf("As integer: 0x%08X\n", p.int_value);
    
    // Store bytes
    p.bytes[0] = 0xAA;
    p.bytes[1] = 0xBB;
    p.bytes[2] = 0xCC;
    p.bytes[3] = 0xDD;
    printf("\nAfter setting bytes:\n");
    printf("  int: 0x%08X\n", p.int_value);
    printf("  float: %f\n", p.float_value);
    
    return 0;
}
```

## Practical Applications
### 1. Type Punning (Reinterpreting Data)
```c
#include <stdio.h>
#include <string.h>

// Convert between types without casting
union TypePunning {
    float f;
    unsigned int u;
};

// Check floating point representation
void inspect_float(float value) {
    union TypePunning pun;
    pun.f = value;
    
    printf("%f = 0x%08X\n", value, pun.u);
    printf("  Sign: %d\n", (pun.u >> 31) & 1);
    printf("  Exponent: 0x%02X\n", (pun.u >> 23) & 0xFF);
    printf("  Mantissa: 0x%06X\n", pun.u & 0x7FFFFF);
}

// Convert 4 bytes to integer (endian-aware)
int bytes_to_int(unsigned char bytes[4], int little_endian) {
    union {
        unsigned char b[4];
        int i;
    } converter;
    
    if (little_endian) {
        converter.b[0] = bytes[0];
        converter.b[1] = bytes[1];
        converter.b[2] = bytes[2];
        converter.b[3] = bytes[3];
    } else {
        converter.b[0] = bytes[3];
        converter.b[1] = bytes[2];
        converter.b[2] = bytes[1];
        converter.b[3] = bytes[0];
    }
    
    return converter.i;
}

int main() {
    inspect_float(3.14159f);
    inspect_float(0.0f);
    inspect_float(1.0f);
    
    unsigned char bytes[] = {0x01, 0x02, 0x03, 0x04};
    printf("\nBytes: %02X %02X %02X %02X\n", 
           bytes[0], bytes[1], bytes[2], bytes[3]);
    printf("As little-endian int: %d (0x%08X)\n", 
           bytes_to_int(bytes, 1), bytes_to_int(bytes, 1));
    printf("As big-endian int: %d (0x%08X)\n", 
           bytes_to_int(bytes, 0), bytes_to_int(bytes, 0));
    
    return 0;
}
```

### 2. Network Packet Parsing
```c
#include <stdio.h>
#include <stdint.h>
#include <string.h>

// Network packet header (simplified)
typedef struct {
    uint8_t version;
    uint8_t type;
    uint16_t length;
    uint32_t sequence;
    uint32_t timestamp;
} PacketHeader;

// Union for packet parsing
union PacketBuffer {
    uint8_t raw[1024];      // Raw byte buffer
    struct {
        PacketHeader header;
        uint8_t payload[1012];
    } packet;
};

// Parse incoming network data
void process_packet(uint8_t *data, size_t len) {
    union PacketBuffer buffer;
    
    // Copy data to union
    memcpy(buffer.raw, data, len);
    
    printf("Packet received:\n");
    printf("  Version: %u\n", buffer.packet.header.version);
    printf("  Type: %u\n", buffer.packet.header.type);
    printf("  Length: %u\n", buffer.packet.header.length);
    printf("  Sequence: %u\n", buffer.packet.header.sequence);
    printf("  Timestamp: %u\n", buffer.packet.header.timestamp);
    printf("  Payload: %.10s...\n", buffer.packet.payload);
}

int main() {
    // Simulate received packet
    uint8_t packet[] = {
        0x01, 0x02, 0x00, 0x20,  // version, type, length
        0x12, 0x34, 0x56, 0x78,  // sequence
        0xAA, 0xBB, 0xCC, 0xDD,  // timestamp
        'H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd', '!'
    };
    
    process_packet(packet, sizeof(packet));
    
    return 0;
}
```

### 3. Memory-Mapped Hardware Registers
```c
#include <stdio.h>
#include <stdint.h>

// Hardware register representation
typedef union {
    uint32_t reg;  // Whole register
    
    struct {
        uint32_t enable     : 1;   // Bit 0
        uint32_t mode       : 2;   // Bits 1-2
        uint32_t speed      : 2;   // Bits 3-4
        uint32_t interrupt  : 1;   // Bit 5
        uint32_t reserved   : 2;   // Bits 6-7
        uint32_t threshold  : 8;   // Bits 8-15
        uint32_t status     : 8;   // Bits 16-23
        uint32_t error      : 8;   // Bits 24-31
    } bits;
} ControlRegister;

// Simulate hardware register at address
#define CONTROL_REG ((volatile ControlRegister*)0x40004000)

int main() {
    // In real embedded code, you'd access hardware:
    // CONTROL_REG->bits.enable = 1;
    // CONTROL_REG->bits.mode = 2;
    
    // For demonstration, use local variable
    ControlRegister reg = {0};
    
    // Set individual bits
    reg.bits.enable = 1;
    reg.bits.mode = 2;
    reg.bits.speed = 1;
    reg.bits.threshold = 100;
    
    printf("Register value: 0x%08X\n", reg.reg);
    printf("Enable: %u\n", reg.bits.enable);
    printf("Mode: %u\n", reg.bits.mode);
    printf("Speed: %u\n", reg.bits.speed);
    printf("Threshold: %u\n", reg.bits.threshold);
    
    // Access as whole register
    reg.reg = 0xDEADBEEF;
    printf("\nAfter setting whole register:\n");
    printf("Enable: %u\n", reg.bits.enable);
    printf("Mode: %u\n", reg.bits.mode);
    printf("Threshold: %u\n", reg.bits.threshold);
    
    return 0;
}
```

## Important Note for structs and unions (accessing to them)
### 1. The Dot Operator (.)
You use the dot operator when you have a direct variable (the actual instance) of a struct or union.
* **Syntax:** object.member
* **Analogy:** You have the box in your hands, and you are opening it to see what’s inside.

```c
struct Player {
    int health;
};

struct Player p1; 
p1.health = 100; // Direct access
```

### 2. The Arrow Operator (->)
You use the arrow operator when you have a pointer to a struct or union. It is shorthand for "dereference the pointer first, then access the member."
* **Syntax:** pointer->member
* **The Math:** ptr->member is exactly the same as (*ptr).member.
* **Analogy:** You have a map with an address. You have to follow the map to the house before you can walk through the door.

```c
struct Player *ptr = &p1;
ptr->health = 80; // Accessing via address
```
