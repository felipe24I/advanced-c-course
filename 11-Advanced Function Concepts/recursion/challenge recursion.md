## Challenge recursion
### Challenge 1
Write a program which will calculate the sum of numbers from 1 to n using recursion

### Sample Input/Output
```text
Input the last number of the range starting from 1: 5
The sum of numbers from 1 to 5: 15
```

## Solution
```c
#include <stdio.h>

// Recursive function to calculate sum from 1 to n
int sumRecursive(int n) {
    // Base case: if n is 0 or 1, return n
    if (n == 0 || n == 1) {
        return n;
    }
    // Recursive case: n + sum of numbers from 1 to (n-1)
    return n + sumRecursive(n - 1);
}

int main() {
    int n;
    
    // Get input from user
    printf("Input the last number of the range starting from 1: ");
    scanf("%d", &n);
    
    // Check for valid input
    if (n < 1) {
        printf("Please enter a number greater than or equal to 1.\n");
        return 1;
    }
    
    // Calculate and display the sum
    int result = sumRecursive(n);
    printf("The sum of numbers from 1 to %d: %d\n", n, result);
    
    return 0;
}
```

### How the recursion works:
* The function sumRecursive(n) calls itself with n-1 until it reaches the base case
* Base case: when n is 0 or 1, the function returns n
* Each recursive call adds the current number to the sum of all smaller numbers

### Example for n = 5:
```text
sumRecursive(5) = 5 + sumRecursive(4)
                = 5 + (4 + sumRecursive(3))
                = 5 + (4 + (3 + sumRecursive(2)))
                = 5 + (4 + (3 + (2 + sumRecursive(1))))
                = 5 + (4 + (3 + (2 + 1)))
                = 5 + (4 + (3 + 3))
                = 5 + (4 + 6)
                = 5 + 10
                = 15
```
