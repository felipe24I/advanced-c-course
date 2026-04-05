## Profiling
Profiling is the process of analyzing a program's execution to identify performance bottlenecks, memory usage, function call frequencies, and runtime behavior. 
It helps answer questions like:
* Which functions take the most time?
* Where is memory being allocated?
* How often are functions called?

### gprof
gprof is a performance analysis tool that generates call graph profiles, showing how much time is spent in each function and how many times functions are called. It combines:
* Flat profile - Time spent in each function
* Call graph - Which functions call which and how often

### How gprof Works
gprof uses two mechanisms:
1. Compile-time instrumentation (-pg flag) - Adds counting code to track function calls
2. Sampling - Records program counter at regular intervals

```bash
# Compilation steps
gcc -pg program.c -o program   # Add profiling instrumentation
./program                        # Run program (creates gmon.out)
gprof program gmon.out > report.txt  # Generate report
```

### Basic Example
### Simple Program
```c
// math_ops.c
#include <stdio.h>
#include <math.h>

int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    int result = 0;
    for (int i = 0; i < b; i++) {
        result = add(result, a);
    }
    return result;
}

double power(double x, int n) {
    double result = 1.0;
    for (int i = 0; i < n; i++) {
        result = result * x;
    }
    return result;
}

int main() {
    int sum = 0;
    int product = 1;
    
    // Call add many times
    for (int i = 1; i <= 100; i++) {
        sum = add(sum, i);
        product = multiply(product, i);
    }
    
    double p = power(2.0, 20);
    
    printf("Sum: %d\n", sum);
    printf("Product: %d\n", product);
    printf("2^20: %.0f\n", p);
    
    return 0;
}
```

```bash
# Compile with profiling
gcc -pg math_ops.c -o math_ops -lm

# Run program
./math_ops

# Generate report
gprof math_ops gmon.out > report.txt

# View report
cat report.txt
```

### Understanding gprof Output
### Flat Profile Section
```text
Flat profile:

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total           
 time   seconds   seconds    calls  ms/call  ms/call  name    
 60.0      0.06     0.06    10000     0.01     0.01  add
 30.0      0.09     0.03    10000     0.00     0.00  multiply
 10.0      0.10     0.01        1    10.00    10.00  power
  0.0      0.10     0.00        1     0.00     0.00  main
```

* % time:	Percentage of total time in this function
* cumulative second:s	Cumulative time including called functions
* self seconds:	Time spent in this function alone
* calls	Number: of times function was called
* self ms/call:	Average time per call (self only)
* total ms/call:	Average time per call (including children)
* name:	Function name

### Call Graph Section
```text
Call graph (profile):

granularity: each sample hit covers 2 byte(s) for 0.10% of 0.10 seconds

index % time    self  children    called     name
                0.01    0.00   10000/10000     main [1]
[2]     10.0    0.01    0.00   10000         add [2]
-----------------------------------------------
                0.01    0.00   10000/10000     main [1]
[3]     10.0    0.01    0.00   10000         multiply [3]
-----------------------------------------------
                0.01    0.00       1/1         main [1]
[4]     10.0    0.01    0.00       1         power [4]
```

**Call Graph Symbols:**
* [n] - Index number for the function
* called - Number of times called / total calls from this parent
* children - Time spent in functions called by this function

## What is Valgrind
Valgrind is an instrumentation framework for building dynamic analysis tools. It's primarily used for:
* Memory leak detection (Memcheck)
* Cache profiling (Cachegrind)
* Call graph profiling (Callgrind)
* Heap profiling (Massif)

```bash
# Basic usage
valgrind ./program

# With leak detection
valgrind --leak-check=full ./program

# With detailed output
valgrind -v --leak-check=full ./program
```
