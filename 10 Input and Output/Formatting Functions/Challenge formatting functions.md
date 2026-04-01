## Challenge formatting functions
You need to write a C program that reads integers from a file named numbers.txt using either fscanf or a combination of fgets and sscanf. 
For each number read, the program must classify it as even, odd, or prime and print the result to standard output in the format "Even number found: X", "Odd number found: X", or "Prime number found: X". 
Prime classification takes precedence over odd classification, and the number 2 should be classified as prime rather than even, while negative numbers and zero are only classified as even or odd.

### Example
#### File numbers.txt:
```text
73771782    81296771    79982326    75332246    10128193
81643413    76259734    94432076    50063976    91748657
42311916    -1920042    90747362    53851612    43498487
73193311    96685173    39019033    8630045    59322952
```

#### Expected Output:
```text
File opened successfully. Reading integers from file.

Even number found: 73771782
Odd number found: 81296771
Even number found: 79982326
Even number found: 75332246
Prime number found: 10128193
Odd number found: 81643413
Even number found: 76259734
Even number found: 94432076
Even number found: 50063976
Odd number found: 91748657
Even number found: 42311916
Even number found: -1920042
Even number found: 90747362
Even number found: 53851612
Odd number found: 43498487
Prime number found: 73193311
Odd number found: 96685173
Prime number found: 39019033
Odd number found: 8630045
Even number found: 59322952
```

## Solution
```c
#include <stdio.h>

// Function to check if a number is prime
int isPrime(const int num)
{
    int i = 0;

    // Only positive integers are prime
    if (num < 0)
        return 0;

    for ( i=2; i<=num/2; i++ )
    {
        /*
         * If the number is divisible by any number
         * other than 1 and self then it is not prime
         */
        if (num % i == 0)
        {
            return 0;
        }
    }

    return 1;
}

int main()
{
    FILE *file;
    int num;
    int numbers_read;

    file = fopen("numbers.txt", "r");

    if (file == NULL)
    {
        printf("Error, could not open the file\n");
        return 1;
    }

    printf("File opened successfully. Reading integers from file\n");

    // Read numbers one by one directly from the file
    while ((numbers_read = fscanf(file, "%d", &num)) != EOF)
    {
        if (numbers_read == 1)  // Successfully read an integer
        {
            if (num % 2 == 0)
            {
                printf("Even number found: %d\n", num);
            }
            else if (isPrime(num))
            {
                printf("Prime number found: %d\n", num);
            }
            else
            {
                printf("Odd number found: %d\n", num);
            }
        }
    }

    fclose(file);

    return 0;
}
```
