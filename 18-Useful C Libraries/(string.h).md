## string.h
string.h is a standard header file in the C programming language that provides a set of functions for manipulating character arrays (strings) and memory blocks

### Key Aspects of <string.h>:
- **String Manipulation:** It contains functions to copy, concatenate, compare, and search null-terminated strings.
- **Memory Management:** Despite its name, the header also includes functions to manipulate generic memory blocks (e.g., copying or filling raw memory).
- **Usage:** It is included in a C program using the preprocessor directive: #include <string.h>

### Commonly Used Functions in <string.h>:
- **strlen(str):** Calculates the length of a string, excluding the null terminator.
- **strcpy(dest, src):** Copies a string from the source to the destination.
- **strcat(dest, src):** Appends (concatenates) one string to the end of another.
- **strcmp(str1, str2):** Compares two strings lexicographically.
- **strchr(str, c):** Finds the first occurrence of a character c in a string.
- **strstr(str1, str2):** Finds the first occurrence of a substring str2 in string str1.

### 1. memcpy()
```c
void *memcpy(void *dest, const void *src, size_t n);
```

Copies **n bytes** from source to destination

#### Example
```c
int a[3] = {1, 2, 3};
int b[3];

memcpy(b, a, 3 * sizeof(int));
```

#### VERY IMPORTANT
Does NOT handle overlapping memory

#### Wrong usage
```c
memcpy(arr + 1, arr, 3);  // undefined behavior
```

### 2. memmove()
```c
void *memmove(void *dest, const void *src, size_t n);
```

Same as memcpy, but **safe for overlapping memory**

#### Example
```c
memmove(arr + 1, arr, 3 * sizeof(int));  // safe
```

### 3. strdup()
```c
char *strdup(const char *s);
```

Creates a **copy of a string in heap memory**

#### Example
```c
char *copy = strdup("hello");
```

#### Important
- Uses malloc() internally
- You must free() it

```c
free(copy);
```

#### Equivalent to:
```c
char *copy = malloc(strlen(s) + 1);
strcpy(copy, s);
```

### 4. strndup()
```c
char *strndup(const char *s, size_t n);
```

Copies **at most n characters** into a new string

#### Example
```c
char *copy = strndup("hello", 3);
```

#### Output
```text
"hel"
```

#### Notes
- Always null-terminated
- Must free() it

## Notes
### size_t
**size_t** is an unsigned integer data type defined in several header files (like <stddef.h>, <stdio.h>, or <string.h>).
It is specifically designed to represent the size of objects in memory.

### NULL
macro is the value of a null pointer constant
