## Input 
Data coming **into** your program

## 1. getc()
is a **character input function** that reads **one single character** from a file or input stream.

```c
int getc(FILE *stream);
```

* **stream:** where to read from (can be stdin, a file, etc.)
* **Returns:** the character read as an int, or EOF if an error occurs or end-of-file is reached

### Example 1: Reading from Keyboard
```c
#include <stdio.h>

int main() {
    char ch;
    
    printf("Enter a character: ");
    ch = getc(stdin);  // reads one character from keyboard
    
    printf("You entered: %c\n", ch);
    
    return 0;
}
```

### Example 2: Reading from a File
```c
#include <stdio.h>

int main() {
    FILE *file = fopen("data.txt", "r");
    char ch;
    
    if (file != NULL) {
        ch = getc(file);  // reads one character from file
        printf("First character in file: %c\n", ch);
        fclose(file);
    }
    
    return 0;
}
```

## 2. getchar()
is a **character input function** that reads **one single character** from **standard input (stdin)** — usually the keyboard.

```c
int getchar(void);
```

* **No arguments:** it always reads from stdin
* **Returns:** the character read as an int, or EOF if an error occurs or end-of-file is reached

**Note:** getchar() is equivalent to getc(stdin) — just a shorter, more convenient version.

### Example

```c
#include <stdio.h>

int main() {
    char ch;
    
    printf("Enter a character: ");
    ch = getchar();  // reads one character from keyboard
    
    printf("You entered: %c\n", ch);
    
    return 0;
}
```

## 3. fgetc()
is a **character input function** that reads **one single character** from **any input stream** (file, stdin, etc.). It's the **safe, standard version** of character reading.

```c
int fgetc(FILE *stream);
```

* **stream:** where to read from (file pointer or stdin)
* **returns:** the character read as an int, or EOF if an error occurs or end-of-file is reached

**Note:** In practice, they behave similarly, but fgetc() is guaranteed to be a function, while getc() might be a macro.

### Example 1: Reading from a File

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("data.txt", "r");
    char ch;
    
    if (file != NULL) {
        ch = fgetc(file);  // reads one character from file
        printf("First character: %c\n", ch);
        fclose(file);
    }
    
    return 0;
}
```

### Example 2:  Reading from Keyboard

```c
#include <stdio.h>

int main() {
    char ch;
    
    printf("Enter a character: ");
    ch = fgetc(stdin);  // reads one character from keyboard
    
    printf("You entered: %c\n", ch);
    
    return 0;
}
```

## 4. ungetc()
is a **character input function** that **pushes a character back** into the input stream. It allows you to "un-read" a character so it can be read again later.

```c
int ungetc(int char, FILE *stream);
```

* **char:** the character to push back into the stream

* **stream:** the input stream (file or stdin)

* **Returns:** the character pushed back on success, or EOF on failure

### Example 1: 

```c
#include <stdio.h>

int main() {
  char ch = 0;

  while ( isspace(ch = (char)getchar())); // Read as long as there are space
  ungetc(ch, stdin); // Put back the nonspace charcater

  printf("Char is %c\n", getchar());
  return 0;
}
```

### Example 2: Reading Until a Digit
```c
#include <stdio.h>
#include <ctype.h>

int main() {
    char ch;
    
    printf("Enter something: ");
    ch = getchar();
    
    if (!isdigit(ch)) {
        // Not a digit — put it back
        ungetc(ch, stdin);
        printf("That wasn't a digit. First character returned to stream.\n");
    } else {
        printf("You entered a digit: %c\n", ch);
    }
    
    return 0;
}
```

### Example 3: Peeking at the Next Character
```c
#include <stdio.h>

int main() {
    char ch;
    
    printf("Enter a character: ");
    ch = getchar();  // read first character
    
    printf("You entered: %c\n", ch);
    
    // Put it back
    ungetc(ch, stdin);
    
    // Read it again
    ch = getchar();
    printf("Read again: %c\n", ch);
    
    return 0;
}
```

### Output:

```text
Enter a character: A
You entered: A
Read again: A
```


