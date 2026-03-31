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
