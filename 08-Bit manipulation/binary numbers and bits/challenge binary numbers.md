## Challenge binary numbers
You need to write two functions: one that converts a binary number (given as a long long) into its decimal integer equivalent, and another that converts a decimal integer into its binary representation (returned as a long long). Together, these challenges test your understanding of converting between binary and decimal number systems.

```c
#include <stdio.h>
#include <math.h>

int convertBinaryToDecimal(long long n);
long long convertDecimalToBinary(int n);

int main() {
    long long n1;
    int n2 = 0;
    long long result = 0;
    printf("Enter a binary number: ");
    scanf("%lld", &n1);
    printf("%lld in binary = %d in decimal", n1, convertBinaryToDecimal(n1));

    printf("Enter a decimal number: ");
    scanf("%d", &n2);
    result = convertDecimalToBinary(n2);
    printf("%d in decimal = %lld to binary", n2 , result);

    return 0;
}

int convertBinaryToDecimal(long long n) {
    int decimalNumber = 0, i = 0, remainder;
    while (n!=0)    {
        remainder = n%10;
        n /= 10;
        decimalNumber += remainder*pow(2,i);
        ++i;
    }
    return decimalNumber;
}

long long convertDecimalToBinary(int n){
   long long binaryNumber = 0;
   int remainder, i=1;

   while(n != 0) {
     remainder = n%2;
     n = n / 2;
     binaryNumber += remainder * i;
     i = i * 10;
   }

   return binaryNumber;

}
```
