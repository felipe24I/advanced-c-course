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

### Step 2: Add Debugging to main()
Add print statements to trace execution in main:

```c
int main(int argc, char *argv[]) {
  int i, j, k;
  int result;
  
  fprintf(stderr, "Program started with %d arguments\n", argc);
  
  if (argc == 1) {
     printf("Please specify three numbers as parameters.\n");
     exit(1);
  }
  
  fprintf(stderr, "Converting arguments to integers...\n");
  i = atoi(argv[1]);
  j = atoi(argv[2]);
  k = atoi(argv[3]);
  fprintf(stderr, "i=%d, j=%d, k=%d\n", i, j, k);
  
  fprintf(stderr, "About to call sum(i,j,12)\n");
  result = sum(i,j,12) + sum(j,k,19) + sum(i,k,13);
  fprintf(stderr, "All sum calls completed\n");
  
  printf("%d\n", result);
  
  return 0;
}
```

### Step 3: Run with Different Parameters
#### Test 1 - No parameters:
```bash
main.exe
```
##### Compiling
<img width="1323" height="48" alt="image" src="https://github.com/user-attachments/assets/d07359d9-1a10-4944-a70a-8b9ae146e45a" />

##### Running with no parameters
<img width="1115" height="87" alt="image" src="https://github.com/user-attachments/assets/223c0843-cae2-4752-8cb4-0552de0008c7" />

#### Test 2 - Only one parameter:
```bash
main.exe 5
```

This will crash when accessing argv[2] (doesn't exist)

##### Running with one parameter
<img width="1115" height="87" alt="image" src="https://github.com/user-attachments/assets/aba8711d-af4d-4c4e-8d0c-714f58d3fa52" />

#### Test 3 - Three parameters (will crash in sum):
```bash
main.exe 5 10 15
```

Output shows it reaches "About to write to *a" then crashes

##### Running with three parameters
<img width="1222" height="378" alt="image" src="https://github.com/user-attachments/assets/60e1f05d-13f5-462b-8426-9223b50d88be" />

##### Why Didn't It Crash?
The uninitialized pointer int *a contains whatever garbage value was on the stack. In your case, that random memory address happened to be writable memory, so *a = 5 succeeded. But this is dangerous - you just corrupted some random memory location that might belong to another variable or function!

### Step 4: The "Aha!" Debug Statement
The challenge specifically asks to add this line after the last successful print:

```c
int sum(int x, int y, int z) {
  char c = 2;
  int *a;
  
  fprintf(stderr, "a=%ld\n", (long)a);  // Print the garbage address
  
  *a = 5;  // This will fail because 'a' points to invalid memory
  
  return (c + x + y + z + *a) / 3;
}
```

This prints the random address that a contains (like a=140723845490128), showing it's not pointing to valid memory.

<img width="1212" height="187" alt="image" src="https://github.com/user-attachments/assets/45f2b6ad-edee-4b58-aaee-b983e9f631ea" />


##### Why is a=0 instead of garbage?
This is happening because your compiler is automatically initializing local variables to zero for safety. Many modern compilers (especially with debugging flags) will initialize stack variables to zero to make bugs more predictable.

##### Why the sum wasn't completed
The program crashed at *a = 5 because a contains 0 (NULL), and writing to NULL causes a segmentation fault!

### Conclusion
#### Before (worked):
The uninitialized pointer a randomly contained a valid memory address (like 0x12345678) that your program was allowed to write to.

#### Now (crashes):
Adding the debug statement fprintf(stderr, "a=%ld\n", (long)a); changed the stack layout, so now a randomly contains 0 (NULL) - an invalid address that crashes when you try to write to it.

#### Bottom line:
Undefined behavior is unpredictable. Same bug, different results, just because you added one line of code. The program was never correct - you just got lucky the first time.


