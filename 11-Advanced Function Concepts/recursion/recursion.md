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
Every recursive function must have:

* **1. Base Case** - The stopping condition (when to stop calling itself)
* **2. Recursive Case** - The function calling itself with a smaller problem

```c
int factorial(int n) {
    // Base case: stop here
    if (n == 0 || n == 1) {
        return 1;
    }
    // Recursive case: call myself with smaller problem
    return n * factorial(n - 1);
}
```

## How Recursion Works (The Stack)
```c
#include <stdio.h>

void explain_recursion(int n) {
    printf("Entering level %d\n", n);
    
    if (n == 0) {
        printf("Base case reached at level 0\n");
    } else {
        explain_recursion(n - 1);
    }
    
    printf("Exiting level %d\n", n);
}

int main() {
    explain_recursion(3);
    return 0;
}

/* Output:
Entering level 3
Entering level 2
Entering level 1
Entering level 0
Base case reached at level 0
Exiting level 0
Exiting level 1
Exiting level 2
Exiting level 3
*/
```

## Classic Recursion Examples
### 1. Factorial
```c
#include <stdio.h>

// Recursive factorial
int factorial_recursive(int n) {
    if (n <= 1) return 1;
    return n * factorial_recursive(n - 1);
}

// Iterative factorial (for comparison)
int factorial_iterative(int n) {
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

int main() {
    printf("5! = %d (recursive)\n", factorial_recursive(5));
    printf("5! = %d (iterative)\n", factorial_iterative(5));
    return 0;
}
```

### 2. Fibonacci Sequence
```c
#include <stdio.h>

// Recursive Fibonacci (inefficient - exponential time)
int fib_recursive(int n) {
    if (n <= 1) return n;
    return fib_recursive(n - 1) + fib_recursive(n - 2);
}

// Optimized recursive with memoization
int fib_memo(int n, int memo[]) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo);
    return memo[n];
}

// Iterative Fibonacci (efficient)
int fib_iterative(int n) {
    if (n <= 1) return n;
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}

int main() {
    int n = 10;
    printf("Fibonacci(%d):\n", n);
    printf("  Recursive: %d\n", fib_recursive(n));
    
    int memo[100];
    for (int i = 0; i < 100; i++) memo[i] = -1;
    printf("  Memoized: %d\n", fib_memo(n, memo));
    printf("  Iterative: %d\n", fib_iterative(n));
    
    return 0;
}
```

### 3. Power Function
```c
#include <stdio.h>

// Simple recursive power
double power_recursive(double base, int exp) {
    if (exp == 0) return 1;
    if (exp < 0) return 1 / power_recursive(base, -exp);
    return base * power_recursive(base, exp - 1);
}

// Efficient recursive power (exponentiation by squaring)
double power_fast(double base, int exp) {
    if (exp == 0) return 1;
    if (exp < 0) return 1 / power_fast(base, -exp);
    
    if (exp % 2 == 0) {
        double half = power_fast(base, exp / 2);
        return half * half;
    } else {
        return base * power_fast(base, exp - 1);
    }
}

int main() {
    printf("2^10 = %.0f\n", power_recursive(2, 10));
    printf("2^10 (fast) = %.0f\n", power_fast(2, 10));
    printf("3^-2 = %.3f\n", power_recursive(3, -2));
    
    return 0;
}
```

### 4. Greatest Common Divisor (GCD)
```c
#include <stdio.h>

// Euclidean algorithm recursively
int gcd_recursive(int a, int b) {
    if (b == 0) return a;
    return gcd_recursive(b, a % b);
}

// Iterative version
int gcd_iterative(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

int main() {
    printf("GCD(48, 18) = %d\n", gcd_recursive(48, 18));
    printf("GCD(100, 35) = %d\n", gcd_recursive(100, 35));
    printf("GCD(1071, 462) = %d\n", gcd_recursive(1071, 462));
    
    return 0;
}
```

## Practical Recursion Examples
### 1. Binary Search
```c
#include <stdio.h>

int binary_search(int arr[], int left, int right, int target) {
    if (left > right) return -1;  // Base case: not found
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) return mid;
    if (arr[mid] > target) return binary_search(arr, left, mid - 1, target);
    return binary_search(arr, mid + 1, right, target);
}

int main() {
    int sorted[] = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    int size = sizeof(sorted) / sizeof(sorted[0]);
    
    int target = 13;
    int index = binary_search(sorted, 0, size - 1, target);
    
    if (index != -1) {
        printf("%d found at index %d\n", target, index);
    } else {
        printf("%d not found\n", target);
    }
    
    return 0;
}
```

**Note:** iteration generally uses less memory than recursion, as recursion requires storing multiple function calls on the stack, whereas iteration utilizes a single execution frame.
