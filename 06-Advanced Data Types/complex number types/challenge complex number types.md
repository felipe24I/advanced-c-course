## Challenge complex number types
You need to write a program using complex.h to create and display complex numbers. First, compute Euler's formula $e^iπ$ and output its real and imaginary parts. 
Then, create the complex number $1 + 2i$and its conjugate $1−2i$, displaying the real and imaginary parts of both using the creal() and cimag() functions.

### Output example
```text
Euler's formula: e^(i * pi) = real: -1.000000, imag: 0.000000
Complex number 1 + 2i: real: 1.000000, imag: 2.000000
Conjugate 1 - 2i: real: 1.000000, imag: -2.000000
```

## Solution
```c
//Program that performs some calculations on complex numbers
#include <stdio.h>
#include <complex.h>
#include <math.h>

int main()
{
    printf("Number 1\n");
    double complex num = 2.0 + 1.0 *(I*I);
    printf("number: %.2f %+.2fi\n", creal(num), cimag(num));
    printf("Real number: %.2f\n", (double)num);
    printf("Imaginary number: %.2fi\n", cimag(num));

    printf("Number 2\n");
    double complex num2 = 2.0 + 1.0 *cpow(I,2);
    printf("\nnumber: %.2f %+.2fi\n", creal(num2), cimag(num2));
    printf("Real number: %.2f\n", (double)num2);
    printf("Imaginary number: %.2fi\n", cimag(num2));

    printf("\nNumber3");
    double PI = acos(-1);
    double complex num3 = cexp(I*PI);
    printf("\nReal number: %.2f\n", creal(num3));
    printf("Imaginary number: %.2fi\n", cimag(num3));

    printf("\nNumber4");
    double complex num4 = 1.0 + 2.0*I;
    double complex num4_conj = conj(num4);
    printf("\nReal number: %.2f\n", creal(num4));
    printf("Imaginary number: %.2fi", cimag(num4));
    printf("\nReal number conj: %.2f\n", creal(num4_conj));
    printf("Imaginary number conj: %.2fi", cimag(num4_conj));
    printf("\nReal number mult: %.2f\n", creal(num4 * num4_conj));
    printf("Imaginary number mult: %.2fi", cimag(num4 * num4_conj));
}
```
