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
