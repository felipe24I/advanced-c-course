## Communication between files
Communication between files in C refers to sharing data, functions, and types across multiple source files. 
This is achieved through a combination of header files, extern declarations, and proper build techniques.

## Core Concepts
### 1. Header Files (.h)
Header files declare what a module provides to other modules.

```c
// calculator.h
#ifndef CALCULATOR_H
#define CALCULATOR_H

// Function declarations (what this module offers)
int add(int a, int b);
int subtract(int a, int b);
int multiply(int a, int b);
double divide(int a, int b);

// Type definitions
typedef struct {
    int x;
    int y;
} Point;

// Constants
#define PI 3.14159

// Global variable declaration (defined elsewhere)
extern int calculation_count;

#endif
```

### 2. Source Files (.c)
Source files implement the declared functionality.

```c
// calculator.c
#include "calculator.h"

// Definition of global variable
int calculation_count = 0;

// Function implementations
int add(int a, int b) {
    calculation_count++;
    return a + b;
}

int subtract(int a, int b) {
    calculation_count++;
    return a - b;
}

int multiply(int a, int b) {
    calculation_count++;
    return a * b;
}

double divide(int a, int b) {
    calculation_count++;
    if (b != 0) return (double)a / b;
    return 0.0;
}
```

### 3. Main File Using the Module
```c
// main.c
#include <stdio.h>
#include "calculator.h"

int main() {
    printf("10 + 5 = %d\n", add(10, 5));
    printf("10 - 5 = %d\n", subtract(10, 5));
    printf("10 * 5 = %d\n", multiply(10, 5));
    printf("10 / 5 = %.2f\n", divide(10, 5));
    
    printf("Total calculations: %d\n", calculation_count);
    
    return 0;
}
```

## Communication Mechanisms
### 1. Function Communication
```c
// ===== math_ops.h =====
#ifndef MATH_OPS_H
#define MATH_OPS_H

// Public API
int power(int base, int exp);
int factorial(int n);
int gcd(int a, int b);

#endif

// ===== math_ops.c =====
#include "math_ops.h"

int power(int base, int exp) {
    int result = 1;
    for (int i = 0; i < exp; i++) {
        result *= base;
    }
    return result;
}

int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// ===== main.c =====
#include <stdio.h>
#include "math_ops.h"

int main() {
    printf("2^10 = %d\n", power(2, 10));
    printf("5! = %d\n", factorial(5));
    printf("GCD(48, 18) = %d\n", gcd(48, 18));
    return 0;
}
```

### 2. Global Variable Communication
```c
// ===== config.h =====
#ifndef CONFIG_H
#define CONFIG_H

// Declare extern variables (defined elsewhere)
extern int debug_level;
extern char log_file[256];
extern int max_connections;

// Function to initialize configuration
void init_config(void);

#endif

// ===== config.c =====
#include "config.h"
#include <string.h>

// Define global variables (storage allocated here)
int debug_level = 1;
char log_file[256] = "app.log";
int max_connections = 100;

void init_config(void) {
    // Can be called to override defaults
    debug_level = 1;
    strcpy(log_file, "app.log");
    max_connections = 100;
}

// ===== main.c =====
#include <stdio.h>
#include "config.h"

int main() {
    printf("Debug level: %d\n", debug_level);
    printf("Log file: %s\n", log_file);
    printf("Max connections: %d\n", max_connections);
    
    // Modify global variables
    debug_level = 2;
    printf("Updated debug level: %d\n", debug_level);
    
    return 0;
}
```

### 3. Structure Communication
```c
// ===== player.h =====
#ifndef PLAYER_H
#define PLAYER_H

typedef struct {
    int id;
    char name[50];
    int health;
    int mana;
    int x, y;
} Player;

// Function declarations
void init_player(Player *p, int id, const char *name);
void move_player(Player *p, int dx, int dy);
void damage_player(Player *p, int amount);
void heal_player(Player *p, int amount);
void print_player(const Player *p);

#endif

// ===== player.c =====
#include "player.h"
#include <stdio.h>
#include <string.h>

void init_player(Player *p, int id, const char *name) {
    p->id = id;
    strcpy(p->name, name);
    p->health = 100;
    p->mana = 100;
    p->x = 0;
    p->y = 0;
}

void move_player(Player *p, int dx, int dy) {
    p->x += dx;
    p->y += dy;
}

void damage_player(Player *p, int amount) {
    p->health -= amount;
    if (p->health < 0) p->health = 0;
}

void heal_player(Player *p, int amount) {
    p->health += amount;
    if (p->health > 100) p->health = 100;
}

void print_player(const Player *p) {
    printf("Player %d: %s (HP: %d, MP: %d) at (%d, %d)\n",
           p->id, p->name, p->health, p->mana, p->x, p->y);
}

// ===== game.c =====
#include <stdio.h>
#include "player.h"

int main() {
    Player hero;
    init_player(&hero, 1, "Aragorn");
    print_player(&hero);
    
    move_player(&hero, 5, 3);
    damage_player(&hero, 25);
    print_player(&hero);
    
    heal_player(&hero, 10);
    print_player(&hero);
    
    return 0;
}
```

### 4. Enum Communication
```c
// ===== status.h =====
#ifndef STATUS_H
#define STATUS_H

typedef enum {
    STATUS_OK = 0,
    STATUS_ERROR = 1,
    STATUS_PENDING = 2,
    STATUS_TIMEOUT = 3,
    STATUS_CANCELLED = 4
} StatusCode;

typedef enum {
    LOG_DEBUG,
    LOG_INFO,
    LOG_WARNING,
    LOG_ERROR
} LogLevel;

// Function declarations
const char* status_to_string(StatusCode code);
void log_message(LogLevel level, const char *msg);

#endif

// ===== status.c =====
#include "status.h"
#include <stdio.h>

const char* status_to_string(StatusCode code) {
    switch (code) {
        case STATUS_OK: return "OK";
        case STATUS_ERROR: return "ERROR";
        case STATUS_PENDING: return "PENDING";
        case STATUS_TIMEOUT: return "TIMEOUT";
        case STATUS_CANCELLED: return "CANCELLED";
        default: return "UNKNOWN";
    }
}

void log_message(LogLevel level, const char *msg) {
    const char *level_str[] = {"DEBUG", "INFO", "WARNING", "ERROR"};
    printf("[%s] %s\n", level_str[level], msg);
}

// ===== main.c =====
#include <stdio.h>
#include "status.h"

int main() {
    StatusCode result = STATUS_OK;
    printf("Status: %s\n", status_to_string(result));
    
    log_message(LOG_INFO, "Application started");
    
    result = STATUS_ERROR;
    printf("Status: %s\n", status_to_string(result));
    log_message(LOG_ERROR, "Something went wrong");
    
    return 0;
}
```

## Advanced Communication Patterns
### 1. Callback Functions
```c
// ===== event_handler.h =====
#ifndef EVENT_HANDLER_H
#define EVENT_HANDLER_H

// Callback function type
typedef void (*EventCallback)(int event_id, void *user_data);

// Register callback
void register_callback(EventCallback cb, void *user_data);

// Trigger event
void trigger_event(int event_id);

#endif

// ===== event_handler.c =====
#include "event_handler.h"
#include <stdio.h>

static EventCallback saved_callback = NULL;
static void *saved_user_data = NULL;

void register_callback(EventCallback cb, void *user_data) {
    saved_callback = cb;
    saved_user_data = user_data;
}

void trigger_event(int event_id) {
    if (saved_callback) {
        saved_callback(event_id, saved_user_data);
    }
}

// ===== main.c =====
#include <stdio.h>
#include "event_handler.h"

void my_handler(int event_id, void *user_data) {
    int *counter = (int*)user_data;
    (*counter)++;
    printf("Event %d received! Counter: %d\n", event_id, *counter);
}

int main() {
    int counter = 0;
    register_callback(my_handler, &counter);
    
    trigger_event(1);
    trigger_event(2);
    trigger_event(3);
    
    return 0;
}
```

### 2. Module Initialization/Shutdown
```c
// ===== database.h =====
#ifndef DATABASE_H
#define DATABASE_H

typedef struct Database Database;  // Opaque type

// Public API
Database* db_connect(const char *host, int port);
void db_disconnect(Database *db);
int db_query(Database *db, const char *sql);
const char* db_get_error(void);

#endif

// ===== database.c =====
#include "database.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Database {
    char host[256];
    int port;
    int connected;
};

static char last_error[512] = {0};

Database* db_connect(const char *host, int port) {
    Database *db = malloc(sizeof(Database));
    if (!db) {
        strcpy(last_error, "Memory allocation failed");
        return NULL;
    }
    
    strcpy(db->host, host);
    db->port = port;
    db->connected = 1;
    
    sprintf(last_error, "Connected to %s:%d", host, port);
    return db;
}

void db_disconnect(Database *db) {
    if (db) {
        db->connected = 0;
        free(db);
        strcpy(last_error, "Disconnected");
    }
}

int db_query(Database *db, const char *sql) {
    if (!db || !db->connected) {
        strcpy(last_error, "Not connected");
        return -1;
    }
    
    printf("Executing: %s\n", sql);
    return 0;
}

const char* db_get_error(void) {
    return last_error;
}

// ===== main.c =====
#include <stdio.h>
#include "database.h"

int main() {
    Database *db = db_connect("localhost", 3306);
    if (db) {
        db_query(db, "SELECT * FROM users");
        db_disconnect(db);
    } else {
        printf("Error: %s\n", db_get_error());
    }
    
    return 0;
}
```
