## Comma Operator
The comma operator allows multiple expressions to be evaluated in sequence, returning the value of the last expression

```c
expr1, expr2, expr3, ..., exprN
// Evaluates left to right, returns value of exprN
```

### How it works
```c
#include <stdio.h>

int main() {
    int a, b, c;
    
    // Comma operator in action
    a = (b = 3, c = 5, b + c);
    // b gets 3, c gets 5, then b+c=8 is assigned to a
    
    printf("a = %d, b = %d, c = %d\n", a, b, c);
    // Output: a = 8, b = 3, c = 5
    
    return 0;
}
```

### Example 1: In For Loops
```c
// Initialize multiple variables
for (int i = 0, j = 10; i < j; i++, j--) {
    printf("i=%d, j=%d\n", i, j);
}

// Update multiple variables in loop
for (int i = 0; i < 10; i++, process_count++) {
    // Do something
}
```

### Example 2: Multiple Operations in One Statement
```c
// Swap values
a = (b, b = a, a = b);  // Confusing, don't do this!

// Better with comma operator
int temp = a;
a = b;
b = temp;
```

