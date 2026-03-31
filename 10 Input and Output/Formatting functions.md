## 1. sprintf()
**"string printf"**, is a **formatted output function** that writes formatted data to a **string** (character array) instead of printing to the screen. It's like printf(), but the output goes into a buffer.

```c
int sprintf(char *str, const char *format, ...);
```

* **str:** Pointer to character array where output will be stored
* **format:** Format string (like printf())
* **...:** Variables to be formatted
* **Returns:** 	Number of characters written (excluding null terminator)

### Example 1: Basic Usage
```c
#include <stdio.h>

int main()
{
    char buffer[100];
    int age = 25;
    char name[] = "Jason";
    
    sprintf(buffer, "Name: %s, Age: %d", name, age);
    
    printf("Buffer contains: %s\n", buffer);
    
    return 0;
}
```

### Output:
```text
Buffer contains: Name: Jason, Age: 25
```

### Example 2: Building a String
```c
#include <stdio.h>

int main()
{
    char message[200];
    int score = 95;
    char grade = 'A';
    
    sprintf(message, "Your score is %d, which gives you a grade of %c", score, grade);
    
    printf("%s\n", message);
    
    return 0;
}
```

### Output:
```text
Your score is 95, which gives you a grade of A
```

### Example 3: Multiple Data Types
```c
#include <stdio.h>

int main()
{
    char result[256];
    int day = 31;
    char month[] = "March";
    int year = 2026;
    float temperature = 25.5;
    
    sprintf(result, "Date: %s %d, %d | Temperature: %.1f°C", month, day, year, temperature);
    
    printf("%s\n", result);
    
    return 0;
}
```

### output:
```text
Date: March 31, 2026 | Temperature: 25.5°C
```

### Example 4: Building a File Path
```c
#include <stdio.h>

int main()
{
    char path[200];
    char folder[] = "Documents";
    char filename[] = "data.txt";
    
    sprintf(path, "C:\\Users\\%s\\%s\\%s", "ASUS", folder, filename);
    
    printf("Full path: %s\n", path);
    
    // Now you can use this path with fopen
    FILE *file = fopen(path, "r");
    
    if (file == NULL)
    {
        printf("File not found\n");
    }
    else
    {
        printf("File opened successfully!\n");
        fclose(file);
    }
    
    return 0;
}
```

**Note:** sprintf() does NOT check if the buffer is large enough. This can cause buffer overflow!

## 2. fprintf()
**"file printf"**, is a **formatted output function** that writes formatted data to a **file** (or any output stream) instead of printing to the screen. It's like printf(), but the output goes to a file.

```c
int fprintf(FILE *stream, const char *format, ...);
```

* **stream:** File pointer (where to write)
* **format:** Format string (like printf())
* **...:** Variables to be formatted
* **Returns:** 	Number of characters written, or negative value on error

### Example 1: Writing to a file
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("output.txt", "w");
    
    if (file == NULL)
    {
        printf("Error creating file\n");
        return 1;
    }
    
    fprintf(file, "Hello, World!\n");
    fprintf(file, "The answer is: %d\n", 42);
    fprintf(file, "Pi is approximately: %.2f\n", 3.14159);
    
    fclose(file);
    
    printf("Data written to output.txt\n");
    
    return 0;
}
```

### File output.txt content:
```text
Hello, World!
The answer is: 42
Pi is approximately: 3.14
```

### Example 2: Writing to stderr
fprintf() can also write to stderr (standard error) to display error messages:

```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("nonexistent.txt", "r");
    
    if (file == NULL)
    {
        fprintf(stderr, "Error: Could not open file\n");
        return 1;
    }
    
    fclose(file);
    
    return 0;
}
```

### Example 3: Writing Multiple Data Types
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("students.txt", "w");
    
    if (file == NULL)
    {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }
    
    int id = 101;
    char name[] = "Jason";
    float gpa = 3.85;
    char grade = 'A';
    
    fprintf(file, "Student Information:\n");
    fprintf(file, "-------------------\n");
    fprintf(file, "ID: %d\n", id);
    fprintf(file, "Name: %s\n", name);
    fprintf(file, "GPA: %.2f\n", gpa);
    fprintf(file, "Grade: %c\n", grade);
    
    fclose(file);
    
    return 0;
}
```

### File students.txt content:
```text
Student Information:
-------------------
ID: 101
Name: Jason
GPA: 3.85
Grade: A
```

## 4. sscanf()
**"string scan formatted"**, is a **formatted input function** that reads formatted data from a **string** (character array) instead of from the keyboard or a file. It's like scanf(), but the input comes from a string.

```c
int sscanf(const char *str, const char *format, ...);
```

* **str:** Source string to parse
* **format:** Format string specifying expected input
* **...:** Pointers to variables where values will be stored
* **Returns:** 	Number of successfully matched and assigned items, or EOF on error

### Example 1: Parsing a Simple String
```c
#include <stdio.h>

int main()
{
    char data[] = "Jason 25 85.5";
    char name[50];
    int age;
    float score;
    
    sscanf(data, "%s %d %f", name, &age, &score);
    
    printf("Name: %s\n", name);
    printf("Age: %d\n", age);
    printf("Score: %.1f\n", score);
    
    return 0;
}
```

### Output:
```text
Name: Jason
Age: 25
Score: 85.5
```

### Example 2: Parsing Different Data Types
```c
#include <stdio.h>

int main()
{
    char input[] = "ID:101, Name:Jason, GPA:3.85";
    int id;
    char name[50];
    float gpa;
    
    sscanf(input, "ID:%d, Name:%s, GPA:%f", &id, name, &gpa);
    
    printf("ID: %d\n", id);
    printf("Name: %s\n", name);
    printf("GPA: %.2f\n", gpa);
    
    return 0;
}
```

### Output:
```text
ID: 101
Name: Jason
GPA: 3.85
```

### Example 3: Parsing CSV Line
```c
#include <stdio.h>

int main()
{
    char csvLine[] = "101,Jason,25,New York";
    int id;
    char name[50];
    int age;
    char city[50];
    
    sscanf(csvLine, "%d,%[^,],%d,%[^\n]", &id, name, &age, city);
    
    printf("ID: %d\n", id);
    printf("Name: %s\n", name);
    printf("Age: %d\n", age);
    printf("City: %s\n", city);
    
    return 0;
}
```

### Output:
```text
ID: 101
Name: Jason
Age: 25
City: New York
```






