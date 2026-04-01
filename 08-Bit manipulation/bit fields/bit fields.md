## Bit Fields
are a C language feature that allow you to specify the exact number of bits for structure members, enabling efficient data packing without manual bitwise operations.

### Basic Bit Field Syntax
```c
#include <stdio.h>
#include <stdint.h>

// Basic bit field structure
struct BitFieldExample {
    unsigned int flag1 : 1;   // 1 bit
    unsigned int flag2 : 1;   // 1 bit
    unsigned int value : 4;    // 4 bits
    unsigned int counter : 10; // 10 bits
    // Total: 16 bits (2 bytes) typically
};

int main() {
    struct BitFieldExample bf;
    
    bf.flag1 = 1;
    bf.flag2 = 0;
    bf.value = 9;      // 4 bits: 0-15
    bf.counter = 512;  // 10 bits: 0-1023
    
    printf("Size: %zu bytes\n", sizeof(bf));
    printf("flag1: %u\n", bf.flag1);
    printf("flag2: %u\n", bf.flag2);
    printf("value: %u\n", bf.value);
    printf("counter: %u\n", bf.counter);
    
    return 0;
}
```

## Practical Bit Field Examples
### 1. Hardware Register Mapping
```c
#include <stdio.h>
#include <stdint.h>

// GPIO Control Register (typical ARM Cortex-M)
typedef struct {
    uint32_t MODER0   : 2;   // Mode for pin 0
    uint32_t MODER1   : 2;   // Mode for pin 1
    uint32_t MODER2   : 2;   // Mode for pin 2
    uint32_t MODER3   : 2;   // Mode for pin 3
    uint32_t MODER4   : 2;   // Mode for pin 4
    uint32_t MODER5   : 2;   // Mode for pin 5
    uint32_t MODER6   : 2;   // Mode for pin 6
    uint32_t MODER7   : 2;   // Mode for pin 7
    uint32_t MODER8   : 2;   // Mode for pin 8
    uint32_t MODER9   : 2;   // Mode for pin 9
    uint32_t MODER10  : 2;   // Mode for pin 10
    uint32_t MODER11  : 2;   // Mode for pin 11
    uint32_t MODER12  : 2;   // Mode for pin 12
    uint32_t MODER13  : 2;   // Mode for pin 13
    uint32_t MODER14  : 2;   // Mode for pin 14
    uint32_t MODER15  : 2;   // Mode for pin 15
} GPIO_ModeRegister;

// More compact representation using arrays in bit fields (C99)
typedef struct {
    uint32_t MODER : 32;  // Use as array of 2-bit fields via macros
} GPIO_Compact;

// Control and Status Register with mixed field sizes
typedef struct {
    uint32_t ENABLE     : 1;   // Bit 0: Enable peripheral
    uint32_t RESET      : 1;   // Bit 1: Software reset
    uint32_t MODE       : 2;   // Bits 2-3: Operation mode
    uint32_t SPEED      : 2;   // Bits 4-5: Clock speed
    uint32_t INTERRUPT  : 1;   // Bit 6: Interrupt enable
    uint32_t RESERVED0  : 1;   // Bit 7: Reserved
    uint32_t STATUS     : 8;   // Bits 8-15: Status flags
    uint32_t ERROR_CODE : 8;   // Bits 16-23: Error code
    uint32_t RESERVED1  : 8;   // Bits 24-31: Reserved
} ControlStatusRegister;

int main() {
    GPIO_ModeRegister gpio = {0};
    gpio.MODER0 = 0x01;  // Output mode
    gpio.MODER1 = 0x02;  // Alternate function
    gpio.MODER2 = 0x03;  // Analog mode
    
    printf("GPIO Register size: %zu bytes\n", sizeof(gpio));
    printf("MODER0: %u\n", gpio.MODER0);
    printf("MODER1: %u\n", gpio.MODER1);
    printf("MODER2: %u\n", gpio.MODER2);
    
    ControlStatusRegister csr = {
        .ENABLE = 1,
        .MODE = 2,
        .SPEED = 1,
        .STATUS = 0x55,
        .ERROR_CODE = 0xAA
    };
    
    printf("\nCSR size: %zu bytes\n", sizeof(csr));
    printf("ENABLE: %u\n", csr.ENABLE);
    printf("MODE: %u\n", csr.MODE);
    printf("STATUS: 0x%02X\n", csr.STATUS);
    
    return 0;
}
```

### 2. Network Protocol Headers
```c
#include <stdio.h>
#include <stdint.h>

// IPv4 Header (simplified)
#pragma pack(push, 1)  // Disable padding for network headers
typedef struct {
    uint8_t  version     : 4;   // IP version
    uint8_t  ihl         : 4;   // Header length
    uint8_t  tos;               // Type of service (8 bits)
    uint16_t total_length;      // Total packet length
    uint16_t identification;    // Identification
    uint16_t flags_frag  : 13;  // Flags and fragment offset
    uint8_t  ttl;               // Time to live
    uint8_t  protocol;          // Protocol
    uint16_t checksum;          // Header checksum
    uint32_t src_ip;            // Source IP
    uint32_t dest_ip;           // Destination IP
} IPv4Header;

// TCP Header with bit fields
typedef struct {
    uint16_t source_port;
    uint16_t dest_port;
    uint32_t sequence_num;
    uint32_t ack_num;
    uint8_t  data_offset : 4;   // Header length
    uint8_t  reserved    : 3;   // Reserved
    uint8_t  ns          : 1;   // ECN-nonce
    uint8_t  cwr         : 1;   // Congestion window reduced
    uint8_t  ece         : 1;   // ECN echo
    uint8_t  urg         : 1;   // Urgent pointer
    uint8_t  ack         : 1;   // Acknowledgment
    uint8_t  psh         : 1;   // Push
    uint8_t  rst         : 1;   // Reset
    uint8_t  syn         : 1;   // Synchronize
    uint8_t  fin         : 1;   // Finish
    uint16_t window;
    uint16_t checksum;
    uint16_t urgent_ptr;
} TCPHeader;
#pragma pack(pop)

int main() {
    IPv4Header ip = {
        .version = 4,
        .ihl = 5,  // 5 * 4 = 20 bytes
        .tos = 0,
        .total_length = 40,
        .identification = 0x1234,
        .flags_frag = 0,
        .ttl = 64,
        .protocol = 6,  // TCP
        .checksum = 0,
        .src_ip = 0xC0A80101,  // 192.168.1.1
        .dest_ip = 0xC0A80102   // 192.168.1.2
    };
    
    TCPHeader tcp = {
        .source_port = 0x1F90,   // 8080
        .dest_port = 0x0050,     // 80
        .sequence_num = 0x12345678,
        .ack_num = 0,
        .data_offset = 5,         // 5 * 4 = 20 bytes
        .syn = 1,
        .window = 0x2000,
        .checksum = 0,
        .urgent_ptr = 0
    };
    
    printf("IPv4 Header size: %zu bytes\n", sizeof(ip));
    printf("TCP Header size: %zu bytes\n", sizeof(tcp));
    printf("Version: %u, IHL: %u\n", ip.version, ip.ihl);
    printf("TCP Flags: SYN=%u, ACK=%u, FIN=%u\n", tcp.syn, tcp.ack, tcp.fin);
    
    return 0;
}
```

### 3. Sensor Data Packing
```c
#include <stdio.h>
#include <stdint.h>
#include <string.h>

// Pack multiple sensor readings
#pragma pack(push, 1)
typedef struct {
    int16_t temperature : 12;   // -2048 to 2047 (actual range -40 to 85)
    uint8_t humidity    : 7;    // 0-100% RH
    uint8_t pressure    : 10;   // 0-1023 (scaled 900-1100 hPa)
    uint8_t battery     : 6;    // 0-100%
    uint8_t quality     : 3;    // 0-7 signal quality
    uint8_t reserved    : 2;    // Reserved for future use
    // Total: 12+7+10+6+3+2 = 40 bits = 5 bytes
} SensorPacket;

// Version with proper scaling
typedef struct {
    uint16_t temp_integer : 8;   // Integer part (-40 to 85)
    uint8_t  temp_fraction : 4;  // Fractional part (0-15)
    uint8_t  humidity : 8;       // 0-100%
    uint16_t pressure : 12;      // 900-1100 hPa with 0.1 resolution
    uint8_t  battery : 7;        // 0-100%
    uint8_t  flags : 5;          // Status flags
    // Total: 8+4+8+12+7+5 = 44 bits = 6 bytes
} CompactSensorData;
#pragma pack(pop)

// Function to convert and pack temperature
void pack_temperature(CompactSensorData *data, float temp) {
    int16_t temp_scaled = (int16_t)(temp * 10);  // 0.1°C resolution
    data->temp_integer = (temp_scaled / 10) + 40;  // Offset by 40 to handle negative
    data->temp_fraction = abs(temp_scaled % 10);
}

float unpack_temperature(CompactSensorData *data) {
    int16_t temp_scaled = (data->temp_integer - 40) * 10 + data->temp_fraction;
    return temp_scaled / 10.0f;
}

int main() {
    printf("SensorPacket size: %zu bytes\n", sizeof(SensorPacket));
    printf("CompactSensorData size: %zu bytes\n", sizeof(CompactSensorData));
    
    CompactSensorData sensor = {0};
    
    // Pack temperature
    pack_temperature(&sensor, 23.7f);
    sensor.humidity = 65;
    sensor.pressure = 1013;  // 101.3 hPa
    sensor.battery = 87;
    sensor.flags = 0x05;  // Some flags
    
    printf("Packed data: temp_integer=%u, temp_fraction=%u\n",
           sensor.temp_integer, sensor.temp_fraction);
    printf("Unpacked temperature: %.1f°C\n", unpack_temperature(&sensor));
    
    // Show raw memory
    uint8_t *bytes = (uint8_t*)&sensor;
    printf("Raw bytes (%zu): ", sizeof(sensor));
    for (size_t i = 0; i < sizeof(sensor); i++) {
        printf("%02X ", bytes[i]);
    }
    printf("\n");
    
    return 0;
}
```
