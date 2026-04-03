## Recursion
Recursion is a programming technique where a function calls itself to solve a problem by breaking it down into smaller, similar subproblems.

```c
#include <stdio.h>

void countdown(int n) {
    if (n == 0) {
        printf("Blastoff!\n");
        return;
    }
    printf("%d... ", n);
    countdown(n - 1);  // Function calls itself
}

int main() {
    countdown(5);
    // Output: 5... 4... 3... 2... 1... Blastoff!
    return 0;
}
```

## The Two Essential Parts of Recursion

