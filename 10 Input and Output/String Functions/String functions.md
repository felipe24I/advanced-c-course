## 1. gets()
**"get string"**, is a **string input function** that reads a line of text from **standard input (stdin)** — usually the keyboard — and stores it in a character array.

```c
char *gets (char *str);
```

* **str:** pointer to a character array where the input will be stored
* **Returns:** str on success, or NULL on error or end-of-file

### Example
```c
#include <stdio.h>

int main()
{
    char name[100];
    
    printf("Enter your name: ");
    gets(name);
    
    printf("Hello, %s!\n", name);
    
    return 0;
}
```

### Output
```text
Enter your name: Jason
Hello, Jason!
```

**Note:** **gets()** is extremely dangerous and has been removed from the C standard (since C11).

**Note:** In C, an **array name** (like **name**) already acts as a **pointer** to the first element of the array.

**Note:** The function **gets()** expects a char* (pointer to char)

* **name:** char*
* **&name:** char (*)[100] (pointer to array of 100 chars) 

## 2. fgets()
**"file get string"**, is a **safe string input function** that reads a line of text from **any input stream** (file, keyboard, etc.) and stores it in a character array. It's the **safe replacement** for the dangerous gets() function.

```c
char *fgets(char *str, int n, FILE *stream);
```

* **str:** Pointer to character array where input will be stored
* **n:** Maximum number of characters to read (including null terminator)
* **stream:** Input source (stdin for keyboard, or file pointer)
* **Returns:** str on success, NULL on error or end-of-file

**Note:** fgets() includes the newline character in the string

### Example 1: Reading from Keyboard
```c
#include <stdio.h>

int main()
{
    char name[100];
    
    printf("Enter your name: ");
    fgets(name, sizeof(name), stdin);
    
    printf("Hello, %s!", name);
    
    return 0;
}
```

### Output:
```text
Enter your name: Jason
Hello, Jason!
```

### Example 2: Reading from a File
```c
#include <stdio.h>

int main()
{
    FILE *file = fopen("data.txt", "r");
    char line[200];
    
    if (file == NULL)
    {
        printf("Error opening file\n");
        return 1;
    }
    
    // Read and print each line
    while (fgets(line, sizeof(line), file) != NULL)
    {
        printf("%s", line);
    }
    
    fclose(file);
    
    return 0;
}
```

### How to remove the newline = using strcspn()
```c
#include <stdio.h>
#include <string.h>

int main()
{
    char name[100];
    
    printf("Enter your name: ");
    fgets(name, sizeof(name), stdin);
    
    // Remove newline
    name[strcspn(name, "\n")] = '\0';
    
    printf("Hello, %s!\n", name);
    
    return 0;
}
```

## 3. getline()
is a **powerful string input function** that reads an entire line of text from any input stream. Unlike fgets(), it **automatically allocates memory** for the string, so there's no buffer size limit.

```c
ssize_t getline(char **lineptr, size_t *n, FILE *stream);
```

* **lineptr:** Pointer to a character pointer (will be allocated/reallocated)
* **n:** Pointer to a size_t variable storing the allocated size
* **stream:** Input source (stdin for keyboard, or file pointer)
* **Returns:** Number of characters read (including newline, excluding null), or -1 on error/EOF

**Note:** getline() is not part of standard C — it's a POSIX/GNU extension (available on Linux, Mac, and Unix systems). On Windows (Code::Blocks/MinGW), it may not be available.

### Example 1: Reading from Keyboard
```c
#define _GNU_SOURCE
#include <stdio.h>
#include <stdlib.h>

int main()
{
    char *line = NULL;  // getline will allocate memory
    size_t len = 0;     // size of allocated buffer
    ssize_t read;
    
    printf("Enter some text: ");
    read = getline(&line, &len, stdin);
    
    if (read != -1)
    {
        printf("You entered: %s", line);
        printf("Number of characters read: %zd\n", read);
        printf("Buffer size allocated: %zu\n", len);
    }
    
    free(line);  // Don't forget to free the memory!
    
    return 0;
}
```

### Output:
```text
Enter some text: Hello World!
You entered: Hello World!
Number of characters read: 13
Buffer size allocated: 120
```

### Example 2: Reading from a File
```c
#define _GNU_SOURCE
#include <stdio.h>
#include <stdlib.h>

int main()
{
    FILE *file = fopen("example.txt", "r");
    char *line = NULL;
    size_t len = 0;
    ssize_t read;
    int lineNum = 1;
    
    if (file == NULL)
    {
        printf("Error opening file\n");
        return 1;
    }
    
    while ((read = getline(&line, &len, file)) != -1)
    {
        printf("Line %d: %s", lineNum++, line);
        printf("  (Read %zd characters, buffer size: %zu)\n", read, len);
    }
    
    free(line);
    fclose(file);
    
    return 0;
}
```

## 4. puts()
is a **string output function** that writes a string to **standard output (stdout)** — usually the screen — and **automatically adds a newline** at the end.

```c
int puts(const char *str);
```

* **str:** Pointer to the string to be printed
* **Returns:** Non-negative value on success, or **EOF** on failure

### Example 1: Basic Usage
```c
#include <stdio.h>

int main()
{
    puts("Hello, World!");
    puts("This is a new line");
    
    return 0;
}
```

### Example 2: Printing an Array of Strings
```c
#include <stdio.h>

int main()
{
    char *fruits[] = {"Apple", "Banana", "Cherry", "Date", NULL};
    int i = 0;
    
    while (fruits[i] != NULL)
    {
        puts(fruits[i]);
        i++;
    }
    
    return 0;
}
```

### Output:
```text
Apple
Banana
Cherry
Date
```



