##  Complex Numbers in C
C99 introduced native support for complex numbers through the complex.h header, providing built-in types and functions for complex arithmetic.

### Basic Complex Types
```c
#include <stdio.h>
#include <complex.h>

int main() {
    // Three complex types
    float complex f = 1.0f + 2.0f * I;      // float complex
    double complex d = 1.0 + 2.0 * I;        // double complex
    long double complex ld = 1.0L + 2.0L * I; // long double complex
    
    printf("float complex: %.1f + %.1fi\n", crealf(f), cimagf(f));
    printf("double complex: %.1f + %.1fi\n", creal(d), cimag(d));
    printf("long double complex: %.1Lf + %.1Lfi\n", creall(ld), cimagl(ld));
    
    return 0;
}
```

### Creating Complex Numbers
```c
#include <stdio.h>
#include <complex.h>

int main() {
    // Method 1: Using I macro
    double complex z1 = 3.0 + 4.0 * I;
    
    // Method 2: Using complex macro
    double complex z2 = CMPLX(3.0, 4.0);
    
    // Method 3: Using CMPLXF for float, CMPLXL for long double
    float complex z3 = CMPLXF(1.0f, 2.0f);
    long double complex z4 = CMPLXL(1.0L, 2.0L);
    
    // Method 4: From real and imaginary parts
    double real = 5.0;
    double imag = 6.0;
    double complex z5 = real + imag * I;
    
    printf("z1 = %.1f + %.1fi\n", creal(z1), cimag(z1));
    printf("z2 = %.1f + %.1fi\n", creal(z2), cimag(z2));
    printf("z3 = %.1f + %.1fi\n", crealf(z3), cimagf(z3));
    printf("z5 = %.1f + %.1fi\n", creal(z5), cimag(z5));
    
    return 0;
}
```

### Basic Arithmetic Operations
```c
#include <stdio.h>
#include <complex.h>

int main() {
    double complex a = 2.0 + 3.0 * I;
    double complex b = 4.0 + 5.0 * I;
    
    // Addition
    double complex sum = a + b;
    printf("Sum: (%.1f + %.1fi) + (%.1f + %.1fi) = %.1f + %.1fi\n",
           creal(a), cimag(a), creal(b), cimag(b),
           creal(sum), cimag(sum));
    
    // Subtraction
    double complex diff = a - b;
    printf("Difference: (%.1f + %.1fi) - (%.1f + %.1fi) = %.1f + %.1fi\n",
           creal(a), cimag(a), creal(b), cimag(b),
           creal(diff), cimag(diff));
    
    // Multiplication
    double complex prod = a * b;
    printf("Product: (%.1f + %.1fi) * (%.1f + %.1fi) = %.1f + %.1fi\n",
           creal(a), cimag(a), creal(b), cimag(b),
           creal(prod), cimag(prod));
    
    // Division
    double complex quot = a / b;
    printf("Quotient: (%.1f + %.1fi) / (%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(a), cimag(a), creal(b), cimag(b),
           creal(quot), cimag(quot));
    
    return 0;
}
```

## Complex Functions
### 1. Basic Functions
```c
#include <stdio.h>
#include <complex.h>
#include <math.h>

int main() {
    double complex z = 3.0 + 4.0 * I;
    
    // Real and imaginary parts
    printf("z = %.1f + %.1fi\n", creal(z), cimag(z));
    
    // Conjugate
    double complex conj = conj(z);
    printf("Conjugate: %.1f + %.1fi\n", creal(conj), cimag(conj));
    
    // Magnitude (absolute value)
    double magnitude = cabs(z);
    printf("Magnitude: %.2f\n", magnitude);
    
    // Phase angle (argument)
    double phase = carg(z);
    printf("Phase: %.2f radians (%.2f degrees)\n", phase, phase * 180 / M_PI);
    
    // Projection (Riemann sphere)
    double complex proj = cproj(z);
    printf("Projection: %.1f + %.1fi\n", creal(proj), cimag(proj));
    
    return 0;
}
```

### 2. Exponential and Logarithmic Functions
```c
#include <stdio.h>
#include <complex.h>
#include <math.h>

int main() {
    double complex z = 1.0 + 1.0 * I;
    
    // Exponential
    double complex exp_z = cexp(z);
    printf("exp(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(exp_z), cimag(exp_z));
    
    // Natural log
    double complex log_z = clog(z);
    printf("log(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(log_z), cimag(log_z));
    
    // Base-10 log
    double complex log10_z = clog10(z);
    printf("log10(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(log10_z), cimag(log10_z));
    
    // Power
    double complex power = cpow(z, 2.0 + 0.0 * I);
    printf("(%.1f + %.1fi)^2 = %.1f + %.1fi\n",
           creal(z), cimag(z), creal(power), cimag(power));
    
    // Square root
    double complex sqrt_z = csqrt(z);
    printf("sqrt(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(sqrt_z), cimag(sqrt_z));
    
    return 0;
}
```

### 3. Trigonometric Functions
```c
#include <stdio.h>
#include <complex.h>
#include <math.h>

int main() {
    double complex z = 1.0 + 0.5 * I;
    
    // Sine
    double complex sin_z = csin(z);
    printf("sin(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(sin_z), cimag(sin_z));
    
    // Cosine
    double complex cos_z = ccos(z);
    printf("cos(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(cos_z), cimag(cos_z));
    
    // Tangent
    double complex tan_z = ctan(z);
    printf("tan(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(tan_z), cimag(tan_z));
    
    // Hyperbolic sine
    double complex sinh_z = csinh(z);
    printf("sinh(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(sinh_z), cimag(sinh_z));
    
    // Hyperbolic cosine
    double complex cosh_z = ccosh(z);
    printf("cosh(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(cosh_z), cimag(cosh_z));
    
    // Hyperbolic tangent
    double complex tanh_z = ctanh(z);
    printf("tanh(%.1f + %.1fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(tanh_z), cimag(tanh_z));
    
    return 0;
}
```

### 4. Inverse Trigonometric Functions
```c
#include <stdio.h>
#include <complex.h>
#include <math.h>

int main() {
    double complex z = 0.5 + 0.3 * I;
    
    // Arc sine
    double complex asin_z = casin(z);
    printf("asin(%.2f + %.2fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(asin_z), cimag(asin_z));
    
    // Arc cosine
    double complex acos_z = cacos(z);
    printf("acos(%.2f + %.2fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(acos_z), cimag(acos_z));
    
    // Arc tangent
    double complex atan_z = catan(z);
    printf("atan(%.2f + %.2fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(atan_z), cimag(atan_z));
    
    // Inverse hyperbolic functions
    double complex asinh_z = casinh(z);
    printf("asinh(%.2f + %.2fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(asinh_z), cimag(asinh_z));
    
    double complex acosh_z = cacosh(z);
    printf("acosh(%.2f + %.2fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(acosh_z), cimag(acosh_z));
    
    double complex atanh_z = catanh(z);
    printf("atanh(%.2f + %.2fi) = %.2f + %.2fi\n",
           creal(z), cimag(z), creal(atanh_z), cimag(atanh_z));
    
    return 0;
}
```
