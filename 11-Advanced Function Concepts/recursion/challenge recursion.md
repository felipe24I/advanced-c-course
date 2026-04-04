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

### Challenge 2
Write a program which will find GCD (greatest common denominator) of two numbers using recursion

### Sample Input/Output
```text
Input 1st number: 10
Input 2nd number: 50

The GCD of 10 and 50 is : 10
```

## Solution 
```c
#include <stdio.h>

int GCD (int n1, int n2)
{
    int residuo = n1 % n2;
    int mcd;
    if (residuo == 0)
    {
        mcd = n2;
        return mcd;
    }
    mcd = GCD(n2, residuo);

}

int main()
{
    int result;
    int n1, n2;

    printf("Input 1st number: ");
    scanf("%d", &n1);

    printf("Input 2nd number: ");
    scanf("%d", &n2);

    result = GCD(n1, n2);
    printf("The GCD of %d and %d is : %d", n1, n2, result);
}
```

### Challenge 3
Write a program which will find reverse a string using recursion

### Sample Input/Output
```Enter the string: studytonight

The original string is: studytonight
The reverse string is: thginotyduts
```

## Solution
```c
#include <stdio.h>
#include <string.h>

void reverse_string_recursive(char *message, int start, int end) {
    // Base case: if start index >= end index, we're done
    if (start >= end) {
        return;
    }

    // Swap characters at start and end positions
    char temp = message[start];
    message[start] = message[end];
    message[end] = temp;

    // Recursively reverse the remaining substring
    reverse_string_recursive(message, start + 1, end - 1);
}

// Wrapper function that's easier to use
void reverse_string(char *message) {
    int length = strlen(message);
    reverse_string_recursive(message, 0, length - 1);
}

// Example usage
int main() {
    char str[100];  // Create a buffer to store user input

    // Ask user to enter a string
    printf("Enter a string to reverse: ");
    fgets(str, sizeof(str), stdin);  // Read string including spaces

    // Remove newline character if present (fgets includes it)
    str[strcspn(str, "\n")] = '\0';

    printf("Original: %s\n", str);
    reverse_string(str);
    printf("Reversed: %s\n", str);

    return 0;
}
```
