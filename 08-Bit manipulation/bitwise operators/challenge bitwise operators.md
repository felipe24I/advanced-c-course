## Challenge bitwise operators
You need to write a program that reads two integers from the user and applies several bitwise operators (~, &, |, ^, <<, >>). 
The results must be displayed as binary strings using a helper function that converts decimal to binary. The output should follow the format shown in the example.

### Expected output
```text
Enter an integer: 10
Enter another integer: 11

The result of applying the ~ operator on number 10 (1010) is: -1011
The result of applying the ~ operator on number 11 (1011) is: -1100
The result of applying the & operator on number 10 (1010) and number 11 (1011) is: 1010
The result of applying the | operator on number 10 (1010) and number 11 (1011) is: 1011
The result of applying the ^ operator on number 10 (1010) and number 11 (1011) is: 1
The result of applying the left shift operator << on number 10 (1010) by 2 places is number (101000) is: 40
```
## Solution
```c
#include <stdio.h>

long long decimalToBinary (long long num)
{
    long long binaryNumber = 0;
    int remainder;
    int i = 1;
    while (num != 0)
    {
        remainder = num%2;
        num = num/2;
        binaryNumber += remainder*i;
        i = i*10;
    }
    return binaryNumber;
}

int main ()
{
    long long num1, num2;
    long long cnum1, cnum2, result1, result2, result3, result4, result5;

    printf("Enter num1: ");
    scanf("%lld", &num1);
    printf("Enter num2: ");
    scanf("%lld", &num2);

    cnum1 = ~num1;
    cnum2 = ~num2;

    result1 = num1 & num2;
    result2 = num1 | num2;
    result3 = num1 ^ num2;
    result4 = num1 >> 2;
    result5 = num2 << 2;

    printf("num1 = %lld\n", decimalToBinary(num1));
    printf("num2 = %lld\n", decimalToBinary(num2));
    printf("~num1 = %lld\n", decimalToBinary(cnum1));
    printf("~num2 = %lld\n", decimalToBinary(cnum2));
    printf("num1 & num2 = %lld\n", decimalToBinary(result1));
    printf("num1 | num2 = %lld\n", decimalToBinary(result2));
    printf("num1 ^ num2 = %lld\n", decimalToBinary(result3));
    printf("num1 >> 2 = %lld\n", decimalToBinary(result4));
    printf("num2 << 2 = %lld\n", decimalToBinary(result5));
}
```
