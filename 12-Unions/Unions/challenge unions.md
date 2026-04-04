## challenge unions
You need to write a program that uses a union with three members: letter grade, rounded grade, and exact grade. 
Using one union variable, you assign a value to the exact grade and then print all three members. 
Using a second union variable, you assign values to the letter grade and rounded grade, then print all three. 
The program should highlight the difference in output between the two variables and explain why that difference occurs due to how unions share memory.

### Example Output
```text
Union record1 values example
Letter Grade : 
Rounded Grade : 1118633984
Exact Grade : 86.500000

Union record2 values example
Letter Grade : A
Rounded Grade : 100
Exact Grade : 99.500000
```

## Solution
```c
#include <stdio.h>

union student
{
    char letterGrade;
    int roundedGrade;
    float exactGrade;
};

int main()
{
    union student s1;
    union student s2;

    s1.letterGrade = 'A';
    s1.roundedGrade = 5;
    s1.exactGrade = 4.95;

    printf("student 1\n"); // student 1
    printf("letter grade: %c\n", s1.letterGrade); // letter grade: f
    printf("rounded grade: %d\n", s1.roundedGrade); // rounded grade: 1084122726
    printf("exact grade: %.2f\n", s1.exactGrade); // exact grade: 4.95

    printf("\nstudent 2\n"); // student 2

    s2.letterGrade = 'A';
    printf("letter grade: %c\n", s2.letterGrade); // letter grade: A

    s2.roundedGrade = 5;
    printf("rounded grade: %d\n", s2.roundedGrade); // rounded grade: 5

    s2.exactGrade = 4.95;
    printf("exact grade: %.2f\n", s2.exactGrade); // exact grade: 4.95

    return 0;
}
```

**Note:** In a union, all members share the same memory. Writing to one overwrites the others, causing incorrect values when reading a different member.
In the first variable, only the exact grade is set, so the other fields (letter and rounded) contain garbage because they share memory
In the second variable, each field is filled and immediately printed before the next one is written. 
This prevents overwriting, so the output shows the correct values for each field.
