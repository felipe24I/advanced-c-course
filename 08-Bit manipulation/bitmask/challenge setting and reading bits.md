## challenge setting and reading bits
You need to write a C program that takes an integer and a bit position from the user. 
The program should check whether that specific bit is set to 1 or not, then set it to 1, and finally display the number before and after the change.

### Expected output
```text
Enter any number: 10
Enter nth bit to check and set (0-31): 2
The 2 bit is set to 0

Bit set successfully.

Number before setting 2 bit: 10 (in decimal)
Number after setting 2 bit: 14 (in decimal)
```

## Solution
```c
#include<stdio.h>
#include<math.h>

void number_bit(long long *num, int *bit);
void reading_bit(long long num, int bit); //pass by value since no modification needed
void setting_bit(long long *num, int bit);
void print_number(long long num_copy, long long num, int bit);
void convertBinaryToDecimal(long long *num);
void convertDecimalToBinary(long long *num);


int main()
{
    long long num, num_copy;
    int bit;

    number_bit(&num, &bit);
    num_copy = num; // save original before conversion

    // Convert binary input to actual integer value
    convertBinaryToDecimal(&num);

    reading_bit(num, bit);
    setting_bit(&num, bit);

    convertDecimalToBinary(&num);
    print_number(num_copy, num, bit);


    return 0;
}

void number_bit(long long *num, int *bit)
{
    printf("Enter a binary number: ");
    scanf("%lld", num);
    printf("\nEnter nth bit to check and set (0-31): ");
    scanf("%d", bit);
}

void reading_bit(long long num, int bit)
{
    int v_bit = (num >> bit) & 1;
    printf("\nthe %d bit is %d\n", bit, v_bit);
}

void setting_bit(long long *num, int bit)
{
    *num |= (1 << bit); // dereference pointer
    printf("\n%d bit set successfully\n", bit);
}

void convertDecimalToBinary (long long *num)
{
    long long temp = *num; //store original
    long long binaryNumber = 0;
    int remainder;
    int i = 1;
    while (temp != 0)
    {
        remainder = temp%2;
        temp = temp/2;
        binaryNumber += remainder*i;
        i = i*10;
    }
    *num = binaryNumber; // assign back through pointer
}

void convertBinaryToDecimal(long long *num)
{
    long long decimalNumber = 0;
    int i = 0, remainder;
    long long temp = *num; //store original
    while (temp!=0)    {
        remainder = temp%10;
        temp /= 10;
        decimalNumber += remainder*pow(2,i);
        i++;
    }
    *num = decimalNumber;
}

void print_number(long long num_copy, long long num, int bit)
{
    printf("\nNumber before setting %d bit: %lld\n", bit, num_copy);
    printf("\nNumber after setting %d bit: %lld\n", bit, num);
}
```
