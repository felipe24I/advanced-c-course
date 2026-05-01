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
