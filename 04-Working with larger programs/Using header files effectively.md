## What Makes a Good Header File?
A good header file is:
* **Self-contained** - Includes all headers it needs
* **Idempotent** - Can be included multiple times safely
* **Minimal** - Only exposes what's necessary
* **Documented** - Clearly explains the API

## Header File Structure
### Basic Template
```c
/* ============================================
 * module_name.h - Brief description
 * ============================================
 * Author: Name
 * Date: 2024
 * 
 * This module provides functionality for...
 * ============================================ */

#ifndef MODULE_NAME_H
#define MODULE_NAME_H

/* ---------------------------
 * Includes
 * --------------------------- */
#include <stdio.h>
#include "other_module.h"

/* ---------------------------
 * Constants and Macros
 * --------------------------- */
#define MAX_SIZE 100
#define DEFAULT_VALUE 42

/* ---------------------------
 * Type Definitions
 * --------------------------- */
typedef struct {
    int id;
    char name[50];
} MyStruct;

typedef enum {
    STATUS_OK,
    STATUS_ERROR
} Status;

/* ---------------------------
 * Global Variables (rare)
 * --------------------------- */
extern int global_counter;

/* ---------------------------
 * Function Prototypes
 * --------------------------- */
/**
 * Initialize the module
 * @return 0 on success, -1 on error
 */
int module_init(void);

/**
 * Process data
 * @param input Input value
 * @return Processed result
 */
int module_process(int input);

/**
 * Clean up resources
 */
void module_cleanup(void);

#endif /* MODULE_NAME_H */
```

## Include Guards
### 1. Traditional Include Guards
```c
// Traditional method (most compatible)
#ifndef MY_HEADER_H
#define MY_HEADER_H

// Header content

#endif /* MY_HEADER_H */
```

### 2. Pragmas (GCC/Clang)
```c
// Modern method (not standard C, but widely supported)
#pragma once

// Header content
```
### 3. Naming Convention for Guards
```c
// Good: Project prefix + file name
#ifndef MYPROJECT_MODULE_H
#define MYPROJECT_MODULE_H

// Good: Path-based
#ifndef SRC_UTILS_STRING_HELPER_H
#define SRC_UTILS_STRING_HELPER_H

// Bad: Too generic
#ifndef HEADER_H
#define HEADER_H
```

## Self-Contained Headers
### Always Include What You Use
```c
// BAD: Relies on caller to include dependencies
// utils.h - Missing includes
void process_string(char *str);  // Uses char* - needs nothing
void write_to_file(FILE *fp);    // Uses FILE* - needs stdio.h!
size_t get_length(const char *s); // Uses size_t - needs stddef.h!

// GOOD: Self-contained with proper includes
// utils.h
#ifndef UTILS_H
#define UTILS_H

#include <stdio.h>   // For FILE
#include <stddef.h>  // For size_t

void process_string(char *str);
void write_to_file(FILE *fp);
size_t get_length(const char *s);

#endif
```

### Testing Header Independence
```bash
# Test if header is self-contained
gcc -c myheader.h -o /dev/null

# Or in a test file
echo '#include "myheader.h"' > test.c
gcc -c test.c -o /dev/null
```

## Organizing Header Content
### 1. Group Related Declarations
```c
// network.h
#ifndef NETWORK_H
#define NETWORK_H

/* ========== Constants ========== */
#define MAX_PACKET_SIZE 4096
#define DEFAULT_PORT 8080
#define TIMEOUT_SECONDS 30

/* ========== Types ========== */
typedef enum {
    SOCKET_TCP,
    SOCKET_UDP
} SocketType;

typedef struct {
    char host[256];
    int port;
    SocketType type;
} NetworkConfig;

typedef struct NetworkConnection NetworkConnection;  // Opaque

/* ========== Initialization ========== */
NetworkConnection* network_init(const NetworkConfig *cfg);
void network_cleanup(NetworkConnection *conn);

/* ========== Data Transfer ========== */
int network_send(NetworkConnection *conn, const void *data, size_t len);
int network_receive(NetworkConnection *conn, void *buffer, size_t len);

/* ========== Status ========== */
int network_is_connected(const NetworkConnection *conn);
const char* network_get_last_error(void);

#endif
```
