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
