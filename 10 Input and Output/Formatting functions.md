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

### 5. fscanf() vs fgets() + sscanf()
* **fscanf():** Reads and parses directly from file in one step
* **fgets() + sscanf():** Reads a line first, then parses it

### Example 1: Reading Integers from File
#### Using fscanf() Only
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("numbers.txt", "r");
    int a, b, c;
    
    if (file == NULL)
    {
        printf("Error opening file\n");
        return 1;
    }
    
    // Read directly
    fscanf(file, "%d %d %d", &a, &b, &c);
    printf("Read: %d, %d, %d\n", a, b, c);
    
    fclose(file);
    return 0;
}
```

#### Using fgets() + sscanf()
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("numbers.txt", "r");
    char line[256];
    int a, b, c;
    
    if (file == NULL)
    {
        printf("Error opening file\n");
        return 1;
    }
    
    // Read line first
    if (fgets(line, sizeof(line), file) != NULL)
    {
        // Then parse
        if (sscanf(line, "%d %d %d", &a, &b, &c) == 3)
        {
            printf("Read: %d, %d, %d\n", a, b, c);
        }
        else
        {
            printf("Parse error! Line: %s", line);
        }
    }
    
    fclose(file);
    return 0;
}
```

### Example 2: Error Handling with Bad Data
#### File with Bad Data
```text
10 20 abc
30 40 50
```

#### Using fscanf() (Hard to Recover)
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("data.txt", "r");
    int a, b, c;
    int result;
    
    while (1)
    {
        result = fscanf(file, "%d %d %d", &a, &b, &c);
        
        if (result == EOF) break;
        
        if (result == 3)
        {
            printf("Good: %d %d %d\n", a, b, c);
        }
        else
        {
            printf("Bad data! Can't recover easily\n");
            break;  // Stops reading
        }
    }
    
    fclose(file);
    return 0;
}
```

#### output:
```text
Good: 10 20 30
Bad data! Can't recover easily
```

The second good line is never read!

#### Using fgets() + sscanf() (Easy Recovery)
```c
#include <stdio.h>
#include <string.h>

int main()
{
    FILE *file = fopen("data.txt", "r");
    char line[256];
    int a, b, c;
    int lineNum = 0;
    
    while (fgets(line, sizeof(line), file) != NULL)
    {
        lineNum++;
        
        // Remove newline for clean display
        line[strcspn(line, "\n")] = '\0';
        
        if (sscanf(line, "%d %d %d", &a, &b, &c) == 3)
        {
            printf("Line %d (GOOD): %d %d %d\n", lineNum, a, b, c);
        }
        else
        {
            printf("Line %d (BAD): '%s' - Skipping\n", lineNum, line);
            // Continue reading next lines!
        }
    }
    
    fclose(file);
    return 0;
}
```

#### Output
```text
Line 1 (GOOD): 10 20 30
Line 2 (BAD): '10 20 abc' - Skipping
Line 3 (GOOD): 30 40 50
```

All lines processed, bad line skipped!

## 6. fflush()
is a function that **flushes** (forces writing) the output buffer of a stream. It ensures that any data waiting to be written is immediately sent to the destination (file, screen, etc.).

```c
int fflush(FILE *stream);
```

* **stream:** File pointer to flush (or NULL to flush all output streams)
* **Returns:** 0 on success, EOF on error

**When to Use fflush()**
* **Displaying progress bars:** Yes
* **Interactive prompts:** Yes (ensure prompt appears before input)
* **Before critical operations:** Yes (ensure data is saved)
* **Before program crash risk:** Yes
* **Normal output:** Not necessary (flushed automatically)
* **Clearing input buffer:** NEVER (undefined behavior)

### Example 1: Without fflush() (May Not Show Immediately)
```c
#include <stdio.h>
#include <unistd.h>  // for sleep()

int main()
{
    printf("Loading");
    
    for (int i = 0; i < 5; i++)
    {
        printf(".");
        sleep(1);  // Wait 1 second
    }
    
    printf("\nDone!\n");
    
    return 0;
}
```

**Issue:** All dots may appear at once at the end because of buffering!

### Example 2: With fflush() (Shows Progress)
```c
#include <stdio.h>
#include <unistd.h>  // for sleep()

int main()
{
    printf("Loading");
    fflush(stdout);  // Force "Loading" to appear
    
    for (int i = 0; i < 5; i++)
    {
        printf(".");
        fflush(stdout);  // Force each dot to appear immediately
        sleep(1);
    }
    
    printf("\nDone!\n");
    fflush(stdout);
    
    return 0;
}
```

### Output (appears progressively):
```text
Loading.....
Done!
```

Each dot appears one by one!

### Example 3: Flushing a File
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("output.txt", "w");
    
    if (file == NULL)
    {
        printf("Error opening file\n");
        return 1;
    }
    
    fprintf(file, "Writing data...\n");
    fflush(file);  // Force data to be written to disk NOW
    
    // Data is guaranteed to be on disk even if program crashes
    
    fprintf(file, "More data...\n");
    
    fclose(file);  // fclose automatically flushes
    
    return 0;
}
```
