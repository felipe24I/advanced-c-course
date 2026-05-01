## Assert
is a debugging tool used to check if a condition is true while the program runs.

### Definition
assert is a macro from:

```c
#include <assert.h>
```

It verifies a condition, and if the condition is false, the program:

- Prints an error message
- Shows file and line number
- Terminates execution

### Basic syntax
```c
assert(condition);
```

### Example
```c
#include <stdio.h>
#include <assert.h>

int divide(int a, int b) {
    assert(b != 0);  // check condition
    return a / b;
}

int main() {
    printf("%d\n", divide(10, 2)); // OK
    printf("%d\n", divide(10, 0)); // FAIL
    return 0;
}
```

### What happens if it fails?
If b == 0, you get something like:

```text
Assertion failed: (b != 0), file main.c, line 6
```

And the program stops immediately

### Important behavior
#### Only for debugging
- Used to catch logic errors
- Not for handling user input errors

#### Can be disabled
If you compile with:

```bash
gcc -DNDEBUG main.c
```

All assert() statements are removed

### When to use assert
Valid use:

```c
assert(ptr != NULL);
```

**Meaning:** “I expect ptr to NEVER be NULL. If it is, something is broken in my code.”

This is checking your program logic

another valid use:

```c
assert(index >= 0);
```

**Meaning:** Index should never be negative. If it is, I made a mistake somewhere.”

Bad use:

```c
assert(user_input != 0); // user errors should be handled, not asserted
```

It is bad because:
- The user can enter anything
- It’s not your program’s fault

### switching off assertions
It means **disable all** assert() **checks** in your program, so they are **ignored completely** at compile time

### How to disable assert
You do it using this flag when compiling:

```c
gcc -DNDEBUG main.c -o main.exe
```

### What is NDEBUG?
- It’s a special macro
- When it is defined → assert() does nothing

### What actually happens internally
When NDEBUG is defined:

```c
assert(condition);
```

becomes:

```c
((void)0);
```

Meaning: **do nothing**

### Why disable assertions?
#### In production
- Faster execution
- No debug interruptions

#### In development
- You WANT assertions ON to catch bugs

### What are compile-time assertions?
They are checks that happen **during compilation**, not when the program runs.

If the condition is false:
- The program **does NOT compile**

### Key difference
- **assert():**	Is checked in runtime, if false the result is program crashes
- **Compile-time assert:**	Is checked in compile time, if false the result is compilation error

### In C (modern way)
C11 introduced:

```c
_Static_assert(condition, "message");
```

### Example
```c
#include <stdio.h>

_Static_assert(sizeof(int) == 4, "int must be 4 bytes");

int main() {
    printf("Program runs\n");
    return 0;
}
```

### What happens?
#### If condition is true
Program compiles normally

#### If condition is false
```text
error: static assertion failed: "int must be 4 bytes"
```

Compilation stops

### Why use it?
To verify things like:
- Data type sizes
- Structure layout
- Constants
- Assumptions about the system
