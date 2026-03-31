## Output
data going **out** of your program

## 1. putc()
is a **character output function** that writes **one single character** to a file or to the screen.

```c
int putc(int char, FILE *fp);
```

* **char:** 	The character you want to write
* **fp:** Where to write it (file pointer or stdout)

**How it works:**

* **1.** You give it a **character** to write
* **2.** You tell it **where** to write (file or screen)
* **3.** It writes **one character** to that location

### Example 1: Writing to Screen
```c
putc('A', stdout);  // prints A to screen
putc('\n', stdout); // prints newline to screen
```

### Example 2: Writing to a File
```c
FILE *file = fopen("output.txt", "w");
putc('B', file);    // writes B to the file
putc('\n', file);   // writes newline to the file
fclose(file);
```

## 2. putchar()
is a **character output function** that writes **one single character** to **standard output (stdout)** — usually the screen.

```c
int putchar(int char);
```

* **char:**  the character you want to print
* **Returns:** the character written on success, or EOF on failure

**Note:** putchar(ch) is equivalent to putc(ch, stdout) — just a shorter, more convenient version.

### Example 1: "Printing 'Hi!' Using putchar()
```c
#include <stdio.h>

int main() {
    putchar('H');
    putchar('i');
    putchar('!');
    putchar('\n');
    
    return 0;
}
```

### Output:
```text
Hi!
```

### Example 2: Printing a String While Omitting Newline Characters Using putchar()
```c
#include <stdio.h>

int main()
{
    char string [] = "Hello Jason, \nwhatever!";
    int i = 0;

    while( string[i] != '\0')
    {
        if ( string[i] != '\n')
        {
            putchar(string[i]);
            i++;
        }
    }
    return 0;
}
```

### Output:
```text
Hello Jason, whatever!
```

