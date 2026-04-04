## Conditional Compilation
Conditional compilation allows parts of code to be included or excluded from compilation based on conditions evaluated at compile time. 
It's like if statements for the preprocessor.

Includes a set of commands that tell the compiler to accept or ignore blocks of information or code according to conditions at the time of compilation

```c
#include <stdio.h>

#define DEBUG 1

int main() {
    #if DEBUG
        printf("Debug mode enabled\n");
        printf("This code only compiles when DEBUG is non-zero\n");
    #endif
    
    printf("This code always compiles\n");
    
    return 0;
}
```

## Core Directives for Conditional Compilation
* **#if:** Conditional (constant expression = is a calculation that the compiler can solve at compile time, before the program even runs) 
* **#ifdef:** If macro is defined
* **#ifndef:** If macro is NOT defined
* **#else:** Alternative branch
* **#elif:** Else if (combined condition = compound condition)
* **#elifdef:** Else if defined (C23)
* **#elifndef:** Else if not defined (C23)
* **#endif:** End conditional block
* **#defined():** Operator to check if a specific macro name has been created using #define

Every **#if** construct ends with and **#endif**

Directives **#ifdef** and **ifndef** are provided as shorthand for:
* **#if** defined (name)
* **#if** !defined (name)

### Why use defined() instead of #ifdef?
While **#ifdef** only lets you check **one** macro at a time, defined() allows you to create **compoud conditions** in a single line.
* Using **#ifdef** (Limited):

```c
#ifdef DEBUG
    // You can only check one thing here
#endif
```

* Using **defined()** (Flexible):

```c
#if defined(DEBUG) && !defined(VERSION_LITE)
    // Checks if DEBUG exists AND VERSION_LITE does NOT exist
#endif
```

### 1. Basic #if / #else / #endif
```c
#include <stdio.h>

#define VERSION 2

int main() {
    #if VERSION == 1
        printf("Running version 1.0\n");
        printf("Legacy mode enabled\n");
    #elif VERSION == 2
        printf("Running version 2.0\n");
        printf("New features active\n");
    #elif VERSION == 3
        printf("Running version 3.0\n");
        printf("Experimental features active\n");
    #else
        printf("Unknown version\n");
    #endif
    
    return 0;
}
```

### 2. #ifdef and #ifndef
```c
#include <stdio.h>

#define FEATURE_LOGGING
// #define FEATURE_NETWORK  // Commented out - not defined

int main() {
    #ifdef FEATURE_LOGGING
        printf("Logging feature is enabled\n");
        // Initialize logging system
    #endif
    
    #ifndef FEATURE_NETWORK
        printf("Network feature is DISABLED\n");
        printf("Using offline mode\n");
    #endif
    
    // Common pattern: header guards
    // #ifndef MYHEADER_H
    // #define MYHEADER_H
    // ... header content ...
    // #endif
    
    return 0;
}
```

### 3. The defined() Operator
```c
#include <stdio.h>

#define DEBUG 1
// #define TEST  // Commented out

int main() {
    // Check if macros are defined (regardless of value)
    #if defined(DEBUG)
        printf("DEBUG is defined\n");
    #endif
    
    #if !defined(TEST)
        printf("TEST is NOT defined\n");
    #endif
    
    // Combine conditions
    #if defined(DEBUG) && defined(TEST)
        printf("Both DEBUG and TEST are defined\n");
    #elif defined(DEBUG)
        printf("Only DEBUG is defined\n");
    #endif
    
    // Complex conditions
    #if defined(DEBUG) && DEBUG > 1
        printf("DEBUG level > 1\n");
    #endif
    
    return 0;
}
```








