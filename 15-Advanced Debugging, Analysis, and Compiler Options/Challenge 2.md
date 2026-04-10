## Challenge 2
This challenge is about debugging a buggy C program that contains a serious memory error

```c
#include <stdlib.h>
#include <stdio.h>

int sum(int x, int y, int z) {
  char c = 2;
  int *a;

  *a = 5;

  return (c + x + y + z + *a) / 3;
}

int main(int argc, char *argv[]) {
  int i, j, k;
  int result;

  if (argc == 1) {
     printf("Please specify three numbers as parameters.\n");
     exit(1);
  }

  i = atoi(argv[1]);
  j = atoi(argv[2]);
  k = atoi(argv[3]);

  result = sum(i,j,12) + sum(j,k,19) + sum(i,k,13);

  printf("%d\n", result);

  return 0;
}
```

### The Problem
The program has a critical bug in the sum function:

```c
int sum(int x, int y, int z) {
  char c = 2;
  int *a;        // a is uninitialized - it points to random memory
  *a = 5;        // CRASH! Writing to a random memory address
  return (c + x + y + z + *a) / 3;
}
```

The pointer a is never set to point to valid memory. When you do *a = 5, you're writing to some random location in memory. 
This causes undefined behavior - typically a segmentation fault/crash.

### What the Challenge Wants You To Do
1. **Add a debug statement** right after the last successful fprintf in the sum function:

```c
fprintf(stderr, "a=%ld\n", (long)a);
```

This will print the garbage address that a contains, helping explain why the next operation fails.

2. **Run with only one parameter** to see how it fails (the program expects 3 numbers)
3. **Add debugging statements to** main() to pinpoint exactly which line causes the crash
4. **Always use** \n in fprintf/printf format strings to ensure output isn't buffered
5. **Don't fix the actual bugs** - the challenge wants you to understand the debugging process, not correct the program

### Expected Behavior
* With 3 parameters: The program will likely crash when sum tries to write to *a = 5
* With fewer parameters: It will crash earlier when accessing argv[2] or argv[3] (array out of bounds)

### The Learning Goal
To practice debugging techniques:
* Adding strategic print statements
* Understanding why crashes happen (uninitialized pointers vs. missing arguments)
* Seeing how buffering can hide debug output if you forget \n

## Solution
### Step 1: Add Debugging to the sum Function
Add print statements to see what's happening right before the crash:

```c
#include <stdlib.h>
#include <stdio.h>

int sum(int x, int y, int z) {
  char c = 2;
  int *a;
  
  fprintf(stderr, "sum() called with x=%d, y=%d, z=%d\n", x, y, z);
  fprintf(stderr, "About to write to *a\n");
  
  *a = 5;  // This line will crash
  
  fprintf(stderr, "Successfully wrote to *a\n");  // This won't execute
  
  return (c + x + y + z + *a) / 3;
}
```

