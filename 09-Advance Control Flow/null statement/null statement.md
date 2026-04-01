## Null statement
is a statement that performs no operation. It's represented by a single semicolon (;)

```c
;  // Null statement - does nothing
```

### Example 1: Empty Loop Bodies
```c
// Skip characters until newline
while (getchar() != '\n')
    ;  // Null statement - loop body does nothing

// Or more clearly with braces:
while (getchar() != '\n') {
    // Empty body
}
```

### Example 2: In For Lopps
```c
// All work done in the loop header
for (int i = 0; i < strlen(str); i++, process_char(str[i]))
    ;  // Null statement

// Copy string using pointers
char *src = "Hello", *dest = buffer;
while (*dest++ = *src++)
    ;  // Null statement - assignment happens in condition
```

