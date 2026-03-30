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
´´´c
putc('A', stdout);  // prints A to screen
putc('\n', stdout); // prints newline to screen
´´´

### Example 2: Writing to a File
´´´c
FILE *file = fopen("output.txt", "w");
putc('B', file);    // writes B to the file
putc('\n', file);   // writes newline to the file
fclose(file);
´´´

