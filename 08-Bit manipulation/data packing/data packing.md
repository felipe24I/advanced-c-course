## Data packing
Data packing is the technique of storing multiple smaller values into a single larger variable using bitwise operations. This is essential for:
* Reducing memory usage
* Creating compact data structures
* Network protocol implementation
* Hardware register manipulation
* File format encoding

## Basic Packing Concepts
### 1. Packing Multiple Values into a Single Integer
```c
#include <stdio.h>
#include <stdint.h>

int main() {
    // Example: Pack 4 small values into a 32-bit integer
    uint8_t  a = 15;     // 4 bits needed (0-15)
    uint8_t  b = 7;      // 3 bits needed (0-7)
    uint8_t  c = 3;      // 2 bits needed (0-3)
    uint16_t d = 500;    // 9 bits needed (0-1023)
    
    // Total bits: 4 + 3 + 2 + 9 = 18 bits (fits in 32-bit)
    
    uint32_t packed = 0;
    
    // Pack values (assuming positions from LSB)
    packed |= (a << 0);   // Place a at bits 0-3
    packed |= (b << 4);   // Place b at bits 4-6
    packed |= (c << 7);   // Place c at bits 7-8
    packed |= (d << 9);   // Place d at bits 9-17
    
    printf("Packed value: 0x%08X (%u)\n", packed, packed);
    
    // Unpack values
    uint8_t  a_unpack = (packed >> 0) & 0x0F;   // Mask 4 bits
    uint8_t  b_unpack = (packed >> 4) & 0x07;   // Mask 3 bits
    uint8_t  c_unpack = (packed >> 7) & 0x03;   // Mask 2 bits
    uint16_t d_unpack = (packed >> 9) & 0x01FF; // Mask 9 bits
    
    printf("Unpacked: a=%u, b=%u, c=%u, d=%u\n", 
           a_unpack, b_unpack, c_unpack, d_unpack);
    
    return 0;
}
```

## Packing Techniques
### 1. RGB Color Packing
```c
#include <stdio.h>
#include <stdint.h>

// Pack 8-bit RGB into 24-bit or 32-bit color
uint32_t pack_rgb888(uint8_t r, uint8_t g, uint8_t b) {
    // Pack as 0x00RRGGBB (24-bit color)
    return (r << 16) | (g << 8) | b;
}

// Pack 8-bit RGB with alpha into 32-bit
uint32_t pack_rgba8888(uint8_t r, uint8_t g, uint8_t b, uint8_t a) {
    // Pack as 0xAARRGGBB or 0xRRGGBBAA depending on convention
    return (a << 24) | (r << 16) | (g << 8) | b;
}

// Pack 5-6-5 RGB (common in embedded displays)
uint16_t pack_rgb565(uint8_t r, uint8_t g, uint8_t b) {
    // R: 5 bits, G: 6 bits, B: 5 bits
    return ((r >> 3) << 11) | ((g >> 2) << 5) | (b >> 3);
}

// Unpack functions
void unpack_rgb888(uint32_t rgb, uint8_t *r, uint8_t *g, uint8_t *b) {
    *r = (rgb >> 16) & 0xFF;
    *g = (rgb >> 8) & 0xFF;
    *b = rgb & 0xFF;
}

void unpack_rgb565(uint16_t rgb, uint8_t *r, uint8_t *g, uint8_t *b) {
    *r = ((rgb >> 11) & 0x1F) << 3;
    *g = ((rgb >> 5) & 0x3F) << 2;
    *b = (rgb & 0x1F) << 3;
}

int main() {
    // RGB888 example
    uint32_t color = pack_rgb888(255, 128, 64);
    printf("RGB888: 0x%06X\n", color);
    
    uint8_t r, g, b;
    unpack_rgb888(color, &r, &g, &b);
    printf("Unpacked: R=%u, G=%u, B=%u\n", r, g, b);
    
    // RGB565 example
    uint16_t color565 = pack_rgb565(255, 128, 64);
    printf("RGB565: 0x%04X\n", color565);
    
    unpack_rgb565(color565, &r, &g, &b);
    printf("Unpacked (approximated): R=%u, G=%u, B=%u\n", r, g, b);
    
    return 0;
}
```

### 2. Date and Time Packing
```c
#include <stdio.h>
#include <stdint.h>
#include <time.h>

// Pack date into 16 bits
// Format: YYYY-MM-DD
// Year: 7 bits (0-127, offset from 2000)
// Month: 4 bits (1-12)
// Day: 5 bits (1-31)
// Total: 16 bits
uint16_t pack_date(uint8_t year, uint8_t month, uint8_t day) {
    return ((year & 0x7F) << 9) | ((month & 0x0F) << 5) | (day & 0x1F);
}

void unpack_date(uint16_t packed, uint8_t *year, uint8_t *month, uint8_t *day) {
    *year = (packed >> 9) & 0x7F;
    *month = (packed >> 5) & 0x0F;
    *day = packed & 0x1F;
}

// Pack time into 16 bits
// Format: HH:MM:SS
// Hours: 5 bits (0-23)
// Minutes: 6 bits (0-59)
// Seconds: 5 bits (0-59) - actually 6 bits needed, but we use 5 (0-31)
// Total: 16 bits
uint16_t pack_time(uint8_t hours, uint8_t minutes, uint8_t seconds) {
    return ((hours & 0x1F) << 11) | ((minutes & 0x3F) << 5) | ((seconds >> 1) & 0x1F);
}

void unpack_time(uint16_t packed, uint8_t *hours, uint8_t *minutes, uint8_t *seconds) {
    *hours = (packed >> 11) & 0x1F;
    *minutes = (packed >> 5) & 0x3F;
    *seconds = (packed & 0x1F) << 1;
}

// Pack full timestamp into 32 bits (Unix-like but compact)
// Year: 7 bits (2000-2127)
// Month: 4 bits
// Day: 5 bits
// Hour: 5 bits
// Minute: 6 bits
// Second: 5 bits (0-31, with 2-second resolution)
uint32_t pack_timestamp(uint16_t year, uint8_t month, uint8_t day,
                        uint8_t hour, uint8_t minute, uint8_t second) {
    uint32_t packed = 0;
    packed |= ((year - 2000) & 0x7F) << 25;
    packed |= (month & 0x0F) << 21;
    packed |= (day & 0x1F) << 16;
    packed |= (hour & 0x1F) << 11;
    packed |= (minute & 0x3F) << 5;
    packed |= ((second >> 1) & 0x1F);
    return packed;
}

int main() {
    // Pack date
    uint16_t date = pack_date(24, 12, 25);  // 2024-12-25
    uint8_t year, month, day;
    unpack_date(date, &year, &month, &day);
    printf("Date: %u-%02u-%02u\n", 2000 + year, month, day);
    
    // Pack time
    uint16_t time = pack_time(14, 30, 45);  // 14:30:45
    uint8_t hours, minutes, seconds;
    unpack_time(time, &hours, &minutes, &seconds);
    printf("Time: %02u:%02u:%02u\n", hours, minutes, seconds);
    
    // Pack full timestamp
    uint32_t timestamp = pack_timestamp(2024, 12, 25, 14, 30, 45);
    printf("Packed timestamp: 0x%08X\n", timestamp);
    
    return 0;
}
```

